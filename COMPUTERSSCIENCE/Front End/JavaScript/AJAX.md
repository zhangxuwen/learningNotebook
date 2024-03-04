**AJAX** 实现浏览器和服务器之间通信，动态数据交互

# axios 使用

语法

1. 引入axios.js:http://cdn.jsdelivr.net/npm/axios/dist/axios.min.js

2. 使用 **axios** 函数

   * 传入**配置对象**

     ```javascript
     axios({
         url:'目标资源地址'
     }).then(result) => {
         // 对服务器返回的数据做后续处理
     }
     ```

   * 再用**.then**回调函数接受结果，并做后续处理







# URL

全称**统一定位符**，时**因特网**上标准的资源的地址（Address）



## 协议

HTTP协议：超文本传输协议，规定浏览器和服务器之间传输数据的**格式**



## 域名

域名：标记服务器在互联网中**方位**



## 资源路径

资源路径：标记在服务器下的**具体位置**



## URL 查询参数

定义：浏览器提供给服务器的**额外信息**，让服务器返回浏览器想要的数据



## 常用请求方法

请求方法：对服务器**资源**，要执行的**操作**

| 请求方法 | 操作             |
| -------- | ---------------- |
| **GET**  | **获取数据**     |
| **POST** | **提交数据**     |
| PUT      | 修改数据（全部） |
| DELETE   | 删除数据         |
| PATCH    | 修改数据（部分） |







# axios



## axios - 查询参数

语法：使用**axios**提供的**params**选项

注意：**axios**在运行时把参数名和值，会拼接到url**?参数名=值**

```javascript
axios({
    url:'目标资源地址',
    params:{
        参数名:值
    }
}).then(result => {
    // 对服务器返回的数据做后续处理
})
```



## axios - 请求配置

url：请求的**URL**网址

**method**：请求的方法，**GET**可以省略

**data**：提交数据

```javascript
axios({
    url:'目标资源地址',
    method:'请求方法',
    data: {
        参数名:值
    }
}).then((result) => {
    // 对服务器返回的数据做后续处理
})
```



## axios - 错误处理

```javascript
axios({
    // 请求选项
}).then(result => {
    // 处理数据
}).catch(error => {
    // 处理错误
})
```







# HTTP协议

## 请求报文

HTTP协议：规定了浏览器发送及服务器返回内容的**格式**

**请求报文**：浏览器按照**HTTP**协议要求的**格式**，发送给服务器的**内容**

请求报文的组成部分有

1. **请求行**：**请求方法**，**URL**，协议
2. **请求头**：以键值对的格式携带的附加信息
3. 空行：分割请求头、空行之后的是发送给服务器的资源
4. **请求体**：**发送的资源**



## 响应报文

HTTP协议：规定了浏览器发送及服务器返回内容的**格式**

**响应报文**：服务器按照HTTP协议要求的**格式**，返回给浏览器的**内容**

1. **响应行（状态行）**：协议、**HTTP响应状态码**、状态信息
2. **响应头**：以键值对的格式携带的附加信息
3. 空行：分隔响应头、空行之后的时服务器返回的资源
4. **响应体**：**返回的资源**



### HTTP 响应状态码

| 状态码  | 说明           |
| ------- | -------------- |
| 1xx     | 信息           |
| **2xx** | **成功**       |
| 3xx     | 重定向消息     |
| **4xx** | **客户端错误** |
| 5xx     | 服务端错误     |







# 接口文档

**接口**：使用**AJAX**和服务器通讯时，使用的**URL**，**请求方法**，**以及参数**







# form-serialize 使用

```html
<script src="./lib/form-serialize.js"></script>
<script>
    document.querySelector('btn').addEventListener('click', () => {
        const form = document.querySelector('.example-form')
        const data = serialize(form, { hash: true, empty: true })
    })
</script>
```

配置对象

* **hash** 设置获取数据结构
  * **true**：**JS**对象（推荐）一般请求体里提交给服务器
  * **false**：查询字符串
* **empty** 设置是否获取空值
  * **true**：获取空值（推荐）数据结构和标签结构一致
  * **false**：不获取空值







# 图片上传

1. 获取**图片文件**对象

2. 使用**FormData**携带图片文件

   ```javascript
   // example
   
   const fd = new FormData()
   fd.append(参数名, 值)
   ```

3. 提交表单数据到服务器，使用**图片url网址**







# AJAX 原理



## XMLHttpRequest

### 定义

**XMLHttpRequest**（**XHR**）对象用于与服务器交互。通过**XMLHttpRequest**可以在不刷新页面的情况下请求特定**URL**，获取数据。这允许网页在不影响用户操作的情况下，更新页面的局部内容。**XMLHttpRequest**在**AJAX**编程中被大量使用

### 关系

**axios**内部采用**XMLHttpRequest**与服务器交互

### 使用 XMLHttpRequest

**步骤**

1. 创建**XMLHttpRequest**对象
2. 配置**请求方法**和请求**url**地址
3. 监听**loadend**事件，接受**响应结果**
4. 发起请求

```javascript
// example

const xhr = new XMLHttpRequest()
xhr.open('请求方法', '请求url网址')
xhr.addEventListener('loadend', () => {
    // 响应结果
    console.log(xhr.response)
    //  const data = JSON.parse(xhr.response)
    //console.log(data)
})
xhr.send()
```



