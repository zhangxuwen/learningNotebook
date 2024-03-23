# GdScript 脚本特点

**GdScript** 是弱类型语言，比较自由，需要一些规范

* 类名必须与文件名相同，可为小写
* 尽量继承与 **Node2D** 节点，**Node2D** 节点中的 **Transform** 是我们用的最多的节点

常用函数内部执行顺序：**_init**、**_ready**，**_process**

```makefile
默认定义了一些事件函数，如
_init()			脚本初始化的时候调用，对象的构造器
_ready()		开始调用一次，可用于初始化脚本
_process(delta)	每帧调用，帧间隔不等，可用于更新游戏
```







# 变量和数据类型

```go
// example
var x=5;
var y=6;
var z=x+y;
```

**数据类型分类**

* bool，一个字节，默认为false

* int，8个字节，默认为0

* float，8个字节，默认为0

* String，默认为**null/88，字符串可以存储一系列字符

* 数组

* 对象

  ```go
  // example
  var dict = {
      2: 3
      "key": "value"
  }
  ```

* null，变量没有被赋值，则默认为**null**







# 导出变量

* **export**关键字可以让变量在编辑器中编辑

  ```python
  # 导出一个数字
  export var a = 1
  # 导出一个节点路径
  export var b:NodePath
  # 导出一个节点路径，不同的写法
  export(NodePath) var c
  # 导出一个文件路径
  export(String, FILE) var e
  # 导出一个文件路径，以txt结尾
  export(String, FILE, "*.txt") var d
  # 导出一个资源文件路径
  export(Resource) var f
  # 导出一个颜色
  export(Color, RGB) var g
  ```







# 函数

* 函数是可以简单的理解为当它被调用时执行的可重复使用的代码块
* 函数就是包裹在花括号中的代码块，前面使用了关键词**func**，当调用该函数时，会执行函数内的代码
* 空函数需要使用**pass**关键字

```python
func helloWorld():
    # 执行代码
```

* 调用带参数的函数，在调用函数时，可以向其传递值，这些值被称为参数

```python
func helloWorld(param1, param2):
    # 执行代码
```

* 带有返回值的函数，有时，会希望函数将值返回调用它的地方，通过使用**return**语句就可以实现
* **return**方法可以指定返回的类型

```python
func helloWorld(parma1, param2):
    # 执行代码
    return x
```







# 变量的作用域

* 局部作用域，变量在函数内声明，变量为局部作用域，只能在函数内部访问
* 全局变量，变量在函数外定义，即为全局变量，整个脚本文件中都可以使用







# 运算符

* 算术运算符

  ```txt
  +	加法
  -	减法
  *	乘法
  /	除法
  %	取模（余数）
  ```

* 赋值运算符，赋值运算符用于给**GdScript**变量赋值

  ```txt
  =	+=	-=	*=	/=	%=
  ```

* 比较运算符，比较运算符在逻辑语句中使用，以测定变量或值是否相等

  ```txt
  ==	!=	>	<	>=	<=
  ```

* 逻辑运算符，逻辑运算符用于测定变量或值之间的逻辑

  ```txt
  &&	||	!
  ```







# 条件语句

* **if**语句 - 只有当指定条件为**true**时，使用该语句来执行代码

  ```python
  if (condition):
      当条件为 true 时执行的代码
  ```

* 
