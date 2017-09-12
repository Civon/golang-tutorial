Go语言快速入门
---

安装之前需要了解及新建几个必要的文件目录：

- GOROOT 目录，该目录为解压压缩包所存放的目录。（建议 linux 环境解压到 /usr/local 目录，windows 环境解压到 C:\ProgramFiles 目录）
- 新建 GOPATH 目录，即为我们的“工作目录”，该目录可以有多个，建议只设置一个。
- 在 GOPATH 目录下新建 src 目录，该目录用于存放第三方库源码，以及存放我们的项目的源码。
- 在 GOPATH 目录下新建 bin 目录，该目录用于存放项目中所生成的可执行文件。
- 在 GOPATH 目录下新建 pkg 目录，该目录用于存放编译生成的库文件。

目录
===

<details>
<summary>点击展开目录菜单</summary>

<!-- TOC -->

- [安装](#安装)
  - [CentOS7中安装](#centos7中安装)
  - [Mac中安装](#mac中安装)
- [运行](#运行)
- [编程基础](#编程基础)
- [基本类型](#基本类型)
- [常量变量](#常量变量)
- [资源导航](#资源导航)

<!-- /TOC -->

</details>

## 安装

### CentOS7中安装

<details>
<summary>yum安装</summary>

CentOS7 可以只用使用yum安装

```bash
yum install golang  
```

</details>

<details>
<summary>源码安装</summary>

源码下载

```bash
wget https://storage.googleapis.com/golang/go1.8.linux-amd64.tar.gz
tar zxvf go1.8.linux-amd64.tar.gz -C /usr/local
```

新建GOPATH目录

```bash
mkdir -p $HOME/gopath
```

编辑 `vim /etc/profile` 添加环境变量。

```bash
export GOROOT=/usr/local/go
export GOBIN=$GOROOT/bin
export PATH=$PATH:$GOBIN
export GOPATH=$HOME/wwwroot/gofile
```

使其立即生效

```bash
source /etc/profile
```

其它命令

```bash
cat $GOROOT/VERSION  # 查看版本
$GOROOT/src/all.bash # 测试用例正确
```

</details>

### Mac中安装

<details>
<summary>通过brew命令安装</summary>

利器[home brew](http://brew.sh/)安装Go

```bash
brew update && brew upgrade # 更新 Homebrew 的信息
brew install git            # 安装 git
brew install go             # 安装 go
```

通过`go env`查看go的详细信息

```bash
→ go env

GOARCH="amd64"
GOBIN=""
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/kenny/go"
GORACE=""
GOROOT="/usr/local/Cellar/go/1.9/libexec"
GOTOOLDIR="/usr/local/Cellar/go/1.9/libexec/pkg/tool/darwin_amd64"
GCCGO="gccgo"
CC="clang"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/j7/3xly5sk567s65ny5dnr__3b80000gn/T/go-build377856897=/tmp/go-build -gno-record-gcc-switches -fno-common"
CXX="clang++"
CGO_ENABLED="1"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
```

如果需要修改默认的环境变量配置修改 `vim ~/.bash_profile` 或者 `vim ~/.zshrc`

```bash
#GOROOT
export GOROOT=/usr/local/Cellar/go/1.9/libexec
#GOPATH root bin
export GOBIN=$GOROOT/bin
export PATH=$PATH:$GOBIN
#GOPATH
export GOPATH=$HOME/go
#GOPATH bin
export PATH=$PATH:$GOPATH/bin
```

</details>

## 运行

<details>
<summary>通过go命令运行</summary>

我们先写一段GO代码，很简单就是打印输出一个`hello world!`, 保存为`print.go`文件

```go
package main
import "fmt" 
func main() {
   fmt.Println("Hello, World!")
}
```

命令运行`go`程序，在`print.go`目录下运行下面命令，可以输出`hello world!`。

```bash
go run print.go
```

</details>

<details>
<summary>通过go命令编译运行</summary>

GO程序的代码是可以直接编译成`exe文件` 或者 `二进制文件`直接运行，在`print.go`目录下运行下面命令，即可把go程序编译成二进制文件

```bash
go build print.go
```

上面命令文件可以编译成一个`print`可执行文件，然后直接在当前目录下 `./print` 运行，可以输出`hello world!`。

</details>

## 编程基础

<details>
<summary>内置关键字</summary>

```go
break      default       func      interface    select
case       defer         go        map          struct
chan       else          goto      package      switch
const      fallthrough   if        range        type
continue   for           import    retrun       var
```

</details>

<details>
<summary>注释方法</summary>

```go
// 单行注释

/*
  多行注释
*/
```

</details>

<details>
<summary>包导入</summary>

```go
import "fmt"
import "os"
import "io"
```

简写方式如下

```go
import (
  "fmt"
  "os"
  "io"
)
```

</details>

<details>
<summary>包别名</summary>

```go
import(
  ff "fmt"
)

// 或者
import ff "fmt"

// 别名包调用
ff.Println('Hello World!')
```

</details>


<details>
<summary>省略调用</summary>

```go
import(
  . "fmt"
)
func main() {
  // 省略调用
  Println('Hello World!')
}
```

</details>


<details>
<summary>可见性规则</summary>

Go语言中约定使用 **大小写** 来决定常量、变量、类型、接口、结构或函数是否可以被外部包所调用

- 函数名字首字母 **小写** 即为 `private` 私有的
- 函数名字首字母 **大写** 即为 `public` 公有

</details>

## 基本类型

<details>
<summary>布尔型</summary>

- 长度：1字节
- 取值范围：true/false
- 只能使用true/false值，不能使用数字代替布尔值

</details>

<details>
<summary>整形 int/uint</summary>

- int/uint
- 根据平台可能为32/64位

</details>

<details>
<summary>8位整型 int8/uint8</summary>

- int8/uint8
- 长度：1字节
- 取值范围：-128~127/0~255

</details>

<details>
<summary>字节型 byte</summary>

- byte(uint8别名)

</details>

<details>
<summary>16位整型 int16/uint16</summary>

- int16/uint16
- 长度：2字节
- 取值范围：-32768~32767/0~65535

</details>

<details>
<summary>32位整型 int32(别名rune)/uint32</summary>

- int32(别名rune)/uint32
- 长度：4字节
- 取值范围：-2^32/2~2^32/2-1/0~2^32-1

</details>

<details>
<summary>64位整型 int64/uint64</summary>

- int64/uint64
- 长度：8字节
- 取值范围：-2^64/2~2^64/2-1/0~2^64-1

</details>

<details>
<summary>浮点型 int64/uint64</summary>

- float32/float64
- 长度：4/8字节
- 小数位：精确到 7/15 位小数

</details>

<details>
<summary>复数 complex64/complex128</summary>

- complex64/complex128
- 长度：8/16

</details>

<details>
<summary>指针整数型 uintptr</summary>

- uintptr
- 保存指正的 32 位或者 64 位整数型

</details>

<details>
<summary>数组类型 array</summary>

```go
// 声明一个长度为5的整数数组
// 一旦数组被声明了，那么它的数据类型跟长度都不能再被改变。
var array [5]int

// 声明一个长度为5的整数数组
// 初始化每个元素
array := [5]int{12, 123, 1234, 12345, 123456}
```

</details>

<details>
<summary>结构类型 struct</summary>

- struct

</details>

<details>
<summary>字符串类型 string</summary>

- string

</details>

<details>
<summary>接口类型 inteface</summary>

- inteface

</details>

<details>
<summary>函数类型 func</summary>

- func

</details>

<details>
<summary>引用类型 func</summary>


**切片**

> 是一种可以动态数组，可以按我们的希望增长和收缩。

- slice

**Map**

> 是一种无序的键值对的集合。是一种集合，所以我们可以像迭代数组和 slice 那样迭代它。

- map

```go
// 通过 make 来创建
dict := make(map[string]int)
// 通过字面值创建
dict := map[string]string{"Red": "#da1337", "Orange": "#e95a22"}

// 给 map 赋值就是指定合法类型的键，然后把值赋给键
colors := map[string]string{}
colors["Red"] = "#da1337"

// 不初始化 map , 就会创建一个 nil map。nil map 不能用来存放键值对，否则会报运行时错误
var colors map[string]string
colors["Red"] = "#da1337"
// Runtime Error:
// panic: runtime error: assignment to entry in nil map

//选择是只返回值，然后判断是否是零值来确定键是否存在。
value := colors["Blue"]
if value != "" {
  fmt.Println(value)
}
```

在函数间传递 map 不是传递 map 的拷贝。所以如果我们在函数中改变了 map，那么所有引用 map 的地方都会改变

```go
func main() {
  colors := map[string]string{
     "AliceBlue":   "#f0f8ff",
     "Coral":       "#ff7F50",
     "DarkGray":    "#a9a9a9",
     "ForestGreen": "#228b22",
  }
  for key, value := range colors {
      fmt.Printf("Key: %s  Value: %s\n", key, value)
  }
  removeColor(colors, "Coral")
  for key, value := range colors {
      fmt.Printf("Key: %s  Value: %s\n", key, value)
  }
}
func removeColor(colors map[string]string, key string) {
    delete(colors, key)
}
```


**通道**

- chan

</details>

<details>
<summary>类型别名</summary>

```go
type (
  byte int8
  rune init32
  文本 string
)
var b 文本
b = "别名类型，可以是中文！"
```

</details>

## 常量变量

<details>
<summary>常量</summary>

```go
const (
  PI     = 3.14
  const1 = "1"
)
```

</details>

<details>
<summary>变量</summary>

- 全局变量名 以大写开头
- 全局变量不可以省略 var ，可以使用并行的方式
- 所有变量都可以使用类型推断
- 局部变量不可以使用`var()`简写的形式

```go
var (
  name  = "gopher"
  name1 = "1"
)
// 变量声明
var a int
a = 11 /* 赋值 */

// 变量声明 并赋值
var b int = 12

// 应用在函数体内的方式
var a, b, c, d int = 1, 2, 3, 4
// a =1 
// b =2 
// c =3 
// d =4 


var a, _, c, d int = 1, 2, 3, 4
// 忽略 _ 返回值忽略
```

</details>

<details>
<summary>变量的类型转换</summary>

```go
// 只能类型显式转换
var a float32 = 1.1
b := int(a)

// 不兼容的类型不能转换类型
```

</details>

## 资源导航

<details>
<summary>官方</summary>

- [Playground](http://play.golang.org)：Go 语言代码在线运行

</details>

<details>
<summary>国内镜像</summary>

- [Go 指南国内镜像](http://tour.golangtc.com/)
- [Go 语言国内下载镜像](http://www.golangtc.com/download)
- [Go 官方网站国内镜像](http://docs.studygolang.com/)

</details>

<details>
<summary>Web 框架</summary>

- [Macaron](https://go-macaron.com/)：模块化 Web 框架
- [Beego](http://beego.me/)：重量级 Web 框架
- [Revel](https://github.com/revel/revel)：较早成熟的重量级 Web 框架
- [Martini](https://github.com/go-martini/martini): 一个强大为了编写模块化 Web 应用而生的 Go 语言框架

</details>

<details>
<summary>ORM 以及数据库驱动</summary>

- [xorm](https://github.com/go-xorm/xorm)：支持 MySQL、PostgreSQL、SQLite3 以及 MsSQL
- [mgo](http://labix.org/mgo)：MongoDB 官方推荐驱动

</details>

<details>
<summary>辅助站点</summary>

- [Go Walker](https://gowalker.org)：Go 语言在线 API 文档
- [gobuild.io](http://gobuild.io/)：Go 语言在线二进制编译与下载
- [Rego](http://regoio.herokuapp.com/)：Go 语言正则在线测试
- [gopm.io](https://gopm.io)：科学下载第三方包

</details>