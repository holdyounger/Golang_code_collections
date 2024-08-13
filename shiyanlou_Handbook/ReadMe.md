---
title: Go基础
source: 实验楼
---

> 

# Go语言顺序编程

## 流程控制

Go 语言提供的流程控制语句包括 `if`、`switch`、`for`、`goto`、`select`，其中 `select` 用于监听 `channel`（通道）在讲解通道的时候再详细介绍。

### if 语句

语法：

```go
if optionalStatement1; booleanExpression1 {
    block1
} else if optionalStatement2; booleanExpression2 {
    block2
} else {
    block3
}
```

其中 `optionalStatement` 是可选的表达式，真正决定分支走向的是 `booleanExpression1` 的值。

### for 语句

Go 语言的  `for` 语句可以遍历数组，切片，映射等类型，也可以用于无限循环。以下是其语法：

```go
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

### 跳转语句

Go 语言中使用 `goto` 关键字实现跳转。`goto` 语句的语义非常简单，就是跳转到本函数内的某个标签，例如：

```go
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

### switch语句

Go 语言中 `switch` 分支既可用于常用的分支就像 C 语言中的 `switch` 一样，也可以用于类型开关，所谓类型开关就是用于判断变量属于什么类型。但是需要注意的是 Go 语言的 `switch` 语句不会自动贯穿，相反，如果想要贯穿需要添加 `fallthrough` 语句。表达式开关 `switch` 的语法如下：

```go
switch optionalStatement; optionalExpression {
    case expression1: block1
    ...
    case expressionN: blockN
    default: blockD
}
```

下面是个例子：

```go
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

```go
switch optionalStatement; typeSwitchGuard {
    case type1: block1
    ...
    case typeN: blockN
    default: blockD
}
```

说了这么多，让我们进行下练习，创建源文件 `switch_t.go`，输入以下代码：

😎😎<b>这个demo很重❗❗❗❗❗❗❗</b>>😎😎

```go
 
```

以上代码中我们首先创建了一个接收任意数量任意类型参数的函数，然后使用 `for ... range aSlice` 的语法迭代了每一个在切片 `items` 中的元素，接着使用了 `switch` 类型开关判断了每一个参数的类型，并打印了其值和类型。程序运行输出如下：

```go
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

### 函数的定义

在 Go 语言中，函数的基本组成包括：关键字 `func`、函数名、参数列表、返回值、函数体和返回语句，这里我们用一个简单的加法函数来对函数的定义进行说明。

```go
package add

func Add(a int, b int) (num int){
    return a + b
}
```

### 函数的调用

函数调用非常简单，先将被调用函数所在的包导入，就可以直接使用该函数了。注意需要把包文件夹放到 `$GOPATH` 目录中，实例如下：

```go
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

---

与 C/C++ 和 JAVA 不同，Go 语言的函数和方法可以有多个返回值，这是 Go 提供的一个优美的特性，示例如下：

```go
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

### 匿名函数

在 Go 语言中，你可以在代码里随时定义匿名函数，匿名函数由一个不带函数名的函数声明和函数体组成，示例如下：

```go
func (a, b, c int) bool {
    return a * b < c
}
```

你可以将匿名函数直接赋值给一个变量，也可以直接调用运行，示例如下：

```go
x := func (a, b, c int) bool {
    return a * b < c
}

func (a, b, c int) bool {
    return a * b < c
} (1, 2, 3) //小括号内直接给参数列表表示函数调用
```

## 类型转换

### 类型转换

Go 语言提供了一种在不同但相互兼容的类型之间相互转换的方式，这种转换非常有用并且是安全的。但是需要注意的是在数值之间进行转换可能造成其他问题，如精度丢失或者错误的结果。以下是类型转换的语法：

- `resultOfType := Type(expression)`

几个例子：

```go
x := int16(2345)        // 声明一个类型为int16的整数，其值为2345
y := int32(x)           // 将int16类型的整数转换为int32类型
a := uint16(65000)       // 声明一个类型为uint16类型的整数
b := int16(a)           // 转换为int16类型，虽然能转换成功，但是由于65000超过in16类型的范围，会导致结果错误，b的值为 -536
```

