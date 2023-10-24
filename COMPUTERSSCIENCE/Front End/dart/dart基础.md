# 	dart基础



## 变量

变量需要声明

* 所有对象都是继承与object

```dart
void main(){
  // var声明
  var username = "zxw"; // 之后如果赋值整数会编译错误
  print(username);
}
```

通过值来判断变量类型，如果显示声明类型

```dart
Object password = 123;
password = "good"; // 编译可以通过
```

而dynamic相当于没有定义任何类型，能调用任何可能的方法。



## 数据类型

```dart
void main(){

  // num: int and double
  int abc = 110;
  int a = int.parse("119");
  double cde = 111.222;
  double b = double.parse("111.222");

  // String
  String good = 123.toString();
  String job = 123.22522.toStringAsFixed(2);
  print(job);
  // output: 123.23

  // bool
  bool result = 123 > 110;
  print(result);
  // output: true
  print(1 == '1');
  // output: false 强类型语言类型不一样直接判断不进行转换


  // List 数组类型
  List list = [1, 3, 7, 9];
  list.add(1);
  list.addAll([4, 6, 8, 10]);
  print(list);
  // output: [1, 3, 7, 9, 1, 4, 6, 8, 10]
  print(list.length);
  // output: 9
  print(list.first);
  // output: 1
  print(list.last);
  // output: 10
  print(list[2]);
  // output: 7

  // Map 对象类型
  Map obj = {"x":1, "y":2};
  Map obj2 = new Map();
  obj2["x"] = 1;
  obj2["y"] = 2;
  print(obj);
  // output: {x: 1, y: 2}
  print(obj2);
  // output: {x: 1, y: 2}
  print(obj.containsKey("x"));
  // output: true
  // 判断是否包含key
  print(obj.containsValue(1));
  // output: true
  // 判断是否包含value
  obj.remove("y");
  print(obj);
  // output: {x: 1}
  
}
```



## function

```dart


void main(){
  String username = getUserName();
  String userInfo = getPersonInfo(111);
  print(userInfo);
  int age2 = addAge2(119);
  print(age2);
}

String getUserName(){
  return "hello world";
}

String getPersonInfo(int userId) {
  Map userInfoObj = {
    "111": "zhangsan",
    "222": "list"
  };
  return userInfoObj[userId.toString()];
}

int addAge2(int age1, {int age2 = 0}){
  return age1 + age2;
}

/* output:
zhangsan
119
*/
```





## 类与继承

```dart

void main(){
  var p = new Person(21, "zhangsan");
  print(p.age);
  print(p.name);
  p.sayHello();

  var w = new Worker(25, "zxw", 800000);
  w.sayHello();
}

class Person {
  int age;
  String name;
  Person(this.age, this.name);
  void sayHello() {
    print("my name is: " + this.name);
  }
}

class Worker extends Person {
  int salary;
  Worker(int age, String name,  this.salary) : super(age, name);

  @override
  void sayHello() {
    super.sayHello();
    print("my salary is: " + this.salary.toString());
  }
}

/*
output:
21
zhangsan
my name is: zhangsan
my name is: zxw
my salary is: 800000
* */
```





## mixin与抽象类

```dart

void main(){
  var p = new Person(21, "zhangsan");
  p.eat();
  p.sleep();
  p.sayHello();
}

mixin class Eat {
  void eat() {
    print("eat");
  }

  void sayHello() {
    print("say Hello in eat");
  }

}

mixin class Sleep {
  void sleep() {
    print("sleep");
  }

  void sayHello() {
    print("say Hello in eat");
  }

}

abstract class Animal {
  // 抽象类(不能被实例化)
  void have_a_baby(); // 抽象方法
}

class Person extends Animal with Eat, Sleep {
  // 如果本身有同名方法优先本身，如果没有最后覆盖的显示
  int age;
  String name;
  Person(this.age, this.name);
  void sayHello() {
    print("my name is: " + this.name);
  }
  void have_a_baby() {
    print("have a baby");
  }
}

class Worker extends Person {
  int salary;
  Worker(int age, String name,  this.salary) : super(age, name);

  @override
  void sayHello() {
    super.sayHello();
    print("my salary is: " + this.salary.toString());
  }
}

/*
output:
eat
sleep
my name is: zhangsan
* */
```





