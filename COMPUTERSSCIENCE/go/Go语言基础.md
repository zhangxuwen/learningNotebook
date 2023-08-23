# Go语言基础







## 入门



### `go run filename.go`

可以将一个或者多个以.go为后缀的源文件进行编译、链接，然后运行生成的可执行文件。



### `go build filename.go`

一般用于不是一次性的实验，编译输出成一个可复用的程序。



### `:=`

用于```短变量声明```，这个宏语句声明一个或多个变量，并且更具初始化的值给予合适的类型



### `i++`

等价于`i += 1`，又等价于`i = i + 1`，对应的递减语句`i--`同理。这些是语句，不同于`C`族语句一样是表达式，`j = i++`是不合法的，并且只支持后缀。



### `for`

`Go`里面唯一的循环语句



`_`

空标识符，可以用在任何语法需要变量名但是程序逻辑不需要的地方



## 程序结构

### 名称

* 名称的开头是一个字母或下划线、后面可以跟任意数量的字符、数字和下划线并区分大小写。
* 风格上采用“驼峰式”。

#### 25个关键字

```go
break：退出循环

default：选择结构默认项（switch、select）

func：定义函数

interface：定义接口

select：channel

case：选择结构标签

chan：定义 channel

const：常量

continue：跳过本次循环

defer：延迟执行内容（收尾工作）

go：并发执行

map：map 类型

struct：定义结构体

else：选择结构

goto：跳转语句

package：包

switch：选择结构

fallthrough：流程控制

if：选择结构

range：从 slice、map 等结构中取元素

type：定义类型

for：循环

import：导入包

return：返回

var：定义变量
```

#### 预声明常量、类型和函数

```go
常量：true    false    iota    nil

类型：int       int8      int16     int32     int64

           uint     uint8    uint16   uint32   uint64    uintptr

           float32   float64    complex128      complex64

           bool     byte     rune      string    error

函数： make    len    cap    new    append    copy    close    delete   

            complex     real    imag

            panic      recover
```



### 声明

4个主要的声明：变量(`var`)，常量(`const`)，类型(`type`)和函数(`func`)。



### 变量

`var`声明创建一个具体类型的变量。每一个声明都有一个通用的形式：

 <center> var name type = expression </center>

类型和表达式部分可以省略一个，但是不能都省略，Go里面不存在未初始化变量。

可以声明一个变量列表，忽略类型允许声明多个不同类型的变量，如：

```go
var i, j, k int		//int, int, int
var b, f, s = true, 2.3, "four"		//bool, float64, string
```

#### 短变量声明

一种称作`短变量声明`的可选形式可以用来声明和初始化局部变量。它使用`name := expression`的形式，`name`的类型由`expression`的类型决定。

* 短变量声明不需要声明所有在左边的变量

在如下代码中，第一条语句声明了`in`和`err`。第二条语句仅声明了`out`，但向已有的`err`变量赋了值。

```go
in, err := os.Open(infile)
// ...
out, err := os.Create(outfile)
```

* 短变量声明最少声明一个变量，否则，代码编译将无法通过。

```go
f, err := os.Open(infile)
// ...
f, err := os.Create(outfile) // 编译错误：没有新的变量
```

#### 指针

指针的值是一个变量的地址。使用指针，可以在无须知道变量名字的情况下，间接读取或更新变量的值。

```go
x := 1
p := &x				// p 是整型指针，指向 x 
fmt.Println(*p)		// "1"
*p = 2				// 等于 x = 2
fmt.Println(x)		// 结果 "2"
```

* 指针是可比较的，两个指针当且仅当指向同一个变量或者两者都是`nil`的情况才相等。

```go
var x, y int
fmt.Println(&x == &x, &x == &y, &x == nil) // "true false false"
```

* 函数返回局部变量的地址是非常安全的。

例如，通过下面代码，调用函数f产生局部变量v即使在调用返回后依然存在，指针p依然引用它：

```go
var p = f()
func f() *int {
    v := 1
    return &v
}
```

每次调用f都会返回一个不同的值：

```go
fmt.Println(f() == f()) // "false"
```

#### new函数

使用内置的`new`函数也是创建变量的一种方式。表达式`new(T)`创建一个未命名的`T`类型变量，初始化为`T`类型的零值，并返回其地址（地址类型为`*T`）。

```go
func newInt() *int {
    return new(int)
}
p := new(int)
q := new(int)
fmt.Println(p == q) // "false"
```

* `new`是一个预声明的函数，不是一个关键字，所以他可以重定义为另外的其他类型。

####  变量的生命周期

生命周期指在程序执行过程中变量存在的时间段。变量的生命周期是通过它是否可达来确定。

* 局部变量可在包含它的循环的一次迭代中之外继续存活，即使包含它的循环已经返回，它的存在还可能延续。

**C++如果使用new操作申请的内存是分配在堆上的要自己利用delete进行回收，如果是声明的局部变量会在栈上分配内存，并且在函数退出后由系统自动回收。但是Golang在这方面与传统语言发生了非常大的区别，go语言编译器会做逃逸分析（escape analysis），分析局部变量的作用域是否逃出函数的作用域，要是没有，那么就放在栈上；要是变量的作用域超出了函数的作用域，那么就自动放在堆上。所以不用担心会不会memory leak，因为go语言有强大的垃圾回收机制。这样可以释放程序员的内存使用限制，让程序员关注程序逻辑本身。go语言也允许利用new来分配内存，但是new分配的内存也不是一定就放在堆上，而是根据其是否超出了函数作用域来判断是否放在堆上还是栈上。这点和C/C++很不一样。**

### 赋值

赋值语句用来更新变量所指的值，它最简单的形式由赋值符`=`，以及符号左边的变量和右边的表达式组成。

#### 多重赋值

在实际更新变量前，右边所有表达式被推演，当变量同时出现在赋值符两侧的时候这种形式特别有用。

* 交换两个变量

```go
x, y = y, x
a[i], a[j] = a[j], a[i]
```

* 使一个普通的赋值序列变得紧凑

```go
i, j, k = 2, 3, 5
```

#### 可赋值性

可赋值性根据类型不同有着不同的规则，类型必须精确匹配，`nil`可以被赋给任何接口变量或引用类型。

