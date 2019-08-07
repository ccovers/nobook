# FastDFS

## 系统介绍
分布式文件系统采用的是`FastDFS`，该系统是一个开源的轻量级分布式文件系统，它对
文件进行管理，功能包括：文件存储、文件同步、文件访问（上传、下载）等，解决了大容
量存储和负载均衡的问题。特别适合以文件为载体的在线服务，如相册网站、视屏网站等等。

## 组成
1. FastDFS 有两个角色：跟踪服务（`tracker`）和存储服务（`storage`）。
    1. 跟踪服务负责控制、调度文件以及负载均衡的方式访问。
    2. 存储服务负责文件存储、文件同步、提供文件访问接口，同时以`key-value`的方式管理元
数据。（跟踪和存储服务可以由一台或多台服务器组成，同时可以动态的添加、删除跟踪和存储服
务而不会对在线的服务产生影响，在集群中`tracker`服务是对等的。）
2. 存储服务中的group
    1. 存储系统是由一个或多个`group`组成，`group`与`group`之间是相互独立的，所有`group`的文件容量累加起来就是整个存储系统中的文件容量。
    2. 一个`group`可以由一台或多台存储服务器组成，一个`group`下的存储服务器中的文件是相同的，`group`中的多台服务器起到了冗余备份和负载均衡的作用。
    3. 在`group`中增加服务时，同步已有的文件由系统自动完成，同步完成后，系统自动将新增服务器切换到线上提供服务。
    4. 当存储空间不足或即将耗尽时，可以动态添加group。只需要增加一台或多台服务器，并将它们配置为一个新的`group`，这样就扩大了存储系统的容量。
3. 常用的部署框架结构
    1. `tracker`部署了两个，而`storage`部署了6 个分为A、B、C 三组。
    2. 其中每一组中的两个存储服务器（如A1和A2）之间是相互数据同步的。
            tracker_ABC1       tracker_ABC2
    storage_A_1     storage_B_1     storage_C_1
    storage_A_2     storage_B_2     storage_C_2

## 正式环境部署框架结构
1. 在实际环境当中，xx的应用服务需要在全世界各地进行部署，为了满足数据的实时性要求，采用的部署方式采用`tracker`和`storage`一对一的部署方式。
    1. 在每一个服务器上分别部署一个`tracker`和`storage`，当在本地上传文件时优先保存在本地，再同步至同组的`storage`。
    2. 并且由于`storage`与每一个`tracker`相连，因此，在其它地域，通过`tracker`也能索引到任一`storage`的文件。
    3. 任一个`tracker`或`storage`宕掉，都不会对其它服务造成影响，后续服务也可以自由增减。