## 使用库

dart3升级了，需要用的命令是`dart pub get http`

```dart
// pubspec.yaml文件
name: dartStudy
version: 0.0.1

environment:
  sdk: ^3.0.0

dependencies:
  http: ^1.0.0
```



```dart
// pkg/Calec.dart文件
int add(int x, int y) {
  return x + y;
}

class Calec {
  int x;
  int y;
  Calec(this.x, this.y);

  minus() {
    print(this.x - this.y);
  }
}
```



```dart
import 'pkg/Calec.dart';
import 'package:http/http.dart' as http;
import 'dart:math';
// import 'dart:math' deferred as math; 这里延时加载


void main() async {
  int result = add(3, 4);
  print(result);

  var w = new Calec(10, 5);
  w.minus();

  var url = Uri.https('baidu.com', '');
  var response = await http.post(url, body: {'name': 'doodle', 'color': 'blue'});
  print('Response status: ${response.statusCode}');
  print('Response body: ${response.body}');

  print(await http.read(Uri.https('www.baidu.com', '')));

  var r = new Random();
  print(r);
  print(r.nextInt(10));
}
/*
output:
7
5
Response status: 302
Response body: <html>
<head><title>302 Found</title></head>
<body bgcolor="white">
<center><h1>302 Found</h1></center>
<hr><center>bfe/1.0.8.18</center>
</body>
</html>

<html>
<head>
	<script>
		location.replace(location.href.replace("https://","http://"));
	</script>
</head>
<body>
	<noscript><meta http-equiv="refresh" content="0;url=http://www.baidu.com/"></noscript>
</body>
</html>
Instance of '_Random'
9
*/
```





## 异步处理

```dart

void main() async {
  print("say Hello");

  Future.delayed(new Duration(seconds: 5), (){
    print("not hunger 1");
  });
  print("play game");
  // await会等待异步处理完再进行下去, 这时候需要在函数出声明async
  await Future.delayed(new Duration(seconds: 2), (){
    print("not hunger 2");
  });
  Future.wait([
    Future.delayed(new Duration(seconds: 1), (){
      print("001");
    }),
    Future.delayed(new Duration(seconds: 1), (){
      print("002");
    })
  ]).then((List result){
    print("all over");
  });
  print("play game");
}
/*
output:
say Hello
play game
not hunger 2
play game
001
002
all over
not hunger 1
*/
```





# flutter



## 对于国内项目前置

对于国内**会有文件加载失败所导致，因此需要更换 gradle 文件中的镜像路径** 。

**1. 依次展开项目目录：【android】→【build.gradle】文件**

**2. 打开 build.gradle 文件进行编辑，请直接移步到我写注释的地方，从而对照你的文件进行更改。**

```dart
buildscript {
    ext.kotlin_version = '1.7.10'
    repositories {
        // 注释掉这两货
        // google()
        // mavenCentral()
        // 用下面的路径：
        maven { url 'https://maven.aliyun.com/repository/google' }//google
        maven { url 'https://maven.aliyun.com/repository/central' }//central
        maven { url 'https://maven.aliyun.com/repository/public' }//jcenter//public
        maven { url 'https://maven.aliyun.com/repository/gradle-plugin'}//gradle-plugin
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:7.3.0'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}

allprojects {
    repositories {
        // 一样的将这两货注释掉
        // google()
        // mavenCentral()
        // 用下面的路径：
        maven { url 'https://maven.aliyun.com/repository/google' }//google
        maven { url 'https://maven.aliyun.com/repository/central' }//central
        maven { url 'https://maven.aliyun.com/repository/public' }//jcenter//public
        maven { url 'https://maven.aliyun.com/repository/gradle-plugin'}//gradle-plugin
    }
}

rootProject.buildDir = '../build'
subprojects {
    project.buildDir = "${rootProject.buildDir}/${project.name}"
}
subprojects {
    project.evaluationDependsOn(':app')
}

tasks.register("clean", Delete) {
    delete rootProject.buildDir
}

```



## widgets(flutter)



### 文本和字体

