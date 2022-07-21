# VUE

基础用法 => 八皇后问题

高阶用法 => 

响应式源码分析 => 

vue生态 => vuex 、vue-router、nuxt、

[TOC]




## 初识Vue（复习Vue）

### 条件渲染

`v-if`和`v-show`

`v-if`：通过创建和销毁DOM元素。（出现和消失仅一次）

- “真正”的条件渲染，在切换过程中条件块内的事件监听器和自组建会被适当的销毁和重建
- 是「惰性」的，直到条件第一次变为真时，才会开始渲染条件块

`v-show`：通过display（频繁的出现和消失）

- 带有`v-show`的元素始终会被渲染在DOM中，它只是简单的切换CSS中的`display`

### 列表循环

不推荐同一元素上使用v-if和v-for

因为v-for的优先级大于v-if

### ref

给组件加上ref属性

在模版渲染后通过`$refs`去访问组件或元素

### 响应式



### data为什么是函数

因为只有返回一个生产data的函数，这个组件产生的每一个实例才能维持一份被返回对象的独立拷贝



### 计算属性

计算属性是基于其内部的「响应式依赖」进行缓存的

只在「响应式依赖」更新时才会去计算

> 我们可以通过计算属性的缓存保存一些常量

### 侦听器

相比于计算属性，它更关注于依赖的变化过程，比如进行异步操作，或者复杂计算等等。如果这些操作放在计算属性里，可能会造成线程阻塞。

### 数组的响应式

`vue2`内部是通过`Object.defineProperty`来将变量转化为`getter`、`setter`函数，以方便`vue`内部的「观察者」追踪变量的依赖。出于对性能的考量，`vue2`不会对数组元素和对象成员监听。

- 不能检测对象属性的「删除」和「添加」
- 不能检测数组的「长度变化」
- 已创建的对象`vue`是没办法添加「根级别」的「响应式属性」

#### Vue.set()或者this.$set()

都是可以给数组或对象修改「响应式的Object属性」

相应的也有delete删除方法

#### 一些重构的API

将一些数组的方法重构后，使其可以加入响应式依赖

`push()`、`pop()`、`shift()`、`unshift()`、`sort()`、`splice()`、`reverse()`

#### 另辟蹊径

或者可以先手动修改「对象或数组」里的一些「基础类型」属性，使vue进行响应式的跟踪「此数组或此对象」

因为「基础类型」的变量是会被vue加上响应式依赖的，如果你修改它

`arr = [{name:"1"},{name:"2"},{name:"3"},{name:"4"}]`

如果去这样子修改`arr[0] = "555"/{name:"555"}`是没有响应式依赖的，也就是模版里的数据不会变。

但是如果这样子修改`arr[0].name = ""`，那么这个「基础类型」的属性就会被加入响应式依赖，之后再进行这样子的操作`arr[0] = "555"/{name:"555"}`会在模版里修改生效了



### 事件修饰符

其目的在于使methods里的函数只有纯粹的数据逻辑，为了更纯粹的业务流程，减少DOM操作，与DOM事件解藕。



### Scoped

避免样式全局污染，会将选择器、类名、id编译成一个随机哈希值的一个loader



### 生命周期函数

![生命周期的作用](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/vue的生命周期函数作用.png)

### keep-alive

![image-20220212203813012](/Users/jasonk0/Library/Application Support/typora-user-images/image-20220212203813012.png)



### 自定义指令

//:TODO

### 双向绑定

//:TODO

### 插槽

#### 作用域插槽

编译作用域：

父级模版里的所有内容是在父组件编译的

子级模版的所有内容是在自组件编译的

问题：在保证子组件数据的独立性的同时，达到父组件控制子组件的业务变化，也就是说子组件中数据A、B的展示，由父组件控制，如何做到呢？

通过`v-bind:`，也就相当于插槽的`prop`，将A、B提升到父组件中，这样父组件就可以访问并控制A、B。

子组件中：`<slot :A = "A" name="header">{{A.a}}</slot>`

