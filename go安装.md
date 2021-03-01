<!-- TOC -->

- [go 安装](#go-安装)
    - [go 环境安装](#go-环境安装)
        - [安装 go](#安装-go)
        - [安装 delve](#安装-delve)
        - [总结](#总结)
    - [goland 远程开发调试配置](#goland-远程开发调试配置)
    - [参考](#参考)

<!-- /TOC -->

# go 安装
## go 环境安装
### 安装 go
```
wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
tar -C /usr/local -xzf  go1.14.4.linux-amd64.tar.gz
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
```

修改/etc/profile, 添加
```
GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

### 安装 delve
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io
git clone https://github.com/go-delve/delve.git $GOROOT/src/github.com/go-delve/delve
cd $GOROOT/src/github.com/go-delve/delve
make install
```

### 总结

```
#! /bin/bash
echo "get go tar"
go_tar="go1.14.4.linux-amd64.tar.gz"
go_tar_url="https://dl.google.com/go/"
url=$go_tar_url$go_tar
echo "$url"
if [ ! -f "$go_tar" ]; then
    wget $url
fi
    echo "start to install go"
tar -C /usr/local -xzf  go1.14.4.linux-amd64.tar.gz
grep GOROOT  /etc/profile
if [ $? == 1 ]; then
  echo "export GOROOT=/usr/local/go" >> /etc/profile
fi
GOROOT=/usr/local/go
grep "GOROOT/bin"  /etc/profile
if [ $? == 1 ]; then
  echo "export PATH=\$PATH:\$GOROOT/bin" >> /etc/profile
fi
export PATH=$PATH:$GOROOT/bin
echo $PATH
echo "start to install delve"
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io
git clone https://github.com/go-delve/delve.git $GOROOT/src/github.com/go-delve/delve
cd $GOROOT/src/github.com/go-delve/delve
make install
```

## goland 远程开发调试配置
[参考](https://blog.csdn.net/u013536232/article/details/104123861)  

简单总结:  
- goland 创建project
- goland 安装 remote host 插件
- goland configuration 配置文件同步, 配置mapping.  
- goland 配置远程调试
- 服务器端, delve 开启端口监听 debug 请求. (涉及 go mod/delve, 程序写完需要调试时再开启.)  

## 参考
https://blog.csdn.net/u013536232/article/details/104123861
https://blog.csdn.net/tianshi1017/article/details/104653560  

