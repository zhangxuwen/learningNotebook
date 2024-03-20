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

## 组件关系分类

1. 父子关系

   组件通信解决方案：**props** 和 **$emit**

   ```html
   <!-- example -->
   
   <!-- App.vue -->
   <template>
       <div>
           <Son :title="myTitile"></Son>
       </div>
   </template>
   <script>
       import Son from './components/Son.vue'
       export default {
           data () {
               return {
                   myTitle: '父组件'
               }
           },
           components: {
               Son
           }
       }
   </script>
   
   
   
   <!-- Son.vue -->
   <template>
   	<div>
           {{ title }}
       </div>
   </template>
   <script>
       export default {
           props: ['title']
       }
   </script>
   ```

   ### prop

   定义：**组件上**注册的一些**自定义属性**

   作用：向子组件传递数据

   特点

   * 可以传递**任意数量**的**prop**
   * 可以传递**任意类型**的**prop**

   #### props 校验

   作用：为组件的**prop**指定**验证要求**，不符合要求，控制台就会有**错误提示** -> 帮助开发者，快速发现错误

   语法

   1. 类型校验

      ```javascript
      props: {
          校验的属性名: 类型 // Number String Boolean
      }
      ```

   2. 非空校验

   3. 默认值

   4. 自定义校验

   ```vue
   // example
   
   props: {
   	校验的属性名: {
   		type: 类型,	// Number String Boolean ...
   		required: true,	// 是否必填
   		default: 默认值,	// 默认值
   		validator (value) {
   			// 自定义校验逻辑
   			return 是否通过校验
   		}
   	}
   }
   ```

   **prop & data**、单向数据流

   共同点：都可以给组件提供数据

   区别

   * **data**的数据是**自己**的 -> 随便改
   * **prop**的数据是**外部**的 -> 不能直接改，要遵循**单向数据流**

   单向数据流：父级**prop**的数据更新，会向下流动，影响子组件。这个数据流动是单向的

2. 非父子关系

   组件通信解决方案

   1. **provide** 和 **inject**

      作用：**跨层级**共享数据

      * 父组件 **provide** 提供数据

        ```javascript
        export default {
            provide () {
                return {
                    // 普通类型【非响应式】
                    color: this.color,
                    // 复杂类型【响应式】
                    userInfo: this.userInfo,
                }
            }
        }
        ```

      * 子/孙组件 **inject** 取值使用

        ```javascript
        export default {
            inject: ['color', 'userInfo'],
            created () {
                console.log(this.color, this.userInfo)
            }
        }
        ```

   2. **event bus**

      作用：非父子组件之间，进行建议消息传递（复杂场景 -> **Vuex**）

      * 创建一个都能访问到的事件总线（空**Vue**实例） ->  **utils/EventBus.js**

        ```javascript
        import Vue from 'vue'
        const Bus = New Vue()
        export default Bus
        ```

      * A组件（接收方），监听**Bus实例**的事件

        ```javascript
        created () {
            Bus.$on('sendMsg', (msg) => {
                this.msg = msg
            })
        }
        ```

      * B组件（发送方），触发**Bus实例**的事件

        ```javascript
        Bus.$emit('sendMsg', '这是一个消息')
        ```

3. 组件通信通用解决方案

   **Vuex** （适合复杂业务场景）







# v-model 原理

原理：**v-model**本质上是一个**语法糖**。例如应用在输入框上，就是**value**属性和**input**事件的合写

作用：提供数据的双向绑定

1. 数据变，视图跟着变：**value**
2. 视图变，数据跟着变：**@input**

注意：**$event** 用于在模板中，获取事件的形参







# .sync 修饰符

作用：可以实现**子组件**与**父组件数据**的**双向绑定**，简化代码

特点：**prop**属性名，可以**自定义**，非固定为**value**

场景：封装弹框类的基础组件，**visible**属性，**true**显示 **false**隐藏

本质：就是**:属性名**和**@update:属性名**合写







# ref 和 $refs

作用：利用**ref**和**$refs**可以用于**获取dom元素**或**组件实例**

特点：查找范围 -> 当前组件内（更精确稳定）

1. 获取dom

   * 目标标签 - 添加 **ref** 属性

     ```html
     <div ref="charRef">渲染图标的容器</div>
     ```

   * 恰当时机，通过**this.$refs.xxx**，获取目标标签

     ```javascript
     mounted() {
         console.log(this.$refs.charRef)
     }
     ```

2. 获取组件

   * 目标组件 - 添加 **ref** 属性

     ```html
     <BaseForm ref="baseForm"></BaseForm>
     ```

   * 通过 **this.$refs.xxx**，获取目标组件，就可以**调用组件对象里面的方法**

     ```javascript
     this.$refs.baseForm.组件方法()
     ```







# Vue异步更新、$nextTick

**$nextTick**：**等DOM更新后**，才会触发执行此方法里的函数体

语法：**this.$nextTick(函数体)**

```javascript
this.$nextTick(() => {
    this.$refs.inp.focus()
})
```







# 自定义指令

自定义指令：自己定义的指令，可以**封装一些dom操作**，扩展额外功能

* 全局注册 - 语法

  ```javascript
  Vue.directive('指令名', {
      "inserted" (el) {
          // 可以对 el 标签，扩展额外功能
          el.focus()
      }
  })
  ```

