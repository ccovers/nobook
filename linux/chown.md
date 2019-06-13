# chown

1. 改变拥有者和群组
```
chown mail:mail server.log
```

2. 改变文件拥有者和群组
```
chown root: server.log
```

3. 改变文件群组
```
　　chown :mail server.log
```

4. 改变指定目录以及其子目录下的所有文件的拥有者和群组
```
　　chown -R -v root:mail test6
```
5. 其它
```
-R 处理指定目录以及其子目录下的所有文件
-v 显示详细的处理信息
```
