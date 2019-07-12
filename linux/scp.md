# scp

## 将本地文件拷贝远程
```
scp 文件名 用户名@计算机IP地址或名称:远程路径
scp -r ./common root@198.121.10.12:/root/package/
```

## 将远程文件拷贝到本地
```
scp 用户名@计算机IP地址或名称:远程路径 文件名
scp -r root@198.121.10.12:/root/package/common ./
```