### 类型声明

`type`声明定义一个新的命名类型，它和某个已有类型使用相同的`底层类型`。

* 命名类型提供了一种方式来区分底层类型的不同或者不兼容使用，这样它们就不会在无意中混用。







## 基本数据

计算机底层全是位，而实际操作则是基于大小固定的单元中的数值，称为字（word）。

* Golang的二元操作符按优先级的降序排列如下。

```go
*	/	%	<<	>>	&	&^(AND NOT)
+	-	|	^	
==	!=	<	>	>=	
&&	
||
```

 

### 整数

有符号整数分四种大小：8位、16位、32位、64位，用`int8`，`int16`，`int32`，`int64`表示，对应的无符号整数是`uint8`，`uint16`，`uint32`，`uint64`。此外还有`int`和`uint`。

* `rune`类型是`int32`类型的同义词，常常用于指明一个值是`Unicode`码点。同样，`byte`类型是`uint8`类型的同义词，强调一个值是原始数据，而非量值。
* 无符号整数`uintptr`其大小并不明确，但足以完整存放指针。

* 若表示算术运算结果所需的位超出该类型的范围，就称为`溢出`。
* 比较表达式本身类型是布尔型。

* 如果将整数以位模式处理，须使用无符号整型。

* 无符号整数往往只用于位运算和特定算术运算符，如实现位集时，解析二进制格式的文件，或散列和加密。一般而言，无符号整数极少用于表示非负值。

##### fmt小技巧

* 通常```Printf```的格式化字符串含有多个`%`谓词，这要求提供相同数目的操作数，而`%`后的副词`[1]`告知```Printf```重复使用第一个操作数。
* `%o`，`%x`，或`%X`之前的副词`#`告知```Printf```输出相应的前缀`0`、`0x`、`0X`。
* ```Printf```用谓词`%b`以二进制形式输出数值，副词`08`能在这个输出结果前补0，补够8位。
* 用`%c`输出文字符号，如果希望输出带有单引号则用`%q`。
* `%*s`中的`*`号输出带有可变数量空格的字符串。



### 浮点数

Go具有两种大小的浮点数`float32`和`float64`。常量`math.MaxFloat32`是`float32`的最大值，大约为`3.4e38`，而`math.MaxFloat64`则大约为`1.8e308`。相应地，最小的正浮点值大约位`1.4e-45`和`4.9e-324`。

##### fmt小技巧

* `%g`会自动保持足够的精度，并选择最简洁的表达方式，但是对于数据表，`%e`（有指数）或`%f`（无指数）的形式可能更合适。



### 复数

Go具备两种大小的复数`complex64`和`complex128`，二者分别由`float32`和`float64`构成。内置的`complex`函数根据给定的实部和虚部创建复数，而内置的`real`函数和`imag`函数则分别提取

* 源码中，如果在浮点数或十进制整数后面紧接着写字母`i`，如`3.1415926i`或`2i`，它就变成了一个虚数，表示一个实部为0的复数。



### 布尔值

`bool`型的值或布尔值（`boolean`）只有两种可能：真（`true`）和假（`false`）。



### 字符串

字符串是不可变的字节序列，它可以包含任意数据，包括0值字节，但主要是人类可读的文本。

* 不可变一位置两个字符串能安全地共用同一段底层内存，使得复制任何长度字符串的开销都低廉。

#### 字符串字面量

`Go`的源文件总是按`UTF-8`编码，并且习惯上`Go`的字符串会按`UTF-8`解读。