```dart
body:Column(
        children: <Widget>[
          Text("hello body hello body hello body hello body hello body hello body hello body",
            textAlign: TextAlign.right,
            maxLines: 2,
            overflow: TextOverflow.ellipsis,
            textScaleFactor: 2,
          ),
          Text("样式测试",
            style: styles,
          ),
          Text.rich(TextSpan(
            children: [
              TextSpan(text: "主页",
                style: TextStyle(
                  fontSize: 30
                )
              ),
              TextSpan(text: "https://www.baidu.com",
                style: TextStyle(
                  color: Colors.green,
                  fontSize: 25
                )
              )
            ]
          ))
        ],
      ),
```



### 按钮

| **Old Widget** | **Old Theme** | **New Widget** | **New Theme**       |
| -------------- | ------------- | -------------- | ------------------- |
| FlatButton     | ButtonTheme   | TextButton     | TextButtonTheme     |
| RaisedButton   | ButtonTheme   | ElevatedButton | ElevatedButtonTheme |
| OutlineButton  | ButtonTheme   | OutlinedButton | OutlinedButtonTheme |

```dart
      body: Column(
        children: <Widget>[
          ElevatedButton(
            child: Text("RaiseButton"),
            onPressed: () => {
              print("RaiseButton pressed")
            },
          ),
          TextButton(
            child: Text("FlatButton"),
            onPressed: () => {
              print("FlatButton pressed")
            },
          ),
          OutlinedButton(
            child: Text("OutlineButton"),
            onPressed: () => {
              print("OutlineButton pressed")
            },
          ),
          TextButton(
            child: Text("FlatButton自定义",
              style: TextStyle(
                color: Colors.red
              ),
            ),
            onPressed: () => {
              print("FlatButton pressed")
            },
          ),
        ],
      ),
```





### 图片和图表

pubspec.yaml配置

```dart
flutter:

  # The following line ensures that the Material Icons font is
  # included with your application, so that you can use the icons in
  # the material Icons class.
  uses-material-design: true

  # To add assets to your application, add an assets section, like this:
  assets:
      - static/pic/1.png
  #   - images/a_dot_ham.jpeg
```



```dart
      body: Column(
        children: <Widget>[
          Image.asset("static/pic/1.png",
            //width: 200,
            fit: BoxFit.fill,
            color: Colors.pink,
            colorBlendMode: BlendMode.darken,
          ),
          Icon(Icons.alarm)
        ],
      ),
```





### 下拉框

* MyDropDownButton

```dart
// 实现
class _MyDropDownButton extends State<MyDropDownButton> {
  List<DropdownMenuItem> getCityList(){
    List<DropdownMenuItem> cityList = [];
    cityList.add(DropdownMenuItem(child: new Text("上海"), value: "shanghai"));
    cityList.add(DropdownMenuItem(child: new Text("北京"), value: "beijing"));
    cityList.add(DropdownMenuItem(child: new Text("广州"), value: "guangzhou"));
    cityList.add(DropdownMenuItem(child: new Text("深圳"), value: "shenzhen"));
    return cityList;
  }

  //存选中的参数
  var selectCity;

  @override
  Widget build(BuildContext context) {
    return new Column(
      children: <Widget>[
        new DropdownButton(
            items: getCityList(),
            hint: new Text("请选择城市"),
            value: selectCity,
            onChanged: (val) {
              setState((){
                this.selectCity = val;
        });
        })
      ],
    );
  }
}
```

* body

```dart
body: new Column(
        children: <Widget>[
          new MyDropDownButton(),
        ],),
```





### 单选框和多选框

`SwitchAndCheckboxComponent`

```dart
class _SwitchAndCheckboxComponent extends State<SwitchAndCheckboxComponent> {

  var _switchVal = false;
  var _checkboxVal = false;

  @override
  Widget build(BuildContext context) {

    return new Column(
      children: <Widget> [
        Switch(
          value: _switchVal,
          onChanged: (val) {
            setState(() {
              this._switchVal = val;
            });
          },
        ),
        Checkbox(
          value: _checkboxVal,
          onChanged: (val) {
            setState(() {
              this._checkboxVal = val!;
            });
          },
        )
      ],
    );
  }
}
```

`body`

