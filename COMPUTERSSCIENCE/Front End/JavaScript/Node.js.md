基于**Chrome**的**V8引擎**封装，独立执行**JavaScript**代码的环境



# fs模块 - 读写文件

模块：类似插件，封装了**方法/属性**

语法

1. **加载** **fs**模块对象

   ```javascript
   const fs = require('fs')
   ```

2. **写入**文件内容

   ```javascript
   fs.writeFile('文件路径', '写入内容', err => {
       // 写入后的回调函数
   })
   ```

3. **读取**文件内容

   ```javascript
   fs.readFile('文件路径', (err, data) => {
       // 读取后的回调函数
       // data是文件内容的 Buffer 数据流
       
   })
   ```








# path模块 - 路径处理

**Node.js**代码中，相对路径是根据**终端所在路径**来查找的

建议：在**Node.js**代码中，使用**绝对路径**

补充：**__dirname** 内置变量（获取当前模块目录 - 绝对路径）

注意：**path.join()** 会使用特定于平台的分隔符，作为定界符，将所有给定的路径片段连接在一起

语法：

1. 加载**path**模块

   ```javascript
   const path = require('path')
   ```

2. 使用 **path.join** 方法，拼接路径

   ```javascript
   path.join('路径1', '路径2', ...)
   ```







# 案例 - 压缩前端 HTML

步骤

1. **读取**源HTML文件内容
2. 正则**替换**字符串
3. **写入**到新的HTML文件中

```javascript
// example

const fs = require('fs')
const path = require('path')
fs.readFile(path.join(__dirname, 'public/index.html'), (err, data) => {
    if (err) console.log(err)
    else {
        const htmlStr = data.toString()
        // 正则替换字符串
        const resultStr = htmlStr.replace(/[\r\n]/g, '')
        console.log(resultStr)
        fs.writeFile(path.join(__dirname, 'dist/index.html'), resultStr, err => {
            if (err) console.log(err)
            else console.log('写入成功')
        })
    }
})
```







# URL 中的端口号

**URL**：统一资源定位符，简称网址，用于访问服务器里的资源

**端口号范围**：**0-65535**之间的任意整数







# http模块 - 创建 Web 服务

步骤：

1. 加载**http**模块，创建**Web**服务对象
2. 监听**request**请求事件，设置响应头和响应体
3. 配置**端口号**并**启动** Web服务

```javascript
// example

const http = require('http')
const server = http.createServer()

server.on('request', (req, res) => {
    // 设置响应头：内容类型，普通文本：编码格式为 utf-8
    res.setHeader('Content-Type', 'text/plain;charset=utf-8')
    res.end('welcome to node.js')
})

server.listen(3000, () => {
    console.log('web start')
})
```







# 模块化

概念：项目是由很多个模块文件组成的

好处：提高代码复用性，按需加载，**独立作用域**

使用：需要标准语法**导出**和**导入**进行使用



## CommonJS 标准

使用

1. **导出：module.exports = {}**

   ```javascript
   // example
   module.exports = {
       xxx: xxx,
       xxx: xxx
   }
   ```

2. **导入：require('模块名或路径')**

模块名或路径

* 内置模块：直接写名字
* 自定义模块：写模块文件路径



## ECMAScript标准

### 默认导出和导入

默认标准使用

1. 导出：**export default{}**
2. 导入：**import 变量名 from '模块名或路径'**

**注意**：**Node.js**默认支持**CommonJS**标准语法

如需使用 **ECMAScript**标准语法，在运行模块所在文件夹新建**package.json**文件，并配置**{"type": "module"}**

### 命名导出和导入

命名标准使用

1. 导出：**export**修饰定义语句
2. 导入：**import** **{同名变量}** **from** **'模块名或路径'**







# 包

包：将**模块**，**代码**，**其他资料**聚合成一个文件夹

包分类

* 项目包：主要用于编写项目和业务逻辑
* 软件包：**封装工具和方法**进行使用

要求：根目录中，必须有**package.json**文件（记录包的清单信息）

```json
{
    "name": "simple_utils",	软件包名称
    "version": "1.0.0",	软件包当前版本
    "description": "一个简单JS工具包",	软件包简短描述
    "main": "index.js",	软件包入口点
    "author": "hello",	软件包作者
    "license": "MIT",	软件包许可证
}
```







