go安装及配置

安装目录GOROOT: /usr/local/go

vim /etc/profile
```shell
export GOROOT="/usr/lib/golang"
export PATH=$PATH:GOROOT/bin
export GOPATH=/data/goapp
```

### 工作空间 GOPATH ###
GOPATH目录约定有三个子目录

    src 存入源代码
    pkg编译后生成的文件
    bin编译后生成的可以执行文件

目录规化：

    一个项目一个，在$GOPATH/src/myapp, myapp表示这个应用包或者可执行应用
    package是main就表示是可执行应用
    package是非main就表示应用包

$GOPATH/src/myapp/sqrt.go
```go
package myapp

func Sqrt(x float64) float64 {
    z := 0.0
    for i := 0; i < 1000; i++ {
        z -= (z*z - x) / (2 * x)
    }
    return z
}
```

###  编译应用  ###
编译安装应用包：
1) 进入对应的应用包目录$GOPATH/src/myapp ，然后执行 go install ,就可以安装
2) 在任意目录执行： go install myapp

安装完后，在$GOPATH/pkg/${GOOS}_${GOARCH}目录下，可以看到 myapp.a , 这个.a就是应用包

新建应用程序来调用 myapp表示这个应用包
```shell
cd $GOPATH/src
mkdir myproc
cd myproc
vim main.go
```
$GOPATH/src/myproc/main.go 源码
```go
package main

import (
    "myapp"
    "fmt"
)

func main(){
    fmt.Println("Hello,world. sqrt(2)=%v \n",myapp.Sqrt(2))
}
```

编译可执行应用，进入应用目录，然后执行 go build,该目录下就会生成可以执行文件

### 获取远程包 ###
go语言有一个获取远程包的工具就是go get，目前go get支持多数开源社区(例如：github、googlecode、bitbucket、Launchpad)
```shell
go get github.com/astaxie/beedb
#go get -u 参数可以自动更新包，而且当go get的时候会自动获取该包依赖的其他第三方包
```
go get本质上可以理解为首先第一步是通过源码工具clone代码到src下面，然后执行go install


在代码中如何使用远程包，很简单的就是和使用本地包一样，只要在开头import相应的路径就可以
```go
import "github.com/astaxie/beedb"
```


#### go get golang.org/x 包失败解决方法
go get XXX 时出现：

package golang.org/x/net/websocket: unrecognized import path "golang.org/x/net/websocket" (https fetch: ….

golang 在 github 上建立了一个镜像库，如 https://github.com/golang/net 即是 https://golang.org/x/net 的镜像库


获取 golang.org/x/net 包，其实只需要以下步骤：

> mkdir -p $GOPATH/src/golang.org/x

> cd $GOPATH/src/golang.org/x

> git clone https://github.com/golang/net.git

其它 golang.org/x 下的包获取皆可使用该方法


-------------------

windows 安装 golang

下载golang的msi安装文件，直接安装。<br>
Golang在安装完成后会在系统变量中自动添加一个GOROOT变量，这个变量就是Golang的安装目录,默认C:\Go\ <br>
在Path变量中自动添加一个 D:\Go\bin 变量，这是Golang的安装目录下的bin目录。<br>

##### 设置Golang的工作目录 #####
在D盘新建文件夹GoWorks，在GoWorks中在新建三个子目录：
    
    src(此目录用来存放项目源代码) 
    pkg(此目录用来存放项目编译后的生成文件) 
    bin(此目录用来存放编译后生成的可执行文件)<br>

文件夹都新建完成后，我们在回到系统环境变量中，手动添加 GOPATH 变量，值为：D:\GoWorks。在找到Path变量，然后点击编辑按钮，添加D:\GoWorks\bin