```dart
body: new Column(
        children: <Widget>[
          new SwitchAndCheckboxComponent()
        ],
      ),
```





### 输入框

`body1用到的变量`

```dart
class _MyHomePageState extends State<MyHomePage> {

  GlobalKey _fromKey = new GlobalKey();
```

`body1`

```dart
body: Column(
        children: <Widget>[
          TextField(
            autofocus: true,
            decoration: InputDecoration(
              labelText: "用户名",
              hintText: "请输入用户名",
              prefixIcon: Icon(Icons.assignment_ind)
            ),
            keyboardType: TextInputType.number,
            onChanged: (val){
              print(val);
            },
          ),
          TextField(
            autofocus: false,
            decoration: InputDecoration(
              labelText: "密码",
              hintText: "请输入密码",
              prefixIcon: Icon(Icons.remove_red_eye)
            ),
            keyboardType: TextInputType.text,
            obscureText: true,
            controller: _pswController,
          )
        ],
      ),
```

自动验证已弃用，取而代之的是枚举。因此，你应该迁移到新版本。

你需要做的就是将 **autovalidate： true** 替换为 **autovalidateMode： AutovalidateMode.always**



```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // 渲染界面
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'I0KUN'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {

  GlobalKey _fromKey = new GlobalKey();

  TextEditingController _usernameController = new TextEditingController();

  @override
  Widget build(BuildContext context) {
    const styles = TextStyle(
      color: const Color(0xff000020), //Colors.blue,
      fontSize: 30,
      fontFamily: 'yahei',
      decoration: TextDecoration.underline,
      decorationStyle: TextDecorationStyle.dashed
    );


    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Column(
        children: <Widget>[
          Form(
            key: _fromKey,
            autovalidateMode: AutovalidateMode.always,
            child: Column(
              children: <Widget>[
                TextFormField(
                  autofocus: true,
                  controller: _usernameController,
                  decoration: InputDecoration(
                    labelText: "用户名",
                    hintText: "请输入用户名",
                    icon: Icon(Icons.perm_contact_cal)
                  ),
                  validator: (val) {
                    return val!.trim().length > 0 ? null : "请输入用户名";
                  },
                ),
                ElevatedButton(
                  child: Text("提交"),
                  onPressed: () {
                    if((_fromKey.currentState as FormState).validate()){
                      print("提交数据给后台");
                      print(_usernameController.text);
                    }
                  },
                )
              ],
            ),
          )
        ],
      ),
    );
  }
}
```





## 线性布局

```dar
//      body: Row(
//        crossAxisAlignment: CrossAxisAlignment.center,
//        children: <Widget>[
//          Container(
//            width: 200,
//            height: 200,
//            color: Colors.red,
//          ),
//          Container(
//            width: 180,
//            height: 300,
//            color: Colors.blue,
//          )
//        ],
//      ),
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.end, // start, end, center
        children: <Widget>[
          Container(
            width: 200,
            height: 300,
            color: Colors.red,
          ),
          Container(
            width: 300,
            height: 200,
            color: Colors.blue,
          )
        ],
      ),
```





## 弹性布局

`body`

```dart
body: Flex(
        direction: Axis.horizontal, // 横向
        children: <Widget>[
          Expanded(
            flex: 1,
            child: Container(
              height: 300,
              color: Colors.red,
            ),
          ),
          Expanded(
            flex: 1,
            child: Container(
              height: 300,
              color: Colors.blue,
            ),
          ),
          Expanded(
            flex: 2,
            child: Container(
              height: 300,
              color: Colors.green,
            ),
          )
        ],
      ),
```





## 布局定位

```dart
body: SizedBox(
        height: 400,
        width: 400,
        child: Stack(
          alignment: Alignment.center,
          children: <Widget>[
            Positioned(
              top: 30,
              child: Container(
                width: 100,
                height: 100,
                color: Colors.red,
              ),
            ),
            Positioned(
              right: 100,
              top: 100,
              child: Container(
                width: 200,
                height: 200,
                color: Colors.blue,
              ),
            )
          ],
        ),
      ),
```





## 流式布局