* 原生的字符串字面量的书写形式是\`...\`，使用反引号而不是双引号。

#### Unicode

文字符号的序列表示成`int32`值序列，这种表示方式称作`UTF-32`或`UCS-4`，每个`Unicode`码点的编码长度相同，都是`32`位。

#### UTF-8

```import "unicode/utf8"```

`UTF-8`以字节为单位对`Unicode`码点做变长编码。

* 若最高位为`0`，则标示着它是`7`为的`ASCII`码，其文字符号的编码仅占`1`字节；若最高几位是`110`，则文字符号的编码占用`2`个字节，第二个字节以`10`开始。更长的编码以此类推。

#### 字符串和字节slice

4个标准包对字符串操作特别重要：`bytes`，`strings`，`strconv`和`unicode`。

* 字符串包含一个字节数组，创建后它就无法改变。相反地，字节`slice`的元素允许随意修改。

`strings`包具备下面6个函数：

```go
func Contains(s, substr string) bool
func Count(s, sep string) int
func Fields(s string) []string
func HasPrefix(s, prefix string) bool
func Index(s, sep string) int
func Join(a []string, sep string) string
```

`bytes`包里面的对应函数为：

```go
func Contains(b, subslice []byte) bool
func Count(s, sep []byte) int
func Fields(s []byte) [][]byte
func HasPrefix(s, prefix []byte) bool
func Index(s, sep []byte) int
func Join(s [][]byte, sep []byte) []byte
```

`bytes`包为高效处理字节`slice`提供了`Buffer`类型。`bytes.Buffer`类型无须初始化，原因是零值本来就有效。

* 若要在`bytes.Buffer`变量后面添加任意文字符号的`UTF-8`编码，最好使用`bytes.Buffer`的`WriteRune`方法，而追加`ASCII`字符，则使用`WriteByte`亦可。

#### 字符串和数字的相互转换

* 要将整数转换成字符串，一种选择是使用`fmt.Sprintf`，另一种做法是用函数`strconv.Itoa("integer to ASCII")`。
* `strconv`包内的`Atoi`函数或`ParseInt`函数用于解释表示整数的字符串，而`ParseUint`用于无符号整数。

```go
x, err := strconv.Atoi("123")				// x 是整型
x, err := strconv.ParseInt("123", 10, 64)	// 十进制，最长为64位
```

````ParseInt`的第三个参数指定结果必须匹配何种大小的整型```



### 常量

常量是一种表达式，其可以保证在编译阶段就计算出表达式的值，并不需要等到运行时，从而使编译器得以知晓其值。

* 所有常量本质上都属于基本类型：布尔型、字符串或数字。

* 常量声明可以同时指定类型和值，如果没有显示指定类型，则类型根据右边的表达式推断。
* 若同时声明一组常量，除了第一项之外，其他项在等号右侧的表达式都可以省略，这一位置会复用前面一项的表达式及其类型。

#### 常量生成器`iota`

常量声明中，`iota`从0开始取值，逐项加1。

```go
type Weekday int
const (
	Sunday Weekday = iota
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
)
```

#### 无类型常量

从属类型待定的常量共有6种，分别是`无类型布尔`，`无类型浮点数`，`无类型复数`，`无类型字符串`。

* 类似地，`true`和`false`是无类型布尔值，而字符串字面量则是无类型字符串。
* 只有常量才可以是无类型的。



## 复合数据类型

复合数据类型是由基本数据类型以各种方式组合而构成的。

### 数组

数组是具有固定长度且拥有零个或者多个相同数据类型元素的序列。

* 相比数组，`slice`很多场合下使用得更多。

* 默认情况下，一个新数组中的元素初始值位元素类型的零值，同时可以用`数组字面量`初始化。

```go
var r [3]int = [3]int{1, 2} // 1, 2, 0
```

* 如果省略号`...`出现在数组长度的位置，那么数组的长度由初始化数组的元素个数决定。

```go
q := [...]int{1, 2, 3} // [3]int
```

* 数组的长度是数组类型的一部分。
* 数组的长度必须是常量表达式，也就是说，这个表达式的值在程序编译时就可以确定。

* 如果一个数组的元素类型是可比较的，那么这个数组也是可比较的。
* Go把数组和其他的类型都看成值传递，而别的语言中数组是隐式地使用引用传递。

* 使用数组指针是高效的，但由于数组长度不可变的特性，很少使用数组。

### slice

`slice`通常写作`[]T`，其中元素的类型都是`T`，是一种轻量级的数据结构，可以用来访问数组的部分或者全部元素，这个数组称为`slice`的底层数组。

* `slice`有三个属性：指针，长度和容量。

* $$
  slice操作符s[i:j](其中0\leq i \leq j \leq cap(s))
  $$

这个新的slice引用了序列s中从i到j-1索引位置的所有元素，这里的s既可以是数组或者指向数组的指针，也可以是slice。

* 如果`x`是字符串，那么`x[m:n]`返回的是一个字符串；如果`x`是字节`slice`，那么返回的结果是字节`slice`。
* `slice`包含了指向数组元素的指针，所以将一个`slice`传递给函数的时候，可以在函数内部修改底层数组的元素。

* `slice`字面量看上去和数组字面量很像，都是用逗号分隔并用花括号括起来的一个元素序列，但`slice`没有指定长度。

```go
s := []int{0, 1, 2, 3, 4, 5}
```

* 和数组不同，`slice`不能用`=`来测试两个`slice`是否拥有相同的元素。`bytes.Equal()`可以用来比较两个字节`slice`（`[]byte`）。
* `slice`唯一允许的比较操作是和`nil`作比较。值为`nil`的`slice`没有对应的底层数组。

```go
var s []int		// len(s) == 0, s == nil
s = nil			// len(s) == 0, s == nil
s = []int(nil) 	// len(s) == 0, s == nil
s = []int{}		// len(s) == 0, s != nil
```

* 检查一个`slice`是否是空，使用`len(s) == 0`，而不是`s == nil`。

#### `append`函数

内置函数`append`用来将元素追加到`slice`的后面。

```go
var runes []rune
for _, r := range "Hello, 世界"{
    runes = append(runes, r)
}
```

* 调用`append`函数的情况下需要更新`slice`变量。对于任何函数，只要有可能改变`slice`的长度或者容量，亦或者使得`slice`指向不同的底层数组，都需要更新`slice`变量。
* `slice`底层数组的元素是间接引用的，但是`slice`的指针、长度和容量不是。
* 可以同时给`slice`添加多个元素，甚至添加另一个`slice`里的所有元素。

#### `slice`就地修改

`rotate`和`reverse`等可以就地修改`slice`元素的函数。

### map

一个拥有键值对元素的无序集合。在`golang`中，`map`是散列表的引用，`map`的类型是`map[K]V`，其中`k`和`v`是字典的键和值对应的数据类型。

* 内置函数`make`可以用来创建一个`map`：

```go
mapVal := make(map[string]int)
```

* 可以使用内置函数`delete`来从字典中根据键移除一个元素，即使键不在`map`中，操作也是安全的：

```go
delete(mapVal, "value") // 移除元素mapVal["value"]
```

* `map`元素不是一个变量，不可以获取它的地址。

* `map`顺序是随机的，如果需要按照某种顺序来遍历`map`中的元素，必须显示地给键排序。如果键是字符串类型，可以使用`sort`包中的`Strings`函数来进行键的排序。
* 通过一种下标方式访问`map`中的元素输出两个值，第二个值是一个布尔值，用来报告该元素是否存在。

```go
if mapValue, ok := mapValues["val"]; !ok { /* ... */ }
```

* `map`值类型本身可以是复合数据类型。

### 结构体

将零个或者多个任意类型的命名变量组合在一起的聚合数据类型。

(个人思考)：这里的指针跟`c/c++`有所不同，这里的指针可以用`.`直接访问结构体，而`c/c++`需要`->`来访问或者需要`(*ptr)`之后用`.`访问。

* 如果一个结构体的成员变量名称是首字母大写的，那么这个变量是可导出的，这个是`Go`最主要的访问控制机制。
* 命名结构体类型不可以定义一个拥有相同结构体类型的成员变量，但可以定义一个指针类型。

```go
type tree struct {
    value		int
    left, right	*tree
}
```

#### 结构体字面量

结构体类型的值可以通过结构体字面量来设置。

* 有两种格式的结构体字面量：

1. 按照正确的顺序为每个成员量指定一个值。但这样会给开发和阅读代码的人增加负担。
2. 通过指定部分或者全部成员变量的名称和值来初始化结构体变量。如果在这种初始化方式下没有指定，那么该成员变量类型就是默认的该类型零值。

两种初始化方式不可以混合使用。两种方式都绕不过不可导出变量无法在其他包中适用的规则。

* 结构体类型的值可以作为参数传递给函数或者作为函数的返回值。出于效率的考虑，大型的结构体通常都使用结构体指针的方式直接传递给函数或者从函数中返回。在需要修改结构体内容的时候也是必需的，在`Go`按值调用的语言中，调用的函数接收到的是实参的一个副本，并不是实参的引用。

#### 结构体比较

如果结构体的所有成员变量都可以比较，那么这个结构体就是可比较的。

* 可比较的结构体类型都可以作为`map`的键类型。

#### 结构体嵌套和匿名成员

* 可以定义不带名称的结构体成员，只需指定类型即可，这种结构体成员成为匿名成员。这个结构体成员的类型必须是一个命名类型或者指向命名类型的指针。这样能直接访问到需要的变量而不是指定一大串中间变量。但结构体字面量并没有什么快捷方式来初始化结构体。

```(个人理解)匿名成员是不可导出的，但匿名成员它自己的可导出成员仍然是可见的```

### JSON

`golang`通过标准库`encoding/json`，`encoding/xml`，`encoding/asn1`以及其他库对这些格式的编码和解码提供了非常好的支持，这些库都拥有相同的`API`。`JSON`是`JavaScript`值的`Unicode`编码，这些值包括字符串、数字、布尔值、数组和对象。

* `JSON`最基本的类型是数字（以十进制或者科学计数法表示）、布尔值（`true`或`false`）和字符串（用双引号括起来的）。
* `JSON`的数组是一个有序的元素序列，每个元素之间用逗号分隔，两边使用方括号括起来。
* `JSON`的对象是一个从字符串到值的映射，写成`name:value`对的序列，每个元素之间用逗号分隔，两边使用花括号括起来。
* 把Go的数据结构转换为`JSON`成为`marshal`。`marshal`通过`json/Marshal`实现。

```go
data, err := json.Marshal(valueType)
if err != nil {
    log.Fatalf("JSON marshaling failed: %s", err)
}
```

* `json.MarshalIndent`的变体可以输出整齐格式化过的结果。

```go
data, err := json.MarshalIndent(valueType, "", "    ") // 一个定义每行输出的前缀字符串，另外一个定义缩进的字符串。
```

* `marshal`使用`Go`结构体成员的名称作为`JSON`对象里面字段的名称。只有可导出的成员可以转换为`JSON`字段。

* `marshal`的逆操作将`JSON`字符串解码为`Go`数据结构，这个过程叫做`unmarshal`，这个是由`json.Unmarshal`实现的。通过合理地定义`Go`的数据结构，可以选择将哪部分`JSON`数据解码到结构体对象中，哪些数据可以丢弃。

### 文本和HTML模板

`text/template`和`html/template`两个包可以实现格式和代码彻底分离，这里提供了一种机制，可以将程序变量的值带入到文本或者`HTML`模板中。

* 模板是一个字符串或者文件，它包含一个或者多个两边用双大括号包围的单元`{{...}}`，这称为`操作`。操作可以引发其他行为。
* 每个操作都对应一个表达式，提供输出值，选择结构体成员，调用函数和方法，描述控制逻辑，实例化其他模板等功能。
* 在操作中，符号`|`会将前一个操作的结果当作下一个操作的输入。
* 在操作中，用点号`.`表示当前值的标记。

模板输出结果需要两个步骤。

* 首先需要解析模板并转换为内部的表示方法。
* 然后在指定的输入上面执行。解析模板只需要执行一次。

```go
report, err := template.New("report").Funcs(template.FuncMap{}).Parse(templ)
if err != nil {
    log.Fatal(err)
}
```

* 模板通常是在编译期间就固定下来，因此无法解析模板将是程序程序中的一个严重`bug`。帮助函数`template.Must`提供了一种便捷的错误处理方式，它接受一个模板和错误作为参数，检查错误是否为`nil`（如果不是`nil`，则宕机），然后返回这个模板。

```go
report := template.Must("report").Funcs(template.FuncMap{}).Parse(templ)
```

## 函数

### 函数声明

每个函数声明都包含一个名字、一个形参列表、一个可选的返回列表以及函数体。

```go
func name(parameter-list) (result-list) {
    body
}
```

* 如果一个函数既省略返回列表也没有任何返回值，那么设计这个函数的目的是调用函数之后所带来的附加效果。

* 空白标识符用来强调这个形参在函数中未使用。
* 函数的类型称作函数签名，当两个函数拥有相同的形参列表和返回列表是，认为这两个函数的类型或签名是相同的。
* `golang`没有默认参数值的概念也不能指定实参名。
* 实参都是按值传递，所以函数接收到的是每个实参的副本。
* 如果提供的实参包含引用类型，比如指针、`slice`、`map`、`函数`或者通道，那么当函数使用形参变量时就有可能会间接地修改实参变量。

### 递归

函数可以递归调用，这一位置函数可以直接或间接地调用自己。

* 许多编程语言使用固定长度的函数调用栈；大小在`64KB`到`2MB`之间。
* 递归的深度会受限于固定长度的栈大小，所以当进行深度递归调用时必须谨防栈溢出。
* `golang`语言的实现使用了可变长度的栈，栈的大小会随着使用而增长，可达到`1GB`左右的上限。

### 多返回值

一个函数能够返回不止一个结果。

* `golang`的垃圾回收机制将回收未使用的内存，但不能指望它会释放未使用的操作系统资源，比如打开的文件以及网络连接。必须显示地关闭它们。

* 返回一个多值结果可以是调用另一个多值返回的函数。
* 一个函数如果有命名的返回值，可以省略`return`语句的操作数，这称为裸返回。

### 错误

* `error`是内置的接口类型。
* 一般当一个函数返回一个非空错误时，它其他的结果都是未定义的而且应该忽略。
* 与许多其他语言不同，`golang`通过使用普通的值而非异常来报告错误。

#### 错误处理策略

* 调用`log.Fatalf`就和所有的日志函数一样，它默认会将时间和日期作为前缀添加到错误消息前。

* 如果检测到的失败导致函数返回，成功的逻辑一般不会放在`else`块中而是在外层的作用域中。

#### 文件结束标识

* `io`包保证任何由文件结束引起的读取错误，始终都将会得到一个相同的错误------`io.EOF`。

### 函数变量

就像其他值，函数变量也有类型，而且它们可以赋给变量或者传递或者从其他函数中返回。

```go
func square(n int) int { return n * n }
f := square
```

* `strings.Map`对字符串中的每一个字符使用一个函数，将结果连接起来变成另一个字符串。

```go
func add1(r rune) rune { return r + 1 }

