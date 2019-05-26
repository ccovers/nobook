# sysctl


## 设置内核网络功能
```
#查看内核网络设置
sysctl -a

#设置监听队列大小
sysctl -w net.ipv4.tcp_max_syn_backlog=204800

#设置syn cookies，监听队列溢出时回复cookies
sysctl -w net.ipv4.tcp_syncookies=1

#设置syn ack重传次数
sysctl -w net.ipv4.tcp_synack_retries=2

#设置syn重传次数
sysctl -w net.ipv4.tcp_syn_retries=3

#设置保活的时间
sysctl -w net.ipv4.tcp_keepalive_time=1800

#设置keepalive探测次数
sysctl -w net.ipv4.tcp_keepalive_probes=5

#设置keepalive探测频率（秒）
sysctl -w net.ipv4.tcp_keepalive_intvl=15

#设置tcp请求放弃前重传次数
sysctl -w net.ipv4.tcp_retries1=3

#设置tcp连接丢弃前重传次数
sysctl -w net.ipv4.tcp_retries2=5

#设置服务超载时，内核将主动地发送RST包
sysctl -w net.ipv4.tcp_abort_on_overflow=1

#重新加载配置的内核功能
sysctl -p
```