```dart
body: Wrap(
        spacing: 25, // 横向间距
        runSpacing: 10, // 垂直方向
        alignment: WrapAlignment.center, // 描述超过部分流动布局方式
        children: <Widget>[
          Container(
            width: 150,
            height: 150,
            color: Colors.black,
          ),
          Container(
            width: 150,
            height: 150,
            color: Colors.red,
          ),
          Container(
            width: 150,
            height: 150,
            color: Colors.blue,
          )
        ],
      ),
```





## 内边距

```dart
body: Container(
        width: 400,
        height: 400,
        color: Colors.red,
        child: Padding(padding: EdgeInsets.all(30), // 也能EdgeInsets.froml.TRB 也能 通过only()单独控制某个方向
          child: Container(
            color: Colors.blue,
          ),
        ),
      ),
```





## 布局限制容器

```dart
//      body: SizedBox(
//        //限制了宽高
//        height: 400,
//        width: 200,
//        child: Container(
//          color: Colors.red,
//        ),
//      ),
      body: ConstrainedBox(
//      constraints: BoxConstraints.expand(), // 填满剩余空间
        constraints: BoxConstraints(
          //自定义规则
          maxWidth: 100,
          minHeight: 200,
          maxHeight: 300
        ),
        child: Container(
          width: 400, // 会被限制，不会生效
          color: Colors.red,
        ),
      ),
```





## 装饰容器

```dart
body: DecoratedBox(decoration: BoxDecoration(
        gradient: LinearGradient(colors: [Colors.blueGrey, Colors.grey]),
        borderRadius: BorderRadius.all(Radius.circular(3.0)),
        boxShadow: [
          BoxShadow(
            color: Colors.black,
            offset: Offset(3, 3),
            blurRadius: 4.0
          )
        ]
      ),
          child: TextButton(onPressed: null, child: Text("test Button"),),
        // 对child进行的修饰
      ),
```





## 变换transform

```dart
      body: Column(
        children: <Widget>[
//          Container(
//            color: Colors.red,
//            child: Transform(transform: new Matrix4.skewY(0.5),
//              child: Container(
//                color: Colors.blue,
//                child: Text("SkewY"),
//              )
//            ),
//          )
//          Container(
//            color: Colors.red,
//            child: Transform.scale(
//                scale: 3, // 内容放大，布局没有放大
//                child: Container(
//                  color: Colors.blue,
//                  child: Text("SkewY"),
//                )
//            ),
//          )
//          Container(
//            width: 300,
//            height: 300,
//            color: Colors.red,
//            child: Transform.rotate(
//                angle: math.pi / 2, // 旋转了
//                child: Container(
//                  color: Colors.blue,
//                  child: Text("SkewY"),
//                )
//            ),
//          )
          Container(
            width: 300,
            height: 300,
            color: Colors.red,
            child: Transform.translate(
                offset: Offset(200, 50), // dx， dy偏移但不影响布局
                child: Container(
                  color: Colors.blue,
                  child: Text("SkewY"),
                )
            ),
          )
        ],
      ),
```





## Container

```dart
      body: Column(
        children: <Widget>[
          Container(
            color: Colors.blue,
            height: 300,
            width: 200,
            alignment: Alignment.topRight, // bottomRight, centenRight ...
            child: Text("C_1"),
          ),
          Container(
            margin: EdgeInsets.all(20), // only(top:20) ...
            color: Colors.red,
            height: 300,
            width: 200,
            alignment: Alignment.topRight, // bottomRight, centenRight ...
            child: Text("C_2"),
          ),
          Container(
            padding: EdgeInsets.all(10),
            color: Colors.yellow,
            child: Text("C_3"),
          )
        ],
      ),
```





## 可滚动widgets

```dar
//      body: Scrollbar(
//        child: SingleChildScrollView(
//          child: Container(
//            height: 3000,
//            color: Colors.blue,
//          ),
//        ),
//      ),

//      body: Column(
//        children: <Widget>[
//          ListTile(title: Text("固定的表头"),),
//          Container(
//            height: 400,
//            child: ListView.builder(
//              itemCount: 50,
//              itemExtent: 50,
//              itemBuilder: (BuildContext context, int index){
//                return Text("列表内容 $index");
//              },
//            )
//          )
//        ],
//      ),
      body: GridView(
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          crossAxisCount: 3,
          childAspectRatio: 1
        ),
        children: <Widget>[
          Text("1"),
          Text("2"),
          Text("3"),
          Text("4"),
          Text("5"),
          Text("6"),
          Text("7"),
          Text("8"),
          Text("9"),
        ],
      ),
```