fmt.Println(strings.Map(add1, "VMS")) // "WNT"
```

### 匿名函数

函数字面量就像函数声明，但在`func`关键字后面没有函数的名称。它是一个表达式，它的值称作`匿名函数`。

```go
func add1(r rune) rune { return r + 1 }
fmt.Println(strings.Map(add1, "VMS")) // "WNT"
// 等价于
fmt.Println(strings.Map(func(r rune) rune { return r + 1 }, "VMS"))
```

* 以这种方式定义的函数能够获取整个词法环境。

```go
func squares() func() int {
    var x int
    return func() int {
        x ++
        return x * x
    }
}
func main() {
    f := squares()
    fmt.Println(f()) // "1"
    fmt.Println(f()) // "4"
    fmt.Println(f()) // "9"
}
```

这次的变量生命周期也不是由它的作用域所决定的：变量`x`在`main`函数中返回`squares`函数后依旧存在（虽然`x`在这个时候是隐藏在函数变量`f`中的）。

* 函数变量类似于使用闭包方法实现的变量，`golang`程序员通常把函数变量成为闭包。

### 警告：捕获迭代变量

```go
var rmdirs []func()
for _, dir := range tempDirs() {
    os.MkdirAll(dir, 0755)
    rmdirs = append(rmdirs, func() {
        os.RemoveAll(dir)	// 不正确
    })
}
```

在上面循环里创建的所有函数变量共享相同的变量------一个可访问的存储位置，而不是固定的值。因此，`dir`变量的实际取值是最后一次迭代时的值并且所有的`os.RemoveAll`调用最终都试图删除同一个目录。

### 变长函数

变长函数被调用的时候可以有可变的参数列表。

* 在参数列表最后的类型名称之前使用省略号`...`表示声明一个变长函数，调用这个函数的时候可以传递该类型任意数目的参数。

```go
func sum(vals ...int) int {
    total = 0
    for _, val = range vals {
        total += val
    }
    return total
}

