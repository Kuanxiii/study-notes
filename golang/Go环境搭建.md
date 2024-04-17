# Go环境搭建

## 环境说明

- Win版本: Windows11
- Go版本: 1.22.2

## 下载Go

[下载地址](https://golang.google.cn/dl/)

- 文本也提供一份: https://golang.google.cn/dl/
- 按着自己系统选择, 如有版本需求, 在`Stable versions`中选取即可
- 本系统选择`go1.22.2.windows-amd64.msi`
- 点击即下载

## 安装Go

- 双击安装包, 默认安装即可, go的安装选项没啥
- 如需改变安装位置, 在选安装位置的时候变变

## 环境变量配置

- 在系统环境变量中添加`GOROOT`和`GOPATH`, 并在Path中添加`%GOPATH%\bin`
- `GOROOT`: 就是Go的安装目录, 可以添加到系统环境变量中
- Path的话, 1.22.2版本的安装已经自动配上去了, 但不是以`%GOROOT%/bin`形式存在的, 想的话可以改一下, 换Go版本的时候可能方便点
- `GOPATH`: Go的工作目录, 遵循GoPath规则, Go默认是配在了用户变量里的用户目录下`%USERPROFILE%\go`, 可以自己修改
  - golang本身就是一个go项目, 可以看看Go的安装目录，也就知道了一个Go项目的完整体系结构
  - 创建`GOPATH`的目的是给go一个默认的输出(一般拉项目啥的..)
    - 比如在`go install xxx`时, 会在`GOPATH`的`bin`目录下生成可执行文件
    - 需要额外依赖某些三方库时, 也需要放在`GOPATH`的`lib`目录下供go使用
  - 以下为Go项目的结构说明, 自己写项目的时候也可以参考参考

  | 目录名 | 说明                                                      |
  | :----- | :-------------------------------------------------------- |
  | api    | 每个版本的 api 变更差异                                   |
  | bin    | 存放Go编译的二进制文件                                    |
  | doc    | 英文版的 Go 文档                                          |
  | lib    | 引用的一些库文件                                          |
  | misc   | 杂项用途的文件，例如 Android 平台的编译、git 的提交钩子等 |
  | pkg    | 存放Go编译的二进制文件                                    |
  | src    | 标准库的源码                                              |
  | test   | 测试用例                                                  |




## 测试

- 环境变量保存完以后(其实不保存也行..go都有默认的配置)
- 打开cmd或者terminal
  - 输入`go version`, 看是否打印出了版本号
  - 输入`go env`, 看是否打印出了环境变量

## 关于GOPROXY

- Go默认的GOPROXY的值是：`GOPROXY=https://proxy.golang.org,direct`
- 因为懂的都懂的原因..`go get`的时候会不成功
- 所以要改一下, 改成七牛的或者阿里的都行
  - `go env -w GOPROXY=https://mirrors.aliyun.com/goproxy`
  - `go env -w GOPROXY=https://goproxy.cn,direct`
- 重新执行`go env`, 看着变了就行

## 关于GO111MODULE...

- 看各种博客..关于GO111MODULE到底开不开的..最后也没个定论
- GO111MODULE影响的其实在于包管理相关
  - GO111MODULE = off
    - 强制 Go 表现出 GOPATH 方式，即使你的项目不在 GOPATH 目录里。
  - GO111MODULE = on
    - 将强制使用 Go 模块
- 在 Go 1.13 下（后）
  - 当存在 go.mod 文件时或处于 GOPATH 外， 其行为均会等同于 GO111MODULE=on。相当于 Go 1.13 下你可以将所有的代码仓库均不存储在 GOPATH 下。
  - 当项目目录处于 GOPATH 内，且没有 go.mod 文件存在时其行为会等同于 GO111MODULE=off。
- go.mod感觉应该是配套项目存在的, 就跟Maven项目的build.xml一样, 我觉得不用特意打开