父组件中：`<template #header = "{A}">{{A.b}}</template>`

### 组件通信

#### 组件跨层级访问

`$root`：访问根组件

`$parent`：访问父组件

`$ref`：访问子组件

前二者是在每个生命周期都可以使用，第三者是只在渲染之后`mounted()`才能使用

<img src="https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/vue-跨层级访问.png" alt="跨层级访问" style="zoom:80%;" />

但是这样的访问方式会导致，父子组件产生强耦合，无法分离复用。

#### 依赖注入

为了优化上述的强耦合，可以使用依赖注入的方式解藕为松耦合。

依赖注入：在父/根组件中声明需要注入给子组件的变量；在子组件中声明这些需要用到的被注入的变量。

父/根组件：`provide(){return{person:{name:'xk'}}}`

子组件：`inject:['person']`

我们可以理解为父组件把`prop`依赖放到了组件之外，当前组件使用时去声明注入`prop`依赖。

> 祖先组件不需要知道这些属性会被那些后代使用
>
> 后代组件也不需要知道这些属性是那些祖先提供的

> 但是
>
> 仍然存在耦合现象
>
> 而且这样的属性传递，是没有响应式依赖的

#### 值和事件的透穿

在我们进行组件库的二次封装时，需要将原本的组件的事件和值都传给外面。通常需要子传父，用到`emit`。

其实可以通过`$attrs`和 `$lisentners`进行穿透，直接将事件和值穿透给外边的组件。

`$attrs`：传递props，class，style

 `$lisentners`：传递监听器、修饰符，原生事件

### Mixin

![Mixin](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/Mixin.png)

全局注册mixin，会影响所有的vue实例

1. 同名的钩子函数，mixin里的钩子会先于组件中的钩子
2. 属性methods、components等会被合并成一个，如果属性里的键值对冲突了，取组件中的。

局部混入mixin，相当于提取出了一个方法，放在了组件外的一个js文件中，通过mixin去引入这个方法。

1. 打破了原有组件的封装
2. 增加组件复杂度
3. 可能会出现命名冲突的问题
4. 仅仅只是对逻辑（js）的复用，模版不能复用

### HOC（Higher Order Component）

函数接收一个组件作为参数，然后返回一个新的组件，可复用逻辑在函数中实现。（类似于装饰者模式，将参数组件通过可复用逻辑逻辑装饰为新组件）（高阶组件，类似于高阶函数，被装饰者装饰了）

![HOC](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/HOC.png)

相比于mixin

1. 模版可复用（本质上是HOC是给组件套了一层父组件）
2. 不会出现命名冲突

BUT：

- 组件复杂度高，多层嵌套，调试很痛苦

### Renderless

通过插槽来达到复用包装组件的目的

我们创建一个中间组件，用来加工复用逻辑、模版；通过插槽的方式，接入父组件，并向父组件暴露出复用的方法，这样父组件既有了想要的逻辑、模版，也不会显得非常冗余

1. 模版可复用
2. 避免了命名冲突
3. 符合依赖导致原则
4. 接口的来源清晰



### 插件

## 深入VUE源码

### 响应式源码

「响应式」是一种面向数据流和变化传播的编程范式。这意味着可以在编程语言中很方便地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。

vue的响应式原理

![原理图](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/Vue的响应式原理.png)

vue的响应式原理，将属性通过`defineProperty`转变为具有`getter`和`setter`的数据。通过`getter`收集与此属性相关的依赖，并放到「依赖集」中，当此属性发生变化，`setter`会通过依赖集分别通知相关依赖也去发生变化。

ref

