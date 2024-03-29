# 作用域

* 作用域（scope）规定了变量能够被访问的”范围“，离开了这个”范围“变量便不能被访问
* 作用域分为
  * 局部作用域
  * 全局作用域

## 局部作用域

1. **函数作用域**

   在函数内部声明的变量只能在函数内部被访问，外部无法直接访问

   ```html
   <!-- example -->
   <script>
       function getSum() {
           // 函数内部是函数作用域
           const num = 10
       }
       console.log(num)
   </script>
   ```

   总结

   1) 函数内部声明的变量，在函数外部无法被访问
   2) 函数的参数也是函数内部的局部变量
   3) 不同函数内部声明的变量无法相互访问
   4) 函数执行完毕后，函数内部的变量实际被清空了

2. **块作用域**

   在JavaScript中使用**{}**包裹的代码称为代码块，代码块内部声明的变量外部将【**有可能**】无法被访问

   ```javascript
   // example
   for(let t = 1; t <= 6; t ++) {
       // t 只能在该代码块中被访问
       console.log(t)
   }
   
   // 超出了 t 的作用域
   console.log(t) // 报错
   ```

   1. **let**声明的变量会产生块作用域，**var**不会产生块作用域
   2. **const**声明的常量也会产生块作用域
   3. 不同代码块之间的变量无法相互访问
   4. 推荐使用 **let** 或 **const**



## 全局作用域

**\<script>**标签 和 **.js** 文件 的[最外层] 就是所谓的全局作用域，在此声明的变量在函数内部也可以被访

问全局作用域中声明的变量，任何其它作用域都可以被访问

**注意**

* 为window对象动态添加的属性默认也是全局的，不推荐
* 函数中未使用任何关键字声明的变量为全局变量，不推荐
* 尽可能少的声明全局变量，防止全局变量被污染



## 作用域链

作用域链本质上是底层的**变量查找机制**

* 在函数被执行时，会**优先查找当前**函数作用域中查找变量
* 如果当前作用域查找不到则会依次**逐级查找父级作用域**直到全局作用域

**总结**

* 嵌套关系的作用域串联起来形成了作用域链
* 相同作用域链中按着从小到大的规则查找变量
* 子作用域能够访问父作用域，父级作用域无法访问子级作用域







# 垃圾回收机制

**垃圾回收机制（Garbage Collection）简称GC**

JS中**内存**的分配和回收都是**自动完成**的，内存在不使用的时候会被**垃圾回收器**自动回收

## 内存的生命周期

JS环境中分配的内存，一般有如下**生命周期**

1. **内存分配**：当声明变量、函数、对象的时候，系统会自动为他们分配内存
2. **内存使用**：即读写内存，也就是使用变量、函数等
3. **内存回收**：使用完毕，由**垃圾回收器**自动回收不再使用的内存

**说明**

* 全局变量一般不会回收（关闭页面回收）
* 一般情况下**局部变量的值**不用了，会被**自动回收**掉

**内存泄漏**：程序中分配的**内存**由于某种原因程序**未释放**或**无法释放**叫做**内存泄漏**



## 算法说明

堆栈空间分配区别

1. 栈（操作系统）：由**操作系统自动分配释放**函数的参数值、局部变量等，基本数据类型放到栈里面
2. 堆（操作系统）：一般由程序员分配释放，若程序员不释放，由**垃圾回收机制**回收。**复杂数据类型**放堆里面

两种常见的浏览器**垃圾回收算法**

* **引用计数**

  IE采用的引用计数算法，定义”**内存不再使用**“，就是看一个**对象**是否有指向它的引用，没有引用了就回收对象

  算法

  1. 跟踪记录被**引用的次数**
  2. 如果被引用了一次，那么就记录次数1，多次引用会**累加 ++**
  3. 如果减少一个引用就**减1 --**
  4. 如果引用次数是**0**，则释放内存

  它存在一个致命的问题：**嵌套引用**（循环引用）

  如果两个对象**相互引用**，尽管他们已不再使用，垃圾回收器不会进行回收、导致内存泄漏

