# go mod

## go mod
go env -w GO111MODULE=on/off/auto
go env -w GOPROXY=https://goproxy.cn,direct  //https://proxy.golang.org
go mod init
go get package@version
go mod vendor
go build

## 相关
https://learnku.com/go/t/39086
https://studygolang.com/articles/13895?from=singlemessage
https://zhuanlan.zhihu.com/p/92992277?utm_source=wechat_session

1.download
下载 modules 到本地缓存

2.edit
提供一种命令行交互修改 go.mod 的方式

3.graph
将 module 的依赖图在命令行打印出来，其实并不是很直观

4.init
初始化 modules，会生成一个 go.mod 文件

5.tidy
清理 go.mod 中的依赖，会添加缺失的依赖，同时移除没有用到的依赖

6.vendor
将依赖包打包拷贝到项目的 vendor 目录下，值得注意的是并不会将 test code 中的依赖包打包到 vendor 中。这种设计在社区也引起过几次争论，但是并没有达成一致。

7.verify
verify 用来检测依赖包自下载之后是否被改动过。

8.why
解释为什么 package 或者 module 是需要，但是看上去解释的理由并不是非常的直观。

