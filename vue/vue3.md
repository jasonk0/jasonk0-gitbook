# Vue3

## 引言

> **Vue是什么？**
>
> MVVM框架？
>
> 不！他是做渲染的框架

vue3将舍去vue2的很多东西，拥抱函数式简约技术，在其中更多的是以架构技术作为主导和关注点。

>  view = f (data)

**纯函数**

其行为是很容易被预测的。代码行为容易预测，那么它是健壮的，反之是不健壮的。

Vue3是一个根据数据渲染的函数，再加上一些特定的副作用。副作用在前端是必须的，比如页面跳转，window操作，绑定时间等等。

>  view = f (data) with effect [ ]



**新东西**（更好用、更快速）

- Composition API
- reactivity
- JSX/Typescript

**新特性**

- 简化状态设计，符合人的直觉

- 拥抱函数式

- 简化API

- 更好的封装和最小粒度服用（关注点分离原则）

   

## Vue3的导读

### 拥抱JSX

SFC没有成为标准所以被抛弃掉了

而且SFC不符合关注点分离原则，也就是其模版和业务代码距离非常远，不方便一起管理。那我们这个业务关注点就被分开了

而JSX会可以聚合业务关注点，且可以更优雅的提供多样的渲染能力

### Typescript



### Composition API

>  生命周期勾子函数已经不重要了



### Reactivity

什么是响应式？

当变量发生变化时，只告诉外界其发生了变化。（类似于观察者模式）

「响应式值」专注于计算逻辑的表达，不关心渲染细节

#### Proactive vs Reactive

主动关系中，是强耦合的，一方的状态与另一方紧密相连

响应关系中，是被动的，是解耦合的，两方只向外界扔出事件，和订阅事件



> P.S.
>
> Reactivity和React都是为了响应式设计和声明式程序
>
> Composition API 是为了提供更好的组合能力（组件之间的组合能力）



## Reactivity

命令式程序 vs 响应式程序

前者是赋值，后者是声明关系。



![reactive](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/reactive的演示图.png)

Get Value

- track 追踪到依赖的组件

Set Value

- Trigger 通知所有依赖的组件

### 代理模式



```typescript
function createRef<T>(val : T){
  const obj = {
    get value(){
      track()
      return val
    },
    set value(v : T){
      trigger()
      val = v
    }
    return obj
  }
}
cosnt a = createRef(0)
a.value = 1
console.log(count.value)
function reactive<T extends object>(obj : T)[
  const proxy = new Proxy(obj,{
  get(target, key) {
    return Reflect.get(target,key)
  },
    set(target,key,value){
      Reflect.set(target,key,value)
      return true
    }
  })
]
```

思考

代理对象和原对象是一种怎样的关系

代理对象和原对象的逻辑并无一定关联，我们只关注他们的数据表现。

Data binding和Reactive的关系？

Reactive是数据绑定的一种方案。

## Ref和Reactive



## Computed

### 第一层：计算函数

将计算封装到一个纯函数里



### 第二层：提供缓存

在递归计算「斐波那契数」时，越大的数计算越耗时，即「昂贵」。如果我们使用函数包装，那么每一次页面更迭，去拿「斐波那契数」都会触发重新计算；但是使用computed的话是具有缓存的，这个「缓存」指的是在「依赖变量」没有变化时去使用；而当页面更迭，「依赖变量」不变化的情况下，是不会重新计算的。

当然，我们也可以通过手写cache来优化斐波那契的计算。

## Effect Scope

 副作用范围：人为设置一个范围，对其内部所有effect进行管理。

感觉没啥吊用



## Provide和Inject

Context上下文，实体间共享的知识

比如：this、window对象、全局用户登录状态。

Provide向组件提供上下文

Inject从上下文中拿数据

缺点：缺少类型

用于组件通信？看场景，如果对于数据的依赖足够多，是可以的。但是大部分都是技术可行，但是是不推荐的。跨组件传参是违反「最小知识原则」，通常可以通过发消息通信。

## 生命周期钩子

Hook：钩子是一种消息机制；

触发钩子的实体内部状态发生了变化从而触发了钩子，去通知钩子的另一端。

比如Git的Webhooks、React的hooks

![状态映射](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/vue生命周期钩子状态映射.png)



### onRenderTrancked、onRenderTriggered

指的是track和trigger行为

trigger是set的时候，也就是被改变值的时候去trigger，会触发多次

track是get的时候，也就是获取值的时候去track，但是**只做一次。**

### onActivated、onDeactivated

关于keep-alive的两个钩子



## ref和expose

ref是vue的一种reactive，用来表示引用（对象、数组、number、组件）

expose可以向上暴露一些属性和方法。

## 实战

### 封装展示类组件

```tsx
export const Component01 = ()=> {
  return <Button>{"hello!"}</Button>
}

const Button = defineComponent({
  setup(props,{slots}){
    const Child = slots.default
    return ()=> <button><Child/></button>
  }
})
```



### 封装输入类组件

```javascript
export const Component02 = definceComponent({
  setup(){
    const form = reactive({
      username: 'abc'
    })
    return () => <Input value={form.username}/>
  }
  
})
  const Input = ({value} : {value: String}) =>{
  return <input value = {value} />
}
```