* **标记清除法**

  现代的浏览器已经不再引用计数算法了

  现代浏览器通用的大多是基于**标记清除算法**的某些改进算法，总体思想都是一致的

  核心

  1. 标记清除算法将”不再使用的对象“定义为”**无法达到的对象**“
  2. 就是从**根部**（在JS中就是全局对象）出发定时扫描内存中的对象。凡是能从**根部到达**的对象，都是还**需要使用**的
  3. 那些**无法**由根部出发触及到的**对象被标记**为不再使用，稍后进行**回收**



## 闭包

概念：一个函数对周围状态的引用捆绑在一起，内层函数中访问到其外层函数的作用域

简单理解：**闭包 = 内层函数 + 外层函数的变量**

闭包作用：封闭数据，提供操作，外部也可以访问函数内部的变量

闭包的基本格式

```javascript
// example

function outer() {
    let i = 1
    function fn() {
        console.log(i)
    }
    return fn
}
const fun = outer()
fun() // 1
// 外层函数使用内部函数的变量
```

闭包应用：实现数据私有（存在内存泄漏风险）







# 变量提升

变量提升是**JavaScript**中比较”“奇怪”的现象，它允许在变量声明之前即被访问（仅存在于**var**声明变量）

**注意**

1. 变量在未声明即被访问时会报语法错误
2. 变量在**var**声明之前即被访问，变量的值为**undefined**
3. **let/const** 声明的变量不存在变量提升
4. 变量提升出现在相同作用域当中
5. **实际开发中推荐先声明再访问变量**







# 函数进阶

## 函数提升

函数提升于变量提升比较类似，是指函数在声明之前即可被调用

```javascript
// example

// 调用函数
foo()
// 声明函数
function foo() {
    console.log('声明之前即被调用...')
}
```

**总结**

1. 函数提升能够使函数的声明调用更灵活
2. 函数表达式不存在提升的现象
3. 函数提升出现在相同作用域当中



## 函数参数

### 动态参数

**argument**是函数内部内置的伪数组变量，它包含了调用函数时传入的所有实参

```javascript
// example

// 求生函数，计算所有参数的和
function sum() {
    // console.log(argument)
    let s = 0
    for(let i = 0; i < arguments.length; i ++) {
        s += arguments[i]
    }
    console.log(s)
}
// 调用求和函数
sum(5, 10)
sum(1, 2, 4)
```

**总结**

1. **arguments**是一个伪数组，只存在于函数中
2. **arguments**的作用时动态获取函数的实参
3. 可以通过**for**循环依次得到传递过来的实参



### 剩余参数

剩余参数允许将一个不定数量的参数表示为一个数组

1. **...**是语法符号，置于最末函数形参之前，用于获取**多余**的实参
2. 借助**...**获取的剩余实参，是个**真数组**

```javascript
function config(baseURL, ...other) {
    console.log(baseURL) // 得到 'http://baidu.com'
    console.log(other) // other 得到['get', 'json']
}
// 调用函数
config('http://baidu.com', 'get', 'json')
```

开发中，还是提倡多使用**剩余参数**

#### 对比展开运算符

展开运算符**(...)**，将一个数组进行展开

经典运用场景：求数组最大值（最小值）、合并数组等

```javascript
const arr1 = [1, 5, 3, 8, 2]
console.log(...arr)
// 求数组最大值
console.log(Math.max(...arr1))
// 合并数组
const arr2 = [3, 4, 5]
const arr = [...arr1, ...arr2]
console.log(arr)
```

说明

1. 不会修改原数组



## 箭头函数

**目的**：引入箭头函数的目的是更简短的函数写法并且不绑定**this**，箭头函数的语法比函数表达式更简洁

**使用场景**：箭头函数更适用于那些本来**需要匿名函数的地方**

```javascript
// example

// 箭头函数
const fn1 = () => {
    console.log(123)
}
fn1()

// 只有一个形参的时候，可以省略小括号
const fn2 = x => {
    console.log(x)
}
fn2(1)

// 只有一行代码的时候，可以省略大括号
const fn3 = x => console.log(x)
fn3(1)

// 只有一行代码的时候，可以省略return
const fn4 = x => x + x
console.log(fn4(1))

// 箭头函数可以直接返回一个对象
const fn5 = (uname) => ({ name :uname })
fn('hello,world')
```

