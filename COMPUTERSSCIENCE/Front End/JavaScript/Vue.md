**Vue**是一个用于**构建用户界面**的渐进式框架

```html
<!-- example -->
<script src="https://cdn/jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```





# 插值表达式

插值表达式是一种 **Vue** 的模板语法

1. **作用**：利用表达式进行插值，渲染到页面中

   表达式：是可以被求值的代码，**JS**引擎会将其计算出一个结果

2. **语法**：```{{ 表达式 }}```





# Vue 核心特性：响应式

数据改变，视图会自动更新





# Vue 指令

指令：带有 **v-** 前缀的特殊 **标签属性**



## v-html

作用：设置元素的 **innerHTML**

语法：```v-html="表达式"```

```html
<!-- example -->
<body>
    <div id="app">
        <div v-html="msg"></div>
    </div>
    <srcipt src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></srcipt>
    <srcipt>
        const app = new Vue({
        	el: '#app'
        	data: {
        		msg: `
        			<a href="https://www.baidu.com">
                        百度
        			</a>
        		`
        	}
        })
    </srcipt>
</body>
```





## v-show

1. 作用：控制元素显示隐藏

2. 语法：```v-show="表达式"``` 表达式值**true**显示，**false**隐藏
3. 原理：切换**display:none** 控制显示隐藏
4. 场景：频繁切换显示隐藏的场景





## v-if

1. 作用：控制元素显示隐藏（**条件渲染**）
2. 语法：```v-if="表达式"``` 表达式值**true**显示，**false**隐藏
3. 原理：基于**条件判断**，是否**创建**或**移除**元素节点
4. 场景：要么显示，要么隐藏，不频繁切换的场景





## v-else v-else-if

1. 作用：辅助 **v-if** 进行判断渲染
2. 语法：**v-else**       **v-else-if="表达式"**
3. 注意：需要紧挨着 **v-if** 一起使用





## v-on

1. 作用：注册事件=**添加监听**+**提供处理逻辑**

2. 语法

   * **v-on:事件名='内联语句'**

     ```html
     <button v-on:click="count++">按钮</button>
     <!-- 也可以简写成 -->
     <button @click="count++">按钮</button>
     ```

   * **v-on:事件名='methods中的函数名'**

     ```html
     <!-- example -->
     <button @click="fn">-</button>
     <script>
     	const app = new Vue({
             el: '#app',
             data: {
                 count: 100
             },
             methods: {
                 fn() {
                     console.log('提供逻辑代码')
                 }
             }
         })
     </script>
     ```

3. 简写：**@事件名**

   ```html
   <!-- example -->
   <button @click="fn">-</button>
   ```

4. 注意：**methods**函数内的**this**指向**Vue**实例

### 调用传参

```html
<!-- example -->
<button @click="fn(参数1,...)">-</button>
<script>
	const app = new Vue({
        el: '#app',
        data: {
            count: 100
        },
        methods: {
            fn(参数1,...) {
                console.log('提供逻辑代码')
            }
        }
    })
</script>
```





## v-bind

1. 作用：动态的设置**html**的**标签属性**
2. 语法：**v-bind:属性名="表达式"**
3. 注意：简写形式 **:属性名="表达式"**

### 对于样式控制的增强

为了方便开发者进行**样式控制**，**Vue**扩展了**v-bind**的语法，可以针对**class类名**和**style行内样式**进行控制

### 操作class

语法：**class="对象/数组"**

1. **对象** -> 键就是类名，值是布尔值。如果值为**true**，有这个类，否则没有这个类

   ```html
   <div class="box" :class="{ 类名1: 布尔值, 类名2: 布尔值 }"></div>
   ```

   使用场景：一个类名，来回切换

   

2. **数组** -> 数组中所有的类，都会添加到盒子上，本质就是一个**class列表**

   ```html
   <div class="box" :class="[ 类名1, 类名2, 类名3 ]"></div>
   ```

   适用场景：批量添加或删除类

### 操作 style

语法：```style="样式对象"```

```html
<div class="box" :style="{ CSS属性名1: CSS属性值, CSS属性名2: CSS属性值 }"></div>
```

适用场景：某个具体属性的动态设置





## v-for

1. 作用：基于**数据**循环，**多次**渲染整个元素 -> **数组**、对象、数字 ...

2. 遍历数组语法

   ```v-for="(item, index) in 数组"```

   * **item** 每一项，**index**下标
   * 省略 **index**：```v-for="item in 数组"```

### v-for中的key

语法：```key属性="唯一标识"```

作用：给列表项添加的**唯一标识**。便于**Vue**进行表项的**正确排序复用**

**v-for 的默认行为会尝试 原地修改元素（就地复用）**

