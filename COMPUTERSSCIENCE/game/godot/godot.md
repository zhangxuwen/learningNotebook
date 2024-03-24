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