**总结**

* 箭头函数属于表达式函数，因此不存在函数提升
* 箭头函数只有一个参数时可以省略圆括号**()**
* 箭头函数函数体只有一行代码时可以省略花括号**{}**，并自动作为返回值被返回
* 加括号的函数体返回对象字面量表达式

### 箭头函数参数

1. 普通函数有**arguments**动态参数
2. **箭头函数**没有**arguments**动态参数，但是**有** **剩余参数** **...args**

```javascript
// example

const getSum = (...args) => {
    let sum = 0
    for(let i = 0; i < args.length; i ++) {
        sum += args[i]
    }
    return sum
}
console.log(getSum(1, 2, 3))
```

### 箭头函数 this

在箭头函数出现之前，每一个新函数根据它是被**如何调用的**来定义这个函数的**this**值

**箭头函数不会创建自己的this**，它只会从自己的作用域链的上一层沿用**this**

在开发中【是用箭头函数前需要考虑函数中**this**的值】，事件回调函数使用箭头函数时，**this**为全局的**window**，因此**DOM事件回调函数为了简便，还是不太推荐使用箭头函数**







# 解构赋值



## 数组解构

数组解构是将数组的单元值快速批量赋值给一系列变量的简洁语法

**基本语法**

1. 赋值运算符 = 左侧的 **[]** 用于批量声明变量，右侧数组的单元值被赋值给左侧的变量
2. 变量的顺序对应数组单元值的位置依次进行赋值操作

```javascript
// example

const arr = [100, 60, 80]
// 数组解构 赋值
const [max, min, avg] = arr
```

### 变量多 单元值少的情况

```javascript
const [a, b, c, d] = [1, 2, 3]
console.log(a) // 1
console.log(b) // 2
console.log(c) // 3
console.log(d) // undefined
```

#### 防止 undefined 传递

```javascript
const [a = 0, b = 0] = []
console.log(a) // 0
console.log(b) // 0
```

### 变量少 单元值多的情况

```javascript
const [a, b, c] = ['小米', '苹果', '华为', '格力']
console.log(a) // 小米
console.log(b) // 苹果
console.log(c) // 华为
```

#### 利用剩余参数解决 变量少 单元多问题

```javascript
const [a, b, ...c] = [1, 2, 3, 4]
console.log(a) // 1
console.log(b) // 2
console.log(c) // [3, 4] 真数组
```



### 按需导入赋值

```javascript
const[a, b, , d] = [1, 2, 3, 4]
console.log(a) // 1
console.log(b) // 2
console.log(d) // 4
```



### 支持多维数组解构

```javascript
// example

const [a, b, c] = [1, 2, [3, 4]]
console.log(a) // 1
console.log(b) // 2
console.log(c) // [3, 4]

const [a, b, [c, d]] = [1, 2, [3, 4]]
console.log(a) // 1
console.log(b) // 2
console.log(c) // 3
console.log(d) // 4
```







## 对象解构

对象解构是将对象属性和方法快速批量赋值给一系列变量的简洁语法

* **基本语法**
  1. 赋值运算符 **=** 左侧 **{}** 用于批量声明变量，右侧对象的属性将赋值给左侧的变量
  2. 对象属性的值将被赋值给与属性名**相同的**变量
  3. 注意解构的变量名不要和外面的变量名冲突否则报错
  4. 对象中找不到与变量名一致的属性时变量值为 **undefined**

```javascript
// example

const { uname, age } = { uname: 'hello', age: 18 }
// 等价于 const uname = obj.uname

// 对象解构的变量名 可以重新改名 旧变量名: 新变量名
const { uname: username, age } = { uname: 'hello', age: 18 }
```

### 解构数组对象

```javascript
// example

const pig = [
    {
        uname: 'pigs',
        age: 3
    }
]
const [{ uname, age }] = pig
```



### 多级对象解构

```javascript
// example

const pig = {
    name: 'pigs',
    family: {
    	mother: 'pigmom'
    	father: 'pigfam'
	},
	age: 3
}

// 多级对象解构
const { name, family: { mother, father }} = pig
```