## 查询参数

定义：浏览器提供给服务器的**额外信息**

```javascript
// example

const xhr = new XMLHttpRequest()
xhr.open('GET', 'http://xxx.xxx.xxx/city?pname=xxx')
xhr.addEventListener('xxx', () => {
    console.log(xhr.response)
})
xhr.send()
```

```javascript
// example

// 创建 URLSearchParams 对象
const paramsObj = new URLSearchParams({
    参数名1: 值1,
    参数名2: 值2
})

// 生成指定格式查询参数 字符串
const queryString = paramsObj.toString()
// 结果：参数名1=值1&参数名2=值2
```



## 数据提交

```javascript
// example

const xhr = new XMLHttpRequest()
xhr.open('请求方法', '请求url网址')
xhr.addEventListener('loadend', () => {
    console.log(xhr.response)
})

// 告诉服务器，传递的内容类型，是JSON字符串
xhr.setRequestHeader('Content-Type', 'application/json')
// 准备数据并转成JSON字符串
const user = { username: 'xxx', password: 'xxxxxx'}
const userStr = JSON.stringify(user)
// 发送请求体数据
xhr.send(userStr)
```







# Promise

**Promise**对象用于表示一个异步操作的最终完成（或失败）以及结果值

```javascript
// example

// 创建Promise对象
const p = new Promise((resolve, reject) => {
    // 执行异步任务 - 并传递结果
    // 成功调用：resolve（值）触发then()执行
    // 失败调用：reject(值)
})
// 接受结果
p.then(result => {
    // 成功
}).catch(error => {
    // 失败
})
```

**好处**

1. 逻辑更清晰
2. 了解**axios**函数内部运作机制
3. 能解决回调函数地狱问题



## 三种状态

概念：一个**Promise**对象，必然处于以下几种状态之一

* **待定（pending）**：初始状态，既没有被兑现，也没有被拒绝
* **已兑现（fulfilled）**：意味着，操作成功完成
* **已拒绝（reject）**：意味着，操作失败

**注意**：**Promise**对象一旦被**兑现**/**拒绝**就是**已敲定**了，状态无法再被改变







# 同步代码和异步代码

同步代码：逐行执行，需**原地等待结果**后，才继续向下执行

异步代码：调用后**耗时**，不阻塞代码继续执行（不必原地等待），在将来完成后触发一个**回调函数**



## 回调函数地狱

概念：在回调函数中**嵌套回调函数**，一直嵌套下去就形成了回调函数地狱

**缺点**：可读性差，异常无法捕获，耦合性严重



## 链式调用

**Promise - 链式调用**

概念：依靠**then()**方法会返回一个**新生成的 Promise 对象**特性，继续串联下一环任务，直到结束

细节：**then()**回调函数中的**返回值**，会影响新生成的 **Promise** 对象**最终状态和结果**

```javascript
// example

const p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resoleve('XX市')
    }, 2000)
})

const p2 = p.then(result => {
    console.log(result)
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(result + '--- XX')
        }, 2000)
    })
})

...
```







# async函数和await

**async**函数是使用**async**关键字声明的函数。**async**和**await**关键字使得可以用一种更简洁的方式写出基于**Promise**的异步行为，而无需刻意地链式调用**promise**

**注意**：**await**必须用在**async**修饰的函数内（**await**会阻止“异步函数内”代码继续执行，原地等待结果）



## 捕获错误

使用：**try...catch**

**try...catch**语句标记要尝试的语句块，并指定一个出现异常时抛出的响应

```javascript
try {
    // 要执行的代码
} catch (error) {
    // error接收的是，错误信息
    // try里代码，如果有错误，直接进入这里执行
}
```







# 事件循环（EventLoop）

**JavaScript**有一个基于**事件循环**的并发模型，事件循环负责执行代码、收集和处理事件以及队列中的子任务

**定义**执行代码和收集异步任务的模型，在调用栈空闲，反复调用任务队列里回调函数的执行机制，就叫事件循环







# 宏任务与微任务

**ES68**之后引入了**Promise**对象，让**JS**引擎也可以发起异步任务

异步任务分为：

* **宏任务**：由**浏览器**环境执行的异步代码
* **微任务**：由**JS引擎**环境执行的异步代码

**Promise**本身是同步的，而**then**和**catch** **回调函数** 是异步的

| 任务（代码）                     | 执行所在环境 |
| -------------------------------- | ------------ |
| **JS**脚本执行事件（**script**） | 浏览器       |
| **setTimeout** / **setInterval** | 浏览器       |
| **AJAX**请求完成事件             | 浏览器       |
| 用户交互事件等                   | 浏览器       |
| **Promise**对象.**then()**       | **JS**引擎   |







# Promise.all 静态方法

概念：合并多个**Promise**对象，等待所有**同时成功**完成（或某一个失败），做后续逻辑

**语法**

```javascript
const p = Promise.all([Promise对象, Promise对象, ...])
p.then(result => {
	// result结果：【Promise对象成果结果，Promise成功结果，...】
}).then({
    // 第一个失败的Promise对象，抛出的异常
})
```

