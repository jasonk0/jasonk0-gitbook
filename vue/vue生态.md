# vue生态

[TOC]



## Vuex

vuex主要是为了兄弟组件之间的沟通，虽然有`$root` `$parrent` `$refs`等属性可以方便的获取父组件的数据，但是兄弟组件之间仍然是很难沟通到，而且耦合度很高。



vuex使用「单一状态树」

- `state`：存储响应式数据

- `getter`：和`computed`类似在发生变化后去返回处理过后的值，

> 是`store`的计算属性

- `mutation`：类似于事件，他们里的方法都有回调，通过`commit`触发时间，`mutate`去通知`state`的数据。

> 是修改`state`里数据的唯一方法，必须是同步的，由`commit`触发

- `action`：在`commit`时，通常是同步的。所有我们有`action `提供异步操作。通过`dispatch`来触发事件，然后再通过`commit`去通知`mutation`

> 由`dispatch`触发，可以包含任意的异步操作，不直接变更状态，而是通过通知`mutation`来改变状态

![vuex](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/vuex的属性及用法.png)



- module

其中子模块的getters，actions是可以获取父模块的getters和actions

但是mutations是不可以获取父模块的mutiations。

所以需要通过actions添加一定参数后，才能去commit通知到父模块的mutations



```javascript
const store= new Vuex.store({
	a: moduleA,
	b: moduleB
})
const moduleA = {
  namespaced: true, //命名空间
  state:{},
  mutations:{},
  getters:{}
}
```

## Vue-Router

### 路由原理

> 路由就是通过互联网把信息从原地址传输到目的地址的活动

![vue-router](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/VueRouter的原理.png)

Mode

window上提供onhashchange和onpopstate两种方法来监听hietory栈的变化

**vue-router中默认是使用HashMode**

#### h5

HTML5 Mode

监听popstate，当history改变的时候触发事件

依赖 HTML5 History API 和服务器配置

#### Abstract

支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式.

#### hash

Hash Mode

使用 URL hash 值来作路由。支持所有浏览器

`https://www.baidu.com#/hashtag`

#后边即为hash值

### 导航守卫

就是一系列钩子函数，可以决定是否跳转。

全局守卫

- beforeEach 

- beforeResolve 

- afterEach：其实没有守卫功能，纯钩子。

路由独享守卫

- beforeEnter

组件内守卫

- beforeRouteEnter

- beforeRouteUpdate：重用组件

- beforeRouteLeave 



## 实践

### keepalive

其底层实现是依赖LRU算法，比对接下来的Virtual DOM和缓存中的Virtual DOM是否一致，一致的话就直接沿用，并更新其为最近使用。不一致的话就创建Virtual DOM并加入缓存；最后验证缓存是否超过规定长度，超过了去销毁最久没有使用的DOM缓存。

### 无限列表&长列表优化

![无限列表](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/无限列表.png)

Show-Container：浏览器展示的内容

Later-Container：浏览器窗口上方划过去的内容

Pre-Container：浏览器窗口下方预加载的内容

「无限列表」：就是通过每个item的高度，算出浏览器可以展示的item的数量，加上划过去的也就是Later-Container中item的数量（这个是自己定的），再加上预加载的Pre-Container中item的数量；这个总数就是当前浏览器需要加载的item。

在发生滑动时，我们去动态的计算item的起始位置，因为item总数是一定的；假设拿过来的数据itemAll有100个，item的加载数量为9，那么第一屏肯定是itemAll.slice(0，9)也就是前九个；随着滑动，第n屏时，我们可能已经加载过三四十个item了，此时的item应该是itemAll.slice(31，40)。

这样既保证了滚动条不会变的特别短，也保证了往上回顾时能找到之前用户看到的信息。而且用户不会加载到重复的信息，还能无限下拉。

## SSR

ServerSideRender

1. 用户请求网页

2. 服务端创建包含数据的HTML并返回给客户端

3. 客户端浏览器能够快速地极细和渲染HTML，但此时页面不能够富交互
4. 浏览器下载Javascript资源
5. 此时用户可以对符合html标准的标签进行操作，如：滚动，a连接
6. 浏览器开始执行JavaScript
7. 用户开始可以随意地操作页面

CSR

1. 用户请求网页
2. 由CDN返回已创建好的静态HTML文件给客户端
3. 客户端浏览器接收到HTML文件，开始请求相应的CSS和JavaScript
4. 浏览器下载JavaScript和CSS文件
5. 浏览器执行JavaScript，开始调用接口，请求数据，页面出于loading
6. 由服务器restful返回数据
7. 浏览器获取数据开始渲染页面，接收后，loading取消，用户可以交互

SSR的特点

- 更好的SEO：由于搜索引擎爬虫抓取可以直接查看完全渲染的页面
- 更快的内容到达时间：特别是对于缓慢的网络情况或运行缓慢的设备
- 每次页面跳转都需要重新加载

CSR的特点

- 随着单页面应用流行而流行，比较合适不强调SEO的中后台富交互应用
- 首次页面加载要等到资源都加载执行完，用户才可以进行操作
- 单页面应用跳转无刷新，用户体验丝滑

同构

1. 用户请求网页

2. 服务端创建包含数据的HTML并返回给客户端

3. 客户端浏览器能够快速地极细和渲染HTML，但此时页面不能够富交互
4. 浏览器下载Javascript资源
5. 浏览器水合事件，由浏览器执行JavaScript来控制页面、请求以及数据
6. 从服务器获取非首屏，优先基低的异步接口数据
7. 用户可以体验丝滑网站  

何时使用SSR？

![SSR的时机](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/SSR的使用时机.png)

### 同构实践

通用代码的一些原则：

- 数据响应： 在纯客户端应用程序（client-only app）中，每个用户会在他们各自的浏览器中使用新的应用程序实例。将数据进行响应式的过程在服务器上是多余的，所以默认情况下禁用。还可以避免将数据转化为响应式对象的性能开销。

- 生命周期钩子函数：只有`beforeCreate`和`created`会在服务器端渲染（SSR）中被调用。要避免`beforeCreate`和`created`时产生全局副作用的代码，例如在其中使用`setInterval`
- 访问特定平台：禁止使用`window`或`document`，以及共享于服务器和客户端，但用于不同平台API的任务（task）。建议使用能够兼容二者的三方库，例如axios
- 自定义指令：推荐使用组件作为抽象机制，并运行在「虚拟DOM」层级。如果自定义指令，不容易替换为组件，则可以在创建服务器对象`renderer`时，使用`directives`提供“服务端版本”。

结构图：

![结构图](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/SSR同构.png)

## Nuxt.js

- 是一个基于vuejs的通用应用框架

- 预设了利用vuejs开发服务端渲染的应用所需要的各种配置

- 提供了对基于vuejs的应用生成对应静态站点的功能

- 提供了强大的模块系统，可以便捷地使用REST或GraphQL接口，PWA以及AMP支持等都是作为模块来提供支持。

响应流程图

![nuxt](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/nuxy响应流程图.png)

页面请求

如果没有缓存 初始化创建实例

通过中间件处理

进入页面hook

async获取数据

最后是渲染

用 户可以通过`<nuxt-link>`进入新的页面
