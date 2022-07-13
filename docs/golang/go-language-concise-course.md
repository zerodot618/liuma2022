# Go 语言介绍
## Go 语言的起源（了解）
- Go 语言起源 2007 年，并于 2009 年正式对外发布。
- 从 2009 年 9 月 21 日开始作为谷歌公司 20% 兼职项目，即相关员工利用 20% 的空余时间来参与 Go 语言的研发工作。
- Go 的三位领导者均是著名的计算机工程师：
    - Robert Griesemer（参与开发 Java HotSpot 虚拟机）
    - Rob Pike（Go 语言项目总负责人，贝尔实验室 Unix 团队成员，参与的项目包括 Plan 9，Inferno 操作系统和 Limbo 编程语言）
    - Ken Thompson（贝尔实验室 Unix 团队成员，C 语言、Unix 和 Plan 9 的创始人之一，与 Rob Pike 共同开发了 UTF-8 字符集规范）

## Go 语言的特性（理解）
1. **并发编程**

    Go 语言引入了 `goroutine`，它是 Go 实现快速高效的并发编程的关键。通过调用 `go` 关键字，就可以让函数以 goroutine 的方式进行运行，也就是以协程为单位进行运行。协程比线程更加的轻量级，也更节省系统资源，这使得我们可以创建大量的 goroutine，从而进行轻松且高质量的并发编程。同时，goroutine 内部采用管道 `channel` 进行消息传递，从而实现共享内存。

2. **错误处理**
    
    Go 语言中的错误处理的哲学和 C 语言一样，函数通过返回错误类型 (`error`) 或者 `bool` 类型（不需要区分多种错误状态时）表明函数的执行结果，调用检查返回的错误类型值是否是 `nil` 来判断调用结果。并引入了 `defer` 关键字用于标准的错误处理流程，并提供了内置函数 `panic`、`recover` 完成异常的抛出与捕捉。

3. **垃圾回收**

    Go 语言自带垃圾自动回收的功能，让 Go 语言不需要 `delete` 关键字，也不需要 `free()` 来释放内存。因此开发者无需考虑何时需要释放之前分配的内存的问题，系统会自动帮我们判断，并在适当的时候进行垃圾处理。垃圾自动回收是 Go 语言的一个特点，也是一大亮点。

4. **多返回值**

    支持函数的多返回值功能，这个特性让开发者可以从原来用各种比较别扭的方式返回多个值的痛苦中解脱出来，不需要为了一次返回多个值而专门定义一个结构体。并且每个返回值都有自己的名字，开发者还可以选择具体需要返回的值，只需要使用下划线 `_` 作为占位符来丢掉不要的返回值即可。

5. **匿名函数**

    Go 语言支持常规的匿名函数和闭包，开发者可以随意对匿名函数变量进行传递和调用，下面就是一个匿名函数样例：
    ```
    f := func(x,y int) int {
        return x+y
    }
    ```

## Go 语言可以做什么（了解）
Go 语言是谷歌发布的第二款开源编程语言。专门针对多处理器系统应用程序的编程进行了优化，使用 Go 编译的程序可以媲美 C 或 C++ 代码的速度，而且更加安全、支持并行进程。

Go 的目标是希望提升现有编程语言对程序库等依赖性 (dependency) 的管理，这些软件元素会被应用程序反复调用。由于存在并行编程模式，因此也被设计用来解决多处理器的任务。目前，已经有很多公司开始使用 Go 语言开发自己的服务，甚至完全转向 Go 开发，也诞生了很多基于 Go 的服务和应用，比如 `Dokcer`、`k8s` 等。


## HelloWorld 程序
让我们通过一个 Go 语言版本的 HelloWorld 来认识这门新语言的模样。
```
package main

import (
    "fmt" //导入fmt包，调用其中的Println()函数
)

func main() {
    fmt.Println("Hello，world！")
}
```
终端下运行结果：
```
go run hello.go
```

# Go 语言基础
## 常量
常量使用关键字 `const` 声明，下面有几个例子：
```
const limit = 512
const top uint16 = 1421
const Pi float64 = 3.1415926
const x,y int = 1,3 //多重赋值
```

常量定义可以限定常量类型，但不是必需的。如果定义常量时没有指定类型，那么该常量就是无类型常量，也叫字面常量。

当需要设置多个常量的时候，不必重复使用 const 关键字，可以使用以下语法：
```
const (
    Cyan = 0
    Black = 1
    White = 2
)
```

Go 语言还预定义了这些常量：`true`、`false`、`iota`。

`iota` 是一个可以被编译器修改的常量，在 `const` 关键字出现时被重置为 0，在下一个 `const` 出现之前，每出现一次 `iota`，其所代表的数字自动加 1。下面通过一个例子讲解 `iota` 的用法：
```
const (
    a = iota  //a == 0
    b = iota  //b ==1
    c = iota  //c == 2
)

const d = iota //d==0,因为const的出现，iota被重置为0
```

## 变量
Go 语言引入了关键字 `var` 对变量进行声明，也可以使用 `:=` 来对变量直接进行初始化，Go 编译器会自动推导出该变量的类型，这大大的方便了开发者的工作。但是需要注意的是 `:=` 左侧的变量不能是已经被声明过的，否则会导致编译器错误。

以下是 Go 声明和初始化变量的各种方法：
```
var a int
var b string
var c float64
var d [5] int  //数组
var e [] int   //数组切片
var f * int    //正确
var v1 int = 5 //正确
var v2 = 5     //正确，编译器自动推导出V2类型
v3 := 5        //正确，编译器自动推导出V3的类型
```

Go 语言提供了大多数语言不支持的**多重赋值**，这使得变量的交换变得十分简单。下面通过一个例子来了解 Go 语言的多重赋值：
```
i := 2
j := 3
i, j = j, i  //交换i和j的值，此时i == 3，j == 2
```

## 数据类型
### 1. 整型
Go 语言提供了 11 种整型，如下列表所示。
|类型|说明|
|:---|:---|
|byte|等同于 uint8|
|int |依赖于不同平台下的实现，可以是 int32 或者 int64|
|int8|[-128, 127]|
|int16|[-32768, 32767]|
|int32|	[-2147483648, 2147483647]|
|int64|	[-9223372036854775808, 9223372036854775807]|
|rune|	等同于 int32|
|uint|	依赖于不同平台下的实现，可以是 uint32 或者 uint64|
|uint8|	[0, 255]|
|uint16|	[0, 65535]|
|uint32|	[0, 4294967295]|
|uint64|	[0, 18446744073709551615]|
|uintptr|	一个可以恰好容纳指针值的无符号整型（对 32 位平台是 uint32, 对 64 位平台是 uint64）|