fmt.Println(sum()) 			// "0"
fmt.Println(sum(3))			// "3"
fmt.Println(sum(1, 2, 3))	// "6"
```

* 变长函数通常用于格式化字符串。

### 延迟函数调用

* `defer`就是一个普通的函数或方法调用，在调用之前加上关键字`defer`。函数和参数表达式会在语句执行时求值，但是无论是正常情况下，执行`return`语句或函数执行完毕，还是不正常的情况下，比如发生宕机，实际的调用推迟到包含`defer`语句的函数结束后才执行。`defer`语句没有限制使用次数；执行的时候伊调用`defer`语句顺序的倒序进行（大概意思是类似栈这样先进后出？）。

```go
var mu sync.Mutex
var m = make(map[string]int)
func lookup(key string) int {
    mu.Lock()
    defer mu.Unlock()
    return m[key]
}
```

* 延迟执行的函数在`return`语句之后执行，并且可以更新函数的结果变量。因为匿名函数可以得到其外层的函数作用域内的变量（包括命名的结果），所以延迟执行的匿名函数可以观察到函数的返回结果。
* 通过命名结果变量和增加`defer`语句，我们能够在每次调用函数的时候输出它的参数结果。

```go
func double(x int) int {
    return x + x
}
```

```go
func double(x int) (result int) {
    defer func() { fmt.Printf("double(%d) = %d\n", x, result) } ()
    return x + x
}
_ = double(4)
// 输出：
// "double(4) = 8"
```

这个技巧的使用相比之前的`double`函数来说有些过了，但对于有很多返回语句的函数来说很有帮助。

```go
func triple(x int) (result int) {
    defer func() { result += x } ()
    return double(x)
}

fmt.Println(triple(4)) // "12"
```

### 宕机

`golang`的类型系统会捕获许多编译时错误，但有些其他的错误都需要在运行时进行检查。

* `goroutine`中的所有延迟函数会执行，然后程序会异常退出并留下一条日志消息。日志消息包括宕机的值，这往往代表某种错误消息，每一个`goroutine`都会在宕机的时候显示一个函数调用的栈跟踪值。

 ### 恢复

* 内置的`recover`函数在延迟函数的内部调用，而且这个包含`defer`语句的函数发生宕机，`recover`会终止当前的宕机状态并且返回宕机的值。函数不会从之前宕机的地方及继续运行而是正常返回。如果`recover`在其他任何情况下运行则它没有任何效果且返回`nil`。
*  `Parse`函数中的延迟函数会从宕机状态恢复，并使用宕机值组成一条错误消息；理想的写法是使用`runtime.Stack`将整个调用栈包含进来。



 ## 方法

 方法是某种特定类型的函数。

### 方法声明

方法的声明和普通函数的声明雷系，只是在函数名字前面多了一个参数。这个参数把这个方法绑定到这个参数对应的类型上。

* 类型拥有的所有方法名都必须是唯一的，但不同的类型可以使用相同的方法名。
* 命名类型与指向它们的指针是唯一可以出现在接收者声明处的类型。而且，为防止混淆，不允许本身是指针的类型进行方法声明。

* 只有符合下面三种形式的语句才能够成立。

1. 实参接收者和形参接受者是同一个类型，比如都是`T`类型或都是`*T`类型：

```go
Point{1, 2}.Distance(q) // Point
pptor.ScaleBy(2)		// *Point
```

2. 实参接收者是`T`类型的变量而形参接收者是`*T`类型。编译器会隐式地获取变量的地址：

```go
p.ScaleBy(2)			// 隐式转换为(*pptr)
```

### `nil`是一个合法的接收者

就像一些函数允许`nil`指针作为实参，方法的接收者也一样，尤其是当`nil`是类型中有意义的零值。 

* 当定义一个类型允许`nil`作为接收者时，应当在文档注释中显式地标明。

这是`net/url`包中`Value`类型的部分定义

```go
package url

