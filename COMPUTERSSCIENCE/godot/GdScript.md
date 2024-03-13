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