### 拖拽组件的封装

```tsx
export const DragExample = defineComponent({
  setup(){
    return <div>
    	<Draggable></Draggable>
    </div>
  }
})
function userDrag(){
  let startX = 0, startY = 0
  const diffX = ref(0)
  const diffY = ref(0)
  const handlers = {
    onDragstart(e : DragEvent){
      startX = e.clientX
      startY = e.clientY
    },
    onDrag(e : DragEvent){
      diffX.value = e.clientX - startX
      diffY.value = e.clientY - startY
    },
    onDragend(e : DragEvent){ }
  }
  return {handlers, x : diffX, y : diffY}
}
const Draggable = defineComponent({
  setup(props,{slots}){
    const {handlers,diffX,diffY} = userDrag()
    return ()=>{
			const vNode = slots.default
      const x = ref(0), y = ref(0)
      vNode.props = _.merge(vNode.props,{
        draggable :true,
        ...handlers,
        style: {
          position: 'absolute',
          top: y.value + 'px',
          left: x.value + 'px',
          transform: `translate(${diffX.value}px, ${diffY.value})`
        }
      })
      return <>
      	{vNode}
      <>
    }
  }
})
```

### 封装业务请求

筛选组件

```tsx
function useList(){
  const data = ref<any>(null)
  request().then(resp=>{
    data.value = resp
  })
  return data
}
export const FilterExample = defineComponent({
  setup(){
    const tableMeta = [{
      title: 'name',
      key: 'name'
    },{
      title: 'score',
      key:'score'
    }]
    const data = useList()
		return () => {
      return <div>
        <Table tableMeta = {tableMeta} 
        	data = {data.value}
          />
      </div>
    }
  }
})
type TableProps = {
  tableMeta: TableMeta[],
	data : Array<Record<string,any>>
}
const Table = ({tableMeta,data}:TableProps) => {
  return (<table>
 	 <THead tableMeta = {tableMeta} />
   <TBody tableMeta = {tableMeta} data = {data} />
  </table>)
}
const THead = ({tableMeta}: Omit<TableProps, "data">) => {
  return <tr>
    {tableMeta.map(item => {
      return <td key = {item.key}>{item.title}</td>
    })}
  </tr>
}     
                                             
const TBody = ({tableMeta} : TableProps) => {
	return <>
 		{data & data.map((item,i) => {
    	return <tr key= {i}>
      	{tableMeta.map(meta => {
        	return <td key={meta.key}>{item[meta.key]}</td>
      	})}
      </tr>
  	})}
 	</>
}
```

## Vite

尤雨溪开发的脚手架

特性：

- No Bundling => 开发的时候非常快，浏览器直接使用ESM
- HMR深度提速 => 热更新提速，尤大大自己开发的
- 基于rollup的体系，rollup插件经过简单包装就可以在vite用了

### 项目结构一

所有的能力在一个项目中实现，

- pages => 所有页面
- components => 按钮等一些组件包括业务组件
- layout => 做布局的组件，flex等
- util => 通用函数
- model => 模型

### 项目结构二

多项目，由多个项目组成最终的系统，可以更好的体现可复用性

- packages
  - util
  - components
  - model
  - ui



## Vue3的源码

### h和createVNode函数

不管是模版类型还是JSX在vue3中都会被转化成h函数

像这样：

![样例](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/h函数.png)

createVNode和h类似，但不常用

### Virtual DOM（概念模型）

虚拟dom是一个在写程序的概念模型，以组件构成的模型。

更改：节点删除、节点更新、属性更新、内容更新……

VirtualDOM的更新是通过创建一个新的的虚拟DOM，然后用DOM-DIFF进行比较计算出来更改，最后再用作渲染真实DOM。

### DOM-DIFF

![DIFF模型](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/DOM-DIFF.png)

比如：

属性变更，样式变了 => 不需要操作节点 => 打一个更新属性的补丁(patch)

节点类型变更 => 将树直接替换掉

节点插入 => 在末尾插入 => 并更新对应位置的节点

节点插入(带key) => 根据key去插入 => 只插入了新节点

### Reactivity原理

什么是依赖？

 render返回vNode

每个「reactive值」需要记录依赖，依赖就是副作用，多个依赖(被包装成函数)保存在「reactive值」里的一个集合，通过`Track()`去收集依赖，在更新「值」的时候通过`Trigger()`去通知执行依赖。

所以reactive值不关心在那里被用到，只需要记录依赖

通过ES6的`Proxy`来实现响应式的`Track`和 `Trigger`

### Ref源码

https://github.com/vuejs/core/blob/main/packages/reactivity/src/ref.ts



### Reactive源码

https://github.com/vuejs/core/blob/main/packages/reactivity/src/reactive.ts



### Watch源码

https://github.com/vuejs/core/blob/main/packages/runtime-core/src/apiWatch.ts

### vue3的ts类型



## vue3的组件通信

https://juejin.cn/post/7069251532339806238

props

emit

attrs

provide/inject

expose/ref

vuex

mitt
