# gotest

## 测试覆盖率
1.输出
go test -run . -coverprofile=c.out

2.web展示
go tool cover -html=c.out


## 基准测试
go test -bench=.

## 剖析
1.输出
go test -cpuprofile=cpu.out
go test -blockprofile=block.out
go test -memprofile=mem.out

go test -bench=. -cpuprofile=cpu.out
go test -bench=. -blockprofile=block.out
go test -bench=. -memprofile=mem.out

2.分析
go tool pprof -text -nodecount=20 . cpu.out
go tool pprof -text -nodecount=20 . block.out
go tool pprof -text -nodecount=20 . mem.out

