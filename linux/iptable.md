# iptable

## 说明
1. `iptable`是`linux`一个命令行工具，将用户的安全设定执行到对应的安全框架`netfilter`，`iptable`能够完成封包过滤、封包重定向核网络地址转换`NAT`等功能。
2. `netfilter`是`linux`操作系统核心层内部的一个数据包处理模块，具有如下功能：
    1. 网络地址转换`Network Address Translate`
    2. 数据包内容修改
    3. 数据包过滤的防火墙功能
3. 更多[`iptable`](http://www.zsythink.net/archives/1199)

## 配置iptables防火墙
1. 初始化SRVINPUT链
```
iptables -F SRVINPUT             # 清空链
iptables -Z SRVINPUT             # 计算器清零
iptables -D INPUT -j SRVINPUT    # 删除链
iptables -X SRVINPUT             # 删除自定义空链，如果链内有规则，则无法删除
iptables -N SRVINPUT             # 添加SRVINUT链，并初始化

#允许外网每个IP最多100个tcp连接,超过的丢弃（网段掩码设置：--connlimit-mask 32）
iptables -A SRVINPUT -p tcp --syn -m connlimit --connlimit-above 100 -j DROP 
iptables -A SRVINPUT -p tcp -m state --state NEW -m recent --name NEWPOOL --set

#每个IP每秒只能连接10次，超过的丢包
iptables -A SRVINPUT -p tcp -m state --state NEW -m recent --name NEWPOOL --rcheck --seconds 1 --hitcount 10 -j DROP

#抵御DDOS ，允许外网最多5000个初始连接,然后服务器每秒新增5000个
iptables -A SRVINPUT -p tcp --syn -m limit --limit 5000/s --limit-burst 5000 -j ACCEPT
iptables -A SRVINPUT -p tcp --syn -j DROP

#限制icmp连接
iptables -A SRVINPUT -p icmp -m limit --limit 100/s --limit-burst 100 -j ACCEPT
iptables -A SRVINPUT -p icmp -j DROP

#子链处理完，返回父链
iptables -A SRVINPUT -j RETURN

#将新建的链添插入到INPUT链中
iptables -I INPUT -j SRVINPUT
```
2. 初始化SRVFORWARD链
```
iptables -F SRVFORWARD
iptables -Z SRVFORWARD
iptables -D FORWARD -j SRVFORWARD
iptables -X SRVFORWARD

#创建新的SRVFORWARD链
iptables -N SRVFORWARD

#只允许服务器内部每秒100个连接进行转发
iptables -A SRVFORWARD -p tcp --syn -m limit --limit 100/s -j ACCEPT
iptables -A SRVFORWARD -p tcp --syn -j DROP

#防止端口扫描
iptables -A SRVFORWARD -p tcp --tcp-flags SYN,ACK,FIN,RST RST -m limit --limit 100/s -j ACCEPT
iptables -A SRVFORWARD -p tcp --tcp-flags SYN,ACK,FIN,RST RST -j DROP

#防止死亡之ping
iptables -A SRVFORWARD -p icmp --icmp-type echo-request -m limit --limit 100/s -j ACCEPT
iptables -A SRVFORWARD -p icmp --icmp-type echo-request -j DROP

#子链处理完成，返回父链
iptables -A SRVFORWARD -j RETURN

#将新建的链插入到FORWARD链中
iptables -I FORWARD -j SRVFORWARD
```