// Values 映射字符串到字符串列表
type Values map[string][] string

// Get 返回第一个具有给定key的值
// 如不存在，则返回空字符串
func (v Values) Get(key string) string {
    if vs := v[key]; len(vs) > 0 {
        return vs[0]
    }
    return ""
}

// Add 添加一个键值到对应key列表中
func (v Values) Add(key, value string) {
    v[key] = append(v[key], value)
}
```

### 通过结构体内嵌组成类型

* 匿名字段类型可以是个指向命名类型的指针，这个时候，字段和方法间接地来自于所指向的对象。这可以更加动态、多样化共享通用的结构以及对象关系。

### 方法变量与表达式

通常情况在相同的表达式里使用和调用方法，但是把两个操作分开也是可以的。选择子可以赋予一个方法变量，它是一个函数，把方法绑定到一个接收者上。函数只需要提供

* 如果包内的`API`调用一个函数值，并且使用者期望这个函数的行为时调用一个特定接收者的方法，方法变量就非常有用。

```go
type Rocket struct { /* ... */ }
func (r *Rocket) Launch() { /* ... */ }

r := new(Rocket)
time.AfterFunc(10 * time.Second, func() { r.Launch() }) --->
---> 可以简洁成 time.AfterFunc(10 * time.Second, r.Launch)
```

* 与方法变量相关的是方法表达式。和调用一个普通的函数不同，在调用方法的时候必须提供接收者，并且按照选择子的语法进行调用。而方法表达式写成`T.f`或者`(*T).f`，其中`T`时类型，是一种函数变量，把原来方法的接收者替换成函数的第一个形参，英雌它可以像平常的函数一样调用。

```go
p := Point{1, 2}
q := Point{4, 6}

distance := Point.Distance	// 方法表达式
fmt.Println(distance(p, q))	// "5"
```

如果需要用一个值来表达多个方法中的一个，而方法都属于同一类型，方法变量可以帮助调用这个值所对应的方法来处理不同的接收者。

### 封装

如果变量或者方法是不能通过对象访问到的，则称作封装的变量或方法。

* `golang`只有一种方法控制命名的可见性：定义的时候，首字母大写的标识符是可以从包中导出的，而首字母没有大写的则不导出。同样的机制同样作用于结构体内的字段和类型中的方法。
* 要封装一个对象，必须使用结构体。
* `golang`封装的单元是包而不是类型。
* 封装提供了三个优点：

1. 不需要更多的语句检查变量的值（使用方不能直接修改对象的变量）。
2. 隐藏实现细节可以防止使用方依赖的属性发生改变。
3. 防止使用者肆意地改变对象内的变量。

## 接口

`golang`的接口的独特之处在于它是隐式实现的。

### 接口即约定

接口是一种抽象类型，它所提供的仅仅是一些方法而已。

* `fmt.Printf`和`fmt.Sprintf`通过接口机制解决了重复部分问题。两个函数都封装了第三个函数`fmt.Fprintf`。

```go
package fmt

func Fprintf(w io.Writer, format string, args ...interface{}) (int, error)

func Printf(format string, args ...interface{}) string {
    var buf bytes.Buffer
    Fprintf(&buf, format, args...)
    return buf.String
}

```

`Fprintf`的第一个形参也不是文件类型，而是`io.Writer`接口类型，其声明如下：

```go
package io

// Writer接口封装了基础的写入方法
type Writer interface {
    // Write 从p向底层数据流写入len(p)个字节的数据
    // 返回实际写入的字节数 (0 <= n <= len(p))
    // 如果没有写完，那么会返回遇到的错误
    // 在Write返回n<len(p)时，err必须为非nil
    // Write不允许修改p的数据，即使是临时修改
    
    // 实现时不允许残留p的引用
    Write(p []byte) (n int, err error)
}
```

`io.Writer`接口定义了`Fprintf`和调用者之间的约定。一方面，这个约定要求调用者提供的具体类型（比如`*os.File`或者`*bytes.Buffer`）包含一个与其签名和行为一致的`Write`方法。另一方面，这个约定保证了`Fprintf`能使用任何满足`io.Writer`接口的参数。

* 可以把一种类型替换为满足同意接口的另一种类型的特征称为***可取代性***，这也是面向对象语言的典型特征。
* 定义一个`String`方法就可以让类型满足这个广泛使用的接口`fmt.Stringer`：

```go
package fmt

// 在字符串格式化时如果需要一个字符串
// 那么就调用这个方法来把当前值转化为字符串
// Print这种不带格式化参数的输出方式也是调用这个方法
type Stringer interface {
    String() string
}
```

### 接口类型

一个接口类型定义了一套方法，如果一个具体类型要实现该接口，那么必须实现接口类型定义中的所有方法。

* `io`包定义了很多有用的接口。`io.Writer`抽象了所有可以写入字节的类型的抽象；`io.Reader`抽象了所有可以读取字节的类型；`io.Closer`抽象了所有可以关闭的类型以及等等。

```go
package io

type Reader interface {
    Read(p []byte) (n int, err error)
}

type Closer interface {
    Close() error
}
```

另外，还可以通过组合已有接口得到的新街口。

```go
type ReadWriter interface {
    Reader
    Writer
}
```

如上语法称为嵌入式接口。

### 实现接口

接口的赋值规则仅当一个表达式实现了一个接口时，这个表达式才可以赋给该接口。

* 非空的接口类型通常由一个指针类型来实现。
* 指针类型肯定不是实现接口的唯一类型，即使是那些包含了会改变接受者方法的接口类型，也可以有`golang`的其他引用类型来实现。
* 从具体类型出发、提取其共性而得出的每一种分组方式都可以表示为一种接口类型。

### 实验：接口能不能调用赋值的类型其他方法

```go
package main

import "fmt"

type a struct {
        a_str string
        b_str string
}

type Ralue interface {
        sstring() string
}

func (val a) sstring() string {
        return val.a_str
}

func (val *a) resString() string {
        return val.b_str
}


