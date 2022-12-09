### go官方网站

```
https://go.dev/
```

### go API标准库网站

```
https://studygolang.com/pkgdoc
```

### go编译

```
go build test1.go
```

默认情况下，生成的二进制文件包含调试信息和字符集。这回使文件变大。为减小文件占用的空间，可以在构建过程中，使用一些选项，以从二进制文件中剥离这些信息。以下命令可以使二进制文件减小大约30%

```
go build -ldflags "-w -s"
```

#### 交叉编译

假设现在在macos系统上，使其可以在Linux 64位架构上运行，则可以通过在运行build命令时设置GOOS和GOARCH约束限制来完成此操作。

```
GOOS="linux" GOARCH="amd64" go build hello.go

```

![截图](94d168dcf9b875093d0e858adcf2a952.png)

<br/>

#### go转义字符

|\n|换行符|
|:--:|:--:|
|\t|制表符对齐|
|\\\|一个||
|\"|一个"|
|\r|一个回车|

练习：请用一句话输出以下格式

![42ee08227f39ad1d3f258b4eaa861847.png](https://cdn.jsdelivr.net/gh/Vipersec1/mynote/images/42ee08227f39ad1d3f258b4eaa861847.png)

.go

```golang
package main

import "fmt"

// "年龄\n12", "籍贯河北", "住址北京"
func main() {
    fmt.Println("姓名", "\t年龄", "\t籍贯", "\t住址", "\njohn", "\t12", "\t河北", "\t北京")
}

//简化版本
func main() {
  fmt.Println("姓名\t年龄\t籍贯\t住址\njohn\t12\t河北\t北京")
}
```

![fb0f6bf066be1fe7e59a435a34f0f673.png](https://cdn.jsdelivr.net/gh/Vipersec1/mynote/images/fb0f6bf066be1fe7e59a435a34f0f673.png)

### go 变量

go声明变量有三种方式

#### 指定变量类型

指定变量类型，声明后若不赋值，使用默认值。（比如int的默认值是0）

如：

```golang
var a int = 10
var email string = "123@qq.com"
var name string = "viper"
```

`var:`声明变量关键字

`a:`声明的变量名

`int：`声明的变量类型

#### 类型推导

根据值自行判断变量类型（类型推导）

如

```golang
var a = 10
var email = "123@qq.com"
var name = "viper"
```

#### 批量变量声明

使用一个var关键字，把要声明的变量声明到（）当中

如：

```golang
var (
    n1   = 100
    name = "viper"
    n3   = 10.10
)
```

#### 短变量声明

省略var，注意 := 左侧的变量不应该是已经声明过的，否则会导致编译报错，:=的:不能省略，否则错误。

```golang
package main

import "fmt"

func main() {

    a := 10
    name := "viper"
    email := "123@qq.com"
}

```

#### 多重变量声明

```golang
a, name, email := 1,"viper","123@qq.com"
```

#### 全局变量声明

如何一次性声明多个全局变量【全局变量：在go中函数外部定义变量就是全局变量】

```golang
//定义全局变量方式：
var n1 = 100
var name = "viper"
var n3 = 10.10
//上面的方式过于繁杂，可以变成下面的方式
var (
    n1   = 100
    name = "viper"
    n3   = 10.10
)

func main() {
    fmt.Println("n1 =", n1, "name =", name, "n3 =", n3)
```

![d5a5fd4247981ed2d4d49f413525067c.png](https://cdn.jsdelivr.net/gh/Vipersec1/mynote/images/d5a5fd4247981ed2d4d49f413525067c.png)

#### 匿名变量声明

如果我们接收到多个变量，有一些变量使用不到，可以使用下划线_表示变量名称，这种变量叫做匿名变量。

```golang
package main

import "fmt"

func GetNameandaAge() (string, int) {
    return "viper", 30
}

func main() {
    _, age := GetNameandaAge()
    fmt.Println("age:", age)

}

```

<br/>

![46576f1fd6a1a407f1e6c89e83501126.png](https://cdn.jsdelivr.net/gh/Vipersec1/mynote/images/46576f1fd6a1a407f1e6c89e83501126.png)

<br/>

### go 常量

常量，就是在==程序编译阶段就确定下来的值，而程序在运行时则无法更改该值==。在Go程序中，常量可以时数值类型（包括整型、浮点型和复数类型）、布尔类型、字符串类型等。

定义一个常量使用const关键字，语法格式如下

```golang
const constantName [type] = value
```

==const:==   定义常量关键字

==constantName:==   常量名称

==type:==   常量类型

==value:==   常量的值

如：

```golang
func main() {
    const PI float32 = 3.14
    fmt.Println("PI:", PI)
}
```

#### 简单常量定义

可以进行类型推导

```golang
func main() {
    const PI  = 3.14
    fmt.Println("PI:", PI)
}
```

#### 批量常量定义

```golang
func main() {
    //const PI float32 = 3.14
    //fmt.Println("PI:", PI)
    const (
        a    = 100
        b    = 3.14
        name = "viper"
    )
    fmt.Println("a=", a)
    fmt.Println("b=", b)
    fmt.Println("name=", name)

}
```

#### 多重常量定义

```golang
func main() {

    const a, b, name = 1, 3.14, "viper"
    
    fmt.Println("a=", a)
    fmt.Println("b=", b)
    fmt.Println("name=", name)

}

```

#### iota

iota比较特殊，可以被认为是一个可被编译器修改的常量，他默认开始是==0==，每调用一次加==1==。遇到==const==关键字时被重置为==0==

实例：

```golang
package main

import "fmt"

func main() {
    const (
        a1 = iota  //0
        a2 = iota  //1
        a3 = iota  //2
    )
    fmt.Println("a1 : ", a1, "\na2 : ", a2, "\na3 : ", a3)
}

```

![ad8c0eea2a673cdba5b22aaca7beffc8.png](https://cdn.jsdelivr.net/gh/Vipersec1/mynote/images/ad8c0eea2a673cdba5b22aaca7beffc8.png)

运行结果：

```
a1 :  0 
a2 :  1 
a3 :  2
```

<br/>

使用 ==_== 跳过某些值

实例：

```golang
package main

import "fmt"

func main() {

    const (
        a1 = iota //0
        _         //1
        a2 = iota //2
    )
    fmt.Println("a1 : ", a1, "\na2 : ", a2)
}

```

运行结果：

```
a1 :  0 
a2 :  2
```

#### iota声明中间插队

实例：

```golang
package main

import "fmt"

func main() {
    //const (
    //    a1 = iota //0
    //    a2 = iota //1
    //    a3 = iota //2
    //)
    //fmt.Println("a1 : ", a1, "\na2 : ", a2, "\na3 : ", a3)
    //const (
    //    a1 = iota
    //    _
    //    a2 = iota
    //)
    //fmt.Println("a1 : ", a1, "\na2 : ", a2)
    const (
        a1 = iota
        a2 = 100
        a3 = iota
    )
    fmt.Println("a1: ", a1, "\na2: ", a2, "\na3: ", a3)
}

```

运行结果：

```
a1:  0 
a2:  100 
a3:  2
```

### go 数据类型

在go编程语言中，数据类型用于声明函数和变量。
数据类型的出现是为了吧数据分成所需==内存大小==不同的数据，编程的时候需要用大数据的时候才需要申请大内存，就可以充分利用内存。

|序号|数据类型|类型和描述|
|--|--|--|
|1|布尔型|布尔型的值只可以是常量true或者false。一个简单的例子：var a bool = true。|
|2|数字类型|整型int和浮点型float32、float64，Go语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。|
|3|字符串类型|字符串就是一串固定长度的自负连接起来的字符序列。Go的字符串是由单个字节连接起来的。Go语言的字符串的字节使用UTF-8编码标识Unicode文本。|
|4|派生类型|包括：(a)指针类型（Pointer）(b)数组类型(c)结构化类型（struct）(d)Channel类型(e)函数类型(f)切片类型(g)接口类型（interface）(h)Map类型|

实例

```golang
package main

import "fmt"

func main() {
    a := 100
    b := true
    var c string = "name"

    fmt.Printf("%T\n", a)
    fmt.Printf("%T\n", b)
    fmt.Printf("%T\n", c)
}

```

运行结果

```
int
bool
string
```

#### 数字类型

Go也有基于架构的类型，例如：int、uint和uintptr。

|序号|类型|占用的存储空间|描述|
|--|--|--|--|
|1|uint8|1字节|uint8无符号8位整数（0-255）|
|2|uint16|2字节|uint16无符号16位整数（0-65535）|
|3|uint32|4字节|uint32无符号32位整数（0-4294967295）|
|4|uint64|8字节|uint64无符号64位整数（0-18446744073709551615）|
|5|int8|1字节|int8有符号8位整数（-128 - 127）|
|6|int16|2字节|int16有符号16位整数（-32768 ~ 32767）|
|7|int32|4字节|int32有符号32位整数（-2147483648 ~ 2147483647）|
|8|int64|8字节|int64有符号64位整数（-9223372036854775808 ~ 9223372036854775807）|

<br/>

<br/>

<br/>

#### 浮点型

|类型|占用的存储空间|取值范围|精度|
|--|--|--|--|
|float32|4字节||单精度|
|float64|8字节||双精度|

#### 布尔型

go语言中的布尔类型有两个常量值：true和false。布尔类型经常用在== 条件判断 ==语句，或者== 循环语句 == 。也可以用在== 逻辑表达式 == 中。

实例：

```golang
package main
import "fmt"
func main() {
    var b1 bool = true
    var b2 bool = false

    var b3 = true
    var b4 = false

    b5 := true
    b6 := false

    fmt.Printf("%T\n", b1)
    fmt.Printf("%T\n", b2)
    fmt.Printf("%T\n", b3)
    fmt.Printf("%T\n", b4)
    fmt.Printf("%T\n", b5)
    fmt.Printf("%T\n", b6)
}
```

运行结果：

```
bool
bool
bool
bool
bool
bool
```

用在条件判断

```golang
package main
import "fmt"
func main() {

    age := 16
    if age >= 18{
        fmt.Println("成年")
    }else{
        fmt.Println("未成年")
    }
}
```

用在循环

```golang
package main
import "fmt"
func main() {
    count := 10
    for i := 0; i < count; i++ {
        fmt.Printf("i:%v\n", i)
        
    }
}
```

运行结果

```
i:0
i:1
i:2
i:3
i:4
i:5
i:6
i:7
i:8
i:9
```

用在逻辑表达式

```golang
package main
import "fmt"
func main() {
    // count := 10
    // for i := 0; i < count; i++ {
    //     fmt.Printf("i:%v\n", i)
        
    // }
    age := 16
    sex := "男"
    if age >= 18 && sex == "男" {
        fmt.Println("你是一个成年男性")
        
    }else{
        fmt.Println("你不是一个成年男性")
    }
}
```

运行结果：

```
你不是一个成年男性
```

#### 字符串型

一个Go语言字符串是一个任意字节的常量序列。[] byte

##### go语言字符串字面量

在Go语言中，字符串字面量使用双引号 "" 或者反引号 ' 来创建。双引号用来创建可解析的字符串，支持转义，但不能用来引用多行；反引号用来创建原生的字符串字面量，可能由多行组成，但不支持转义，并且可以包含除了反引号外其他所有字符。双引号创建可解析的字符串应用最广泛，反引号用来创建原生的字符串则多用于书写多行消息，HTML以及正则表达式。

go语言字符串切片

实例：

```golang
package main
import "fmt"
func main() {
    n := 3
    m :=5
    str := "Hello World!"

    fmt.Println(str[n])   //获取字符串索引位置为m的原始字节
    fmt.Println(str[n:m]) //获取字符串索引位置为 n 到 m-1 的字符串
    fmt.Println(str[n:])  //截取字符串索引位置为 n 到 len(s)-1的字符串
    fmt.Println(str[:m])  //截取得字符串索引位置为 0 到 m-1 字符串
    
}
```

运行结果：

```
108
lo
lo World!
Hello
```

### golang格式化输出

下面实例用到的结构体

```golang
    type Website struct {
        Name string
    }
    var site = Website{Name:"duoke360"}
```

#### 占位符

##### 普通占位符

|占位符|说明|举例|输出|
|--|--|--|--|
|%v|相应值的默认格式|fmt.Printf("site:%v", site)|site:{duoke360}|
|%#v|相应值的Go语法表示|fmt.Printf("site:%#v\n", site)|site:main.Website{Name:"duoke360"}|
|%T|相应值的类型Go语法表示|fmt.Printf("as:%T\n", a)|a: int|
|%%|字面上的百分号，并非值的占位符|fmt.Printf("%%")|%|

##### 布尔占位符

|占位符|说明|举例|输出|
|--|--|--|--|
|%t|单词true或false|fmt.Printf("b:%t\n", b)|true|

##### 整数占位符

<br/>

|占位符|说明|举例|输出|
|--|--|--|--|
|%b|二进制表示|fmt.Printf("i:%b", 8)|1000|
|%c|相应Unicode码点所表示的字符|fmt.Printf("y:%c",96)|y:'|
|%d|十进制表示|fmt.Printf("y:%d\n",0x12)|y:18|
|%o|八进制表示|fmt.Printf("y:%o\n",10)|y:12|
|%q|单引号围绕的字符字面值|fmt.Printf("y:%q\n",0x4E2D)|y:'中'|
|%x|十六进制表示，字母形式为小写 a-f|fmt.Printf("y:%x\n",13)|y:d|
|%X|十六进制表示，字母形式为大写 A-F|fmt.Printf("y:%X\n",13)|y:D|
|%U|Unicode格式，U+1234,等同于"U+%04X"|fmt.Printf("y:%U\n",0x4E2D)|y:U+4E2D|

##### 浮点数和复数的组成部分（实部和虚部）

|占位符|说明|举例|输出|
|--|--|--|--|
|%b|无小数部分，二进制指数的科学计数法,与 strconv.FormatFloat 的 'b' 转换格式一致|fmt.Printf("%b\n",1.1)|4953959590107546p-52|
|%e|科学计数法，例如 -1234.456e+78|fmt.Printf("%b\n",1.1)|1.100000e+00|
|%E|科学计数法，例如 -1234.456E+78|fmt.Printf("%E\n",1.1)|1.100000E+00|
|%f|有小数点而无指数，例如 123.456|fmt.Printf("%f\n",1.1)|1.100000|
|%g|根据情况选择 %e 或 %f 以产生更紧凑的（无末尾的0）输出|fmt.Printf("%g\n",1.1)|1.1|
|%G|%G      根据情况选择 %E 或 %f 以产生更紧凑的（无末尾的0）输出|fmt.Printf("%G\n",1.1)|1.1|

##### 字符串与字节切片

|占位符|说明|举例|输出|
|--|--|--|--|
|%s|输出字符串表示（string类型或[]byte)|fmt.Printf("%s\n", "Go语言")|Go语言|
|%q|双引号围绕的字符串，由Go语法安全地转义|fmt.Printf("%q\n", "Go语言")|"Go语言"|
|%x|十六进制，小写字母，每字节两个字符|fmt.Printf("%x\n", "Go语言")|476fe8afade8a880|
|%X|十六进制，大写字母，每字节两个字符|fmt.Printf("%X\n", "Go语言")|476FE8AFADE8A880|

##### 指针

<br/>

|占位符|说明|举例|输出|
|--|--|--|--|
|%p|十六进制表示，前缀 0x|x := 100     p := &x     fmt.Printf("%p", p)|0x14000124008|

<br/>

<br/>

### Golang 运算符

Go语言内置的运算符有:

1. 算数运算符
2. 关系运算符
3. 逻辑运算符
4. 位运算符
5. 赋值运算符

|运算符|描述|
|--|--|
|+|相加|
|-|相减|
|*|相乘|
|/|相除|
|%|求余|

注意： == ++ == （自增）和 == -- == （自减）在Go语言中是单独的语句，并不是运算符。

实例：

```golang
package main

import "fmt"


func main() {
    a := 20
    b := 10
    fmt.Printf("(a + b): %v\n",(a + b))
    fmt.Printf("(a - b): %v\n",(a - b))
    fmt.Printf("(a * b): %v\n",(a * b))
    fmt.Printf("(a / b): %v\n",(a / b))
    fmt.Printf("(a %% b): %v\n",(a % b))
    a++
    fmt.Printf("a:%v\n", a)
    a--
    fmt.Printf("a:%v\n", a)

}
```

运行结果：

```
(a + b): 30
(a - b): 10
(a * b): 200
(a / b): 2
(a % b): 0
a:21
a:20
```

#### 关系运算符

|运算符|描述|
|--|--|
|==|检查两个值是否相等，如果相等返回 True 否则返回 False|
|!=|检查两个值是否不相等，如果不相等返回 True 否则返回 False|
|>|检查左边值是否大于右边值，如果是返回 True 否则返回 False|
|>=|检查左边值是否大于等于右边值，如果是返回 True 否则返回 False|
|<|检查左边值是否小于右边值，如果是返回 True 否则返回 False|
|<=|检查左边值是否小于等于右边值，如果是返回 True 否则返回 False|

==  关系运算的结果都是bool型，要么是true，要么是false
  关系表达式经常用在if结构的条件中或循环结构的条件中  ==

实例：

```golang
package main

import "fmt"


func main() {
    a := 20
    b := 10
    r := a == b

    fmt.Printf("r:%v\n", r)
    r = a < b
    fmt.Printf("r:%v\n", r)
    r = a > b
    fmt.Printf("r:%v\n", r)
    r = a >= b
    fmt.Printf("r:%v\n", r)
    r = a <= b
    fmt.Printf("r:%v\n", r)
    r = a != b
    fmt.Printf("r:%v\n", r)

}
  
```

运行结果

```
r:false
r:false
r:true
r:true
r:false
r:true
```

#### 逻辑运算符

|运算符|描述|
|--|--|
|&&|如果两边的操作数都是 True，则为 True，否则为 False|
|\|||
|!|如果条件为 True，则为 False，否则为 True|

实例：

```golang
package main

import "fmt"


func main() {
    a := true
    b := false
    r := a && b
    fmt.Printf("r: %v\n", r)
    r = a || b
    fmt.Printf("r: %v\n", r)

}
  
```

运行结果：

```
r: false
r: true
```

#### 位运算符

|运算符|描述|
|--|--|
|&|参与运算的两数各对应的二进位|
|\||参与运算的两数各对应的二进位相或（两位有一个为1就为1）|
|^|参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1（两位不一样则为1）|
|<<|左移n位就是乘以2的n次方（“a<<b”是把a的各二进位全部左移b位，高位丢弃，低位补0）|
|>>|右移n位就是除以2的n次方（“a>>b”是把a的各二进位全部右移b位）|

#### 赋值运算符

|运算符|描述|
|--|--|
|=|直接将运算符右侧的值赋给左侧的变量或表达式|
|+=|先将运算符左侧的值与右侧的值相加，再将相加和赋给左侧的变量或表达式|
|-=|赋给左侧的变量或表达式侧的值相减，再将相减差赋给左侧的变量或表达式|
|*=|先将运算符左侧的值与右侧的值相乘，再将相乘结果赋给左侧的变量或表达式|
|/=|先将运算符左侧的值与右侧的值相除，再将相除结果赋给左侧的变量或表达式|
|%=|先将运算符左侧的值与右侧的值相除取余数，再将余数赋给左侧的变量或表达式|
|<<=|先将运算符左侧的值按位左移右侧数值指定数量的位置，再将位移后的结果赋给左侧的变量或表达式|
|>>=|先将运算符左侧的值按位右移右侧数值指定数量的位置，再将位移后的结果赋给左侧的变量或表达式|
|&=|先将运算符左侧的值与右侧的值按位与，再将位运算后的结果赋给左侧的变量或表达式|
|\|=|先将运算符左侧的值与右侧的值按位或，再将位运算后的结果赋给左侧的变量或表达式|
|^=|先将运算符左侧的值与右侧的值按位异或，再将位运算后的结果赋给左侧的变量或表达式|

<br/>

<br/>

### Go语言中的流程控制

#### go语言中的条件

条件语句是用来判断给定的条件是否满足(表达式值是否为== true ==或者== false ==)，并根据判断的结果(真或假)决定执行的语句，go语言中的条件语句也是这样的

#### go语言中的条件语句包含如下几种情况

1. ** if 语句 **：== if == 语句 由一个布尔表达式后紧跟一个或多个语句组成。
2. ** if...else 语句 **: == if == 语句 后可以使用可选的== else  ==语句, == else == 语句中的表达式在布尔表达式为 == false == 时执行。
3. ** if 嵌套语句 **: 你可以在 == if == 或== else if == 语句中嵌入一个或多个 == if ==或 == else if ==语句。
4. ** switch语句： ** == switch ==语句用于给予不同条件执行不同动作。
5. ** select语句： ** == select == 语句类似于 == switch == 语句，但是 == select == 会随机执行一个可运行的== case == 。如果没有 == case == 可运行，他将阻塞，直到有 == case == 可运行。

#### Go 语言中的循环语句

go语言中的循环只有for循环，去除了 == while == 、== do while == 循环，使用起来更加简洁。

1. for循环。
2. for range循环。

#### Go语言中的流程关键字

1. ==** break ** ==
2. ==**  continue **==
3. ==** goto**==

### Go中的if语句

go语言中的if语句和其他语言中的类似，都是根据给定的条件表达式运算结果来，判断执行流程。

 #### Go语言if语句语法

```golang
if 布尔表达式 {
    /*在布尔表达式为true时执行*/
}
```

> 注意：在go语言中布尔表达式不用使用括号。

#### go语言if语句实例演示

根据布尔值flag判断

```golang
package main

import "fmt"

func  main()  {
    flag1 := true
    if flag1 {
        fmt.Println("flag is true")
    }else{
        fmt.Println("flag is false")
    }
    
}
```

运行结果：

```
flag is true
```

根据年龄判断是否成年

```golang
package main

import "fmt"

func  main()  {
    age := 20
    if age >=18 {
        fmt.Println("你是成年人")
    }else{
        fmt.Println("你还未成年")
    }
    
}

```

运行结果：

```
你是成年人
```

> ** 初始变量可以声明在布尔表达式里面，注意他的作用域 **

```golang
package main

import "fmt"

func  main()  {
    a := 100
    if a {
        fmt.Println("true")
    }
    
}

```

> ** 不能使用0或非0表示真假 **

#### go语言if语句使用提示：

1. 不需使用括号将条件包含起来
2. 大括号== {} == 必须存在，即使只有一行语句
3. 左括号必须在== if ==  == else == 的同一行
4. 在 == if ==之后，条件语句之前，可以添加变量初始化语句，使用 == : == 进行分割

<br/>

<br/>

### Go语言的 if else 语句

go语言中的if else 语句可以根据给定条件二选一。

#### go 语言中的 if else 语句语法

```golang
    if 布尔表达式 {
        /* 在布尔表达式为 true 时执行 */
    }else{
        /* 在布尔表达式为 false 时执行 */
    }
```

#### Go语言 if else 语句实例

比较两个数的大小

```golang
package main

import "fmt"

func  main()  {
    a := 10
    b := 20
    if a < b {
        fmt.Println("\"a < b\"")
        
    }else{
        fmt.Println("\"a > b\"")
    }

}

```

运行结果

```
"a < b"
```

判断一个数是奇数还是偶数

```golang
package main

import "fmt"

func f1(){
    var num int
    fmt.Println("请输入一个数：")
    fmt.Scan(&num)
    if num % 2 == 0{
        fmt.Printf("你输入的数为偶数，你输入的数为：%v", num)
    }else{
        fmt.Printf("你输入的数为奇数数，你输入的数为：%v", num)
}
}
func  main()  {
    f1()

}

```

运行结果

```golang
viper@viperdeMacBook-Air main % go run test8.go
请输入一个数：
17
你输入的数为奇数数，你输入的数为：17%                                                                                                                       
viper@viperdeMacBook-Air main % go run test8.go
请输入一个数：
100
你输入的数为偶数，你输入的数为：100%                                                                                                                        

```

### Go 语言中的 if else if 语法实例

根据分数判断等级

```golang
package main

import "fmt"

func f1() {
    score := 1
    if score >= 60 && score <= 70 {
        fmt.Println("B")

    } else if score >= 70 && score <= 80 {
        fmt.Println("A")
    } else if score >= 80 && score <= 100 {
        fmt.Println("S")
    } else {
        fmt.Println("C")
    }

}

func main() {
    f1()

}

```

运行结果

```
C
```

输入星期几的第一个字母判断一下是星期几，如果第一个字母一样，则继续判断第二个字母

```golang
package main

import "fmt"

// Sunday Monday Tuesday Wednesday Thursday Friday Saturday
func f1() {
    var c string
    fmt.Println("请输入一个字母")
    fmt.Scan(&c)
    if c == "w" {
        fmt.Printf("您输入的字母为：%v,对应的星期为：Wednesday ", c)

    } else if c == "m" {
        fmt.Printf("您输入的字母为：%v,对应的星期为：Monday ", c)
    } else if c == "f" {
        fmt.Printf("您输入的字母为：%v,对应的星期为：Friday ", c)
    } else if c == "s" {
        fmt.Println("请输入二个字母")
        fmt.Scan(&c)
        if c == "u" {
            fmt.Printf("您输入的字母为：%v,对应的星期为：Sunday ", c)
        } else if c == "a" {
            fmt.Printf("您输入的字母为：%v,对应的星期为：Saturday ", c)

        }
    } else if c == "t" {
        fmt.Println("请输入二个字母")
        fmt.Scan(&c)
        if c == "u" {
            fmt.Printf("您输入的字母为：%v,对应的星期为：Tuesday ", c)
        } else if c == "h" {
            fmt.Printf("您输入的字母为：%v,对应的星期为：Thursday ", c)
        }

    }

}

func main() {
    f1()

}

```

### Go语言中嵌套if语句

go语言if语句可以嵌套多级进行判断。

#### Go语言if嵌套语法

```golang
if 布尔表达式 1 {
   /* 在布尔表达式 1 为 true 时执行 */
   if 布尔表达式 2 {
      /* 在布尔表达式 2 为 true 时执行 */
   }
}
```

#### Go语言if嵌套实例

判断三个数的大小

```golang
package main

import "fmt"

func main() {
    a, b, c := 1, 2, 3
    if a > b {
        if a > c {
            fmt.Println("a")
        }
    } else {
        // a < b
        if b > c {
            fmt.Println("b")
        } else {
            fmt.Println("c")
        }
    }
}

```

运行结果：

```
c
```

判断男生还是女生，还有是否成年

```golang
func f2() {
    sex := "女"
    age := 20

    if sex == "男" {
        if age >= 18 {
            fmt.Println("男性，已成年")
        } else {
            if age <= 18 {
                fmt.Println("男性，未成年")
            }
        }
    } else {
        if sex == "女" {
            if age >= 18 {
                fmt.Println("女性，已成年")
            } else {
                if age <= 18 {
                    fmt.Println("女性，未成年")
                }
            }
        }
    }

}
```

### Go语言 switch 语句

Go语言中的 == switch == 语句，可以非常容易判断多个值的情况。

#### Go语言中switch语句语法

```golang
switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
}
```

#### go语言中switch语句实例

判断成绩

```golang
func f3() {
    sorce := "A"
    switch sorce {
    case "A":
        fmt.Println("A")
    case "B":
        fmt.Println("B")
    case "C":
        fmt.Println("C")
    case "D":
        fmt.Println("D")
    }
}
```

运行结果：

```
A
```

##### 多条件匹配

go语言 == switch == 语句，可以同时匹配多个条件，中间用逗号分隔，有其中一个匹配成功即可。

实例：

```golang
func f4() {
    a := 3
    switch a {
    case 1, 2, 3, 4, 5:
        fmt.Println("工作日")
    case 6, 7:
        fmt.Println("休息日")
    default:
        fmt.Println("非法输入")
    }
}
```

运行结果：

```
工作日
```

##### case可以是条件表达式

 实例：

```golang
func f6() {
    sorce := 80
    switch {
    case sorce >= 90:
        fmt.Println("享受假期")
    case sorce < 90 && sorce >= 75:
        fmt.Println("学中偷乐")
    case sorce < 75 && sorce >= 60:
        fmt.Println("好好学习")
    case sorce < 60:
        fmt.Println("该叫家长了！！！！")
    default:
        fmt.Println("非法输入")
    }
}
```

运行结果：

```
学中偷乐
```

##### == fallthrough == 可以执行满足条件的下一个 == case ==

```golang
func f7() {
    a := 100
    switch a {
    case 100:
        fmt.Println("100")
        fallthrough
    case 200:
        fmt.Println("200")
    case 300:
        fmt.Println("300")
    }
}
```

运行结果：

```
100
200
```

#### Go语言中== switch == 语句的注意事项

1. 支持多条件匹配
2. 不同的 == case == 之间不使用 == break ==分隔，默认只会执行一个== case == .
3. 如果想要执行多个== case == ，需要使用 == fallthrough == 关键字，也可用 == break == 终止。
4. 分支还可以使用表达式，例如 == a > 10 ==

<br/>

<br/>

### Go for 循环语句

go语言中的 == for == 循环，只有 == for == 关键字，去除了其他语言中的 == while ==和== do while ==关键字。

#### Go语言for循环语法

```golang
    for 初始语句; 条件表达式; 结束语句 {
        循环体语句
        
    }
```

> 注意： for表达式不用加括号

#### Go语言for循环实例

循环输出1-10

```golang
func f1() {
    for i := 1; i <= 10; i++ {
        fmt.Printf("i: %v\n", i)

    }

}
```

运行结果；

```
i: 1
i: 2
i: 3
i: 4
i: 5
i: 6
i: 7
i: 8
i: 9
i: 10
```

== ** 初始条件，可以写在外面 ** == 

```golang
func f2() {
    i := 1
    for ; i <= 10; i++ {
        fmt.Printf("i: %v\n", i)
    }
}

```

运行结果：

```
i: 1
i: 2
i: 3
i: 4
i: 5
i: 6
i: 7
i: 8
i: 9
i: 10
```

 == ** 初始条件和结束条件都可以省略 ** ==

```golang
func f3() {
    i := 1  //初始条件

    for i <= 10 {
        fmt.Printf("i: %v\n", i)
        i++  //结束条件

    }
}
```

##### 永真循环

这种情况类似其他语言中的 == while == 循环

```golang
func f4() {
    for {
        fmt.Println("我一直在执行～")
    }
}
```

运行结果：

```
我一直在执行～
我一直在执行～
我一直在执行～
我一直在执行～
......
```

##### 

#### Go语言中== for循环 == 语句的注意事项

for循环可以通过==**break**==、==**goto**==、==**return**==、==**panic**==语句强制退出循环

<br/>

<br/>

### Go for range 循环语句

Go语言中可以使用**==for range==**遍历数组、切片、字符串、map 及通道（channel）。 通过==**for range**==遍历的返回值有以下规律：

1. 数组、切片、字符串返回==**索引和值**==。
2. map返回==**键和值**==。
3. 通道（channel）只返回通道内的值。

##### Go for range实例

遍历**==数组==**信息

```golang
package main

import "fmt"

func f1() {
    var a = [...]int{1, 2, 3, 4, 5}   //数组
    for i, v := range a {
        fmt.Printf("i: %v", i)
        fmt.Printf("  v: %v\n", v)

    }
}
func main() {
    f1()
}

```

运行结果：

```
i: 0  v: 1
i: 1  v: 2
i: 2  v: 3
i: 3  v: 4
i: 4  v: 5
```

==**切片**==

```golang
func f2() {
    var a = []int{1, 2, 3, 4, 5}  //切片
    for _, v := range a {

        fmt.Printf("  v: %v\n", v)

    }
}
```

运行结果：

```
  v: 1
  v: 2
  v: 3
  v: 4
  v: 5
```

==**map**==

```
func f3() {
    //map的表示为：key:value
    m := make(map[string]string, 0)
    m["name"] = "viper"
    m["age"] = "20"
    m["email"] = "viper@gmail.com"

    for key, value := range m {
        fmt.Printf("%v:%v\n", key, value)
    }

}
```

```
name:viper
age:20
email:viper@gmail.com
```

遍历字符串

```
func f4() {
    //string
    s := "hello world"
    for _, v := range s {
        fmt.Printf("%c\n", v)
    }

}
```

运行结果：

```
h
e
l
l
o
 
w
o
r
l
d
```

### Go语言流程控制关键字break

==**break**==语句可以结束==**for**==、**==switch==**和==**select**==的代码块。

#### Go语言使用break注意事项

1. 单独在==**select**==中使用==**break**==和不使用==**break**==没有啥区别。
2. 单独在表达式**==switch==**语句，并且没有==**fallthough**==，使用==**break**==和不使用==**break**==没有啥区别。
3. 单独在表达式**==switch==**语句，并且有==**fallthough**==，使用==**break**==能够终止==**fallthough**==后面的==case==语句的执行。
4. 带标签的==**break**==，可以跳出多层**==select/ switch==**作用域。让==**break**==更加灵活，写法更加简单灵活，不需要使用控制变量一层一层跳出循环，没有带==**break**==的只能跳出当前语句块。

#### Go语言break关键字实例

##### 跳出for循环

```
package main

import "fmt"

func f1() {
    for i := 0; i < 10; i++ {
        if i == 5 {
            break
        }
        fmt.Printf("i: %v\n", i)

    }
}
func main() {
    f1()

}

```

运行结果

```
i: 0
i: 1
i: 2
i: 3
i: 4
```

跳出switch语句

```
func test2() {
    i := 2
    switch i {
    case 1:
        fmt.Println("1")
        break
    case 2:
        fmt.Println("2")
        //break
        fallthrough
    case 3:
        fmt.Println("3")
        break

    }
}
```

运行结果

```
2
3
```

带标签的==**break**==，跳转到标签处

```golang
func test3() {
MYLABEL:
    for i := 0; i < 10; i++ {
        fmt.Printf("i: %v\n", i)
        if i == 5 {
            break MYLABEL
        }
    }
    fmt.Println("END...")
}
```

运行结果：

```
i: 0
i: 1
i: 2
i: 3
i: 4
i: 5
END...
```

<br/>

### Go语言 关键字continue

`continue`只能用在循环中，在go中只能用在`for`循环中，它可以终止本次循环，进行下一次循环。

在 `continue`语句后添加标签时，表示开始标签对应的循环。

#### go语言`continue`实例

输出1到10之间的偶数

```
func test4() {
    for i := 0; i < 10; i++ {
        if i%2 != 0 {
            continue
        }
        fmt.Printf("i: %v\n", i)
    }
}

//也可使用下面的方式

func test4() {
    for i := 0; i < 10; i++ {
        if i%2 == 0 {
            fmt.Printf("i: %v\n", i)
            continue
        }
    }
}


//也可使用下面的方式

func test4() {
    for i := 0; i < 10; i++ {
        if i%2 == 0 {
            fmt.Printf("i: %v\n", i)
        } else {
            continue
        }
    }
}

```

运行结果：

```
i: 0
i: 2
i: 4
i: 6
i: 8
```

<br/>

```
func test5() {

    for i := 0; i < 10; i++ {
        for j := 0; j < 10; j++ {
            if i == 2 && j == 2 {
                continue
            }
            fmt.Printf("i: %v, j: %v\n", i, j)
        }

    }
}
```

运行结果

<br/>

![2b0c7ddaca2e437c2120a12facaaff4b.png](https://cdn.jsdelivr.net/gh/Vipersec1/mynote/images/2b0c7ddaca2e437c2120a12facaaff4b.png)

<br/>

<br/>

### Go语言 流程控制关键字goto

`goto`语句通过标签进行代码间的无条件跳转。`goto`语句可以在快速跳出循环、避免重复退出上有一定的帮助。Go语言中使用`goto`语句能简化一些代码的实现过程。 例如双层嵌套的for循环要退出时：

```

```

#### go语言关键字goto实例

跳转到指定标签

```
func test6() {
    a := 1
    if a >= 2 {
        fmt.Println("2")
    } else {
        goto END
    }
END:
    fmt.Println("END...")
}
```

运行结果：

```
END...
```

跳出双重循环

```
func test7() {
    for i := 0; i < 10; i++ {
        for j := 0; j < 10; j++ {
            if i >= 2 && j >= 2 {
                goto END
            }

        }

    }
END:
    fmt.Println("END...")
}
```

运行结果：

```
END...
```

<br/>

<br/>

### Go语言数组

数组是相同数据类型的一组数据的集合，数组一旦定义长度不能修改，数组可以通过**下标（或者叫索引）**来访问元素。

#### go语言数组的定义

数组定义的语法如下：

```
var variable_name [SIZE] variable_type
```

`variable_name`：数组名称

`SIZE`：数组长度，必须是常量

`variable_type`：数组保存元素的类型

#### go语言数组实例

```
package main

import (
    "fmt"
)

func test1() {
    var a1 [1]int
    var a2 [2]string
    var a3 [3]bool
    fmt.Printf("a1: %T\n", a1)
    fmt.Printf("a2: %T\n", a2)
    fmt.Printf("a3: %T\n", a3)

}
func main() {
    test1()

}

```

运行结果

```
a1: [1]int
a2: [2]string
a3: [3]bool
```

从上面运行结果，我们可以看出，数组和长度和元素类型共同组成了数组的类型。

#### Go语言数组的初始化

初始化，就是给数组的元素赋值，没有初始化的数组，默认元素值都是`零值`，布尔类型是 `false`，字符串是空字符串

##### 没有初始化的数组

```golang
package main

import (
    "fmt"
)

func test1() {
    var a1 [2]int
    var a2 [2]string
    var a3 [3]bool
    fmt.Printf("a1: %T\n", a1)
    fmt.Printf("a2: %T\n", a2)
    fmt.Printf("a3: %T\n", a3)
    fmt.Printf("a1: %v\n", a1)
    fmt.Printf("a2: %v\n", a2)
    fmt.Printf("a3: %v\n", a3)

}
func main() {
    test1()

}

```

运行结果：

```
a1: [2]int
a2: [2]string
a3: [3]bool
a1: [0 0]
a2: [ ]
a3: [false false false]
```

##### 使用初始化列表

实例

```golang
func test2() {
    //数组的初始化
    //初始化列表
    var a1 = [3]int{1, 2}
    fmt.Printf("a1: %v\n", a1)
    var a2 = [2]string{"hello", "world"}
    fmt.Printf("a2: %v\n", a2)
    var a3 = [3]bool{true, false, true}
    fmt.Printf("a3: %v\n", a3)
}
```

运行结果：

```
a1: [1 2 0]
a2: [hello world]
a3: [true false true]
```

最后一位未设置，则为初始值

使用初始化列表，就是将值写在**`大括号`**里

##### 省略数组长度

数组长度可以省略，使用`...`代替，根据初始化值得数量`自动推断`，例如：

```golang
func test3() {
    var a1 = [...]int{1, 2, 3}
    var a2 = [...]bool{true, false}
    var a3 = [...]string{"hello", "world"}
    fmt.Printf("a1: %v\n", a1)
    fmt.Printf("a2: %v\n", a2)
    fmt.Printf("a3: %v\n", a3)
}
```

运行结果

```
a1: [1 2 3]
a2: [true false]
a3: [hello world]

```

##### 指定索引值得方式来初始化

可以通过指定所有的方式来初始化，未指定所有的默认未零值。

实例

```golang
func test4() {
    var a1 = [...]int{0: 1, 3: 100, 5: 10}
    var a2 = [...]bool{1: true, 3: false}
    var a3 = [...]string{0: "hello", 3: "world"}
    fmt.Printf("a1: %v\n", a1)
    fmt.Printf("a2: %v\n", a2)
    fmt.Printf("a3: %v\n", a3)
}
```

运行结果：

```
a1: [1 0 0 100 0 10]
a2: [false true false false]
a3: [hello   world]
```

### Go语言访问数组元素

可以通过下标的方式，来访问数组元素。数组的最大下标为数组长度-1，大于这个下标会发生数组越界。

#### 访问数组元素

实例:

```golang
package main

import "fmt"

func test1() {
    var a1 [2]int
    a1[0] = 100
    a1[1] = 200
    fmt.Printf("a1: %v\n", a1)
    a1[0] = 1000
    a1[1] = 2000
    fmt.Printf("a1: %v\n", a1)

}

func main() {
    test1()

}

```

运行结果

```
a1: [100 200]
a1: [1000 2000]
```

#### 根据数组长度遍历数组

可以根据数组长度，通过`for`循环的方式来遍历数组，数组的长度可以使用`len`函数获得

实例：

获取数组的长度

```golang
func test2() {
    //数组的长度
    var a1 = [3]int{1, 2, 3}
    fmt.Printf("len a1: %v\n", len(a1))
    var a2 = [...]int{1, 2, 3, 4}
    fmt.Printf("len a2: %v\n", len(a2))
}
```

运行结果

```
len a1: 3
len a2: 4
```

数组的遍历 1. 根据长度和小标

```golang
func test3() {
    //数组的遍历 1. 根据长度和小标

    var a1 = [3]int{1, 2, 3}
    for i := 0; i < len(a1); i++ {
        fmt.Printf("a1[i]: %v\n", a1[i])

    }
}
```

运行结果

```
a1[i]: 1
a1[i]: 2
a1[i]: 3
```

 #### 使用`for range`遍历数组

还可以使用 `for range`循环来遍历数组，range返回数组下标和对应的值

实例

```golang
func test4() {
    var a1 = [3]int{1, 2, 3}
    for i, v := range a1 {
        fmt.Printf("a1[%v]: %v\n", i, v)
    }

}
```

运行结果：

```
a1[0]: 1
a1[1]: 2
a1[2]: 3
```

<br/>

### Go 切片

前面我们学习了数组，数组是固定长度，可以容纳相同数据类型的元素的集合。当长度固定时，使用还是带来一些限制，比如：我们申请的长度太大浪费内存，太小又不够用。

鉴于上述原因，我们有了go语言的切片，可以把切片理解为，可变长度的数组，其实它底层就是使用数组实现的，增加了自动扩容功能。切片（`Slice`）是一个拥有相同类型元素的可变长度的序列。

#### Go语言切片的语法

声明一个切片和声明一个数组类似，只要不添加长度就可以了

```golang
var identifier []type
```

切片是引用类型，可以使用`make`函数来创建切片：

```golang
var Slice1 []type = make([]type,len)
也可以简写为：
Slice1 := make([]type,len)
```

也可以指定容量，其中capacity为可选参数

```golang
make([]T, length,capacity)
```

这里len是数组的长度并且也是切片的初始长度。

#### Go语言切片实例

常规声明

```golang
func test1() {
    var name []string //声明切片
    var number []string
    fmt.Printf("name: %v\n", name)
    fmt.Printf("number: %v\n", number)

}
```

```

```

运行结果：

```golang
name: []
number: []
```

make声明

```golang
func test2() {
    var name = make([]string, 2)  ////make声明切片
    fmt.Printf("name: %v\n", name)
}
```

运行结果

```
name: [ ]
```

#### Go语言切片的长度和容量

切片拥有自己的长度和容量，我们可以通过使用内置的`len()`函数求长度，使用内置的`cap()`函数求切片的容量。

实例：

```golang
func test3() {
    var s1 = []int{1, 2, 3}
    fmt.Printf("len(s1:) %v\n", len(s1))
    fmt.Printf("cap(s1:) %v\n", cap(s1))

}
```

运行结果：

```
len(s1:) 3
cap(s1:) 3
```

### Go切片的初始化

切片的初始化方法很多，可以直接初始化，也可以使用数组初始化等。

#### 切片如何切分

```golang
// 切片
func test1() {
    var s1 = []int{1, 2, 3, 4, 5, 6}
    s2 := s1[0:3] // [)
    fmt.Printf("s2: %v\n", s2)
    s3 := s1[3:]
    fmt.Printf("s3: %v\n", s3)
    s4 := s1[2:5]
    fmt.Printf("s4: %v\n", s4)
    s5 := s1[:]
    fmt.Printf("s5
```

#### 直接初始化

```golang
import "fmt"

func test1() {
    s := []int{1, 2, 3}
    fmt.Printf("s: %v\n", s)
}

func main() {
    test1()

}

```

运行结果

```
s: [1 2 3]
```

#### 使用数组初始化

```golang
func test3() {
    var s1 = [...]int{1, 2, 3, 4, 5, 6, 7, 8}
    s2 := s1[0:3]
    fmt.Printf("s2: %v\n", s2)

}
```

运行结果：

```
s2: [1 2 3]
```

#### 使用数组的部分元素初始化

切片的底层就是一个`数组`，所以我们可以`基于数组通过切片表达式得到切片`。切片表达式中的low和high表示一个索引范围（`左包含，又不包含`），得到的切片`长度`=high-low，容量等于得到的切片底层数组的容量。

```golang

func test2() {
    s1 := []int{1, 2, 3, 4, 5, 6, 7, 8}
    s2 := s1[0:3] //截取3位,0-2
    fmt.Printf("s2: %v\n", s2)
    s3 := s1[3:] //从第三位截取后面的全部，包括第三位
    fmt.Printf("s3: %v\n", s3)
    s4 := s1[2:5]
    fmt.Printf("s4: %v\n", s4) //从第二位取到第五位
    s5 := s1[:]                //取全部
    fmt.Printf("s5: %v\n", s5)
}

```

运行结果：

```
s2: [1 2 3]
s3: [4 5 6 7 8]
s4: [3 4 5]
s5: [1 2 3 4 5 6 7 8]
```

#### 空（nil）切片

一个切片在未初始化之前默认为nil，长度为0，容量为0

```golang

func test4() {
    var s1 []int
    fmt.Println(s1 == nil)
    fmt.Printf("s1: %v\n", s1)
    fmt.Printf("len(s1): %v\n", len(s1))
    fmt.Printf("cap(s1): %v\n", cap(s1))
}

```

运行结果：

```
true
s1: []
len(s1): 0
cap(s1): 0

```

<br/>

### Go语言切片的遍历

切片的遍历和数组的遍历非常类似，可以使用for循环索引遍历，或者for range循环。

#### for循环索引遍历

```golang
func test1() {
    s1 := []int{1, 2, 3, 4, 5}
    l := len(s1)
    for i := 0; i < l; i++ {
        fmt.Printf("s1[%v]: %v\n", i, s1[i])

    }

}

//或使用下面的方式
func test3() {
    s1 := []int{1, 2, 3, 4, 5}
    for i := 0; i < len(s1); i++ {
        fmt.Printf("s1[%v]: %v\n", i, s1[i])
    }
}
```

运行结果：

```
s1[0]: 1
s1[1]: 2
s1[2]: 3
s1[3]: 4
s1[4]: 5
```

#### for range 循环索引遍历

```golang
func test2() {
    s1 := []int{1, 2, 3, 4, 5}
    for i, v := range s1 {
        fmt.Printf("i: %v\n", i)
        fmt.Printf("v: %v\n", v)
        fmt.Printf("s1[%v]: %v\n", i, v)

    }

}
```

运行结果：

```
i: 0
v: 1
s1[0]: 1
i: 1
v: 2
s1[1]: 2
i: 2
v: 3
s1[2]: 3
i: 3
v: 4
s1[3]: 4
i: 4
v: 5
s1[4]: 5
```

<br/>

### Go语言切片元素的添加和删除copy

切片是一个动态数组，可以使用==`append()`==函数添加元素，go语言中并没有删除切片元素的专用方法，我们可以使用切片本身的特性来删除元素。由于，切片是引用类型，通过赋值的方式，会修改原有内容，go提供了==`copy()`==函数来拷贝切片

#### 添加元素（add）

```golang
// 添加,append
func test1() {
    s1 := []int{}
    s1 = append(s1, 1)
    s1 = append(s1, 2)
    s1 = append(s1, 3)
    fmt.Printf("s1: %v\n", s1)

}
```

运行结果

```
s1: [1 2 3]
```

#### 删除元素（del）

```golang
// 删除（含义为定义一个新的切片将原来的值去掉要删除的值后重新给切片赋值）
func test2() {
    s1 := []int{1, 2, 3, 4, 5}
    //删除第三位 从三开始，不包含三
    s1 = append(s1[:2], s1[3:]...)
    fmt.Printf("s1: %v\n", s1)
}
```

运行结果

```
s1: [1 2 4 5]
```

**公式：要从切片a中删除索引为`index`的元素，操作方法是：`a = append(a[:index],a[index+1:]...)`** 

#### 修改元素（update）

```golang
// 修改元素
func test3() {
    s1 := []int{1, 2, 3, 4, 5}
    s1[1] = 100 //索引为1，修改的为第二位 0,1
    fmt.Printf("s1: %v\n", s1)

}

```

运行结果

```
s1: [1 100 3 4 5]
```

#### 查询元素 （query）

```golang
func test4() {
    s1 := []int{1, 2, 3, 4, 5, 6}
    for i, v := range s1 {
        key := 2        //查询当值为2时，索引为什么
        if v == key {
            fmt.Printf("i: %v\n", i)
        }

    }

}
```

运行结果

```
i: 1
```

#### 拷贝切片

##### 直接赋值

```golang
func test5() {
    s1 := []int{1, 2, 3, 4, 5}
    s2 := s1
    fmt.Printf("s2: %v\n", s2)
    s2[1] = 100
    fmt.Printf("s2: %v\n", s2)
    fmt.Printf("s1: %v\n", s1)

}
```

运行结果

```
s2: [1 2 3 4 5]
s2: [1 100 3 4 5]
s1: [1 100 3 4 5]
```

直接赋值，相当于吧s1切片的内存地址赋给s2，所以当s2发生变化时，s1也会跟着被修改。

##### `copy()`

```golang
package main

import "fmt"

func test1() {
    s1 := []int{1, 2, 3, 4, 5}
    s2 := make([]int, 5)  //定义一个空切片，并定好长度
    copy(s2, s1)
    fmt.Printf("s1: %v\n", s1)
    fmt.Printf("s2: %v\n", s2)
}

func main() {
    test1()

}

```

运行结果

```
s1: [1 2 3 4 5]
s2: [1 2 3 4 5]
```

### Go map

map是一种`key:value`键值对的数据结构容器。map内部实现是哈希表(`hash`)。

map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。

map是引用类型的。

#### map的语法格式

可以使用内建函数 make 也可以使用 map 关键字来定义 map

```golang
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type
/* 使用 make 函数 */
map_variable = make(map[key_data_type]value_data_type)
```

`map_variable`: map变量名称

`key_data_type`:key的数据类型

`value_data_type`:值的数据类型

实例：

下面声明一个保存个人信息的map

```golang
package main

import "fmt"

//第一种
func test1() {
    var m1 map[string]string     //类型的声明
    m1 = make(map[string]string) //
    fmt.Printf("m1:%v\n", m1)
    fmt.Printf("m1:%T\n", m1) //输出m1的map类型
}

//第二种
func test2() {
    m1 := map[string]string{"name": "viper", "age": "22", "email": "viper@example.com"}
    fmt.Printf("m1: %v\n", m1)
}
func main() {
    //test1()
    test2()

}

```

运行结果：

```
m1: map[age:22 email:viper@example.com name:viper]
m2: map[age:22 email:viper@example.com name:viper]
```

#### 通过key获得value值

```golang
//通过key获得value值
func test3() {
    m1 := map[string]string{"name": "viper", "age": "22", "email": "viper@example.com"}
    var key = "name"
    var value = m1[key]
    fmt.Printf("name= %v", value)

}
```

运行结果

```
name= viper
```

#### 判断某个键是否存在

go语言中有个判断map中键是否存在特殊写法，格式如下：

```golang
value,ok := map[key]
```

如果ok为`true`，存在；否则，不存在。

实例

```golang
func test4() {
    m1 := map[string]string{"name": "viper", "age": "22", "email": "viper@example.com"}
    var k1 = "name"
    var k2 = "age1"
    var k3 = "email"
    v, ok := m1[k1]
    fmt.Printf("v: %v\n", v)
    fmt.Printf("ok: %v\n", ok)
    fmt.Println("---------------")
    v, ok = m1[k2]
    fmt.Printf("v: %v\n", v)
    fmt.Printf("ok: %v\n", ok)
    fmt.Println("---------------")
    v, ok = m1[k3]
    fmt.Printf("v: %v\n", v)
    fmt.Printf("ok: %v\n", ok)
    fmt.Println("---------------")

}
```

运行结果

```
v: viper
ok: true
---------------
v: 
ok: false
---------------
v: viper@example.com
ok: true
---------------
```

<br/>

### Go语言遍历map

可以使用`for range`循环进行map遍历，得到key和value值。

```golang
func test2() {
    m1 := map[string]string{"name": "viper", "age": "22", "email": "viper@example.com"}
    for key, value := range m1 {
        fmt.Printf("key: %v\n", key)
        fmt.Printf("value: %v\n", value)
    }

}
```

运行结果

```
key: email
value: viper@example.com
key: name
value: viper
key: age
value: 22
```

#### 遍历key

```golang
package main

import "fmt"

func test1() {
    m1 := map[string]string{"name": "viper", "age": "22", "email": "viper@example.com"}

    for key := range m1 {
        fmt.Printf("key: %v\n", key)
    }

}
func main() {
    test1()

}

```

运行结果

```
key: name
key: age
key: email
```

#### 遍历key和value

```

```

<br/>

### Go 函数

#### golang函数简介

函数的go语言中的`一级公民`，我们把所有的功能单元都定义在函数中，可以重复使用。函数包含函数的名称、参数列表和返回值类型，这些构成了函数的签名（signature）。

##### go语言中函数特性

1. go语言中有3种函数：普通函数、匿名函数(没有名称的函数)、方法(定义在struct上的函数)。 
2. go语言中不允许函数重载(overload)，也就是说不允许函数同名。
3. go语言中的函数不能嵌套函数，但可以嵌套匿名函数。
4. 函数是一个值，可以将函数赋值给变量，使得这个变量也成为函数。
5. 函数可以作为参数传递给另一个函数。
6. 函数的返回值可以使一个函数。
7. 函数调用的时候，如果有参数传递给函数，则先拷贝参数的副本，再将副本传递给函数。
8. 函数参数可以没有名称。

<br/>

#### go语言中函数的定义和调用

函数在使用之前必须先定义，可以调用函数来完成某个任务。函数可以重复调用，从而达到代码重用。

#### go语言函数定义语法

```golang
func function_name( [parameter list] ) [return_type]
{
  函数体
}
```

语法解析：

- `func`：函数由func开始声明
- `function_name `：函数名称，函数名和参数列表一起构成了函数签名。
- `[parameter list]`：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
- `return_type`：返回值类型，函数返回一列值。`return_types`是该列值的数据类型。有些功能不需要返回值，这种情况下`return_types`不是必须的。
- 函数体：函数定义的代码集合。

<br/>

#### Go语言函数定义实例

定义一个求和的函数

```golang
func sum(a int, b int) (ret int) {
    ret = a + b
    return ret

}

func main() {
    r := sum(1, 2)
    fmt.Printf("r: %v\n", r)

}

```

运行结果：

```
r: 3
```

定义一个比较两个数大小的函数

```golang
func comp(a int, b int) (max int) {
    if a > b {
        max = a
    } else {
        max = b
    }
    return max

}
func main() {
    r := comp(3, 2)
    fmt.Printf("r: %v\n", r)

}
```

<br/>

#### Go语言函数调用

当我们要完成某个任务时，可以调用函数来完成。调用函数要传递参数，如何有返回值可以获得返回值。

### Go 函数的返回值

函数可以有0或多个返回值，返回值需要指定数据类型，返回值通过`return`关键字来指定。

`return`可以有参数，也可以没有参数，这些返回值可以有名称，也可以没有名称。go中的函数可以有多个返回值。

1. `return`关键字中指定了参数时，返回值可以不用名称。如果`return`省略参数，则返回值部分必须带名称。
2. 当返回值有名称时，必须使用括号包围，逗号分割，即使只有一个返回值。
3. 但即使返回值命名了，`return`中也可以强制指定其他返回值的名称，也就是说`return`的优先级更高。
4. 命名的返回值时预先申明好的，在函数内部可以直接使用，无需再次声明。命名返回值的名称不能和函数参数名称相同，否则报错提示变量重复定义。
5. `return`中可以有表达式，但不能出现赋值表达式，这和其他语言可能有所不同。例如 `return a+b` 是正确的，但 `return c=a+b`是错误的。

#### Go语言函数返回值实例

没有返回值

```golang
package main

import "fmt"

func f1() {
    fmt.Println("我没有返回值，只是进行一些计算")
}
func main() {
    f1()

}

```

运行结果：

```
我没有返回值，只是进行一些计算
```

有一个返回值

```golang
package main

import "fmt"


func sum(a int, b int) (ret int) {
    ret = a + b
    return ret
}
func main() {
    r := sum(1, 2)
    fmt.Printf("r: %v\n", r)

}

```

运行结果

```
r: 3
```

多个返回值，且在return中指定返回的内容

```golang
package main

import "fmt"

func test1() (name string, age int) {
    name = "viper"
    age = 22
    return name, age
}

func main() {
    name, age := test1()
    fmt.Printf("name: %v\n", name)
    fmt.Printf("age: %v\n", age)

}

```

运行结果：

```golang
name: viper
age: 22
```

多个返回值，返回值名称没有被使用

```golang
package main

import "fmt"

func test1() (name string, age int) {
	name = "tom"
	age = 22
	return                   //等价于return name,age
}
func main() {
	name, age := test1()
	fmt.Printf("name: %v\n", name)
	fmt.Printf("age: %v\n", age)

}

```

运行结果：

```
name: tom
age: 22
```

return覆盖命名返回值，返回值名称没有被使用

```golang
package main

import "fmt"

func test1() (name string, age int) {
	n := "tom"
	a := 22
	return n, a
}
func main() {
	name, age := test1()
	fmt.Printf("name: %v\n", name)
	fmt.Printf("age: %v\n", age)

}

```

运行结果：

```
name: tom
age: 22
```

> Go中经常会使用其中一个返回值作为函数是否执行成功、是否有错误信息的判断条件。例如`return value，exists`、`return value ok`、
> 
> `return value err`等。

> 当函数的返回值过多时，例如有4个以上的返回值，应该将这些返回值收集到容器中，然后以返回容器的方式去返回。例如，同类型的返回值可以放进slice（切片）中，不同类型的返回值可以放入map中，

> 但函数有多个返回值时，如果其中某个或某几个值不想使用，可以通过下划线 `_` 来丢弃这些返回值。例如下面的` f1 ` 函数两个返回值，调用该函数时，丢弃了第二个返回值b，只保留了第一个返回值a赋值给了变量a。

```golang
package main

import "fmt"

func test1() (int, int) {
	return 1, 2
}
func main() {
	_, x := test1()
	fmt.Printf("x: %v\n", x)
}

```

运行结果

```
x: 2
```

### Go函数的参数

go语言函数可以有0或多个参数，参数需要指定**==数据类型==**。

声明函数时的参数列表叫做形参，调用时传递的参数叫做实参。

go语言是通过**==传值的方式传参==**的，意味着传递给函数的是拷贝后的副本，所以函数内部访问、修改的也是这个副本。

go语言可以使用变长参数，有时候并不能确定参数的个数，可以使用变长参数，可以在函数定义语句的参数部分使用`ARGS...TYPE`的方式。这时会将...代表的参数全部保存到一个名为ARGS的slice中，注意这些参数的数据类型都是TYPE。

#### go语言函数的参数实例

go语言传参

```golang

import "fmt"

// 形参列表
func test1(a int, b int) int {
	if a < b {
		return b
	} else {
		return a
	}
}
func main() {
	//实参列表
	r := test1(1, 2)
	fmt.Printf("r: %v\n", r)

}

```

按值穿参

```
package main

import "fmt"

func test1(a int) {
	a = 100
}
func main() {
	a1 := 200
	test1(a1)
	fmt.Printf("a1: %v\n", a1)

}
```

运行结果

```
a1: 200
```

从运行结果可以看到，调用函数f1后，a的值并没有被改变，说明参数传递是拷贝了一个副本，也就是拷贝了一份新的内容进行运算。

> `map`、`sclice`、`interface`、`channel`这些数据类型本身就是 **指针** 类型的，所以就算是拷贝传值也是拷贝的指针，拷贝后的参数仍然指向底层数据结构，所以修改他们**可能**会影响外部数据结构的值。

```go
package main

import "fmt"

func test1(a []int) {
	a[0] = 100
}

func main() {
	a := []int{1, 2}
	fmt.Printf("a: %v\n", a)

}

```

运行结果

```
a: [1 2]
```

> 从运行结果发现，调用函数后，slice（切片）内容被改变了。

变长参数

```go
package main

import "fmt"

func test1(a []int) {
	a[0] = 100
}
func test3(args ...int) {
	for _, v := range args {
		fmt.Printf("v: %v\n", v)

	}
}

func main() {
	test3(1, 2, 3, 4, 5, 6)
	test3(7, 8, 9, 10)

}

```

运行结果

```
v: 1
v: 2
v: 3
v: 4
v: 5
v: 6
v: 7
v: 8
v: 9
v: 10
```

多类型变长参数

```go
func test4(name string, ok bool, args ...int) {
	fmt.Printf("name: %v\n", name)
	fmt.Printf("ok: %v\n", ok)
	fmt.Printf("args: %v\n", args)

}
func main() {
	test4("viper", true, 1, 2, 3, 4, 5)

}
```

运行结果

```
name: viper
ok: true
args: [1 2 3 4 5]
```

### Go函数类型与函数变量

可以使用`type`关键字来定义一个函数类型，语法格式如下

```go
type fun func(int, int)int{ }
```

上面语句定义了一个`fun` 函数类型，他是一种函数类型，这种函数接收两个`int`类型的参数并返回一个`int`类型的返回值。

下面我们定义两个这样结构的两个函数，一个求和，一个比较大小

```go
func sum(a int, b int) int {
	return a + b
}

func max(a int, b int) int {
	if a < b {
		return b
	} else {
		return a
	}

}
```

下面定义一个fun函数，把sum和max赋值给他

```go
package main

import "fmt"

func sum(a int, b int) int {
	return a + b
}

func max(a int, b int) int {
	if a < b {
		return b
	} else {
		return a
	}

}
func main() {
	type f1 func(int, int) int
	var ff f1
	ff = sum
	r := ff(1, 2)
	fmt.Printf("r: %v\n", r)
	ff = max
	r = ff(1, 2)
	fmt.Printf("r: %v\n", r)

}

```

运行结果

```
r: 3
r: 2
```

### Go高阶函数

go语言的函数，可以作为函数的参数，传递给另外一个**函数**，作为另外一个函数的**返回值**返回。

```go
ppackage main

import "fmt"

func sayHello(name string) {
	fmt.Printf("Hello %s", name)

}

func test(name string, f func(string)) {
	f(name)
}

func main() {
	test("tom", sayHello)

}

```

运行结果

```
Hello tom
```

#### go语言函数作为返回值

```go
package main

import "fmt"

func add(a int, b int) int {
	return a + b
}

func sub(a int, b int) int {
	return a - b
}

func cal(operator string) func(int, int) int {
	switch operator {
	case "+":
		return add
	case "-":
		return sub
	default:
		return nil
	}

}

func main() {

	ff := cal("+")
	r := ff(1, 2)
	fmt.Printf("r: %v\n", r)
	ff = cal("-")
	r = ff(1, 2)
	fmt.Printf("r: %v\n", r)

}

```

运行结果：

```
r: 3
r: -1
```

### Go语言 匿名函数

go语言函数不能嵌套，但是在函数内部可以定义匿名函数，实现一下简单功能调用。

所谓匿名函数，就是没有名称的函数。

语法格式如下

```go
func (参数列表)(返回值)
```

> 当然可以既没有参数，也可以没有返回值。

匿名函数实例

```go
package main

import "fmt"

func main() {
  //匿名函数
	max := func(a int, b int) int {
		if a > b {
			return a
		} else {
			return b
		}
	}
	r := max(1, 2)
	fmt.Printf("r: %v\n", r)

}

```

运行结果

```
r: 2
```

匿名函数自己调用自己

```go
func main() {
	//自己调用自己
	r2 := func(a int, b int) int {
		if a > b {
			return a
		} else {
			return b
		}

	}(1, 2)
   
	fmt.Printf("r2: %v\n", r2)
}
```

运行结果

```
r2: 2
```

### Golang 闭包

闭包可以理解成**定义在一个函数内部的函数**。在本质上，闭包是将函数内部和函数外部连接起来的桥梁。或者说是函数和其引用环境的组合体。

闭包指的是一个函数和与其相关的引用环境组合而成的实体。简单来说，`闭包=函数+引用环境`。首先我们来看一个例子：

```go

```

