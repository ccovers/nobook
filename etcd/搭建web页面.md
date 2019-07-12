# 搭建web页面

## etcd-browser
### 安装
docker run --rm  -d --name etcd-browser -p 8000:8000 --env ETCD_HOST=45.40.194.140 --env ETCD_PORT=2379 buddho/etcd-browser
### 访问
http://127.0.0.1:8000/


## etcdkeeper （支持v3的api）
### 安装
docker run -it -d --name etcdkeeper -p 8080:8080 deltaprojects/etcdkeeper
### 访问
http://127.0.0.1:8080/etcdkeeper/