```javascript
let active = null
let Ax = x => 'A'+ x
// 收集依赖的函数
let watch = (callback)=>{
	active = callback
  active()
  active = null
}
// 异步更新队列
let queue = []
let nextTick = callback=>{
  return Promise.resolve().then(callback)
}
// 其实这个函数是被nextTick包装后才使用的
// 异步的去清空这个queue
let flushJob = ()=>{
  let job
  while(queue.length>0){
 		job = queue.shift()
    job && job()
  }
}
let queueJob = queueJob =>{
  if(!queue.includes(queueJob)){
		queue.push(queueJob)
    //nextTick(flushJob)           
    // 如果放在这里就意味着每向queue加入一个依赖函数，
    // 都要再向微任务队列加入一次清空queue的回调，也就是flushJob
    // 我觉得flushJob应该在所有依赖函数被加入队列后，再去清空
    // 所以放在了notify里，将依赖都加入队列后，在进行异步清空操作
  }
}
class Dep{
  constructor(){
    let deps = new Set()
  }
  // 收集依赖，将依赖加入「依赖集」
  depend(){
    if(active) this.deps.add(active)
  }
  // 通知依赖
  notify(){
    // 这些依赖并不直接直接执行
    // 而是先加入异步队列 然后异步执行
		this.deps.forEach((dep)=>queueJob(dep))
    nextTick(flushJob) 
  }
}
// 响应式函数 它将会这样使用 x = ref(1)
let ref = (initValue)=>{
  let _value = initValue
  let _dep = new Dep()
  return Object.defineProperty({},"value",{
    get(){
      _dep.depend()
      return _value
    },
    set(newValue){
      _value = newValue
    	_dep.notify()
  	}
  })
}
let x = ref(1)

watch(()=>{
  let y = Ax(x.value)
  console.log(y)
})

nextTick(()=>{
	console.log('----start----')
})
x.value = 3
x.value = 4
nextTick(()=>{
  console.log('----end----')
})

```

reactive

```javascript
 let { reacitve, watchEffect, ref, computed } = (function () {
    let active = null
    // effect是一个升级版的依赖收集器
    // 副作用收集器，调用这个东西就可以把副作用（依赖）加入集合中。
    let effect = (fn, options = {}) => {
      let effect = (...args) => {
        active = effect
        let _v = fn(...args)
        active = null
        return _v
      }
      effect.options = options
      // effect.deps = []
      return effect
    }
    let watchEffect = (callback) => {
      let runner = effect(callback)
      runner()
    }
    // 任务队列，异步更新队列的包装、清空队列、加入队列
    let queue = []
    let nextTick = callback => {
      return Promise.resolve().then(callback)
    }
    let flushJob = () => {
      let job
      while (queue.length > 0) {
        job = queue.shift()
        job && job()
      }
    }
    let queueJob = queueJob => {
      if (!queue.includes(queueJob)) {
        queue.push(queueJob)
      }
    }
    
    // 依赖类 依赖的存储（Set），收集depend、通知notify
    class Dep {
      constructor() {
        this.deps = new Set()
      }
      depend() {
        if (active) this.deps.add(active)
      }
      notify() {
        this.deps.forEach((dep) => queueJob(dep))
        this.deps.forEach((dep) => {
          dep.options && dep.options.schedulor && dep.options.schedulor()
        })
        nextTick(flushJob)
      }
    }
		// 创建响应式，可以通过defineProperty和Proxy两种方式，
    let createReacitve = (target = {}, prop, initValue) => {
      let _value = initValue
      let _dep = new Dep()
      // defineProperty添加响应式属性
      // return Object.defineProperty(target, prop, {
      //   get() {
      //     _dep.depend()
      //     return _value
      //   },
      //   set(newValue) {
      //     _value = newValue
      //     _dep.notify()
      //   }
      // })
      // proxy添加响应式属性
      return new Proxy(target, {
        get(target, prop) {
          _dep.depend();
          return Reflect.get(target, prop);
        },
        set(target, prop, value) {
          _dep.notify();
          return Reflect.set(target, prop, value);
        },
      })
    }
    // 使用createReactive添加响应式，这里是为了处理深层次的引用
    let reacitve = (initValue) => {
      let _value = initValue
      if ('object' === typeof _value) {
        let obj = _value
        for (let key in obj) {
          _value = createReacitve(obj, key, obj[key])
        }
      }
      return _value
    }
    // 因为只对值进行响应式封装，所以其实是简单的createReactive
    let ref = initValue => createReacitve({ value: initValue }, 'value', initValue)
		// 计算属性：实际上只是把一个简单的函数计算一下，具有缓存
    // 缓存：通过钩子来判断值是否更新，不更新就使用缓存
    // 与watch的区别： 这里直接返回确定值；watch只是监听变化并执行回调
    let computed = (fn) => {
      let value
      let dirty = true // dirty 是为了缓存，如果没有值没有更新，就返回原来的value
      let runner = effect(fn, {
        schedulor: () => { // 钩子函数schedulor为了在值更新的时候将缓存置为允许
          if (!dirty) dirty = true
        }
      })
      return {
        get value() {
          if (dirty) {
            value = runner()
            dirty = false
          }
          return value
        }
      }
    }
		// watch手动添加、监听依赖，这里只讨论source为函数的情况
    let watch = (source, cb) => {
      // source 是源，可能的值为数组，函数，对象等，这里只针对函数
      const getter = () => { // 关于为什么要包装着一层
        return source() // 为了处理上述情况，这边就一种情况简单处理，以后方便扩展
      }
      let oldValue
      let runner = effect(getter, { //将值加入依赖监听
        schedulor: () => applyCb() //并设置钩子函数来给回调返回新旧值
      })
      const applyCb = () => {
        let newValue = runner()
        if (newValue !== oldValue) cb(newValue, oldValue)
        oldValue = newValue
      }
    }
    return {
      reacitve,
      watchEffect,
      ref,
      computed
    }
  })()
```



