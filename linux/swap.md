# linux命令swap

## 内存资源不足
```
程序环境是1核1G40G的低配云服务器
使用 npm install gitbook-cli -g 安装gitbook时报错
top 统计显示安装时 CPU、内存高占用

可能是因为资源不足导致的进程 kill，因此尝试用swap创建虚拟内存
```

## swap
- 检查swap空间
    swapon -s
- 创建swap分区文件
    dd if=/dev/zero of=/var/swapfile bs=1024 count=1048576
    (通过输入设备源 `/dev/zero` 输出0来初始化文件 `/var/swapfile`，每个block 1024b=1K，初始化1024*1024个，最终初始化一个1G的文件)
- 格式化新建的SWAP分区
    mkswap /var/swapfile
- 将swap文件变成swap分区
    swapon /var/swapfile
- 设置文件的权限，防止误操作
    chmod 600 /var/swapfile

1. 按时
    1. 山东省
        1. s
        2. 1
        3. w
    2. sd

[gitbook](http://www.chengweiyang.cn/gitbook/index.html)