Go 语言中可以通过 `unsafe.Sizeof` 函数查看类型的字节长度:
```
package main

import (
    "fmt"
    "unsafe"
)

func main() {
    a := 12
    fmt.Println("length of a: ", unsafe.Sizeof(a))
    var b int = 12
    fmt.Println("length of b(int): ", unsafe.Sizeof(b))
    var c int8 = 12
    fmt.Println("length of c(int8): ", unsafe.Sizeof(c))
    var d int16 = 12
    fmt.Println("length of d(int16): ", unsafe.Sizeof(d))
    var e int32 = 12
    fmt.Println("length of e(int32): ", unsafe.Sizeof(e))
    var f int64 = 12
    fmt.Println("length of f(int64): ", unsafe.Sizeof(f))
}
```
以上代码中，`fmt` 包用于格式化字符串，`unsafe` 包含了用于获取 Go 语言类型信息的方法。通过 `unsafe.Sizeof` 方法获取该类型的字节长度。运行以上代码，输出结果：
```
$ go run main.go
length of a:  8
length of b(int):  8
length of c(int8):  1
length of d(int16):  2
length of e(int32):  4
length of f(int64):  8
```
### 2. 浮点型

Go 语言提供了两种浮点类型和两种复数类型，具体如下：
|类型 |说明 |
|:----|:----|
|float32| ±3.402 823 466 385 288 598 117 041 834 845 169 254 40x1038 计算精度大概是小数点后 7 个十进制数|
|float64| ±1.797 693 134 862 315 708 145 274 237 317 043 567 981x1038 计算精度大概是小数点后 15 个十进制数|
|complex32|	复数，实部和虚部都是 float32|
|complex64|	复数，实部和虚部都是 float64|

### 3. 布尔类型
Go 语言提供了内置的布尔值 `true` 和`false`。Go 语言支持标准的逻辑和比较操作，这些操作的结果都是布尔值。值得注意的地方是可以通过 `!b` 的方式反转变量 `b` 的真假。需要注意的是**布尔类型不能接受其他类型的赋值，不支持自动或强制的类型转换**。
```
var a bool
a = true
b := (2 == 3) //b也会被推导为bool类型

//错误示范
var b bool
b = 1 //编译错误
b = bool(1) //编译错误
```

### 4. 字符串
Go 语言中的字符串是 UTF-8 字符的一个序列（当字符为 ASCII 码时则占用 1 个字节，其它字符根据需要占用 2-4 个字节）。UTF-8 是被广泛使用的编码格式，是文本文件的标准编码，其它包括 XML 和 JSON 在内，也都使用该编码。由于该编码对占用字节长度的不定性，Go 中的字符串也可能根据需要占用 1 至 4 个字节，这与其它语言如 C++、Java 或者 Python 不同。Go 这样做的好处是不仅减少了内存和硬盘空间占用，同时也不用像其它语言那样需要对使用 UTF-8 字符集的文本进行编码和解码。