另外在 Go 语言中可以通过 `type` 关键字声明类型，如 `type StringsSlice []string` 将 `[]string`（`string` 类型的切片）声明为 `StringSlice` 类型。

### 类型断言

说到类型断言就需要先了解下 Go 语言中的接口。在 Go 语言中接口是一个自定义类型。它声明了一个或者多个方法。任何实现了这些方法的对象（类型）都满足这个接口。

接口是完全抽象的，不能实例化。`interface{}` 类型表示一个空接口，任何类型都满足空接口。也就是说 `interface{}` 类型的值可以用于表示任意 Go 语言类型的值。

这里的空接口有点类似于 Python 语言中的 object 实例。既然 `interface{}` 可以用于表示任意类型，那有的时候我们需要将 `interface{}` 类型转换为我们需要的类型，这个操作称为类型断言。

**一般情况下只有我们希望表达式是某种特定类型的值时才使用类型断言**。Go 语言中可以使用以下语法：

- `resultOfType, boolean := expression.(Type)`：安全的类型断言。
- `resultOfType := expression.(Type)`：非安全的类型断言，失败时程序会产生异常。

创建源文件 `type_t.go`，输入以下源文件：

```go
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

```bash
$ go run type_t.go
type and value of x is: uint16 and 65000
type and value of y is: int16 and -536
type and value of j is: int and 99
[]string -> ["left" "right"]
```

### 类型断言

说到类型断言就需要先了解下 Go 语言中的接口。在 Go 语言中接口是一个自定义类型。它声明了一个或者多个方法。任何实现了这些方法的对象（类型）都满足这个接口。

接口是完全抽象的，不能实例化。`interface{}` 类型表示一个空接口，任何类型都满足空接口。也就是说 `interface{}` 类型的值可以用于表示任意 Go 语言类型的值。

这里的空接口有点类似于 Python 语言中的 object 实例。既然 `interface{}` 可以用于表示任意类型，那有的时候我们需要将 `interface{}` 类型转换为我们需要的类型，这个操作称为类型断言。

**一般情况下只有我们希望表达式是某种特定类型的值时才使用类型断言**。Go 语言中可以使用以下语法：

- `resultOfType, boolean := expression.(Type)`：安全的类型断言。
- `resultOfType := expression.(Type)`：非安全的类型断言，失败时程序会产生异常。

创建源文件 `type_t.go`，输入以下源文件：

```go
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

```bash
$ go run type_t.go
type and value of x is: uint16 and 65000
type and value of y is: int16 and -536
type and value of j is: int and 99
[]string -> ["left" "right"]
```

## 错误处理

错误处理是任何语言都需要考虑到的问题，而 Go 语言在错误处理上解决得更为完善，优雅的错误处理机制是 Go 语言的一大特点。

### error

Go 语言引入了一个错误处理的标准模式，即 `error` 接口，该接口定义如下：

```go
type error interface {
    Error() string
}
```

对于大多数函数，如果要返回错误，可以将 `error` 作为多返回值的最后一个：

```go
func foo(param int)(ret int, err error)
{
  ...
}
```

调用时的代码：

```go
n, err := foo(0)
if err != nil {
    //  错误处理
} else {
    // 使用返回值n
}
```

我们还可以自定义错误类型，创建源文件 `error.go`，输入以下代码：

```go
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

### defer

你可以在 Go 函数中添加多个 `defer` 语句，当函数执行到最后时，这些 defer 语句会按照逆序执行（即最后一个 `defer` 语句将最先执行），最后该函数返回。特别是当你在进行一些打开资源的操作时，遇到错误需要提前返回，在返回前你需要关闭相应的资源，不然很容易造成资源泄露等问题。如下代码所示，我们一般写打开一个资源是这样操作的：

```go
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

```go
defer func(){
    ...
}()
```

