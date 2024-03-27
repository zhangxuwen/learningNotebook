# 变量和数据类型

* 变量是用于存储信息的容器

  ```python
  var x = 123;
  ```

* 数据类型分类

  ```txt
  bool：一个字节，默认为flase
  int：8个字节，默认为0
  float：8个字节，默认为0
  String：默认为null，字符串可以存储一系列字符
  数组
  对象
  null：变量没有被赋值，则默认为null
  ```







# 导出变量

* `@export`关键字使得编辑器可以编辑变量

  ```python
  # 导出一个数字
  @export var a = 1
  # 导出一个节点路径
  @export var b:NodePath
  # 导出一个文件路径
  @export_file var sound_effect_path: String
  # 导出一个文件路径，以txt结尾
  @export_file(".txt") var notes_path: String
  ```







# 条件语句

* **if**语句

* **if...else**语句

* **if...else if...else**语句 

* **match(switch)**语句

  ```gdsprict
  # example
  
  func switch_example():
  	var LocalVar = 7
  	match LocalVar:
  		1:
  			# 执行代码
  		2:
  			# 执行代码
  		_:
  			# 执行代码
  ```







# 静态变量和静态方法

* **const**变量（静态变量）

  ```python
  const EXAMPLE = 1
  ```

* 静态方法

  ```python
  static func Example():
      # 执行代码
  ```





# 对象

```GDsprict
# Inner class，默认继承Object
class Animal:
	extends Object # 如果不指定继承的类，默认基础Object
	const STATIC_VAL = "静态变量"
	# 属性
	var value: int
	
	func _init():
		print("hello world")
		
	static func PASS():
		pass
```







# 内存管理 free

* **godot**中的对象分为两种
  * 引用计数对象，继承于**Reference**，当没有引用时会被自动回收
  * 非引用计数对象，没有继承于**Reference**，能自己手动回收，**free**或**queue_free**
* 在**godot**中，移除一个节点并不会从节点中删除，必须手动调用**free**或**queue_free**







# 引用计数算法

* 对于创建的每一个对象都有一个与之关联的计数器，这个计数器记录着该对象被使用的次数
* 可以立即回收垃圾，因为每个对象在被引用次数为0的时候，是立即就可以知道的
* 没有暂停时间
* 不需要**stop the world**
* 一个致命缺陷是循环引入，例如，**objA**引用了**objB**，**objB**也引用了**objA**。这种情况下，这两个对象不能被回收的
* 可以使用**unreference**去释放引用计数的对象
* 引用计数既保留了性能，也保证了更加高效的性能







# 通过脚本控制节点

```python
// example

extends Node2D

func _ready():
    
    # new一个Sprite节点
    var sprite = Sprite.new()
    
    # 设置图片
    var icon = preload("res://icon.png")
    sprite.set_texture(icon)
    
    # 修改名称
    sprite.name = "mynode"
    
    # 修改坐标居中
    sprite.position.x = 200
    sprite.position.y = 200
    
    # 添加到当前的场景中
    add_child(sprite)
    pass

func _process(delta):
    var sprite = $mynode
    sprite.rotate(0.1)
    pass
```







# 场景树

* **Nodes**（节点）是在**Godot**中创建游戏的基本构建块。当一组节点被添加到树中时，被称为**sence**（场景），树被称为**sence tree**（场景树）







# 帧率

* 帧率**Framerate**，指画面每秒更新多少次

通过代码可以设置，要求**godot**引擎尽量以此帧率运行，但实际帧率还是会有偏差

```python
Engine.max_fps = 120
```

* **delta time**上一帧的间隔

  ```python
  # 匀速移动的优化
  var step = 0.8f * deltaTime
  期中,
  	0.8f 表示每秒移动 0.8 单位
  ```








# process 和 physics_process

* **godot**的**_process**相当于**unity**的**Update**

  内部对代码就会在每一帧之前被执行，就是引擎每渲染一幅的画面之前，都会执行它里面的代码

* **godot**的**_physics_process**相当于**unity**的**FixedUpdate**

  内部代码会在每个物理帧之前被执行

  **godot**的物理模拟时单独进行的，每次进行物理模拟之前都会执行**physics_process**中的代码







# Parent 和 Owner

* **Parent**
  * 一个节点的**Parent**就是场景树上它的父级
* **Owner**
  * 如果不修改默认**Owner**的话，可以把它视为节点所在场景的顶部节点，如果该节点本身就是顶部节点那么它的**Owner**为**null**
* 静态场景结构中默认的**Owner**

* 动态创建的节点的**Owner**是**null**







# 信号 signal

* 信号是用来完成模块或功能之间通信的媒介，其实就是约定了一些方法的回调形式

* 设计模式上叫做观察者设计模式

* 第一种使用方法

  ```python
  # 第一种信号接受方法，通过场景中配置信号的接受方法
  func _on_Button1_pressed():
      print("hello button1")
  ```

* 第二种使用方法

  ```python
  # 第二种信号接受方法，通过代码控制信号的接受，更加的灵活，比较推荐方式
  func _ready():
      $Button2.connect("pressed", self, "onButton2")
      
  func onButton2():
      print("button2 pressed")
  ```



## 自定义信号

* 自定义信号

  ```python
  signal mySignal(a, b)
  ```

* 发送信号

  ```python
  emit_signal("mySignal", 1, 2)
  ```

* 解除绑定信号

  ```python
  disconnect("mySignal", 1, 2)
  ```







# 网络库支持await

语法

* `await + 对象.信号`







# 多线程

```python
# example

extends Button

func _ready():
    self.connect("pressed", self, "onButton2")
    
func onButton2():
    var myThread = Thread.new()
    
    myThread.start(self, "threadTest", null, 0)
    
    print("Create Thread Id: ", myThread.get_id())
    print("Thread Active: ", myThread.is_active())
    
    var waitForThread = myThread.wait_to_finish()
    
    print(waitForThread)
    
    
    
func threadTest():
    return 999
```

