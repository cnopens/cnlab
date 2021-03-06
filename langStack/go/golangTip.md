# golangTip.md
## go 1.14 (focus on updating changes)





## common knoweluge
goto语句,return语句,break语句，continue语句使用总结
1、goto语句，又称为无条件转移语句，其一般格式如下： goto 语句标号；其中语句标号是按标识符规定书写的符号， 放在某一语句行的前面，标号后加冒号(：)。慎重使用，量大了难以维护。但当想跳出多层循环嵌套结构，可以立即跳出，而break只能跳出一层循环。
2、return语句， 表示从被调函数返回到主调函数继续执行，返回时可附带一个返回值，由return后面的参数指定。。如果函数执行到函数体的末尾时隐式的返回，那么说明函数没有返回值，这种没有返回值的函数在声明时应该声明函数的类型为void。
3、break语句，通常break语句总是与if语句在一起使用。当程序执行到满足的条件语句时便跳出该循环；break在while语句和switch－case语句中也经常使用。注意：break对if－else语句不起作用，而且在多层嵌套中，一个break只能跳出一层循环。

4、continue语句，有时候不希望中止整个循环的操作，而只希望提前结束本次循环，而接下来执行下次循环。可以使用continue语句。continue只对直接包含它的循环体有效，而switch｛｝不算循环体；break对直接包含它的for(),while(),switch()有效。

定义指针含义

int *p 定义整型指针p
int *s[10]  定义一个数组指针s,s里存放十个int型指针；
int (*ps)[10]    定义一个数组指针，它指向的数组拥有10个整型元素；
void (*pf)()   pf是一个指向函为空返回值类型为void的函数；
void (*apf[10])()  apf是一个有10个指针元素的数组，每个指针指向参数为空返回值类型为void的函数；


## focums on go relation technologue 

-go template
Go语言的模板与大多数语言一样，是用 {{ 和 }} 来做标识，{{ }} 里可以是表达式，也可以是变量，不过 Go语言的模板比较简陋，没办法像Python中 jinja2 那样使用继承的写法，而只能是用拼接的写法。





## common commands usage

go list -m -json 

go fmt






## install && configure

一、设置 Go 语言环境变量(中间设置变量建议在.bashrc,.profile不建议，会出现命令失效必须source问题)

步骤如下。

1、打开所需修改文件：

$ sudo vim ~/.profile （这个是设置当前用户的，往下继续看可以查看设置全局等更多操作） 

2、在文件最后添加：

export GOROOT="/usr/local/go" //GOROOT设置为Go的安装目录
export GOPATH="/home/dong/Documents/GoCode" //GOPATH设置为Go项目的工作目录
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH //不同路径用冒号分开


3、立即生效：
$ source ~/.profile




二、Ubuntu 保存环境变量的几个文件解读

/etc/profile
在用户登录时，操作系统定制用户环境时使用的第一个文件，此文件为系统的每个用户设置环境信息，当用户第一次登录时，该文件被执行。

/etc /environment
在用户登录时，操作系统使用的第二个文件， 系统在读取用户个人的profile前，设置环境文件的环境变量。

~/.profile
在用户登录时，用到的第三个文件 是.profile文件，每个用户都可使用该文件输入专用于自己使用的shell信息，当用户登录时，该文件仅仅执行一次！默认情况下，会设置一些环境变量，执行用户的.bashrc文件。

/etc/bashrc
为每一个运行bash shell的用户执行此文件，当bash shell被打开时，该文件被读取。

~/.bashrc

该文件包含专用于用户的bash shell的bash信息，当登录时以及每次打开新的shell时，该该文件被读取。

Note： 以上文件可通过$ sudo gedit 文件名 或 $ sudo vim 文件名打开；建议只修改~/.profile文件，如果只修改~/.bashrc文件，后期使用go get 命令时，会提示GOPATH未设置。



## go环境路径说明
GOPATH目录下有三个子目录
src：存放源代码（比如：.go .c .h .s等）。
pkg：编译时生成的中间文件（比如：.a）。
bin：编译后生成的可执行文件（为了方便使用，可以将此目录加入到$PATH变量中）。

## GO111MODULE的设置（及GOPROXY）
因为我的Go version >= 1.13，直接用go env -w 设置（注意大小写）

go env -w GOPROXY=https://goproxy.cn,direct
go env -w GO111MODULE=on

注：可以用go env -u 恢复初始设置；GOPROXY的值应该还可以是https://mirrors.aliyun.com/goproxy/  或 https://goproxy.cn






在Go 1.13中，我们可以通过GOPROXY来控制代理，以及通过GOPRIVATE控制私有库不走代理。

设置GOPROXY代理：
1
    
go env -w GOPROXY=https://goproxy.cn,direct

设置GOPRIVATE来跳过私有库，比如常用的Gitlab或Gitee，中间使用逗号分隔：
1
    
go env -w GOPRIVATE=*.gitlab.com,*.gitee.com

如果在运行go mod vendor时，提示Get https://sum.golang.org/lookup/xxxxxx: dial tcp 216.58.200.49:443: i/o timeout，则是因为Go 1.13设置了默认的GOSUMDB=sum.golang.org，这个网站是被墙了的，用于验证包的有效性，可以通过如下命令关闭：
1
    
go env -w GOSUMDB=off

 

可以设置 GOSUMDB="sum.golang.google.cn"， 这个是专门为国内提供的sum 验证服务。
1
    
go env -w GOSUMDB="sum.golang.google.cn"







========================测试：=========================

package main
import(
log "github.com/sirupsen/logrus"
)
func main(){
   log.WithFields(log.Fields{
    "flower": "rose",
  }).Info("i like rose!")
}

go mod init xxx
go build

========================测试：=========================
## go command
执行成功后生成go.mod文件

其他指令

    go get -u //更新现有的依赖

    go mod tidy //整理模块（拉取缺少的模块，移除不用的模块）

    go mod download//下载依赖包

    go mod graph //打印现有依赖结构

    go mod vendor //将依赖复制到vendor下

    go mod verify //校验依赖

    go.mod文件解析

    module：模块名称，使用指令go mod init <OPTIONAL_MODULE_PATH>可设置

    require：依赖包列表以及版本

    exclude：禁用依赖包列表

    replace：替换依赖包列表



## tools

### swag Cli install
1. go get -u github.com/swaggo/swag/cmd/swag

2. gin-swagger 中间件
   go get github.com/swaggo/gin-swagger

3. swagger 内置文件
$ go get github.com/swaggo/gin-swagger/swaggerFiles