**注意点**

1. **key**的值只能是**字符串**或**数字类型**
2. **key**的值必须具有**唯一性**
3. 推荐使用**id**作为**key**（唯一），不推荐使用**index**作为**key**（会变化，不对应）

```html
<li v-for="(item, index) in xxx" :key="唯一值"></li>
```





## v-model

1. 作用：给**表单元素**使用，**双向数据绑定** -> 可以快速 **获取或设置** 表单元素内容
   * 数据变化 -> 视图自动更新
   * **视图**变化 -> **数据**自动更新
2. 语法：```v-model='变量'```

### 应用于其它表单元素

常见的表单元素都可以用 **v-model** 绑定关联 -> 快速 **获取** 或 **设置** 表单元素的值

他会根据 **控件类型** 自动选取 **正确的方法** 来更新元素





## 指令修饰符

通过**"."**指明一些指令**后缀**，不同**后缀**封装了不同的处理操作 -> 简化代码

1. 按键修饰符

   **@keyup.enter** -> 键盘回车监听

2. **v-model**修饰符

   **v-model.trim** -> 去除首尾空格

   **v-model.number** -> 转数字

3. 事件修饰符

   **@事件名.stop** -> 阻止冒泡

   **@事件名.prevent** -> 阻止默认行为







# 计算属性

概念：基于**现有的数据**，计算出来的**新属性**。**依赖**的数据变化，**自动**重新计算

语法

1. 声明在 **computed** 配置项中，一个计算属性对应一个函数

   ```javascript
   // example
   
   const app = new Vue({
       // ...
       computed: {
           totalCount() {
               // 基于现有数据，编写求值逻辑
               // ...
           }
       }
   })
   ```

2. 适用起来和普通属性一样使用 ```{{ 计算属性名 }}```



## computed 计算属性 与 methods方法区别

* **computed**计算属性

  作用：封装了一段对于**数据**的处理，求得一个**结果**

  语法

  1. 写在**computed**配置项中
  2. 作为属性，直接使用 --> **this.计算属性** **{{ 计算属性 }}**

  **缓存特性（提升性能）**

  计算属性会对计算出来的**结果缓存**，再次使用直接读取缓存，依赖项变化了，会**自动**重新计算 -> 并**再次缓存**

* **methods** 方法

  作用：给实例提供一个**方法**，调用以处理**业务逻辑**

  语法

  1. 写在**methods**配置项中
  2. 作为方法，需要调用 -> **this.方法名()** **{{ 方法名() }}** **@事件名="方法名"**



## 计算属性完整写法

计算属性默认的简写，只能读取访问，**不能"修改"**

```javascript
computed: {
    
    计算属性名() {
        一段代码逻辑（计算逻辑）
        return 结果
    }
    
}
```

如果要**"修改"** -> 需要写计算属性的**完整写法**

```javascript
computed: {
    计算属性名: {
        get() {
            一段代码逻辑 （计算逻辑）
            return 结果
        },
        // 当计算属性被修改赋值时，执行set修改的值。传递给set方法的形参
        set(修改的值) {
            一段代码逻辑 （修改逻辑）
        }
    }
}
```





# watch 侦听器（监视器）

作用：**监视数据变化**，执行一些 业务逻辑 或 异步操作

语法

1. **简单写法** -> **简单类型数据，直接监视**

   ```javascript
   // example
   
   const app = new Vue({
       el: '#app',
       data: {
           words: ''
       },
       watch: {
           /*
           该方法会在数据变化时调用执行
           newValue新值，oldValue老值（一般不用）
           */
           words(newValue) {
               // 操作
           }
           /*
           	...
           	data: {
           		obj: {
           			words: ''
           		}
           	}
           	...
           	watch: {
           		'obj.words' (newValue) {
           			...
           		}
           	}
           */
       }
   })
   ```

2. 完整写法 -> 添加额外**配置项**

   1. **deep:true** 对复杂类型深度监视
   2. **immediate: true** 初始化立刻执行一次 **handler** 方法

   ```javascript
   // example
   
   // ...
   watch: {
       obj: {
           deep: true, // 深度监视（针对复杂类型）
           immediate: true, // 是否立刻执行一次handler
           handler (newValue) {
               clearTimeout(this.timer)
               this.timer = setTimeout(async () => {
                   const res = await axios({
                       url: 'https://xxx.xxx.xxx/xxx',
                       params: newValue
                   })
                   this.result = res.res.data.data
                   console.log(res.data.data)
               }, 300)
           }
       }
   }
   ```





# 生命周期 和 生命周期的四个阶段

**Vue**生命周期：一个**Vue**实例从**创建**到**销毁**的整个过程

