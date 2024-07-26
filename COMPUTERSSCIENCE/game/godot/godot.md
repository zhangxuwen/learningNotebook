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
    print("Thread Active: ", myThread.is_active()) # 4.0版本后 active改成了alive
    
    var waitForThread = myThread.wait_to_finish()
    
    print(waitForThread)
    
    
    
func threadTest():
    return 999
```







# 向量的长度

**Vector2**：长度=**sqrt(x$^2$+y$^2$)**

**Vector3**：长度=**sqrt(x$^2$+y$^2$+z$^2$)**

直接使用**API**求长度

**float len = v.length();**

 ## 向量算术：乘法

乘法分为3种：

* 标量乘法 **b=a\*2**
* 点积 **c=a.dot(b)**
* 差积 **c-a.cross(b)**

 ## 移动脚本例子

```python
extends Sprite2D

var speed = 400

var screen_size

# Called when the node enters the scene tree for the first time.
func _ready():
	screen_size = get_viewport_rect().size


# Called every frame. 'delta' is the elapsed time since the previous frame.
func _process(delta):
	var velocity = Vector2.ZERO
	if Input.is_action_pressed("ui_right"):
		velocity.x += 1
	if  Input.is_action_pressed("ui_left"):
		velocity.x -= 1
	if Input.is_action_pressed("ui_down"):
		velocity.y += 1
	if Input.is_action_pressed("ui_up"):
		velocity.y -= 1
	if velocity.length() <= 0:
		return
	
	velocity = velocity.normalized() * speed
	position += velocity * delta
	position.x = clamp(position.x, 0, screen_size.x)
	position.y = clamp(position.y, 0, screen_size.y)