# npm

**npm** 是 **Node.js** 标准的软件包管理器

使用：

1. 初始化清单文件：**npm init -y**（得到**package.json**文件）
2. 下载软件包：**npm i 软件包名称**
3. 使用软件包
4. 删除软件包 **npm uni 软件包名称**



## 全局软件包 nodemon

**本地软件包**：**当前项目**内使用，封装**属性和方法**，存在于**node_modules**

**全局软件包**：**本机**所有项目使用，封装**命令和工具**，存在于系统设置的位置

**nodemon**作用：替代**node**命令，检测代码更改，自动重启程序

**使用**

1. 安装：**npm i nodemon -g**（**-g** 代表安装到全局环境中）

2. 运行：**nodemon** 待执行的目标 **js** 文件







# Webpack

**Webpack**是一个用于现代**JavaScript**应用程序的静态模块打包工具

**打包**：把静态内容，压缩，整合，转译等（前端工程化）

* 把 **less / sass** 转成 **css** 代码
* 把**ES6+** 降级成 **ES5**
* 支持多种模块标准语法

## 使用 Webpack

打包步骤

1. 新建并初始化项目，编写业务**源代码**

2. 下载**webpack webpack-cli** 到当前项目中（版本独立），并**配置**局部自定义命令

   ```shell
   npm i webpack webpack-cli --save-dev
   ```

   ```json
   "scripts": {
       "build": "webpack"
   }
   ```

3. 运行**打包**命令，自动产生 **dist** 分发文件夹（压缩和优化后，用于最终运行的代码）

   ```shell
   npm run build
   ```



## 修改 Webpack 打包入口和出口

**webpack 配置**：影响 **Webpack** 打包过程和结果

步骤：

1. 项目更目录，新建 **webpack.config.js** 配置文件
2. 导出**配置对象**，配置入口，出口文件的路径
3. 重新打包观察

```javascript
// example

const path = require('path')

module.exports = {
    entry: path.resolve(__dirname, 'src/login/index.js'),
    output: {
    	path: path.resolve(__dirname, 'dist'),
        filename: './login/index.js',
		clean: true // 生成打包后内容之前，清空输出目录
	}
}
```



## 自动生成 HTML 文件

安装

```shell
npm install --save-dev html-webpack-plugin
```

基本用法

```javascript
// example

const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

module.exports = {
    entry: path.resolve(__dirname, 'src/login/index.js'),
    output: {
    	path: path.resolve(__dirname, 'dist'),
        filename: './login/index.js',
		clean: true // 生成打包后内容之前，清空输出目录
	},
    plugins: [new HtmlWebpackPlugin()]
}
```

步骤

1. 下载 **html-webpack-plugin** 本地软件包
2. **配置** **webpack.config.js** 让 **Webpack** 拥有插件功能
3. 重新**打包**观察效果



## 打包 css 代码

**加载器** **css-loader**：解析**css**代码

**加载器** **style-loader**：把解析后的**css**代码插入到**DOM**

步骤

1. 准备 **css** 文件**代码**引入到 **src/login/index.js** 中（压缩转译处理等）
2. **下载** **css-loader** 和 **style-loader** 本地软件包
3. **配置** **webpack.config.js** 让 **Webpack** 拥有该加载器功能
4. 打包后观察效果



## 提取 css 代码

**插件 mini-css-extract-plugin**：提取 css 代码

步骤

1. **下载** **mini-css-extract-plugin** 本地软件包
2. **配置** **webpack.config,js** 让 **Webpack** 拥有该插件功能
3. 打包后观察效果

```shel
npm install --save-dev mini-css-extract-plugin
```

**注意**：不能和**style-loader**一起使用

example：

**style.css**

```javascript
body {
    background: green;
}
```

**component.js**

```javascript
import "./style.css";
```

**webpack.config.js**

```javascript
const MinCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
    plugins: [new MiniCssExtractPlugin()],
    module: {
        rules: [
            {
                test: /\.css$/i,
                use: [MiniCssExtractPlugin.loader, "css-loader"],
            },
        ],
    },
    plugins: [
        // ...
        new MiniCssExtractPlugin()
    ]
};
```