#### 数组情况

```javascript
const pig = [{
    name: 'pigs',
    family: {
    	mother: 'pigmom'
    	father: 'pigfam'
	},
	age: 3
}]

// 多级对象解构
const [{ name, family: { mother, father }}] = pig
```







# forEach遍历数组

* **forEach()**方法用于调用数组的每个元素，并将元素传递给回调函数

* 主要使用场景：**遍历数组的每个元素**

* **语法**

  ```javascript
  被遍历的数组.forEach(fucntion (当前数组元素, 当前元素索引号) {
                 // 函数体
  })
  ```

**注意**

1. **forEach** 主要是遍历数组
2. 参数当前数组元素是必须要写的，索引号可选







# 筛选数组 filter 方法

* **filter()** 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素

* 主要使用场景：**筛选数组符合条件的元素，并返回筛选之后元素的新数组**

* **语法**

  ```javascript
  被遍历的数组.filter(fucntion (currentValue, index) {
  	return 筛选条件              
  })
  ```







# 深入对象

## 创建对象三种方式

1. 利用对象字面量创建对象

   ```javascript
   const o = {
       name: 'pig'
   }
   ```

2. 利用 **new Object** 创建对象

   ```javascript
   const o = new Object({ name: 'pig' })
   console.log(o)
   ```

3. 利用构造函数创建对象

   * **构造函数**

     是一种特殊的函数，主要用来初始化对象

   * 两个约定

     1. 它们的命名以大写字母开头
     2. 它们只能由**new**操作符来执行

   ```javascript
   // example
   
   // 创建构造函数
   function Pig(name) {
       this.name = name
   }
   
   // new 关键字调用函数
   // 接受创建的对象
   const peppa = new Pig('pig')
   console.log(peppa)
   ```

   说明

   1. 使用 **new** 关键字调用函数的行为被称为 **实例化**
   2. 实例化构造函数时没有参数时可以省略**()**
   3. 构造函数内部无需写**return**，返回值即为新创建的对象
   4. 构造函数内部的 **return** 返回的值无效，所以不要写 **return**
   5. **new  Object()** 、**new Date()** 也是实例化构造函数

## 构造函数

* 实例化执行过程
  1. 创建新对象
  2. 构造函数**this**指向新对象
  3. 执行构造函数代码，修改**this**，添加新属性
  4. 返回新对象



## 实例成员&静态成员

**实例成员**

通过构造函数创建的对象称为实例对象，**实例对象**中的属性和方法称为**实例成员**（实例属性和实例方法）

**说明**

1. 为构造函数传入参数，创建结构相同但值**不同的对象**
2. 构造函数创建的实例对象**彼此独立**互不影响



**静态成员**

**构造函数**的属性和方法被称为**静态成员**（静态属性和静态方法）

**说明**

1. 静态成员只能构造函数来访问
2. 静态方法中的**this**指向构造函数







# 内置构造函数

在**JavaScript**中**最主要**的数据类型有六种：

**基本数据类型**

* 字符串、数值、布尔、undefined、null

**引用类型**

* 对象

其实字符串、数值、布尔等基本类型也有专门的构造函数，这些称为包装类型



**引用类型**

* Object、Array、RegExp、Date 等

**包装类型**

* String、Number、Boolean 等



## Object

三个常用静态方法（静态方法就是只有构造函数Object可以调用的）

* **作用**：**Object.keys** 静态方法获取对象中所有属性

* **作用**：**Object.values** 静态方法获取对象中所有值

* 语法

  ```javascript
  // example
  
  const o = { name: 'pig', age: 3 }
  // 获得对象的所有键，并且返回是一个数组
  const arr = Object.keys(o)
  console.log(arr) // ['name', 'age']
  ```

  注意：返回的是一个数组



* **作用**：**Object.assign** 静态方法常用于对象拷贝

* **使用**：经常使用的场景给对象添加属性

* 语法

  ```javascript
  // example
  
  // 拷贝对象 把 o 拷贝给 obj
  const o = { name: 'pig', age: 3 }
  const obj = {}
  Object.assign(obj, o)
  console.log(obj) // { name: 'pig', age: 3 }
  ```





## Array