func main() {
        var w Ralue
        var test a
        test.a_str = "kk"
        test.b_str = "hh"
        w = test
        fmt.Println(w.sstring()) // "kk"
        fmt.Println(test.resString()) // "hh"
        fmt.Println(w.resString()) // "w.resString undefined (type Ralue has no field or method resString)"
}
```

看来是不可以的。

### 使用flag.Value来解析参数

* 标准接口`flag.Value`帮助定义行标志。
* 支持自定义类型其实也不难，只须定义一个满足`flag.Value`接口的类型。

```go
package flag

// Value 接口代表了存储在标志内的值
type Value interface {
    String() string
    Set(string) error
}
```

### 接口值

从概念来说，一个接口类型的值（简称接口值）其实有两部分：一个具体类型和该类型的一个值。二者称为接口的动态类型和动态值。

* `golang`中，类型仅仅是一个编译时的概念，所以类型不是一个值。在概念模型中，用**类型描述符**来提供每个类型的具体信息，比如它的名字和方法。
* 接口值可以用`==`和`!=`操作符来做比较。如果两个接口值都是`nil`或者二者的动态类型完全一致且二者动态值相等（使用动态类型的`==`操作符来做比较），那么两个接口值相等。因为接口值是可以比较的，所以它们可以作为`map`的键，也可以作为`switch`语句的操作数。
* 在比较两个接口值时，如果两个接口值的动态类型一致，但对应的动态值是不可比较的，那么这个比较会以崩溃的方式失败。从这点看，接口类型是非平凡的。

#### 注意：含有空指针的非空接口

空的接口值与仅仅动态值为`nil`的接口值是不一样的。

### 使用```sort.Interface```来排序

`sort.Interface`接口指定通用排序算法和每个具体的序列类型之间的协议。这个接口的实现确定了序列的具体布局（经常是一个`slice`），以及元素期望的排序方式。

* 一个原地排序算法需要知道三个信息：序列长度、比较两个元素的含义以及如何交换两个元素，所以`sort.Interface`接口就有三个办法。

```go
package sort

type Interface interface {
    Len() int
    Less(i, j int) bool // i, j是序列元素的下标
    Swap(i, j int)
}
```

要对序列排序，需要先确定一个实现了如上三个方法的类型，接着把`sort.Sort`函数应用到上面这类方法的实例上。

* `sort.Reverse`使用了一个重要概念：组合。

```go
package sort

type reverse struct { Interface } // that is, sort.Interface

func (r reverse) Less(i, j int) bool { return r.Interface.Less(j, i) }

func Reverse(data Interface) Interface { return reverse{ data } }
```

`sort`包定义了一个未导出的类型`reverse`，这个类型是一个嵌入了`sort.Interface`的结构。`reverse`的`Less`方法直接调用了内嵌的`sort.Interface`值的`Less`方法，但只交换传入的下标，就可以颠倒排序的结果。

`reverse`的另外两个方法`Len`和`Swap`，由内嵌的`sort.Interface`隐式提供。导出的函数`Reverse`则返回一个包含原始`sort.Interface`值的`reverse`实例。

### ```http.Handler```接口

作为服务端`API`基础的`http.Handler`接口。

```go
package http

type Handler interface {
    ServeHTTP(W ResponseWriter, r *Request)
}

func ListenAndServe(address string, h Handler) error
```

`ListenAndServe`函数需要一个服务器地址，比如`"localhost:8000"`，以及一个`Handler`接口的实例。

* 普通处理`URL`

```go
// req *http.Request

req.URL.Path
```

* 普通地处理请求参数

```go
// /xxx?item=value

switch req.URL.Path{
    case "/xxx":
		item := req.URL.Query().Get("item")
    	/*
    		xxx
    	*/
}
```

`URL`的`Query`方法，把`Http`的请求参数解析为`map`，或者更精确来讲，解析为一个`multimap`，由`net/url`包的`url.Values`类型实现。

* `net/http`包提供了一个请求多工转发器`ServeMux`，用来简化`URL`和处理程序之间的关联。
* 满足同一个接口的多个类型是可以**互相替代**的，`Web`服务器可以把请求分发到任意一个`http.Handler`。
* 对于一个更复杂的应用，多个`ServeMux`会组合起来，用来处理更复杂的分发需求。

`ServeMux`例子

```go
mux := http.NewServeMux()
mux.Handle(URL1 string, http.HandlerFunc(mothod.func1))
max.Handle(URL2 string, http.HandlerFunc(mothod.func2))
log.Fatal(http.ListenAndServe(ip_port string, mux))

func (md mothod) func1(w http.ResponseWriter, req *http.Request) {
    /*
    	xxx
    */
}

func (md mothod) func2(w http.ResponseWriter, req *http.Request) {
    /*
    	xxx
    */
}
```

表达式`http.HandlerFunc(xxx.func)`其实是类型转换，而不是函数调用。注意，`http.HandlerFunc`是一个类型。

``` 
package http

type HandlerFunc func(w ResponseWriter, r *Request)

func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
	f(w, r)
}
```

* `net/http`包提供一个全局的`ServeMux`实例`DefaultServeMux`，以及包级别的注册函数`http.Handle`和`http.HandlerFunc`。要让`DefaultServeMux`作为服务器的主处理程序，无须把它传给`ListenAndServe`，直接传`nil`即可。

### ```error```接口

这只是一个接口类型。

```go
type error interface {
    Error() string
}
```

构造`error`最简单的方法是调用`errors.New`，它会返回一个包含指定的错误消息的新`error`实例。

* 完整的`error`包只有如下4行代码。

```go
package errors

func New(text string) error { return &errorString{ text } }

type errorString struct { text string }

func (e *errorString) Error() string { return e.text }
```

* 底层的`errorString`类型是一个结构，而没有直接用字符串，主要是为了避免将来无意间的布局变更。满足`error`接口的是`*errorString`指针，而不是原始的`errorString`，主要是为了让每次`New`分配的`error`实例都互不相等。

```go
fmt.Println(errors.New("EOF") == errors.New("EOF"))		// false
```

* 直接调用`errors.New`比较罕见，因为有一个更易用的封装函数`fmt.Errorf`，它还额外提供了字符串格式化功能。

```go
package fmt

import "errors"

