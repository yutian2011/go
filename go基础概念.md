<!-- TOC -->

- [1. go 基础概念](#1-go-基础概念)
    - [1.1. go 环境变量](#11-go-环境变量)
        - [1.1.1. 环境变量 GOROOT](#111-环境变量-goroot)
        - [1.1.2. 环境变量 GOPATH](#112-环境变量-gopath)
        - [1.1.3. go 环境变量详解](#113-go-环境变量详解)
    - [1.2. go 包管理](#12-go-包管理)
    - [1.3. go 命令](#13-go-命令)
        - [1.3.1. go env](#131-go-env)
        - [1.3.2. go get](#132-go-get)
        - [1.3.3. go fmt](#133-go-fmt)
        - [1.3.4. go list](#134-go-list)
        - [1.3.5. go build](#135-go-build)
        - [1.3.6. go install](#136-go-install)
        - [1.3.7. go run](#137-go-run)
        - [1.3.8. go clean](#138-go-clean)
    - [1.4. go module](#14-go-module)
        - [1.4.1. go mod 简单使用](#141-go-mod-简单使用)
        - [1.4.2. go.mod 文件](#142-gomod-文件)
    - [1.5. package](#15-package)
    - [开启module区别](#开启module区别)

<!-- /TOC -->



# 1. go 基础概念

## 1.1. go 环境变量

查看 go 环境变量命令.  

```
go env
```

go 环境变量与shell的环境变量不完全一样. 配置 go 环境变量尽量使用 go env 命令配置, 不要使用 export xxx命令处理 go 环境变量. 网上很多网页直接教人使用export, 了解不清楚的话, 说不定哪天自己把自己埋进去.    

shell 环境变量中必须设置的参数:  
下面两种方式, 找到 go 相关命令.  

```
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
# export PATH=$PATH:/usr/local/go/bin
```  

可选设置的环境参数:  
目的, shell找到 go 项目中的可执行文件/命令.  

```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
这里设置 GOPATH 的目的是为了让shell直接识别使用 go install 之后的放入$GOPATH/bin目录中的可执行文件/命令, 如果不加上述变量也是可以的, 执行相关文件/命令时填写好绝对/相对路径即可. 所以这里是非必须.   

如果shell变量中设置GOPATH时, 保持与 go env GOPATH 一致.  

其他的 go env 相关的变量 通过命令设置即可.  
```
go env -w xx=xx
```



### 1.1.1. 环境变量 GOROOT

go 解压的路径.  
将$GOROOT/bin 添加到PATH环境变量中.  
go 解压出来后, 也是一个工作目录, 里面有src lib等目录.   

### 1.1.2. 环境变量 GOPATH

简单理解为go开发的工作目录.  
GOPATH 是一系列路径的集合, 每个路径都是一个 project.   

下面这个图是有点误导性的, 不是在GOPATH路径下创建多个子目录就是多个工作区. 实际上, 可能由多个不同的路径构成了GOPATH.  

![](https://img2018.cnblogs.com/blog/800902/201812/800902-20181226221959762-982631114.png)  

GOPATH 最初目的解决引入声明, 管理源码, 编译文件, 依赖等问题.  
GOPATH 列出了查找go源码的路径.   
GOPATH 可以是以";"分割的字符串.  
GOPATH 有默认值:  $HOME/go  通过go env GOPATH 查看.  

pkg 目录保存了安装的包对象, 有一层系统架构层文件夹.  
bin 目录保存编译后的可执行文件.  

```

    GOPATH=/home/user/go

    /home/user/go/
        src/
            foo/
                bar/               (go code in package bar)
                    x.go
                quux/              (go code in package main)
                    y.go
        bin/
            quux                   (installed command)
        pkg/
            linux_amd64/
                foo/
                    bar.a          (installed package object)

```


重点: 最早GOPATH是承担了包管理的工作, 后面使用go module 代替 GOPATH 的包管理工作.  
所以很多用法跟以前是不一样了的.    
理解GOPATH的必要性, 在使用 go module 之后, GOPATH用于限定软件安装路径, 方便其他代码导入引用包.  

### 1.1.3. go 环境变量详解
[参考](https://juejin.im/post/5cac9b73e51d456e8c1d3bfc) 该链接即可, 不再重复.   
最基础的 GOPATH 与 GOROOT 已经论述过了.  


## 1.2. go 包管理
[参考](https://tonybai.com/2019/09/21/brief-history-of-go-package-management/) 该链接内容, 重点内容如下, 描述的很清楚了:  

>Go 1.11引入了对Go模块(module)的初步支持。下面摘自Go Wiki：
一个模块是一组相关的Go包的集合，这个包集合被当做一个独立的单元进行统一版本管理。模块精确记录了依赖要求并支持创建可复制的构建。

>Go模块带来了三个重要的内置功能：
go.mod文件，它与package.json或Pipfile文件的功能类似。
机器生成的传递依赖项描述文件 – go.sum。
不再有GOPATH限制。模块可以位于任何路径中。  



## 1.3. go 命令
```
Usage:

        go <command> [arguments]

The commands are:

        bug         start a bug report
        build       compile packages and dependencies
        clean       remove object files and cached files
        doc         show documentation for package or symbol
        env         print Go environment information
        fix         update packages to use new APIs
        fmt         gofmt (reformat) package sources
        generate    generate Go files by processing source
        get         add dependencies to current module and install them
        install     compile and install packages and dependencies
        list        list packages or modules
        mod         module maintenance
        run         compile and run Go program
        test        test packages
        tool        run specified go tool
        version     print Go version
        vet         report likely mistakes in packages

Use "go help <command>" for more information about a command.

Additional help topics:

        buildmode   build modes
        c           calling between Go and C
        cache       build and test caching
        environment environment variables
        filetype    file types
        go.mod      the go.mod file
        gopath      GOPATH environment variable
        gopath-get  legacy GOPATH go get
        goproxy     module proxy protocol
        importpath  import path syntax
        modules     modules, module versions, and more
        module-get  module-aware go get
        module-auth module authentication using go.sum
        module-private module configuration for non-public modules
        packages    package lists and patterns
        testflag    testing flags
        testfunc    testing functions

Use "go help <topic>" for more information about that topic.

```
### 1.3.1. go env
go env 打印或设置相关的参数. 

```
go env
go env NAME
go env -w NAME=VALUE  
```

### 1.3.2. go get
执行两个操作:  
- 远程下载包, 从master分支拉取
- 执行 go install  

这里涉及包管理相关内容. 现在不推荐使用 get 命令了, 只能拉取主分支, 没有版本管理. go mod开启会影响 go get使用. 推荐使用go mod 进行包管理.   

### 1.3.3. go fmt
格式化代码  

### 1.3.4. go list
默认用于输出 package/module 信息. 
-json 参数: 以json格式打印详细的信息.  
-m 参数, 列举modules信息, 而不是package信息. package管理源码文件的, module 是封装的 packages, packages 的管理.  
-u 参数; 显示可用的升级, 与-m一起使用.  
-versions; 与-m一起使用, 列举module所有版本.  

```
go list -json  XXX/all
go list -m -u -json all
go list -m -versions  github.com/gin-gonic/gin
```

### 1.3.5. go build
go 文件第一句都是 package xxx 开头.  

编译时读取xxx名字, 并不会安装.  
- 编译 package 时, 忽略_test.go结尾的文件. 
- 如果是编译目录下.go结尾的文件, 当做一个的包处理.  
- 当编译非main包时(与路径名一致的包), 只会编译, 不会有生成, 可作为编译测试.  
- 编译 main 包时, 生成相关可执行文件.  


### 1.3.6. go install 
当module功能没有开启时:  
将编译的文件和包安装在指定目录. 默认安装到 `$GOBIN` 目录下.  
如果GOBIN 为空时, 将会安装到 `$GOPATH/bin` 或者 `$GOROOT/bin` 目录下. 优先是 `$GOPATH/bin`    

当module功能开启时:  
对于库文件, 编译, 缓存, 但是没有安装, 测试没有找到对应的库文件放哪了.  



### 1.3.7. go run
编译并运行项目

### 1.3.8. go clean
清理对象文件和缓存

## 1.4. go module
go mod 命令如下:  
```
Usage:

        go mod <command> [arguments]

The commands are:

        download    download modules to local cache
        edit        edit go.mod from tools or scripts
        graph       print module requirement graph
        init        initialize new module in current directory
        tidy        add missing and remove unused modules
        vendor      make vendored copy of dependencies
        verify      verify dependencies have expected content
        why         explain why packages or modules are needed

```
比较常用的几个, download, edit, graph, tidy, init.  

### 1.4.1. go mod 简单使用

1. 使用 go mod 首先开启go mod开关.  
```
go env -w GO111MODULE=on
```
在下载相关依赖包时, 因为万能的墙存在, 需要提前设置相关代理:  
```
go env -w GOPROXY=https://goproxy.io/zh/,direct
```

2. 初始化: go mod init xxx
在当前文件夹下初始化一个新的module, 创建 go.mod 文件, 里面包含依赖包信息.  
编译安装时生成 go.sum 文件包含版本校验值, 防止被篡改, 也可以关闭校验.  


3. 修改 go.mod: go mod edit xx
修改 go.mod 内容:  
```
go mod edit -require="xxx@xxx" //写入 go.mod 文件
go tidy //更新依赖
```


查看项目中所有依赖:  
```
go list -m all
```

查看某个包所有版本:  
```
go list -m -versions xxx
```


参考:  
https://www.sulinehk.com/post/go-modules-details/  
https://segmentfault.com/a/1190000020543746  
https://learnku.com/articles/27401  


### 1.4.2. go.mod 文件
```
        module my/thing
        go 1.12
        require other/thing v1.0.2
        require new/thing/v2 v2.3.4
        exclude old/thing v1.2.3
        replace bad/thing v1.4.5 => good/thing v1.4.5

```
module: 定义模块路径
go: go 版本
require: 依赖模块module和版本号
exclude: 排除特定版本模块的使用, 不允许的模块版本被视为不可用, 并且查询无法返回.  
replace: 使用不同的模块版本替换原有模块版本.  

exclude, replace 只在main模块中使用, 不在依赖包中使用.  

require exclude replace 也是上述go mod edit 备选项.  

查询详细内容:  
```
 go help go.mod
```

## 1.5. package  
在源码第一句package中使用:  
- 如果是可执行文件, 同一文件夹下, 使用package main
- 如果是库文件, 同一文件夹下, 使用 package xxx, 与文件夹名一致.  

## 开启module区别  