**Array**是内置的构造函数，用于创建数组

```javascript
const arr = new Array(3, 5)
console.log(arr)
```

创建数组建议使用**字面量**创建，不用**Array**构造函数创建

**数组常见实例方法**

| 方法    | 作用         |
| ------- | ------------ |
| forEach | **遍历**数组 |
| filter  | **过滤**数组 |
| map     | **迭代**数组 |
| reduce  | **累计器**   |

### 数组reduce累计方法

**作用**：**reduce**返回**累计处理的结果**，经常用于**求和等**

* 基本语法

  ```javascript
  arr.reduce(function(){}, 初始值)
  ```

  ```javascript
  arr.reduce(function(上一次值，当前值){}, 初始值)
  ```

  **参数**

  1. 如果有**起始值**，则把初始值累加到里面

* **reduce** 执行过程

  1. 如果**没有起始值**，则**上一次值**以数组的**第一个数组元素**
  2. 每一次循环，把**返回值**给作为下一次循环的**上一次值**
  3. 如果有**起始值**，则起始值作为**上一次值**

### 常见实例方法 - 其他方法

* **join** 数组元素拼接为字符串，返回字符串（重点）
* **find **查找元素，返回符合测试条件的第一个数组元素值，如果没有符合条件的则返回 **undefined**（重点）
* **every** 检测数组所有元素是否都符合指定条件，如果**所有元素**都通过检测返回**true**，否则返回**false**（重点）
* **some** 检测数组中的元素是否满足指定条件 **如果数组中有**元素满足条件返回**true**，否则返回**false**
* **concat** 合并两个数组，返回生成新数组
* **sort** 对原数组单元值排序
* **splice** 删除或替换原数组单元
* **reverse** 反转数组
* **findIndex** 查找元素的索引值



## String

在**JavaScript**中的字符串、数值、布尔具有对象的使用特征，如具有属性和方法

```javascript
// example

// 字符串类型
const str = 'hello world'
// 统计字符的长度（字符数量）
console.log(str.length)

// 数值类型
const price = 12.345
// 保留两位小数
price.toFixed(2)
```

之所以具有对象特征的原因是字符串、数值、布尔类型数据是**JavaScript**底层使用**Object**构造函数“包装”来的，被称为**包装类型**

### 常见实例方法

1. **lenght**用来获取字符串的度长（重点）
2. **split('分隔符')**用来将字符串拆分数组（重点）
3. **substring(需要截取的第一个字符的索引[,结束的索引号])**用于字符串截取（重点）
4. **stratsWith(检测字符串[,检测位置索引号])**检测是否以某字符开头（重点）
5. **includes(搜索的字符串[,检测位置索引号])**判断一个字符串是否包含在另一个字符串中，根据情况返回**true**或**false**（重点）
6. **toUpperCase**用于将字母转换成大写
7. **toLowerCase** 用于将就转换成小写
8. **indexOf**检测是否包含某字符
9. **endsWith** 检测是否以某字符结尾
10. **replace** 用于替换字符串，支持正则匹配
11. **math** 用于查找字符串，支持正则匹配



## Number

**Number** 是内置的构造函数，用于创建数值

常用方法：

**toFixed()** 设置保留小数位的长度

```javascript
// example

// 数值类型
const price = 12.345
// 保留两位小数 四舍五入
console.log(price.toFixed(2)) // 12.35
```







# 编程思想

## 面向过程编程

* **面向过程**就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候再一个一个的依次调用就可以了
* **面向过程，就是按照我们分析好了的步骤，按照步骤解决问题**



## 面向对象编程（oop）

* **面向对象**是把事务分解成一个个对象，然后由对象之间分工与合作
* **面向对象是以对象功能来划分问题，而不是步骤**

* 在面向对象程序开发思想中，每一个对象都是功能中心，具有明确分工
* 面向对象编程具有灵活、代码可复用、容易维护和开发的优点，更适合多人合作的大型软件项目
* 面向对象的特性
  * 封装性
  * 继承性
  * 多态性







# 构造函数