生命周期四个阶段：创建、挂载、更新、销毁

**Vue**生命周期过程中，会**自动运行一些函数**，被称为**【生命周期钩子】** -> 让开发者可以在**【特定阶段】**运行**自己的代码**

```javascript
// example

const app = new Vue({
    el: '#app',
    data: {
        count: 100,
        title: '计数器'
    },
    // 创建阶段（准备数据）
    beforeCreate() {
        console.log('beforeCreate 响应式数据准备好之前')
    },
    created() {
        console.log('created 响应式数据准备好之后')
        // 可以开始发送初始化渲染的请求
    },
    
    // 挂载阶段（渲染模板）
    beforeMount() {
        console.log('beforeMount 模板渲染之前')
    },
    mounted() {
        console.log('mounted 模板渲染之后')
        // 可以操作dom了
    }
    
    // 更新阶段
    beforeUpdate() {
    	console.log('beforeUpdate 数据修改了，视图还没更新')
	},
	updated() {
        console.log('updated 数据修改了，视图已经更新')
    },
    
    // 卸载阶段
    beforeestroy() {
        console.log('beforeestroy 卸载前')
    },
    destroyed() {
        console.log('destroyed 卸载后')
    }
    
})
```







# 工程化开发

开发**Vue**的两种方式

1. 核心包传统开发模式：基于**html / css / js** 文件，直接引入核心包，开发**Vue**
2. **工程化开发模式：基于构建（例如：webpack）的环境中开发 Vue**

## 脚手架 Vue CLI

基本介绍

**Vue CLI**是**Vue**官方提供的一个**全局命令工具**

开以帮助**快速创建**一个开发**Vue**项目的**标准化基础架子**【集成**webpack**配置】

使用步骤

1. 全局安装（一次）：```yarn global add @vue/cli``` 或 ```npm i @vue/cli -g```
2. 查看**Vue**版本：```vue --version```
3. 创建项目架子：```vue create project-name ```(项目名-不能用中文)
4. 启动项目：```yarn serve```或```npm run serve```(找**package.json**)







# 组件化开发

组件化：一个页面可以拆分成**一个个组件**，每个组件有着自己独立的**结构**、**样式**、行为

* 好处：便于**维护**，利于**复用** -> 提升**开发效率**

* 组件分类：普通组件、根组件

## App.vue 文件（单文件组件）的三个组成部分

* 三部分组成
  * **template**：结构（有且只能有一个根元素）
  * **script**：**js**逻辑
  * **style**：样式（可支持**less**，需要装包）
* 让组件支持**less**
  1. **style**标签，**lang="less"** 开启**less**功能
  2. 装包：**yarn add less less-loader**



## 普通组件的注册使用

组件注册的两种方式

1. **局部注册**：只能在注册的组件内使用

   1. 创建**.vue**文件（三个组成部分）
   2. 在使用的组件内导入并注册

2. **全局注册**：所有组件内都能使用

   1. 创建**.vue**文件（三个组成部分）

   2. **main.js**中进行全局注册

      ```javascript
      // example
      
      // 导入需要全局注册的组件
      import ExampleButton from './components/ExampleButton'
      
      // 调用 Vue.component 进行全局注册
      // Vue.component('组件名', 组件对象)
      Vue.component('ExampleButton', ExampleButton)
      ```

      

使用

* 当成**html**标签使用 **<组件名></组件名>**

**注意**

* 组件名规范 -> 大驼峰命名法



## 组件的样式冲突 scoped

默认情况：写在组件中的样式会**全局生效** -> 因此很容易造成多个组件之间的样式冲突问题

1. **全局样式**：默认组件中的样式会作用到全局
2. **局部样式**：可以给组件加上**scoped**属性，**可以让样式只作用于当前组件**

### scoped原理

1. 给当前组件模板的所有元素，都会被添加上一个自定义属性 **data-v-hash** 值

2. 利用**hash**值区分不同组件

3. **css**选择器后面，被自动处理，添加上了属性选择器

   ```css
   div[data-v-xxxxxx]
   ```

最终效果：**必须是当前组件的元素**，才会有这个自定义属性，才会被这个样式作用到







# data 是一个函数

一个组件的**data**选项必须是一个**函数** -> 保证每个组件实例，维护**独立**的一份数据对象

每次创建新的组件实例，都会新执行一次**data**函数，得到一个新对象

```javascript
// example

data () {
    return {
        count: 100
    }
}
```





# 组件通信

组件通信，就是指**组件与组件**之间的**数据传递**

* 组件的数据是**独立**的，无法直接访问其他组件的数据
* 想用其他组件的数据 -> 组件通信