注意，`defer` 语句是在 `return` 之后执行的，新建源文件 `defer.go` 输入以下代码：

```go
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

### panic

`panic()` 函数用于抛出异常，`recover()` 函数用于捕获异常，这两个函数的原型如下：

```go
func panic(interface{})
func recover() interface{}
```

当在一个函数中调用 `panic()` 时，正常的函数执行流程将立即终止，但函数中之前使用 `defer` 关键字延迟执行的语句将正常展开执行，之后该函数将返回到调用函数，并导致逐层向上执行 `panic()` 流程，直至所属的 `goroutine` 中所有正在执行的函数被终止。错误信息将被报告，包括在调用 `panic()` 函数时传入的参数，这个过程称为错误流程处理。

`panic()` 接受一个 `interface{}` 参数，可支持任意类型，例如：

```go
panic(404)
panic("network broken")
panic(Error("file not exists"))
```

在 `defer` 语句中，可以使用 `recover()` 终止错误处理流程，这样可以避免异常向上传递，但要注意 `recover()` 之后，程序不会再回到 `panic()` 那里，函数仍在 `defer` 之后返回。新建一个源文件 `error1.go`，输入以下代码：

```go
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

## 面向对象编程

### 自定义类型

Go 语言的中结构体 `struct` 与 C++、JAVA 中的类 `class` 相似，但 Go 放弃了传统面向对象的诸多特性，只保留了组合。

- **type** typeName typeSpecification

其中，`typeName` 可以是一个包或者函数内唯一合法的 Go 标示符。`typeSpecification` 可以是任何内置的类型，一个接口或者是一个结构体。所谓结构体，它的字段是由其他类型或者接口组成。例如我们通过结构体定义了一下类型：

```go
type ColorPoint struct {
    color.Color     // 匿名字段(嵌入)
    x, y int        // 具名字段(聚合)
}
```

以上代码我们通过结构体自定义了类型 `ColorPoint`，结构体中 `color.Color` 字段是 Color 包的类型 color，这个字段没有名字，所以被称为匿名的，也是嵌入字段。字段 `x` 和 `y` 是有变量名的，所以被称为具名字段。假如我们创建了类型 `ColorPoint` 的一个值 `point`（通过语法：`point := ColorPoint{}` 创建），那么这些字段可以通过 `point.Color`、`point.x`、`point.y` 访问。其他面向对象语言中的"类 (`class`)"、"对象 (`object`)"、"实例 (`instance`)"在 Go 语言中我们完全避开使用。相反的我们使用"类型 (`type`)"和其对应的"值"，其中自定义类型的值可以包含方法。

定义了结构体后如何创建并初始化一个对象实例呢？Go 语言支持以下几种方法进行实现：

```go
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

```go
func NewMan(name string, age int) *Man {
    return &Man{name, age}
}
```

### 方法

方法是作用在自定义类型上的一类特殊函数，通常自定义类型的值会被传递给该函数，该值可能是以指针或者复制值的形式传递。定义方法和定义函数几乎相同，只是需要在 `func` 关键字和方法名之间必须写上接接受者。例如我们给类型 `Count` 定义了以下方法：

```go
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

另外如果结构体的字段也有方法，我们也可以直接通过结构体访问字段中的方法。下面让我们练习下，创建源文件 `struct_t.go`，输入以下代码：

```go
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

以上代码中，我们创建了 `Count` 类型，然后在其基础上又创建了结构体类型 `Part`。我们为 `Count` 类型定义了 3 个方法，并在 `Part` 类型中创建了方法 `IsZero()` 覆盖了其匿名字段 `Count` 中 `IsZero()` 方法。但是我们还是可以二次访问到匿名字段中被覆盖的方法。执行代码，输出如下：

```bash
$ go run struct_t.go
Start "Count" test:


Origin value of count: -1
Value of count after increment: 0
Count is zero t/f? : true

Start: "Part" test:
Part: <<232, 0>>
Part is zero t/f? : false
Count in Part is zero t/f?: true
```