### nextTick

`Vue.nextTick([callback,context])`

`vm.$nextTick([callback])`

将回调函数延迟到下次DOM更新循环结束之后，进行执行

1. 通常用于在修改数据之后，调用此方法在回调中去获得更新之后的DOM
2. 如果在钩子函数mounted，。。。中不确定DOM元素是否已经渲染完毕，可以调用nextTick方法来保证拿到的DOM，是已经渲染完毕的。



### diff算法

是关于Virtual DOM之间的差异比较，来决定渲染的地方。

Virtual DOM是一个模拟DOM的JS对象，在操作真实DOM之前，我们先操作Virtual DOM，然后通过Diff & Patch来比对差异性，尽可能减少DOM操作之后，再将其DOM操作反映到真实DOM上。

Virtual DOM也给我们提供了多端开发的可能性，毕竟将JS对象转化为IOS端、H5端、Android端等还是比较容易的

#### Diff策略

按DOM树的层级（level by level）

![lbl](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/lbl.png)

按类型

如果前后类型不一，会将此DOM节点以及他的字节点一并删除，并重新创建。

列表Diff

新旧Virtual DOM节点的列表进行比对，会有四个指针分别指向旧节点的头尾，新节点的头尾，进行遍历比较（比较过程：o1,o2,n1,n2，旧头旧尾新头新尾。o1 n1 => o2 n2 => o1 n2 => o2 n1 => 根据key去keyMap里查找 => 继续挪动），发现相同，则将真实DOM节点按照新的DOM进行位置挪动，遍历完后，旧DOM里没有的节点会被创建。



### Computed和watch

`Computed`、`watch`、`watchEffect`

vue3.0的三个重要API

`watchEffect`：类似于之前的`onExchane`，但是会返回一个`stop()`，用于停止监听

## Vue2/3的设计实现细节（4.24直播）

vue2是一个设计优秀的框架

值得关注的点：

- 响应式系统
- DSL特定领域的语言（专门的语法）（内置有编译器）
- 数据更新



编译器compile()有三个阶段：

- parse

​		解析template、词法分析、找Token、提取信息、生成AST

- optimize

​		标注静态节点

- generate

更新updata

- initial render



- change  => diff（preVnode和Vnode）

​		

parseHTML：解析template、找Token并提取信息的函数，通过while循环，提取一段信息，就删除掉一段字符串。

Token指的是解析模版时的一些正则表达式。



####

##### 