## 文件操作
1. 客户端首先向tracker 发起文件操作的请求；
    1. ·tracker`根据请求的`group`名称，返回该`group`中可操作的一个`storage`。
    2. 客户端得到storage 地址之后，向该storage 发起请求，storage 再执行操作。
    3. `storage`执行完毕之后，再将执行的操作同步给同一`group`的其它`storage`。

## 配置
1. 分布式文件系统配置不同，具体运行方式也不一样，为了保证文件存储、访问的最优方
式，需要进行仔细配置。 配置文件一共有三个`client.conf`、`storage.conf`、`tracker.conf`，分别是客户端、存储服务、跟踪服务对应的配置文件。
    1. 跟踪服务器配置文件`tracker.conf`需要根据实际环境修改以下数据，其余的默认即可:
    ```
    disabled=false #启用配置文件
    port=22122 #tracker 服务端口
    base_path=/opt/fastdfs/tracker/data-log #tracker 基础数据存储路径及日志存放路径
    store_lookup=1 #选择上传文件模式0 代表group 轮询1 指定特定group 2 选择空间最
    大的group
    store_group=group_name #上传文件组，如果模式为1，则必须设置成核特定group 一致的
    组名
    store_server=0 #选择哪块存储盘上传文件0 代表轮询，1 代表按照IP 排序，排在第一的
    server，2 代表优先最大存储空间盘(路径)
    store_path=0 #选择哪块存储盘上传文件0 代表轮询，2 代表优先最大存储空间盘(路径)
    download_server=1 #选择哪台存储服务器下载文件0 代表轮询，1 代表当前文件上传的源
    服务器
    reserved_storage_space=10% #系统保留存储空间10%
    rotate_error_log=true #是否定期轮转error log，目前仅支持一天轮转一次
    error_log_rotate_time=00:00 #如果按天轮转错误日志，具体生成新错误日志文件的时间
    rotate_error_log_size=0 #是否在错误日志文件达到一定大小时生成新的错误日志文件( 0 代
    表对日志文件大小不敏感)
    log_file_keep_days=7 #日志文件保存日期
    ```

    2. 存储服务器配置文件`storage.conf`需要根据实际环境修改以下数据，其余的默认即可
    ```
    disabled=false #这个配置文件是否失效
    group_name=group_name #本storage server 所属的group 名
    port=23000 #storage server 监听端口
    #指定tracker服务器有多个可以分行列出
    tracker_server=x.x.x.x:22122
    tracker_server=x.x.x.x:22122
    base_path=/opt/fastdfs/storage/data-log #工作文件夹，日志也存在此(这里不是上传的文件
    存放的地址)
    store_path0=/opt/fastdfs/storage/images-data #工作路径列表，如果store_path0 不设置，那
    么使用base_path 存储
    rotate_error_log=true #是否定期轮转错误日志，目前仅支持一天轮转一次
    error_log_rotate_time=00:00 #如果按天轮转错误日志，具体生成新错误日志文件的时间
    rotate_access_log_size=0 #是否在错误访问文件达到一定大小时生成新的访问日志文件0
    代表对日志文件大小不敏感
    rotate_error_log_size=0 #是否在错误日志文件达到一定大小时生成新的错误日志文件0 代
    表对日志文件大小不敏感
    log_file_keep_days=7 #日志文件保存日期0 表示永久保存，不删除
    ```

    
    3. 客户端配置文件`client.conf`只需要修改跟踪服务器地址，一般指定为延时最少的跟踪服务器，如果
    本地存在跟踪服务器则可指定为本地地址（不能指定为127.0.0.1）
    ```
    #指定tracker服务器有多个可以分行列出
    tracker_server=x.x.x.x:22122 
    base_path=/opt/fastdfs/client/data-log #工作文件夹，日志存在此
    ```

## Fastdfs操作
1. 启动和停止
    1. 启动和停止跟踪服务器
    ```
    /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf
    /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf stop
    ```
    2. 启动和停止存储服务器
    ```
    /usr/bin/fdfs_storaged /etc/fdfs/storage.conf
    /usr/bin/fdfs_storaged /etc/fdfs/storage.conf stop
    ```
3. 运行如下命令可查看存储服务器状态：
    ```
    /usr/bin/fdfs_monitor /etc/fdfs/client.conf （正常状态下storage server 的状态的应该为ACTIVE）

    FASTDFS的存储服务器的状态存在七种情况：
    FDFS_STORAGE_STATUS：INIT :初始化，尚未得到同步已有数据的源服务器
    FDFS_STORAGE_STATUS：WAIT_SYNC :等待同步，已得到同步已有数据的源服务器
    FDFS_STORAGE_STATUS：SYNCING :同步中
    FDFS_STORAGE_STATUS：DELETED :已删除，该服务器从本组中摘除
    FDFS_STORAGE_STATUS：OFFLINE :离线
    FDFS_STORAGE_STATUS：ONLINE :在线，尚不能提供服务
    FDFS_STORAGE_STATUS：ACTIVE :在线，可以提供服务
    ```
    
3. 删除存储服务器节点
    1. 当需要从分布式系统当中删除一个存储节点时，应首先停止指定存储服务器，然后使用如下命令在`tracker`中删除：
    ```
    /usr/bin/fdfs_monitor /etc/fdfs/client.conf delete group_name tracker_ip
    
    group_name：要删除的存储服务节点所属于的group 名称
    tracker_ip：一个跟踪服务器的地址
    ```