* 局部注册 - 语法

  ```javascript
  directives: {
      "指令名": {
          inserted () {
              // 可以对 el 标签，扩展额外功能
              el.focus()
          }
      }
  }
  ```



## 自定义指令 - 指令的值

* 语法：在绑定指令是，可以通过“等号”的形式为指令绑定**具体的参数值**

  ```html
  <div v-color="color"></div>
  ```

* 通过 **binding.value** 可以拿到指令值，指令值修改会 **触发 update 函数**

  ```javascript
  directives: {
      color: {
          inserted (el, binding) {
              el.style.color = binding.value
          },
          update (el, binding) {
              el.style.color = binding.value
          }
      }
  }
  ```





# 插槽

作用：让组件内部的一些 **结构** 支持 **自定义**

插槽基本语法

1. 组件内需要定制的结构部分，改用**\<slot>\</slot>**占位
2. 使用组件时，**\<MyDialog>\</MyDialog>**标签内部，传入结构替换**slot**



## 后备内容（默认值）

通过插槽完成了内容的定制，传什么显示什么，但是如果不传，则是空白

插槽后备内容：封装组件时，可以为预留的`<slot>`插槽提供**后备内容**（默认内容）

* 语法：在**\<slot>**标签内，放置内容，作为默认显示内容

* 效果

  * 外部使用组件时，不传东西，则**slot**会显示后备内容

    ```html
    <MyDialog></MyDialog>
    ```

  * 外部使用组件时，传东西了，则**slot**整体会被换掉

    ```html
    <MyDialog>我是内容</MyDialog>
    ```



## 具名插槽

具名插槽语法

1. 多个**slot**使用**name**属性区分名字

   ```html
   <!-- example -->
   <div class="dialog-header">
       <slot name="head"></slot>
   </div>
   <div class="dialog-content">
       <slot name="content"></slot>
   </div>
   <div class="dialog-footer">
       <slot name="footer"></slot>
   </div>
   ```

2. **template**配合**v-slot: 名字**来分发对应标签

   ```html
   <MyDialog>
       <template v-slot:head>
           大标题
       </template>
       <template v-slot:content>
           内容文本
       </template>
       <template v-slot:footer>
           <button>按钮</button>
       </template>
   </MyDialog>
   ```

3. **v-slot:插槽名**可以简写成**#插槽名**



## 作用域插槽

作用域插槽：定义**slot**插槽的同时，是可以**传值**的。给**插槽**上可以**绑定数据**，将来**使用组件时可以使用**

基本使用步骤

1. 给**slot**标签，以添加属性的方式传值

   ```html
   <slot :id="item.id" msg="测试文本"></slot>
   ```

2. 所有添加的属性，都会被收集到一个对象中

   ```json
   { id: 3, msg: '测试文本'}
   ```

3. 在**template**中，通过`#插槽名="obj"`接收，默认插槽名为**default**

```html
<MyTable :list="list">
	<template #default="obj">
    	<button @click="del(obj.id)">删除</button>
    </template>
</MyTable>
```







# 单页应用程序

* 单页面应用（SPA）：所用功能在 **一个html页面** 上实现

## Vue中的路由

**路径**和**组件**的**映射**关系

## VueRouter

作用：**修改**地址栏路径时，**切换显示**匹配的**组件**

1. 下载：下载**VueRouter**模块到当前工程

   ```shell
   yarn add vue-router@3.6.5
   ```

2. 引入

   ```javascript
   import VueRouter from 'vue-router'
   ```

3. 安装注册

   ```javascript
   Vue.use(VueRouter)
   ```

4. 创建路由对象

   ```javascript
   const router = new VueRouter()
   ```

5. 注入，将路由对象注入到**new Vue**实例中，建立关联

   ```javascript
   new Vue({
       render: h => h(App),
       router
   }).$mount('#app')
   ```

2个核心步骤

1. 创建需要的组件（**views**目录），配置路由规则

   ```javascript
   // example
   
   import Find from './views/Find.vue'
   import My from './views/My.vue'
   import Friend from './views/Friend.vue'
   
   const router = new VueRouter({
       routes: [
           { path: '/find', component: Find },
           { path: '/my', component: My },
           { path: '/friend', component: Friend },
       ]
   })
   ```

2. 配置导航，配置路由出口（路径匹配的组件显示的位置）

   ```html
   <div class="footer_wrap">
       <a href="#/find">发现音乐</a>
       <a href="#/my">我的音乐</a>
       <a href="#/friend">朋友</a>
   </div>
   <div class="top">
       <router-view></router-view>
   </div>
   ```



## 组件存放目录问题（组件分类）

组件分类：**.vue**文件分2类：**页面组件** & **复用组件**

分类开来 **更易维护**

* **src/views** 文件夹
  * **页面组件** - 页面展示 - 配合路由用
* **src/component** 文件夹
  * **复用组件** - 展示数据 -常用于复用







# 路由的封装抽离

* 在**src**文件夹下新建**router**文件通过**index.js**文件

  ```javascript
  // example
  
  import Find from './views/Find.vue'
  import My from './views/My.vue'
  import Friend from './views/Friend.vue'
  
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  Vue.use(VueRouter)
  
  const router = new VueRouter({
      routes: [
          { path: '/find', component: Find },
          { path: '/my', component: My },
          { path: '/friend', component: Friend },
      ]
  })
  
  export default router
  ```

  
