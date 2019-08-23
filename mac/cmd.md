# cmd

## 查询`tcp`连接
netstat -an | grep LISTEN

## 通过端口查询进程
lsof -i tcp:8080