```









# 垂直同步

* ``Vsync``，垂直同步又称场同步（``Vertical Hold``）

当选择"等待垂直同步信号"是，显卡绘制3D图形前会等待垂直同步信号

当打开垂直同步时，游戏的**FPS**要受刷新率的制约，对于高端显卡而言，限制了其性能的发挥

如果取消了垂直同步信号，固然能够换来更快的速度，但是在图像的连续性上势必打折扣

* **CRT**显示器学名为"阴极射线显像管"，是一种使用阴极射线管(**Cathode Ray Tube**)的显示器

  屏幕上的图形图像是由一个个电子束击打屏幕而发光的荧光点组成

  从左到右，从上到下

* 液晶显示器借助于薄膜晶体管驱动的有源矩阵液晶显示器，他主要以电流刺激液晶分子产生点、线、面配合背部灯管构成画面

  在电场的作用下，利用液晶分子的排列方向发送变化，是外光源透光率改变（调制），完成电-光变换







# 刚体

* 刚体是组成物理世界的基本对象
* mass，钢体质量
* weight，刚体加速度
* linear velocity，移动速度
* angular velocity，旋转速度
* applied forces，施加的力
* torque，扭矩
* damp，衰减系数，值越大物体移动的越慢，可以用来模拟空气摩擦力等效果

## 刚体实战

* RigidBody，动态刚体，有质量，可以设置速度，会受到重力影响

* Kinematic，运动刚体，零质量，可以设置速度，不会收到重力的影响，但是可以设置速度来进行移动

* Static，静态刚体，零质量，零速度，既不会受到重力或速度影响，但是可以设置他的位置来进行移动

* Area2D，一块区域，能够检测到物体的碰撞，但是不会做任何操作

* 碰撞检测Layer和Mask，Layer表明当前刚体属于那一层，Mask表面当前物体的遮罩

  * 作用

    **Layer** 和 **Mask**实际上就是 **PhysicsBody** 的分组，可以对碰撞检测进行过滤

    * **Layer** 是当前这个 **PhysicsBody** 的**所属组**，一个 **PhysicsBody** 可以属于 **0至多个** （注意，不属于任何组也是可以的）
    * **Mask**是当前这个 **PhysicsBody** 的**目标组**，即要进行碰撞检测的组，也可以设置**0至多个**

    于是对于A和B两个**PhysicsBody**就会有如下可能

    |                              | 是否发生碰撞 |
    | ---------------------------- | ------------ |
    | A是B的目标，B也是A的目标     | 是           |
    | A是B的目标，B不是A的目标     | 是           |
    | A不是B的目标，B是A的目标     | 是           |
    | A不是B的目标，B也不是A的目标 | 否           |








# Line2D 节点

* 在2D空间中通过几个点连成的线，可以通过代码动态添加和删除

  







# RemoteTransform2D 节点

* RemoteTransform2D，类似与设计模式中的代理模式，代理一个节点







# Path2D 节点

* **Path2D**，包含了一个曲线路径数据
* **PathFollow2D**，要和**Path2D**结合在一起使用







# Tilemap 节点

* **tilemap**的由**tileset**组成，**tileset**由**tile**一个个单个图块组成
* 基本上通过熟练使用**get_cell**、**set_cell**、**world_to_map**这些函数就可以解决大多数普通关卡制作需求
* 自动填充**bitmask**

## Collision

碰撞体







# 射线检测

## RayCast2D

* **RayCast2D**是**godot**内置的节点，大部分情况下已经能够满足日常的使用
* 使用方式也是比较简单

## PhysicsServer2D

* 发生碰撞时，**result**字典包含以下数据

  ```
  {
  	position: Vector2
  	normal: Vector2
  	collider: Object
  	collider_id: ObjectID
  	rid: RID
  	shape: int
  	metadata: Variant()
  }
  ```








# Navigtaion2D 节点

* **Navigation2D** 节点可以实现自动寻路
* 目前使用最广泛的是**AStar**算法，在**Godot**中，**Navigation2D** 完全的集成了此算法，让使用变得易常简单
* **Collision** 和 **Navigation**，**Collision** 区域将是无法通过的，而**Navigation** 可以，且寻路总是在 **Navigation** 的区域里进行
* 引擎的作用就是为了减少大量复杂的工作，就像 **AStar** 寻路一样，在**Godot**中，可以非常简单的使用







# Canvasltem

画布

* 所有的**2D**节点和**GUI**节点都继承与**Canvasltem**节点
* **Canvasltem**是按树的树的深度优先遍历顺序绘制的
* **draw**指定了要绘制的东西
* 当要**draw**绘制的改变了，需要调用**update**
* **hide**和**show**，隐藏和显示节点
* **Canvasltem** 可以绘制直线，正方形，长方形，圆，图片







# 动画系统

* **godot** 内置了通用的动画系统用以实现基于关键帧的动画

## Timer 节点实现动画

* **Timer**节点，意思是计时器秒表，在**godot**中可以利用他的定时器特性实现动画帧

* 也可以不使用节点，直接使用代码**Timer.new()**动态创建一个计时器

  ```shell
  Timer 
  wait_time：等待时间，即计时时长，结束触发 timeout 信号
  one_shot：是否是一次性，如果是，只会触发一次 timeout 信号
  autostart：自动开始，载入场景后计时，也可以使用 start 方法手动开启
  ```

## Tween 节点实现动画

* 在游戏开发过程中，一般使用**AnimationPlayer**节点来实现移动、缩放、颜色渐变等动画效果

* **Tween**即渐进/过渡的意思，从一种状态在一定时间内变化到另一种状态，从而产生一种视觉动画

* 渐变节点使用非常简单，可以对一个物体的任意属性进行动画控制

  ```shell
  repeat：是否重复
  start()：开始渐变，结束后触发 tween_completed 信号
  interpolate_property()：设置进行动画的节点属性以及时长等
  ```







# AnimationPlayer 节点实现动画

* **AnimationPlayer** 是时间和属性的变化，是一种动画的表现







# AnimatedSprite 节点实现动画

* **AnimatedSprite** 是序列帧的简便的用法







# 光照系统 Light2D

* godot的2D动态光照
* 带法线贴图的**Sprite**和普通的**Sprite**擦汗别主要集中在材质上面







# 法线贴图 NormalMap

* 法线贴图的定义

  法线贴图就是在原物体的凹凸表面的每个点上均作法线，通过RBG颜色通道来标记法线的方向

* 法线贴图软件**laigter**







# 光照和阴影 LightOccluder2D

* 光照是指光的的照射，**godot**中的光照的实现模拟了光对真实世界的影响。在场景中添加光源可以使场景产生相应的光照和阴影效果，获得更好的视觉效果







# 粒子系统

* 粒子系统是游戏引擎特效表现的基础







# GUI 系统

* 图形用户界面（**Graphical User Interface**，简称**GUI**，又称图形用户接口）是指采用图形方式显示的计算机操作用户界面

## GUI 的字体选择

* 由于 **Godot** 使用 **OpenGL** 渲染，所以没法直接使用系统字体，需要创建字体资源

  在 **Godot** 中使用自定义字体的步骤如下

  ```shell
  复制字体文件（ttf/tts等格式）到游戏目录内
  创建字体资源（DynamicFont），并配置对应字体文件
  控件中使用字体资源，配置字体大小，字体颜色
  字体资源服用（Make Unique）
  ```

* **godot**默认不显示中文，需要下载中文的字体

* 思源黑体，免费，开源，https://github.com/adobe-fonts/source-han-sans/releases

## GUI 的锚点 Anchor

* 是一个点，锚点描述的是一个对象的**Margin**，相对于锚点的坐标
* 描点的**left**，**top**，**right**，**bottom**是相对于父节点的值
* 主要是用于描述子节点相对于父节点的位置
* 当对一个节点的子节点进行设置描点时，子节点的描点范围只能够是父节点的控件区域内
* 注意任何布局也都是相对于父窗口矩形的
* 主要用于**GUI**中描述字节点相对于父节点的位置







# Camera2D 相机

* **Godot** 的 **Camera2D** 节点，也就是摄像机，提供了不少可以定义的属性来处理游戏中的镜头，并且非常简单
* 摄像机的作用
* 同一时间，只可能有一个摄像机处于激活状态，自动的，激活一个，其它的都会被置为未激活状态







# Viewport 可视化窗口实现小窗口

* **root** 就是根节点的 **viewport**







# CanvasLayer 节点

* 它是一个节点，为所有子代和孙代添加一个单独的**2D**渲染层
* **Viewport**的子节点默认在图层“0”处绘制，而**CanvasLayer**将在任何数字层处绘制
* 数字较大的图层将绘制在数字较小的图层之上。**CanvasLayers**也有自己的变换，不依赖于其他层的变换
* 这使得当对游戏世界的观察发生变化时，**UI**可以固定在屏幕空间中







# 文件系统

* 资源路径

  ```shell
  访问资源时，使用主机OS文件系统布局可能麻烦并且不可移植，为了解决这个问题，创建了特殊路径 res://
  路径res://将始终指向项目根目录
  仅当从编辑器本地运行项目时，此文件系统才是读写的
  导出或在其他设备上运行时，文件系统将变为只读状态，并且将不再允许写入
  ```
  
* 用户路径

  ```shell
  诸如保存游戏状态或下载内容包之类的任务仍需要写入磁盘
  ```







# AudioStreamPlayer 节点播放声音

* **AudioStreamPlayer** 适合用来播放**BGM**，**AudioStreamPlayer2D**、**AudioStreamPlayer3D**播放时声源带有位置信息，会出现左右声道不一致的问题

* 每个**AudioStreamPlayer**可以将音频绑定到某一个**Audio Bus**中，然后直接控制**Bus**的音量

  ```shell
  AudioServer.set_bus_mute(AudioServer.get_bus_index("Master"), mute)
  ```

* 声频信号流就是一个声音从它的源头到输出的整一个流程

* 声频信号是一组数字信号，以二级制的心事存储在文件中







# HTTP网络请求

HTTPRequest









更多参考

https://docs.godotengine.org/zh-cn/4.x/