* 封装是面向对象思想中比较重要的一部分，js面向对象可以通过构造函数实现的封装
* 同样的将变量和函数组合到了一起并能通过**this**实现数据的共享，所不同的是借助构造函数创建出来的实例对象之间是彼此不影响的
* 总结
  * 构造函数体现了面向对象的封装特性
  * 构造函数实例创建的对象彼此独立、互不影响

**前面学过的构造函数方法很好用，但是存在浪费内存的问题（因为function（）指向的不是同一个冗余了）**







# 原型

* 构造函数通过原型分配的函数是所有对象所**共享的**
* JavaScript规定，**每一个构造函数都有一个 prototype 属性**，指向另一个对象，所以也称为原型对象
* 这个对象可以挂载函数，对象实例化不会多次创建原型上函数，节约内存
* **可以把那些不变的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些方法**
* **构造函数和原型对象中的 this 都指向 实例化的对象**

```javascript
// example

function Star(uname, age) {
    this.uname = uname
    this.age = age
}
Star.prototype.sing = function() {
    console.log('唱歌')
}
```

**构造函数和原型对象中的 this 都指向 实例化的对象**



## constructor 属性

**作用**：该属性**指向**该原型对象的**构造函数**

```javascript
// example

/*
function Star() {}
console.log(Star.prototype) // 有constructor
Star.prototype = {
    sing: function () {
        console.log('唱歌')
    },
    dance: function () {
        console.log('跳舞')
    },
}
console.log(Star.prototype) // 没有constructor
*/

function Star() {}
console.log(Star.prototype) // 有constructor
Star.prototype = {
    // 重新指回创造这个原型对象的 构造函数
    constructor: Star,
    sing: function () {
        console.log('唱歌')
    },
    dance: function () {
        console.log('跳舞')
    },
}
console.log(Star.prototype) // 有constructor
```



## 对象原型

**对象都会有一个属性 __proto__ **指向构造函数的**prototype**原型对象，之所以对象可以使用构造函数 **prototype** 原型对象的属性和方法，就是因为对象有 **__proto__ **原型的存在

注意：

* **__proto__** 是**JS**非标准属性
* **[[prototype]]** 和 **__proto__** 意义相同
* 用来表明当前实例对象指向哪个原型对象**prototype**
* **\__proto__** 对象原型里面也有一个 **constructor** 属性，**指向创建该实例对象的构造函数**

<img src = "image/2.png">



## 原型继承

继承是面向对象编程的另一个特征，通过继承进一步提升代码封装的程度，**JavaScript**中大多数是借助原型对象实现继承的特性

```javascript
// example

const Person = {
    eays: 2,
    head: 1
}

function Woman() {
    
}

Woman.prototype = Person
Woman.prototype.constructor = Woman
const red = new Woman()
console.log(red)
console.log(Woman.prototype)

function Man() {
    
}
Man.prototype = Person
Man.prototype.constructor = Man
const blue = new Man()
console.log(blue)
console.log(Man.prototype)
```

### 问题

这里男人和女人都同时使用了同一个对象，根据引用类型的特点，他们都指向同一个对象，修改一个都会影响

### 解决                                                                                                                                        

```javascript
// example

function Person() {
    this.eyes = 2
    this.head = 1
}

function Woman() {
    
}

Woman.prototype = new Person()
Woman.prototype.constructor = Woman

function Man() {
    
}

Man.prototype = new Person()
Man.prototype.constructor = Man
```



## 原型链

基于原型对象的继承使得不同构造函数的原型对象关联在一起，并且这种关联的关系是一种链状的结构，将原型对象的链状结构关系称为原型链

### 查找规则

1. 当访问一个对象的属性（包括方法）时，首先查找这个**对象**自身有没有该属性
2. 如果没有就查找它的原型（也就是 **__proto__** 指向的 **prototype 原型对象**）
3. 如果还没有就查找原型对象的原型（**Object的原型对象**）
4. 依此类推一直找到 **Object** 为止（**null**）
5. **__proto__** 对象原型的意义在于为对象成员查找机制提供一个方向，或者说一条路线
6. 可以使用 **instanceof** 运算符用于检测构造函数的 **prototype** 属性是否出现在某个实例对象的原型链上







# 深浅拷贝

考虑深浅拷贝问题只针对引用类型