Go 语言中字符串的可以使用双引号 (`"`) 或者反引号 (\`) 来创建。双引号用来创建可解析的字符串字面量，所谓可解析的是指字符串中的一些符号可以被格式化为其他内容，如 `\n` 在在输出时候会被格式化成换行符，如果需要按照原始字符输出必须进行转义。而反引号创建的字符串原始是什么样，那输出还是什么，不需要进行任何转义。
```
t1 := "\"hello\""             //内容： "hello"
t2 := `"hello"`               //内容：和t1一致
t3 := "\u6B22\u8FCE"          //内容：欢迎
```

Go 语言中的部分转义字符如下表所示：
|转义字符|	含义|
|:----|:----|
|\\	|表示反斜线|
|\'	|单引号|
|\"	|双引号|
|\n	|换行符|
|\uhhhh	|4 个 16 进制数字给定的 Unicode 字符|

在 Go 语言中单个字符可以使用单引号 (`'`) 来创建。我们知道 `rune` 类型，它等同于 `int32`，在 Go 语言中，一个单一的字符可以用一个单一的 `rune` 来表示。这也是容易理解的，因为 Go 语言的字符串是 UTF-8 编码，其底层使用 4 个字节表示，也就是 32 bit。

在 Go 语言中，字符串支持切片操作，但是需要注意的是如果字符串都是由 ASCII 字符组成，那可以随便使用切片进行操作，但是如果字符串中包含其他非 ASCII 字符，直接使用切片获取想要的单个字符时需要十分小心，因为对字符串直接使用切片时是通过字节进行索引的，但是非 ASCII 字符在内存中可能不是由一个字节组成。如果想对字符串中字符依次访问，可以使用 `range` 操作符。另外获取字符串的长度可能有两种含义，一种是指获取字符串的字节长度，一种是指获取字符串的字符数量。字符串支持以下操作：
|语法	|描述|
|:----|:----|
|s += t	|将字符串 t 追加到 s 末尾|
|s + t	|将字符串 s 和 t 级联|
|s[n]	|从字符串 s 中索引位置为 n 处的原始字节|
|s[n:m]	|从位置 n 到位置 `m-1` 处取得的字符（字节）串|
|s[n:]	|从位置 n 到位置 `len(s)-1` 处取得的字符（字节）串|
|s[:m]	|从位置 0 到位置 `m-1` 处取得的字符（字节）串|
|len(s)	|字符串 s 中的字节数|
|len([]rune(s))	|字符串 s 中字符的个数，可以使用更快的方法 `utf8.RuneCountInString()`|
|[]rune(s)	|将字符串 s 转换为一个 unicode 值组成的串|
string(chars)	|chars 类型是 `[]rune` 或者 `[]int32`, 将之转换为字符串|
|[]byte(s)	|无副本的将字符串 s 转换为一个原始的字节的切片数组，不保证转换的字节是合法的 UTF-8 编码字节|

Go 语言标准库中的 `fmt` 包提供了打印函数将数据以字符串形式输出到控制台，文件，其他满足 `io.Writer` 接口的值以及其他字符串。`fmt.Printf` 就像 C 语言中的 `printf`` 函数一样，我们可以提供一些格式化指令，让 Go 语言对输出的字符串进行格式化。同样的我们可以使用一些格式化修饰符，改变格式化指令的输出结果， 如左对齐等。常用的格式化指令如下：
|格式化指令|含义|
|:----|:----|
|%%	|% 字面量|
|%b	|一个二进制整数，将一个整数格式化为二进制的表达方式|
|%c	|一个 Unicode 的字符|
|%d |十进制数值|
|%o	|八进制数值|
|%x	|小写的十六进制数值|
|%X	|大写的十六进制数值|
|%U	|一个 Unicode 表示法表示的整形码值，默认是 4 个数字字符|
|%s	|输出以原生的 UTF-8 字节表示的字符，如果 console 不支持 UTF-8 编码，则会输出乱码|
|%t	|以 true 或者 false 的方式输出布尔值|
|%v	|使用默认格式输出值，或者使用类型的 String() 方法输出的自定义值，如果该方法存在的话|
|%T	|输出值的类型|

常用的格式化指令修饰符如下：

- `空白` 如果输出的数字为负，则在其前面加上一个减号 `-`。如果输出的是整数，则在前面加一个空格。使用 `%x` 或者 `%X` 格式化指令输出时，会在结果之间添加一个空格。例如 `fmt.Printf("% X", "实")` 输出 E5 AE 9E。
- `#`
    - `%#o` 输出以 `0` 开始的八进制数据。
    - `%#x` 输出以 `0x` 开始的十六进制数据。
- `+` 让格式化指令在数值前面输出 `+` 号或者 `-` 号，为字符串输出 ASCII 字符（非 ASCII 字符会被转义），为结构体输出其字段名。
- `-` 让格式化指令将值向左对齐（默认值为像右对齐）。
- `0` 让格式指令以数字 0 而非空白进行填充。
```
package main

import (
    "fmt"
)

func main() {
    text := "\u5B9E\u9A8C\u697C"
    fmt.Printf("bool output:\n%t\n%t\n\n", true, false)
    fmt.Println("number output, origin value: 64")
    fmt.Printf("|%b|%8b|%-8b|%08b|% 8b|\n", 64, 64, 64, 64, 64)
    fmt.Printf("|%x|%8x|%-8x|%08X|% 8X|\n\n", 64, 64, 64, 64, 64)
    fmt.Println(`text output, origin value: \u5B9E\u9A8C\u697C`)
    fmt.Printf("content: %s\n", text)
    fmt.Printf("hex value: % X\nUnicode value: ", text)
    for _, char := range text {
        fmt.Printf("%U ", char)
    }
    fmt.Println()
    bytes := []byte(text)
    fmt.Printf("value of bytes: %s\n", bytes)
    fmt.Printf("hex value of bytes: % X\n", bytes)
    fmt.Printf("origin value of bytes: %v\n", bytes)

}
```
运行代码，输出如下：
```
$ go run main.go
bool output:
true
false

number output, origin value: 64
|1000000| 1000000|1000000 |01000000| 1000000|
|40|      40|40      |00000040|      40|

text output, origin value: \u5B9E\u9A8C\u697C
content: 实验楼
hex value: E5 AE 9E E9 AA 8C E6 A5 BC
Unicode value: U+5B9E U+9A8C U+697C
value of bytes: 实验楼
hex value of bytes: E5 AE 9E E9 AA 8C E6 A5 BC
origin value of bytes: [229 174 158 233 170 140 230 165 188]
```

### 5. 字符类型
在 Go 语言中支持两个字符类型，一个是 `Byte`（实际上是 Unit8 的别名），代表 UTF-8 字符串的单个字节的值；另一个是 `rune`，代表单个 Unicode 字符。

处于简化语言的考虑，Go 语言的多数 API 都假设字符串为 UTF-8 编码。尽管 Unicode 字符在标准库中有支持，但实际很少使用。

## 数组
Go 语言的数组是一个定长的序列，其中的元素类型相同。多维数组可以简单地使用自身为数组的元素来创建。数组的元素使用操作符号 `[ ]` 来索引，索引从 `0` 开始，到 `len(array)-1` 结束。数组使用以下语法创建：
- `[length]Type`
- `[N]Type{value1, value2, ..., valueN}`
- `[...]Type{value1, value2, ..., valueN}`

如果使用了 `...`（省略符）操作符，Go 语言会为我们自动计算数组的长度。在任何情况下，一个数组的长度都是固定的并且不可修改。数组的长度可以使用 `len()` 函数获得。由于数组的长度是固定的，因此数组的长度和容量都是一样的，因此对于数组而言 `cap()` 和 `len()` 函数返回值都是一样的。数组也可以使用和切片一样的语法进行切片，只是其结果为一个切片，而非数组。同样的，数组也可以使用 `range` 进行索引访问。

## 切片
Go 语言的切片比数组更加灵活，强大而且方便。数组是按值传递的（即是传递的副本），而切片是引用类型，传递切片的成本非常小，而且是不定长的。数组是定长的，而切片可以调整长度。创建切片的语法如下：
- `make([ ]Type, length, capacity)`
- `make([ ]Type, length)`
- `[ ]Type{}`
- `[ ]Type{value1, value2, ..., valueN}`

内置函数 `make()` 用于**创建切片、映射和通道**。当用于创建一个切片时，它会创建一个隐藏的初始化为零值的数组，然后返回一个引用该隐藏数组的切片。该隐藏的数组与 Go 语言中的所有数组一样，都是固定长度，如果使用第一种语法创建，那么其长度为切片的容量 `capacity`；如果是第二种语法，那么其长度记为切片的长度 `length`。一个切片的容量即为隐藏数组的长度，而其长度则为不超过该容量的任意值。另外可以通过内置的函数 `append()` 来增加切片的容量。切片可以支持以下操作：
```
package main

import (
    "fmt"
)

func main() {
    a := [...]int{1, 2, 3, 4, 5, 6, 7}
    fmt.Printf("len and cap of array %v is: %d and %d\n", a, len(a), cap(a))
    fmt.Printf("item in array: %v is:", a)
    for _, value := range a {
        fmt.Printf("% d", value)
    }

    fmt.Println()

    s1 := a[3:6]
    fmt.Printf("len and cap of slice: %v is: %d and %d\n", s1, len(s1), cap(s1))
    fmt.Printf("item in slice: %v is:", s1)
    for _, value := range s1 {
        fmt.Printf("%d", value)
    }

    fmt.Println()

    s1[0] = 456
    fmt.Printf("item in array changed after changing slice: %v is:", s1)
    for _, value := range a {
        fmt.Printf("%d", value)
    }

    fmt.Println()

    s2 := make([]int, 10, 20)
    s2[4] = 5
    fmt.Printf("len and cap of slice: %v is: %d and %d\n", s2, len(s2), cap(s2))
    fmt.Printf("item in slice %v is:", s2)
    for _, value := range s2 {
        fmt.Printf("%d", value)
    }

    fmt.Println()
}
```
运行代码输出如下：
```
$  go run main.go
len and cap of array [1 2 3 4 5 6 7] is: 7 and 7
item in array: [1 2 3 4 5 6 7] is: 1 2 3 4 5 6 7
len and cap of slice: [4 5 6] is: 3 and 4
item in slice: [4 5 6] is: 4 5 6
item in array changed after changing slice: [456 5 6] is: 1 2 3 456 5 6 7
len and cap of slice: [0 0 0 0 5 0 0 0 0 0] is: 10 and 20
item in slice [0 0 0 0 5 0 0 0 0 0] is: 0 0 0 0 5 0 0 0 0 0
```
## 包
Go 语言组织代码的方式是包，包是各种类型和函数的集合。在包中，如果标示符（类型名称，函数名称，方法名称）的首字母是大写，那这些标示符是可以被导出的，也就是说可以在包以外直接使用。

`$GOPATH` 环境变量（指向一个或多个目录），以及其子目录 `src` 目录的，当我们使用 `import` 关键字导入包的时候，Go 语言会在 `$GOPATH` 和 `GOROOT` 目录中搜索包。

# Go 流程控制
## 流程控制
Go 语言提供的流程控制语句包括 `if`、`switch`、`for`、`goto`、`select`，其中 `select` 用于监听 `channel`（通道）。

**if 语句**

语法：
```
if optionalStatement1; booleanExpression1 {
    block1
} else if optionalStatement2; booleanExpression2 {
    block2
} else {
    block3
}
```
其中 `optionalStatement` 是可选的表达式，真正决定分支走向的是 `booleanExpression1` 的值。

**for 语句**

Go 语言的 `for` 语句可以遍历数组，切片，映射等类型，也可以用于无限循环。以下是其语法：
```
for { // 无限循环
    block
}

for booleanExpression { // while循环，在Go语言中没有while关键字

}

for index, char := range aString { // 迭代字符串

}

for item := range aChannel { // 迭代通道

}
```
**跳转语句**

Go 语言中使用 `goto` 关键字实现跳转。`goto` 语句的语义非常简单，就是跳转到本函数内的某个标签，例如：
```
func myfunc(){
    i := 0
    THIS: //定义一个THIS标签
    fmt.Println(i)
    i++
    if i < 1 {
        goto THIS //跳转到THIS标签
    }
}
```

**switch 语句**

Go 语言中 `switch` 分支既可用于常用的分支就像 C 语言中的 `switch` 一样，也可以用于类型开关，所谓类型开关就是用于判断变量属于什么类型。但是需要注意的是 Go 语言的 `switch` 语句不会自动贯穿，相反，如果想要贯穿需要添加 `fallthrough` 语句。表达式开关 `switch` 的语法如下：
```
switch optionalStatement; optionalExpression {
    case expression1: block1
    ...
    case expressionN: blockN
    default: blockD
}
```
下面是个例子：
```
switch {        // 没有表达式，默认为True值，匹配分支中值为True的分支
    case value < minimum:
        return minimum
    case value > maximum:
        return maximum
    default:
        return value
}
```
在上面的例子中，switch 后面没有默认的表达式，这个时候 Go 语言默认其值为 `True`。

在前面我们提到过类型断言，如果我们知道变量的类型就可以使用类型断言，但是当我们知道类型可能是许多类型中的一种时候，我们就可以使用类型开关。其语法如下：
```
switch optionalStatement; typeSwitchGuard {
    case type1: block1
    ...
    case typeN: blockN
    default: blockD
}
```
下面是个例子：
```
package main

import (
    "fmt"
)

func classchecker(items ...interface{}) { // 创建一个函数，该函数可以接受任意多的任意类型的参数
    for i, x := range items {
        switch x := x.(type) { // 创建了影子变量
        case bool:
            fmt.Printf("param #%d is a bool, value: %t\n", i, x)
        case float64:
            fmt.Printf("param #%d is a float64, value: %f\n", i, x)
        case int, int8, int16, int32, int64:
            fmt.Printf("param #%d is a int, value: %d\n", i, x)
        case uint, uint8, uint16, uint32, uint64:
            fmt.Printf("param #%d is a uint, value: %d\n", i, x)
        case nil:
            fmt.Printf("param #%d is a nil\n", i)
        case string:
            fmt.Printf("param #%d is a string, value: %s\n", i, x)
        default:
            fmt.Printf("param #%d's type is unknow\n", i)
        }
    }
}

func main() {
    classchecker(5, -17.98, "AIDEN", nil, true, complex(1, 1))

}
```
程序运行输出如下：
```
$ go run switch_t.go
param #0 is a int, value: 5
param #1 is a float64, value: -17.980000
param #2 is a string, value: AIDEN
param #3 is a nil
param #4 is a bool, value: true
param #5's type is unknow
```

## 函数
Go 语言可以很方便的自定义函数，其中有特殊的函数 `main` 函数。`main` 函数必须出现在 `main` 包里，且只能出现一次。当 Go 程序运行时候会自动调用 `main` 函数开始整个程序的执行。`main` 函数不可接收任何参数，也不返回任何结果。

**函数的定义**

在 Go 语言中，函数的基本组成包括：关键字 `func`、函数名、参数列表、返回值、函数体和返回语句，这里我们用一个简单的加法函数来对函数的定义进行说明。
```
package add

func Add(a int, b int) (num int){
    return a + b
}
```

**函数的调用**

函数调用非常简单，先将被调用函数所在的包导入，就可以直接使用该函数了。注意需要把包文件夹放到 $GOPATH 目录中，实例如下：
```
package main

import (
    "add" //导入 add 包
    "fmt"
)

func main(){
    c := add.Add(1, 2) //调用 add 包中的 add 函数
    fmt.Println(c)
}
```

**函数的多返回值**

Go 语言的函数和方法可以有多个返回值，这是 Go 提供的一个优美的特性，示例如下：
```
package Divide
import "errors"

func divide (a int, b int) (num int, err error){ //定义两个返回值
    if b == 0 {
        err = errors.New("被除数不能为零！")
        return
    }
    return a / b, nil   //支持多个返回值
}
```

**匿名函数**

在 Go 语言中，你可以在代码里随时定义匿名函数，匿名函数由一个不带函数名的函数声明和函数体组成，示例如下：
```
func (a, b, c int) bool {
    return a * b < c
}
```
你可以将匿名函数直接赋值给一个变量，也可以直接调用运行，示例如下：
```
x := func (a, b, c int) bool {
    return a * b < c
}

func (a, b, c int) bool {
    return a * b < c
} (1, 2, 3) //小括号内直接给参数列表表示函数调用
```

## 类型转换

Go 语言提供了一种在不同但相互兼容的类型之间相互转换的方式，这种转换非常有用并且是安全的。但是需要注意的是在数值之间进行转换可能造成其他问题，如精度丢失或者错误的结果。以下是类型转换的语法：
- `resultOfType := Type(expression)` 

几个例子：
```
x := int16(2345)        // 声明一个类型为int16的整数，其值为2345
y := int32(x)           // 将int16类型的整数转换为int32类型
a := uint16(65000)       // 声明一个类型为uint16类型的整数
b := int16(a)           // 转换为int16类型，虽然能转换成功，但是由于65000超过in16类型的范围，会导致结果错误，b的值为 -536
```
另外在 Go 语言中可以通过 `type` 关键字声明类型，如 `type StringsSlice []string` 将 `[]string`（`string` 类型的切片）声明为 `StringSlice` 类型。

## 类型断言
说到类型断言就需要先了解下 Go 语言中的接口。在 Go 语言中接口是一个自定义类型。它声明了一个或者多个方法。任何实现了这些方法的对象（类型）都满足这个接口。

接口是完全抽象的，不能实例化。`interface{}` 类型表示一个空接口，任何类型都满足空接口。也就是说 `interface{}` 类型的值可以用于表示任意 Go 语言类型的值。

既然 `interface{}` 可以用于表示任意类型，那有的时候我们需要将 `interface{}` 类型转换为我们需要的类型，这个操作称为类型断言。

**一般情况下只有我们希望表达式是某种特定类型的值时才使用类型断言**。Go 语言中可以使用以下语法：

- `resultOfType, boolean := expression.(Type)`：安全的类型断言。
- `resultOfType := expression.(Type)`：非安全的类型断言，失败时程序会产生异常。
```
package main

import (
    "fmt"
)

func main() {
    x := uint16(65000)
    y := int16(x) // 将 x转换为int16类型
    fmt.Printf("type and value of x is: %T and %d\n", x, x) // %T 格式化指令的作用是输出变量的类型
    fmt.Printf("type and value of y is: %T and %d\n", y, y)

    var i interface{} = 99 // 创建一个interface{}类型，其值为99
    var s interface{} = []string{"left", "right"}
    j := i.(int) // 我们假设i是兼容int类型，并使用类型断言将其转换为int类型
    fmt.Printf("type and value of j is: %T and %d\n", j, j)

    if s, ok := s.([]string); ok { // 创建了影子变量，if的作用域中覆盖了外部的变量s
        fmt.Printf("%T -> %q\n", s, s)
    }
}
```
运行程序：
```
$ go run main.go
type and value of x is: uint16 and 65000
type and value of y is: int16 and -536
type and value of j is: int and 99
[]string -> ["left" "right"]
```

## 错误处理
错误处理是任何语言都需要考虑到的问题，而 Go 语言在错误处理上解决得更为完善，优雅的错误处理机制是 Go 语言的一大特点。

**error**
Go 语言引入了一个错误处理的标准模式，即 error 接口，该接口定义如下
```
type error interface {
    Error() string
}
```
对于大多数函数，如果要返回错误，可以将 error 作为多返回值的最后一个：
```
func foo(param int)(ret int, err error)
{
  ...
}
```
调用时的代码：
```
n, err := foo(0)
if err != nil {
    //  错误处理
} else {
    // 使用返回值n
}
```
我们还可以自定义错误类型，创建源文件 error.go，输入以下代码：
```
package main

import "fmt"
import "errors"

//自定义的出错结构
type myError struct {
    arg  int
    errMsg string
}
//实现Error接口
func (e *myError) Error() string {
    return fmt.Sprintf("%d - %s", e.arg, e.errMsg)
}

//两种出错
func error_test(arg int) (int, error) {
    if arg < 0  {
         return -1, errors.New("Bad Arguments - negtive!")
     }else if arg >256 {
        return -1, &myError{arg, "Bad Arguments - too large!"}
    }
    return arg*arg, nil
}

//相关的测试
func main() {
    for _, i := range []int{-1, 4, 1000} {
        if r, e := error_test(i); e != nil {
            fmt.Println("failed:", e)
        } else {
            fmt.Println("success:", r)
        }
    }
}
```

**defer**

可以在 Go 函数中添加多个 `defer` 语句，当函数执行到最后时，这些 `defer` 语句会按照逆序执行（即最后一个 `defer` 语句将最先执行），最后该函数返回。特别是当你在进行一些打开资源的操作时，遇到错误需要提前返回，在返回前你需要关闭相应的资源，不然很容易造成资源泄露等问题。如下代码所示，我们一般写打开一个资源是这样操作的：
```
func CopyFile(dst, src string) (w int64, err error) {
    srcFile, err := os.Open(src)
    if err != nil {
        return
    }

    defer srcFile.Close()

    dstFile, err := os.Create(dst)
    if err != nil {
        return
    }

    defer dstFile.Close()

    return io.Copy(dstFile, srcFile)
}
```
如果 `defer` 后面一条语句干不完清理工作，也可以使用一个匿名函数：
```
defer func(){
    ...
}()
```
注意，`defer` 语句是在 `return` 之后执行的：
```
func test() (result int) {
    defer func() {
        result = 12
    }()
    return 10
}

func main() {
    fmt.Println(test())     // 12
}
```

**panic 和 recover**

`panic()` 函数用于抛出异常，`recover()` 函数用于捕获异常，这两个函数的原型如下：
```
func panic(interface{})
func recover() interface{}
```
在一个函数中调用 `panic()` 时，正常的函数执行流程将立即终止，但函数中之前使用 defer 关键字延迟执行的语句将正常展开执行，之后该函数将返回到调用函数，并导致逐层向上执行 `panic()` 流程，直至所属的 `goroutine` 中所有正在执行的函数被终止。错误信息将被报告，包括在调用 `panic()` 函数时传入的参数，这个过程称为错误流程处理。

`panic()` 接受一个 `interface{}` 参数，可支持任意类型，例如：
```
panic(404)
panic("network broken")
panic(Error("file not exists"))
```
在 `defer` 语句中，可以使用 `recover()` 终止错误处理流程，这样可以避免异常向上传递，但要注意 `recover()` 之后，程序不会再回到 `panic()` 那里，函数仍在 `defer` 之后返回。
```
func foo() {
    panic(errors.New("i'm a bug"))
    return
}

func test() (result int) {
    defer func() {
        if r := recover(); r != nil {
            err := r.(error)
            fmt.Println("Cache Exception:", err)
        }
    }()
    foo()
    return 10
}

func main() {
    fmt.Println(test())     // 0
}
```

# Go 面向对象
## 自定义类型及结构体
**自定义类型**

Go 语言的中结构体 `struct` 与 C++、JAVA 中的类 `class` 相似，但 Go 放弃了传统面向对象的诸多特性，只保留了组合。
- type typeName typeSpecification

其中，`typeName` 可以是一个包或者函数内唯一合法的 Go 标示符。`typeSpecification` 可以是任何内置的类型，一个接口或者是一个结构体。所谓结构体，它的字段是由其他类型或者接口组成。例如我们通过结构体定义了一下类型：
```
type ColorPoint struct {
    color.Color     // 匿名字段(嵌入)
    x, y int        // 具名字段(聚合)
}
```
以上代码我们通过结构体自定义了类型 `ColorPoint`，结构体中 `color.Color` 字段是 `color` 包的类型 `Color`，这个字段没有名字，所以被称为匿名的，也是嵌入字段。字段 `x` 和 `y` 是有变量名的，所以被称为具名字段。假如我们创建了类型 ColorPoint 的一个值 `point`（通过语法：`point := ColorPoint{}` 创建），那么这些字段可以通过 `point.Color`、`point.x`、`point.y` 访问。其他面向对象语言中的"类 (`class`)"、"对象 (`object`)"、"实例 (`instance`)"在 Go 语言中我们完全避开使用。相反的我们使用"类型 (`type`)"和其对应的"值"，其中自定义类型的值可以包含方法。

定义了结构体后如何创建并初始化一个对象实例呢？Go 语言支持以下几种方法进行实现：
```
//先定义一个结构体Man
type Man struct{
    name string
    age int
}
//对象创建与初始化
man := new(Man)
man := &Man{}
man := &Man{"Tom", 18}
man := &Man{name: "Tom", age: 18}
```

为了更加方便的创建对象，我们一般会使用一个全局函数来完成对象的创建，这和传统的“构造函数”类似。
```
func NewMan(name string, age int) *Man {
    return &Man{name, age}
}
```

## 方法
方法是作用在自定义类型上的一类特殊函数，通常自定义类型的值会被传递给该函数，该值可能是以指针或者复制值的形式传递。定义方法和定义函数几乎相同，只是需要在 `func` 关键字和方法名之间必须写上接接受者。例如我们给类型 `Count` 定义了以下方法：
```
type Count int

func (count *Count) Increment() { *count++ }  // 接受者是一个 `Count` 类型的指针
func (count *Count) Decrement() { *count-- }
func (count Count) IsZero() bool { return count == 0 }
```
以上代码中，我们在内置类型 `int` 的基础上定义了自定义类型 `Count`，然后给该类型添加了 `Increment()`、`Decrement()` 和 `IsZero()` 方法，其中前两者的接受者为 `Count` 类型的指针，后一个方法接收 `Count` 类型的值。

类型的方法集是指可以被该类型的值调用的所有方法的集合。

一个指向自定义类型的值的指针，它的方法集由该类型定义的所有方法组成，无论这些方法接受的是一个值还是一个指针。如果在指针上调用一个接受值的方法，Go 语言会聪明地将该指针解引用。

一个自定义类型值的方法集合则由该类型定义的接收者为值类型的方法组成，但是不包括那些接收者类型为指针的方法。

其实这些限制 Go 语言帮我们解决的非常好，结果就是我们可以在值类型上调用接收者为指针的方法。假如我们只有一个值，仍然可以调用一个接收者为指针类型的方法，这是因为 Go 语言会自动获取值的地址传递给该方法，前提是该值是可寻址的。

在以上定义的类型 `Count` 中，`*Count` 方法集是 `Increment()`, `Decrement()` 和 `IsZero()`，`Count` 的值的方法集是 `IsZero()`。但是因为 `Count` 类型的是可寻址的，所以我们可以使用 `Count` 的值调用全部的方法。

另外如果结构体的字段也有方法，我们也可以直接通过结构体访问字段中的方法。
```
package main

import "fmt"

type Count int // 创建自定义类型 Count

func (count *Count) Increment()  { *count++ } // Count类型的方法
func (count *Count) Decrement()  { *count-- }
func (count Count) IsZero() bool { return count == 0 }

type Part struct { // 基于结构体创建自定义类型 Part
    stat  string
    Count // 匿名字段
}

func (part Part) IsZero() bool { // 覆盖了匿名字段Count的IsZero()方法
    return part.Count.IsZero() && part.stat == "" // 调用了匿名字段的方法
}

func (part Part) String() string { // 定义String()方法，自定义了格式化指令%v的输出
    return fmt.Sprintf("<<%s, %d>>", part.stat, part.Count)
}

func main() {
    var i Count = -1
    fmt.Printf("Start \"Count\" test:\nOrigin value of count: %d\n", i)
    i.Increment()
    fmt.Printf("Value of count after increment: %d\n", i)
    fmt.Printf("Count is zero t/f? : %t\n\n", i.IsZero())
    fmt.Println("Start: \"Part\" test:")
    part := Part{"232", 0}
    fmt.Printf("Part: %v\n", part)
    fmt.Printf("Part is zero t/f? : %t\n", part.IsZero())
    fmt.Printf("Count in Part is zero t/f?: %t\n", part.Count.IsZero()) // 尽管覆盖了匿名字段的方法，单还是可以访问
}
```
执行代码，输出如下：
```
$ go run main.go
Start "Count" test:


Origin value of count: -1
Value of count after increment: 0
Count is zero t/f? : true

Start: "Part" test:
Part: <<232, 0>>
Part is zero t/f? : false
Count in Part is zero t/f?: true
```

## 组合
Go 语言虽然抛弃了继承，但是却提供了一个更加方便的组合特性。相对于继承的编译期确定实现，组合的运行态指定实现，更加灵活。下面通过一段代码来了解组合的基本属性以及它与继承的不同之处。

先定义一个结构体 `Base`, 并为它添加两个方法 `Foo()` 和 `Bar()`：
```
type Base struct{
    Name string
}

func (b *Base) Foo() {...}
func (b *Base) Bar() {...}

type Seed struct {
    Base
    ...
}

func (s *Seed) Foo() {
    s.Base.Foo()
    s.Bar()
    ...
}
```
上面代码先定义了一个 Base 类，然后定义了一个 Seed 类。Seed 类“继承”了 Base 类的所有成员属性和方法并重写了 `Foo()` 方法。同时在重写 `Foo()` 方法时调用了 Base 类的 `Foo()` 方法和 `Bar()` 方法。需要注意的是，若此时 Seed 的对象通过 `s.Foo()` 调用 `Foo()` 方法时，实际调用的是 Seed 重写过后的 `Foo()` 方法，而不是基类 Base 的 `Foo()` 方法，若想调用 Base 类的 `Foo()` 方法则要使用 `s.Base.Foo`，而调用没有重写的 `Bar()` 方法时，使用 `s.Bar()` 和 `s.Base.Bar()` 效果是一样的。

## 接口
在 Go 中，接口是一组方法签名。当一个类型为接口中的所有方法提供定义时，它被称为实现该接口。它与 oop 非常相似。接口指定类型应具有的方法，类型决定如何实现这些方法。

**接口基础**

之所以说 Go 语言的面向对象很灵活，很大一部分原因是由于接口的存在。接口是一个自定义类型，它声明了一个或者多个方法签名，任何实现了这些方法的类型都实现这个接口。`infterface{}` 类型是声明了空方法集的接口类型。任何一个值都满足 `interface{}` 类型，也就是说如果一个函数或者方法接收 `interface{}` 类型的参数，那么任意类型的参数都可以传递给该函数。接口是完全抽象的，不能实例化。接口能存储任何实现了该接口的类型。
```
package main

import "fmt"

type Human struct { // 结构体
    name  string
    age   int
    phone string
}

//Human实现SayHi方法
func (h Human) SayHi() {
    fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}

//Human实现Sing方法
func (h Human) Sing(lyrics string) {
    fmt.Println("La la la la...", lyrics)
}

type Student struct {
    Human  //匿名字段
    school string
    loan   float32
}

type Employee struct {
    Human   //匿名字段
    company string
    money   float32
}

// Employee重载Human的SayHi方法
func (e Employee) SayHi() {
    fmt.Printf("Hi, I am %s, I work at %s. Call me on %s\n", e.name,
        e.company, e.phone)
}

// Interface Men被Human,Student和Employee实现
// 因为这三个类型都实现了这两个方法
type Men interface {
    SayHi()
    Sing(lyrics string)
}

func main() {
    mike := Student{Human{"Mike", 25, "222-222-XXX"}, "MIT", 0.00}
    paul := Student{Human{"Paul", 26, "111-222-XXX"}, "Harvard", 100}
    sam := Employee{Human{"Sam", 36, "444-222-XXX"}, "Golang Inc.", 1000}
    Tom := Employee{Human{"Tom", 37, "222-444-XXX"}, "Things Ltd.", 5000}

    //定义Men类型的变量i
    var i Men

    //i能存储Student
    i = mike
    fmt.Println("This is Mike, a Student:")
    i.SayHi()
    i.Sing("November rain")

    //i也能存储Employee
    i = Tom
    fmt.Println("This is Tom, an Employee:")
    i.SayHi()
    i.Sing("Born to be wild")

    //定义了slice Men
    fmt.Println("Let's use a slice of Men and see what happens")
    x := make([]Men, 3)
    //这三个都是不同类型的元素，但是他们实现了interface同一个接口
    x[0], x[1], x[2] = paul, sam, mike

    for _, value := range x {
        value.SayHi()
    }
}
```
以上代码中，接口类型声明的变量能存储任何实现了该接口的类型的值。运行代码，输出如下：
```
$ go run main.go
This is Mike, a Student:
Hi, I am Mike you can call me on 222-222-XXX
La la la la... November rain
This is Tom, an Employee:
Hi, I am Tom, I work at Things Ltd.. Call me on 222-444-XXX
La la la la... Born to be wild
Let's use a slice of Men and see what happens
Hi, I am Paul you can call me on 111-222-XXX
Hi, I am Sam, I work at Golang Inc.. Call me on 444-222-XXX
Hi, I am Mike you can call me on 222-222-XXX
```

**接口变量值的类型**

接口类型声明的变量里能存储任何实现了该接口的类型的值。有的时候我们需要知道这个变量里的值的类型，那么需要怎么做呢？可以使用类型断言，或者是 switch 类型判断分支。
```
package main

import (
    "fmt"
    "strconv"
)

type Element interface{}
type List []Element

type Person struct {
    name string
    age  int
}

// 实现了fmt.Stringer接口
func (p Person) String() string {
    return "(name: " + p.name + " - age: " + strconv.Itoa(p.age) + " years)"
}

func main() {
    list := make(List, 3)
    list[0] = 1       //an int
    list[1] = "Hello" //a string
    list[2] = Person{"Dennis", 70}

    for index, element := range list {
        switch value := element.(type) { // switch类型判断开关
        case int:
            fmt.Printf("list[%d] is an int and its value is %d\n", index, value)
        case string:
            fmt.Printf("list[%d] is a string and its value is %s\n", index, value)
        case Person:
            fmt.Printf("list[%d] is a Person and its value is %s\n", index, value)
        default:
            fmt.Println("list[%d] is of a different type", index)
        }
    }
}
```
运行结果：
```
$ go run main.go
list[0] is an int and its value is 1
list[1] is a string and its value is Hello
list[2] is a Person and its value is (name: Dennis - age: 70 years)
```

**嵌入 interface**

在结构体中可以嵌入匿名字段，其实在接口里也可以再嵌入接口。如果一个 interface1 作为 interface2 的一个嵌入字段，那么 interface2 隐式的包含了 interface1 里的方法。如下例子中，Interface2 包含了 Interface1 的所有方法。
```
type Interface1 interface {
    Send()
    Receive()
}

type Interface2 interface {
    Interface1
    Close()
}
```

# Go 并发编程
## 并发与并行
**并发**指在同一时刻只能有一条指令执行，但多个进程指令被快速的轮换执行，使得在宏观上具有多个进程同时执行的效果，但在微观上并不是同时执行的，只是把时间分成若干段，通过 CPU 时间片轮转使多个进程快速交替的执行。

而并行的关键是你有同时处理多个任务的能力。

并发和并行都可以是很多个线程，就看这些线程能不能同时被（多个）CPU 执行，如果可以就说明是并行，而并发是多个线程被（一个）CPU 轮流切换着执行。

## 协程
协程也叫轻量级线程。与传统的进程和线程相比，协程最大的优点就在于其足够“轻”，操作系统可以轻松创建上百万个协程而不会导致系统资源枯竭，而线程和进程通常最多不过近万个。而多数语言在语法层面上是不支持协程的，一般都是通过库的方式进行支持，但库的支持方式和功能不够完善，经常会引发阻塞等一系列问题，而 Go 语言在语法层面上支持协程，也叫 `goroutine`。这让协程变得非常简单，让轻量级线程的切换管理不再依赖于系统的进程和线程，也不依赖 CPU 的数量。

## goroutine
`goroutine` 是 Go 语言并行设计的核心。`goroutine` 是一种比线程更轻量的实现，十几个 `goroutine` 可能在底层就是几个线程。 不同的是，Golang 在 runtime、系统调用等多方面对 `goroutine` 调度进行了封装和处理，当遇到长时间执行或者进行系统调用时，会主动把当前 `goroutine` 的 CPU (P) 转让出去，让其他 `goroutine` 能被调度并执行，也就是 Golang 从语言层面支持了协程。要使用 `goroutine` 只需要简单的在需要执行的函数前添加 `go` 关键字即可。当执行 `goroutine` 时候，Go 语言立即返回，接着执行剩余的代码，`goroutine` 不阻塞主线程。下面我们通过一小段代码来讲解 go 的使用：
```
//首先我们先实现一个 Add()函数
func Add(a, b int) {
    c := a + b
    fmt.Println(c)
}

go Add(1, 2) //使用go关键字让函数并发执行
```
Go 的并发执行就是这么简单，当在一个函数前加上 `go` 关键字，该函数就会在一个新的 goroutine 中并发执行，当该函数执行完毕时，这个新的 goroutine 也就结束了。不过需要注意的是，如果该函数具有返回值，那么返回值会被丢弃。所以什么时候用 `go` 还需要酌情考虑。

接着我们通过一个案例来体验一下 Go 的并发到底是怎么样的。
```
package main

import "fmt"

func Add(a, b int) {
    c := a + b
    fmt.Println(c)
}

func main() {
    for i := 0; i < 10; i++ {
        go Add(i, i)
    }
}
```
执行以上代码会发现屏幕上什么都没有，但程序并不会报错，这是什么原因呢？原来当主程序执行到 for 循环时启动了 10 个 `goroutine`，然后主程序就退出了，而启动的 10 个 `goroutine` 还没来得及执行 `Add()` 函数，所以程序不会有任何输出。也就是说主 `goroutine` 并不会等待其他 `goroutine` 执行结束。那么如何解决这个问题呢？Go 语言提供的通道（`channel`）就是专门解决并发通信问题的。

## channel
`channel` 是 `goroutine` 之间互相通讯的东西。类似我们 Unix 上的管道（可以在进程间传递消息），用来 `goroutine` 之间发消息和接收消息。其实，就是在做 `goroutine` 之间的内存共享。`channel` 是类型相关的，也就是说一个 `channel` 只能传递一种类型的值，这个类型需要在 `channel` 声明时指定。

**声明与初始化**

`channel` 的一般声明形式：`var chanName chan ElementType`。

与普通变量的声明不同的是在类型前面加了 `chan` 关键字，`ElementType` 则指定了这个 `channel` 所能传递的元素类型。示例：
```
var a chan int //声明一个传递元素类型为int的channel
var b chan float64
var c chan string
```
初始化一个 `channel` 也非常简单，直接使用 Go 语言内置的 `make()` 函数，示例：
```
a := make(chan int) //初始化一个int型的名为a的channel
b := make(chan float64)
c := make(chan string)
```
`channel` 最频繁的操作就是写入和读取，这两个操作也非常简单，示例：
```
a := make(chan int)
a <- 1  //将数据写入channel
z := <-a  //从channel中读取数据
```

**channel 的关闭**

channel 的关闭非常简单，使用 Go 语言内置的 close() 函数即可关闭 channel，示例：
```
ch := make(chan int)
close(ch)
```
关闭了 channel 后如何查看 channel 是否关闭成功了呢？很简单，我们可以在读取 channel 时采用多重返回值的方式，示例：
```
x, ok := <-ch
```
通过查看第二个返回值的 bool 值即可判断 channel 是否关闭，若为 false 则表示 channel 被关闭，反之则没有关闭。

## select
`select` 用于处理异步 IO 问题，它的语法与 `switch` 非常类似。由 `select` 开始一个新的选择块，每个选择条件由 `case` 语句来描述，并且每个 `case` 语句里必须是一个 `channel` 操作。它既可以用于 `channel` 的数据接收，也可以用于 `channel` 的数据发送。如果 `select` 的多个分支都满足条件，则会随机的选取其中一个满足条件的分支。
```
package main
import "time"
import "fmt"
func main() {
    c1 := make(chan string)
    c2 := make(chan string)
    go func() {
        time.Sleep(time.Second * 1)
        c1 <- "one"
    }()
    go func() {
        time.Sleep(time.Second * 2)
        c2 <- "two"
    }()
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-c1:
            fmt.Println("received", msg1)
        case msg2 := <-c2:
            fmt.Println("received", msg2)
        }
    }
}
```
以上代码先初始化两个 `channel` c1 和 c2，然后开启两个 `goroutine` 分别往 c1 和 c2 写入数据，再通过 `select` 监听两个 `channel`，从中读取数据并输出。

运行结果如下：
```
$ go run main.go
received one
received two
```

## 超时机制
`channel` 的读写操作非常简单，只需要通过 `<-` 操作符即可实现，但是 `channel` 的使用不当却会带来大麻烦。我们先来看之前的一段代码：
```
a := make(chan int)
a <- 1
z := <-a
```
观察上面三行代码，第 2 行往 `channel` 内写入了数据，第 3 行从 `channel` 中读取了数据，如果程序运行正常当然不会出什么问题，可如果第二行数据写入失败，或者 `channel` 中没有数据，那么第 3 行代码会因为永远无法从 `a` 中读取到数据而一直处于阻塞状态。相反的，如果 `channel` 中的数据一直没有被读取，那么写入操作也会一直处于阻塞状态。如果不正确处理这个情况，很可能会导致整个 `goroutine` 锁死，这就是超时问题。Go 语言没有针对超时提供专门的处理机制，但是我们却可以利用 `select` 来巧妙地实现超时处理机制，下面看一个示例：
```
t := make(chan bool)
go func {
    time.Sleep(1e9) //等待1秒
    t <- true
}

select {
    case <-ch:  //从ch中读取数据

    case <-t:  //如果1秒后没有从ch中读取到数据，那么从t中读取，并进行下一步操作
}
```
这样的方法就可以让程序在等待 1 秒后继续执行，而不会因为 ch 读取等待而导致程序停滞，从而巧妙地实现了超时处理机制，这种方法不仅简单，在实际项目开发中也是非常实用的。