func Errorf(format string, args ...interface{}) error {
    return errors.New(Sprintf(format, args...))
}
```

### 类型断言

类型断言是一个作用在接口值上的操作，写出来类似于`x.(T)`，其中`x`是一个接口类型的表达式，而`T`是一个类型（称为断言类型）。类型断言会检查作为操作数的动态类型是否满足指定得断言类型。如果断言类型`T`是一个具体类型，那么类型断言会检查`x`的动态类型是否就是`T`。

* 如果断言类型`T`是一个接口类型，那么类型断言检查`x`的动态类型是否满足`T`。如果检查成功，动态值并没有提取出来，结果仍然是一个接口值，接口值的类型和值部分也没有变更，只是结果的类型为接口类型`T`。
* 如果类型断言出现在需要两个结果的赋值表达式中，那么断言不会在失败时崩溃，而是会多返回一个布尔型的返回值来指示断言是否成功。

```go
if w, ok := w.(*os.File); ok {
    // ...use w...
}
```

返回值的名字与操作数变量名一致，原有的值就被新的返回值掩盖了。

### 使用类型断言来识别错误

`os`包中有三类原因通常必须单独处理：文件已存储（创建操作），文件没找到（读取操作）以及权限不足。

```go
package os 

func IsExist(err error) bool
func IsNotExist(err error) bool
func IsPermission(err error) bool
```

### 通过接口类型断言来查询特征

给一个特定类型多定义一个方法，就隐式地接受了一个特定约定。

### 类型分支

接口有两种不同的风格。

1. 接口上的各种方法突出了满足这个接口的具体类型之间的相似性，但隐藏了各个具体类型的布局和各自特有的功能。这种风格强调了方法，而不是具体类型。
2. 充分利用·了接口值能够容纳各种具体类型的能力，它把接口作为这些类型的联合（`union`）来使用。这种风格的接口使用方式称为`可识别联合`。



## `goroutine`和通道

### `goroutine`

在`golang`中，每一个并发执行的活动称为`goroutine`。当一个程序启动时，只有一个`goroutine`来调用`main`函数，称它为主`goroutine`。新的`goroutine`通过`go`语句进行创建。`goroutine`与通道`channel`支持CSP（通信顺序进程）模型。

* 语法上，一个`go`语句是在普通的函数或者方法调用前加上`go`关键字前缀。
* `main`函数返回，当它发生时，所有的`goroutine`都暴力地直接终结，然后程序退出。

### 通道

通道是可以让一个`goroutine`发送特定值到另一个`goroutine`的通信机制。每一个通道是一个具体类型的导管，叫做通道的**元素类型**。一个有`int`类型元素的通道写为`chan int`。

```go
ch := make(chan int) // ch 的类型是`chan int`
```

通道是一个使用`make`创建的数据结构的引用。当复制或者作为参数传递到一个函数是，复制的是引用，这样调用者和被调用者都引用同一份数据结构。和其他引用类型一样，通道的零值是`nil`。

* 通道有两个主要操作：发送（`send`）和接收（`receive`），两者统称为`通信`。

```go
ch <- x		// 发送语句
x = <- ch	// 赋值语句中的接收表达式
<- ch		// 接收语句，丢弃结果
```

* 通道支持第三个操作：关闭（`close`），它设置一个标志位来指示值当前已经发送完毕。关闭后的发送操作将导致宕机。在一个已经关闭的通道上进行接受操作，将获取所有已经发送的值，直到通道为空。

```go
close(ch)
```

* 通道还分为无缓冲通道和缓冲通道。

```go
ch = make(chan int)		// 无缓冲通道
ch = make(chan int, 0)	// 无缓冲通道
ch = make(chan int, 3)	// 容量为3的缓冲通道
```

#### 无缓冲通道

无缓冲通道上的发送操作将会阻塞，直到另一个`goroutine`在对应的通道上执行接收操作，这时值传送完成，两个`goroutine`都可以继续执行。

* 使用无缓冲通道进行的通信导致发送和接收`goroutine`同步化。因此，无缓冲通道也称为同步通道。

#### 管道

通道可以用来连接`goroutine`，这样一个的输出是另一个的输入。这个叫管道（`pipeline`）。

* 没有一个直接的方式来判断是否通道已经关闭，但是这里有接收操作的一个变种，它产生两个结果：接收到的通道元素，以及一个布尔值（通常称为`ok`）。
* 通道也是可以通过垃圾回收器根据它是否可以访问来决定是否回收它，而不是根据它是否关闭。（不要将这个`close`操作和对于文件的`close`操作混淆。当结束的时候对每一个文件调用`Close`方法是非常重要的。）

#### 单向通道类型

`golang`的类型系统提供了单向通道类型，仅仅导出发送或接收操作。如类型`chan <- int`是一个只能发送的通道，允许发送但不允许接收。反之，类型`<- chan int`是一个只能接收的`int`类型通道，允许接收但是不能发送。

* 在任何赋值操作中将双向通道转换为单向通道都是允许的，但是反过来是不行的。

#### 缓冲通道

缓冲通道上的发送操作在队列的尾部插入一个元素，接受操作从队列的头部移除一个元素。

* 程序需要知道通道缓冲区的容量，可以通过调用内置的`cap`函数获取它。

* 无缓冲通道提供强同步保障，因为每一次发送都需要和一次对应的接收同步；对于缓冲通道，这些操作则是解耦的。

### 并行循环

* 完全独立的子问题组成的问题称为**高度并行**。高度并行的问题是最容易实现并行的，有许多并行机制来实现线性扩展。
* `sync.WaitGroup`作为计数器类型，可以被多个`goroutine`安全地操作，然后有一个办法一直等到它变为`0`。

### 使用`select`多路复用

* `time.Tick`函数返回一个通道，它定期发送事件，像一个节拍器一样。
* `select`可以实现多路复用

```go
select {
    case <- ch1:
    	// ...
    case x := <- ch2:
    	// ... use x ...
    case ch3 <- y:
    	// ...
    default:
    	// ...
}
```

`select`一直等待，直到一次通信来告知有一些情况可以执行。

* 如果多个情况同时满足，`select`随机选择一个，这样保证每一个通道有相同的机会会被选中。