## 指针事件

```dart
      body: Listener(
        onPointerDown: (e){
          print("down");
        },
        onPointerUp: (e){
          print("up");
        },
        onPointerMove: (e){
          print("move");
        },
        onPointerCancel: (e){
          print("cancel");
        },
        child: Container(
          height: 200,
          width: 200,
          color: Colors.red,
        ),
      ),
```





## http请求

* yaml

```dart
dependencies:
  dio: ^5.2.1+1
```

`body`

```dart

import 'package:dio/dio.dart';
import 'package:flutter/material.dart';

import 'dart:math' as math;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // 渲染界面
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      home: MyPage(),
    );
  }
}




class MyPage extends StatefulWidget {
  const MyPage({super.key});

  @override
  State<MyPage> createState() => _MyPageState();
}

class _MyPageState extends State<MyPage> {
  String _data = "";
  @override
  Widget build(BuildContext context) {

    return Container(
      child: Column(
        children: <Widget>[
          TextButton (
            onPressed: () async{
              Dio dio = new Dio();
              Response response = await dio.get("https://www.baidu.com", queryParameters:
                {"username":"good", "psw":"123456"}) as Response;
              // dio.post("https://www.baidu.com", data: {"username":"good", "psw":"123456"});
              //dio.download("https://www.baidu.com/logo.png", savePath);
              setState(() {
                _data = response.data.toString();
              });
              print("response: $response");
            },
            child: Text("发起HTTP请求")
          ),
          Scrollbar(
            child: Container(
              height: 400,
              child: SingleChildScrollView(
                child: Text(_data),
              ),
            ),
          )
        ],
      ),
    );
  }
}
```





## 页面转跳

```dart
import 'package:flutter/material.dart';
import 'package:dio/dio.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // 渲染界面
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'IKun',
      home: FristPage(),
    );
  }
}


class FristPage extends StatelessWidget {
  const FristPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("第一个页面"),
      ),
      body: Column(
        children: <Widget>[
          TextButton(
            onPressed: () {
              Navigator.push(context, new MaterialPageRoute(builder: (context) => new SecondPage()));
            },
            child: Text("转跳到第二个页面"),
          )
        ],
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  const SecondPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("第二个页面"),
      ),
      body: Column(
        children: <Widget>[
          TextButton(
            onPressed: () {
              Navigator.pop(context);
            },
            child: Text("返回第一个页面"),
          )
        ],
      ),
    );
  }
}

```





## 页面间参数传递

```dart
import 'package:flutter/material.dart';
import 'package:dio/dio.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  static const String _titile = "Flutter Code Simple";

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: _titile,
      home: ListPage(),
    );
  }
}

class ListPage extends StatelessWidget {

  List _data = [
    {"id": 1, "name": "iphone"},
    {"id": 2, "name": "huawei"},
    {"id": 3, "name": "vivo"}
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        height: 400,
        child: Container(
          height: 400,
          child: ListView.builder(
            itemCount: _data.length,
            itemBuilder: (context, index) {
              return new ListTile(
                title: Text(_data[index]["name"]),
                onTap: () async {
                  String result = await Navigator.push(context, new MaterialPageRoute(builder: (context) =>
                    new DetailPage(_data[index]["id"], _data[index]["name"])));
                  print("接收到的返回值" + result);
                },
              );
            }
          ),
        ),
      ),
    );
  }
}

class DetailPage extends StatelessWidget {

  int good_id;
  String good_name;

  DetailPage(this.good_id, this.good_name);

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Column(
        children: <Widget>[
          Text("id: " + this.good_id.toString()),
          Text("name: " + this.good_name),
          TextButton(
            onPressed: (){
              Navigator.pop(context, "这是来自于第二个页面的问候" + this.good_name);
            },
              child: Text("返回"),
          )
        ],
      ),
    );
  }
}
```