## 浅拷贝

**常见方法**

1. 拷贝对象：**Object.assgin()** / 展开运算符 **{...obj}** 拷贝对象
2. 拷贝数组：**Array.prototype.concat()** 或者 **[...arr]**

**如果时简单数据类型拷贝值，引用数据类型拷贝的是地址（简单理解：如果是单层对象没问题，如果有多层就有问题）**



## 深拷贝

拷贝的是对象，不是地址

**常见方法**

1. 通过递归实现深度拷贝

   **函数递归**

   **如果一个函数子啊内部可以调用其本身，那么这个函数就是递归函数**

   * 简单理解：函数内部自己调用自己，这个函数就是递归函数
   * 递归函数的作用和循环效果类似
   * 由于递归很容易发生“栈溢出”错误（**stack overflow**），所以**必须加退出条件 return**

2. **lodash / cloneDeep**

   **JS**库**lodash**里面**cloneDeep**内部实现了深拷贝

   ```javascript
   // example
   
   var objects = [{ 'a': 1 }, { 'b': 2 }];
   var deep = _.cloneDeep(objects);
   console.log(deep[0] === objects[0]);
   // => false
   ```

3. 通过**JSON.stringify()**实现

   ```javascript
   // example
   
   Json.parse(JSON.stringify(obj))
   ```







# 异常处理



## throw 抛异常

异常处理是指预估代码执行过程中可能发生的错误，然后最大程度的避免错误的发生导致整个程序无法继续运行

```javascript
// example

function fn(x, y) {
    if(!x || !y) {
        // throw '没有参数传递进来'
        throw new Error('没有参数传递过来')
    }
    return x + y
}
concole.log(fn())
```



## try / catch 捕获错误信息

可以通过 **try / catch** 捕获错误信息（浏览器提供的错误信息）

```javascript
// example

function fn() {
    try {
        // 可能发送错误的代码 要写到 try
        const p = document.querySelector('.p')
        p.style.color = 'red'
    } catch (err) {
        // 拦截错误，提示浏览器提供的错误信息，但是不中断程序的执行
        console.log(err.message)
        throw new Error('你看看，选择器错误了吧')
        // 需要加return 中断程序
        // return
    }
    finally {
        // 不管你程序对不对，一定会执行的代码
        alert('弹出对话框')
    }
}
fn()
```

**总结**

1. **try...catch** 用于捕获错误信息
2. 将预估可能发生错误的代码写在**try**代码段中
3. 如果**try**代码段中出现错误后，会执行**catch**代码段，并截获错误信息
4. **finally** 不管是否有错误，都会执行



## debugger

```javascript
// example

const arr = [1, 3, 5]
const newArr = arr.map((item, index) => {
    debugger
    console.log(item)
    console.log(index)
    return item + 10
})
console.log(newArr)
```







# 处理this



## this指向 - 普通函数

普通函数的调用方式决定了**this**的值，即【谁调用**this**的值指向谁】

普通函数没有明确调用者时**this**值为**window**，严格模式下没有调用者时**this**的值为**undefined**



## this指向 - 箭头函数

箭头函数中的**this**与普通函数完全不同，也不受调用方式的影响，事实上**箭头函数中并不存在this**

1. 箭头函数会默认帮我们绑定外层**this**的值，所以在箭头函数中**this**的值和外层的**this**是一样的
2. 箭头函数中的**this**应用的就是最近作用域中的**this**
3. 向外层作用域中，一层一层查找**this**，直到有**this**的定义

**注意情况**

在开发中【使用箭头函数前需要考虑函数中**this**的值】，事件回调函数使用箭头函数时，**this**为全局的**window**

因此**DOM**事件回调函数**如果里面需要** **DOM**对象的**this**，则不推荐使用**箭头函数**



## 总结

1. 函数内不存在**this**，沿用上一级的
2. 不适用
   * 构造函数，原型函数，**DOM**事件函数等
3. 适用
   * 需要使用上层**this**的地方





## 改变this

**JavaScript** 中还允许指定函数中**this**的指向，有3个方法可以动态指定普通函数中**this**的指向