## 压缩过程

**解决**：使用 **css-minimizer-webpack-plugin** 插件

步骤

1. **下载** **css-minimizer-webpack-plugin** 本地软件包

   ```javascript
   npm install css-minimizer-webpack-plugin --save-dev
   ```

2. **配置** **webpack.config.js** 让 **webpack** 拥有该功能

   ```javascript
   // example
   
   const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
   
   module.exports = {
       // ...
       // 优化
       optimization: {
           // 最小化
           minimizer: [
               // 在 webpack@5 中，可以使用 `...` 语法来扩展现有的 minimizer （即
               // `terser-webpack-plugin`）， 将下一行取消注释（保证 JS 代码还能被压缩处理）
               `...`,
               new CssMinimizerPlugin(),
           ],
       }
   };
   ```

3. 打包重新观察



## 打包 less 代码

**加载器 less-loader**：把 **less** 代码编译为 **css** 代码

步骤：

1. 新建 **less** 代码（设置背景图）并引入到 **src/login/index.js** 中

2. 安装 **less** 和 **less-loader**

   ```shell
   npm install less less-loader --save-dev
   ```

3. 配置 **webpack.config.js** 让 **Webpack** 拥有功能

   ```javascript
   // ...
   
   module.exports = {
       module: {
           rules: [
               // ...
               {
                   test: /\.less$/i,
                   use: [MiniCssExtractPlugin.loader, "css-loader", "less-loader"]
               }
           ],
       },
   };
   ```



**注意**：**less-loader** 需要配合 **less** 软件包使用



## 打包图片

**资源模块**：**Webpack5** 内置资源模块（字体，图片等）打包，无需额外 **loader**

步骤

1. 配置 **webpack.config.js** 让 **Webpack** 拥有打包图片功能

   ```javascript
   // example
   
   // ...
   module.exports = {
       // ...
       module: {
           rules: [
               // ...
               {
                   test: /\.(png|jpg|jpeg|gif)$/i,
                   type: 'asset',
                   generator: {
                       filename: 'assets/[hash][ext][query]'
                   }
               }
           ]
       }
   }
   ```

   * 占位符 **[hash]** 对模块内容做算法计算，得到映射的数字字母组合的字符串
   * 占位符 **[ext]** 使用当前模块原来的占位符，例如：**.png / .jpg** 等字符串
   * 占位符 **[query]** 保留引入文件时代码中查询参数（只有 **URL** 下生效

**注意**：判断临界值默认为 **8KB**

* 大于 **8KB** 文件：发送一个单独的文件并导出 **URL** 地址
* 小于 **8KB** 文件：导出一个 **data URI** （**base64**字符串）





## 搭建开发环境

**开发环境**：配置 **webpack-dev-server** 快速开发应用程序

作用：启动 Web 服务，**自动**检测代码变化，**热更新**到网页

**注意**：**dist** 目录喝打包内容是在内存里（更新快）

步骤

1. 下载 **webpack-dev-server** 软件包到当前项目

   ```shell
   npm i webpack-dev-server --save-dev
   ```

2. 设置模式为**开发模式**，并配置**自定义命令**

   ```javascript
   module.exports = {
       // ...
       mode: 'development'
   }
   ```

   ```javascript
   "scripts": {
       "build": "webpack",
       "dev": "webpack serve --open"
   },
   ```

3. 使用 **npm run dev** 来启动开发服务器，试试热更新效果



## 打包模式

**打包模式**：告知 **Webpack** 使用相应模式的内置优化

分类

| 模式名称 | 模式名字    | 特点                           | 场景     |
| -------- | ----------- | ------------------------------ | -------- |
| 开发模式 | development | 调试代码，实时加载，模块热替换 | 本地开发 |
| 生产模式 | production  | 压缩代码，资源优化，更轻量等   | 打包上线 |

设置

方式1：在 **webpack.config.js** 配置文件设置 **mode** 选项

```javascript
// ...

module.exports = {
    // ...
    mode: 'production'
}
```

方式2：在 **package.json** 命令行设置 **mode** 参数

