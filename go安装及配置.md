go安装及配置

安装目录GOROOT: /usr/local/go

vim /etc/profile
```shell
export GOROOT=/usr/local/go
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
package package

import (
    "myapp"
    "fmt"
)

func main(){
    fmt.Println("Hello,world.%s \n",myapp.Echo(good))
}
```

编译可执行应用，进入应用目录，然后执行 go build,该目录下就会生成可以执行文件