* **call()**

  使用**call**方法调用函数，同时指定被调用函数中**this**的值

  * **语法**

    ```javascript
    fun.call(thisArg, arg1, arg2, ...)
    ```

    * **thisArg**：在**fun**函数运行时指定的**this**值
    * **arg1，arg2**：传递的其他参数
    * 返回值就是函数的返回值，因为它就是调用函数

* **apply()**

  使用**apply**方法调用函数，同时指定被调用函数中**this**的值

  * **语法**

    ```javascript
    fun.apply(thisArg, [argsArray])
    ```

    * **thisArg**：在**fun**函数运行时指定的**this**值
    * **argsArray**：传递的值，必须包含在**数组**里面
    * 返回值就是函数的返回值，因为它就是调用函数
    * 因此**apply**主要跟数组有关系，比如使用**Math.max()**求数组的最大值

* **bind()**

  * **bind()**方法不会调用函数，但是能改变函数内部**this**指向

  * 语法

    ```javascript
    fun.bind(thisArg, arg1, arg2, ...)
    ```

    * **thisArg**：在**fun**函数运行时指定的**this**值
    * **arg1，arg2**：传递的其他参数
    * 返回由指定的**this**值和初始化参数改造的**原函数拷贝（新函数）**
    * 因此当只想改变**this**指向，并且不想调用这个函数的时候，可以使用**bind**，比如改变定时器内部的**this**指向

### 主要应用场景

* **call**调用函数并且可以传递参数
* **apply**经常跟数组有关系，比如借助于数学对象实现数组最大值最小值
* **bind**不用调用函数，但是还想改变**this**指向，比如改变定时器内部的**this**指向







# 防抖

* **防抖**：单位时间内，频繁触发事件，**只执行最后一次**

实现方式

* **lodash**提供的防抖来处理

  ```html
  <!-- example -->
  <script src="./js/lodash.min.js"></script>
  <script>
      const box = document.querySelector('.box')
      let i = 1
      function mouseMove() {
          box.innerHTML = i ++
      }
      
      box.addEventListener('mousemove', _.debounce(mouseMove, 500))
  </script>
  ```

* **手写**一个防抖函数来处理

  ```javascript
  // example
  
  const box = document.querySelector('.box')
  let i = 1
  function mouseMove() {
      box.innerHTML = i ++
  }
  
  function debounce(fn, t) {
      let timer
      // return 返回一个匿名函数
      return function() {
          if(timer) clearTimeout(timer)
          timer = setTimeout(function(){
              fn() // 加小括号调用 fn 函数
          }, t)
      }
  }
  
  box.addEventListener('mousemove', debounce(mouseMove, 500))
  ```







# 节流 - throttle

* **节流**：单位时间内，频繁触发事件，**只执行一次**

**实现方式**

1. **lodash**提供的**节流函数**来处理

   ```javascript
   _.throttle(func, [wait=0], [options=])
   ```

   ```javascript
   // example
   
   // 利用lodash库实现节流 - 500毫秒之后采用 +1
   box.addEventListener('mousemove', _.throttle(mouseMove, 500))
   ```

2. **手写**一个节流函数来处理

   ```javascript
   // example
   
   function throttle(fn, t) {
       let timer = null
       return function() {
           if(!timer) {
               timer = setTimeout(function() {
                   fn()
                   // 清空定时器
                   timer = null
               }, t)
           }
       }
   }
   
   box.addEventListener('mousemove', \ throttle(mouseMove, 500))
   ```

   ## 案例

   两个事件：

   1. **ontimeupdate** 事件在视频/音频（audio/video）当前的播放位置发送改变时触发
   2. **onloadeddata** 事件在当前帧的数据加载完成且还没有足够的数据播放视频/音频（audio/video）的下一帧时触发

   ```javascript
   // example 记录视频播放时间
   
   const video = document.querySelector('video')
   video.ontimeupdate = _.throttle(() => {
       localStorage.setItem('currentTime', video.currentTime)
   }, 1000)
   
   // 打开页面触发事件，就从本地存储里面取出记录的时间，赋值给 video.currentTime
   video.onloaddata = () => {
       video.currentTime = localStorage.getItem('currentTime') || 0
   }
   ```

   