```json
"scripts": {
    "build": "webpack --mode=production",
    "dev": "webpack serve --mode=development"
}
```

注意：命令行设置的优先级高于配置文件中的，推荐用命令行设置



## 注入环境变量

需求：前端项目中，开发模式下打印语句生效，生产模式下打印语句失效

**解决**：使用 **Webpack** 内置的 **DefinePlugin** 插件

作用：在编译时，将前端代码中匹配的变量名，替换为值或表达式

```javascript
// example

// ...
const webpack = require('webpack')

module.exports = {
    // ...
    plugins: [
        // ...
        new webpack.DefinePlugin({
            // key 是注入到打包后的前端 JS 代码中作为全局变量
            // value 是变量对应的值 （在 corss-env 注入在 node.js 中的环境变量字符串）
            'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
        })
    ]
}
```



## 开发环境调错 - source map

**source map**：可以准确追踪 **error** 和 **warning** 在原始代码的位置

设置：**webpack.config.js** 配置 **devtool**选项

```javascript
// ...

module.exports = {
    // ...
    devtool: 'inline-source-map'
};
```

**inline-source-map** 选项：把源码的位置信息一起打包在**js**文件内

**注意**：**source map** 仅适用与开发环境，不要在生产环境使用（防止被**轻易**查看源码位置）



## 解析别名 alias

**解析别名**：配置模块如何解析，创建**import**引入路径的别名，来确保模块引入变得更简单

**解决**：在 **webpack.config.js** 中配置解析别名 **@** 来代表 **src** 绝对路径

```javascript
// ...

const config = {
    // ...
    resolve: {
        alias: {
            '@': path.resolve(__dirname, 'src')
        }
    }
}
```



## 优化 - CDN使用

**CDN定义**：内容分发网络，指的是一组分布在各个地区的服务器

作用：把静态资源文件/第三方库放在 **CDN** 网络中各个服务器中，供用户就近请求获取

好处：减轻服务器请求压力，就近请求物理延迟低，配套缓存策略

步骤

1. 在 **html** 中引入第三方库的 **CDN地址** 并用模板语法判断

   ```html
   <% if(htmlWebpackPlugin.options.useCdn){ %>
       <link href="https://xxx.xxx.xxx/xxx/xxx.css" rel="stylesheet">
   <% }%>
   ```

2. 配置 **webpack.config.js** 中 **externals** 外部扩展选项（防止某些 **import** 的包被打包

   ```javascript
   // 生产模式下 - 使用
   if (process.env.NODE_ENV === 'production') {
       config.externals = {
           // key: 代码中 import from 后面的模板标识字符串
           // value: 替换在原地的变量名（要和 cdn 暴露在全局的变量名一致）
           'axios': 'axios'
       }
   }
   ```

   ```javascript
   // ...
   const config = {
       // ...
       plugins: [
           new HtmlWebpackPlugin({
               // ...
               // 自定义属性，在 html 模板中 <%=htmlWebpackPlugin.options.useCdn%> 访问使用
               useCdn: process.env.NODE_ENV === 'production'
           })
       ]
   }
   ```
   



## 多页面打包

**单页面**：**单个html**文件，切换 **DOM** 的方式实现不同业务逻辑展示

**多页面**：**多个html**文件，切换页面实现不同业务逻辑展示

步骤：

1. 准备**源码**放入相应位置，并改用模块化语法**导出**
2. 下载 **form-serialize** 包并**导入**到核心代码中使用
3. 配置 **webpack.config.js** **多入口**和**多页面**的设置
4. 重新打包观察效果

```javascript
// ...
const config = {
    entry: {
        '模块名1': path.resolve(__dirname, 'src/入口1.js'),
        '模块名2': path.resolve(__dirname, 'src/入口2.js'),
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: './[name]/index.js'
    }
    plugins: [
    	new HtmlWebpackPlugin({
    		template: './public/页面1.thml', // 模板文件
    		filename: './路径/index.html', // 输出文件
    		chunks: ['模块名1']
		})
        new HtmlWebpackPlugin({
            template: './public/页面2.html', // 模板文件
            filename: './路径/index.html', // 输出文件
            chunks: ['模块名2']
        })
    ]
}
```

