# React



## 前言

基础 + 进阶

facebook的工程师2011年开发的

专注于业务开发，减少与DOM的交互

用于构建用户界面的javascript库

同样是基于虚拟DOM，将更改映射到虚拟DOM，最后渲染成真实DOM

**为何选择？**

相比于vue、angular，应用更加广泛。

生态完善、友好的开发环境

![生态](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/react生态.png)

## 基础入门（前厅）

### 环境配置

NVM：node版本管理

VSCode

Node

Create react App： 脚手架，其实也可以用vite

**Vite**

`npm init vite@latest` 初始化一个vite项目

跟着选项选择就可以了

`npm install` 项目创建好后，安装依赖

`vite/npm run dev`就可以跑起来了



#### 工具插件：bootsrap、第三方插件、eject

扩展：simple react snippets

eject命令将一些隐藏的内容和配置展示出来。 

codesandbox：在线网站（提供各种环境）



### JSX

构建组件的要点：

- 组件位置，在树状图里的位置
- 确定参数，有哪些参数是必须的，那些是缺省的；那些缺省值在其他场景可以复用
- 组件类型，函数组件还是Class组件
- 组件内容，包含哪些数据，返回那些内容
- 组件的通信与交互

定义：

- 不是模版引擎语言（angular、vue）是带语法糖的AST（抽象语法树:christmas_tree:）
- 声明式创建UI，处理UI逻辑
- 遵循JS语法

渲染逻辑是UI中逻辑的耦合(事件绑定等)，这也遵循了「关注点分离原则」

#### 关于Babel

他是一个Javascript编译器，比如将箭头函数编译为低版本亦可使用的正常函数。

babel会把JSX中的XML转化成浏览器可以执行的代码，也就是生成DOM树:evergreen_tree:所需要的DOM操作。

#### Fragment

一个虚拟的节点包裹两个并列的子节点，来保证babel可以解析并列的子节点。但是真实DOM中是没有这个节点的。

简写：`<></>`

除了以上的情形，还可用作表格父子组件中子组件「列」的包裹。

#### 拓展

babel的运行原理：解析（parse），转换（transform），生成（generate）。

![babel原理](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/babel原理.png)

**解析**

使用 babylon 解析器对输入的源代码字符串进行解析并生成初始 AST（File.prototype.parse）

利用 babel-traverse 这个独立的包对 AST 进行遍历，并解析出整个树的 path，通过挂载的 metadataVisitor 读取对应的元信息，这一步叫 set AST 过程

**转换**

transform 过程：遍历 AST 树并应用各 transformers（plugin） 生成变换后的 AST 树

babel 中最核心的是 babel-core，它向外暴露出 babel.transform 接口。

**生成**

利用 babel-generator 将 AST 树输出为转码后的代码字符串

资料阅读https://github.com/barretlee/babel-plugin-ast

### CSS

行内样式

```JSX
render(){
  return <span style={color: '#777',fontSize : '80px'}></span>
}
```

CSS module => 全局污染、命名重复、依赖管理

可以达到：一个class代表一类样式，选择器不堆叠，通过compose进行依赖组合。

```css
.title{
  composes: common from 'common.module.css';
  font-size: 25px;
}
```

一个CSS的解决方案：[styled-Component](https://styled-components.com/)

还有一个工具：Classnames

```jSX
import classnames from 'classnames/bind'
import style from 'style.module.css'

const cls = classnames(style)
const app = ()=>{
  return (
  	<div className={cls('abc')}></div>
    <div className={style.abc}></div>
  )
}
```



### Props

什么是Pros？

当React元素作为自定义组件，将JSX所接受的属性转化为单个对象传递给组件，这个对象被称为props

也就是父组件传给子组件的对象。也是一个组件的固有属性。

#### 函数组件与类组件

他们的不同在于参数的传递和拿取

```jsx
const listItem = (props) => {
  return (
  	<div>
    	<div>{props.data.name}</div>
    </div>
  )
}

class ListItem extends Component {
  constructor(props) {
    this.props = props,
  }
  return (
  	<div>
    	<div>{props.data.name}</div>
    </div>
  )
}
```

函数组件也称作无状态组件，组件内没有this（组件示实例），也没有生命周期。是一个轻量的组件。

#### 列表渲染

```jsx
class ListItem {
	const listData = []
  return (
  	<div>
    	{listData.map(v => {
          return (
          	<ListItem data = {v} key={v.id}></ListItem>
          )
        })}
    </div>
  )
}
```

#### 条件渲染

```jsx
class ListItem extends Component {
  let count = 0
  constructor(props) {
    this.props = props,
  }
  return (
  	<div className = "row">
      <div className = 'col col-1'></div>
        <span>																			{this.props.data.name}
        </span>
        <div className="col col-1">									{this.props.data.price}
        </div>
      <div className ={`themed-grid-col${count ? '' : '-s'}`}>
        {count}
      </div>  
    </div>
  )
}
```









## 进阶知识（正殿）

### 事件处理

React事件和DOM事件大相径庭

在使用this调用其他方法时，会发现this为undefined，此时可以使用bind方法绑定this：

1. 在return的XML中调用方法时bind绑定this
2. 在构造函数里绑定this，并代替原来的方法。
3. 使用箭头函数创建方法

#### 向事件处理函数传递参数

在XML中传入参数：用bind传递；用箭头函数传递参数。

两个方法的前提都是为了this指向明确，然后传递参数。

```jsx
// 自己包装的方法
class ListItem extends Component {
  state = { name:'八戒' } 
  handel = (name)=>{
    return (ev)=>{
      console.log(name + ' hello!')
      console.log(ev)
    } 
  }
  render() { 
    return (<button onClick={this.handel(this.state.name)}>hello</button>);
  }
}
// 老师讲的方法
handel = (name,ev)=>{
	console.log(name + ' hello!')
  console.log(ev)
  }
  render() { 
    return (<button onClick={(ev)=>this.handel(this.state.name,ev)}>hello</button>);
```

#### 向父组件传递参数

在子组件上定义一个事件`onDel`绑定到父组件的处理函数上，然后在子组件里通过并绑定点击事件`onClick`到自定义事件`onDel`上来传参；子组件可以引用父组件的事件`onClick = this.props.onDel(id)`

#### 事件机制

DOM的事件机制：捕获 => 目标事件处理 => 冒泡

所以可以进行「事件委托」

![DOM事件](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/DOM事件处理机制.png)

React事件机制：事件都会被绑定在document上，一般会在冒泡阶段处理

![React事件](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/React的事件机制.png)

React事件是合成事件，在document监听所有的支持事件，使用统一的分发函数dispatchEvent

#### 拓展

> 合成事件（SyntheticEvent）：React自己实现了一套高效的事件注册，存储，分发和重用逻辑，在DOM事件体系基础上做了很大改进，减少了内存消耗，简化了事件逻辑，并最大化的解决了IE等浏览器的不兼容问题。

特点如下：

1. SyntheticEvent最终绑定在document上，相当于做了最高层的事件委托，也简化了DOM原生事件的流程，减少了内存开销
2. 由于原生事件是先于合成事件的，而且合成事件的冒泡机制是通过将目标组件和处理函数加入「事件处理模型」生成「处理函数队列」，再使用统一的分发函数dispatchEvent去执行处理函数的方式实现的；所以使用`stopPropagation()`是无法停止事件冒泡的，应该使用`preventDefault()`来阻止事件传播。
3. React使用对象池来管理合成事件对象的创建和销毁，减少了垃圾生成和新内存的分配，大大提高性能。



### state

React-devTools调试工具

#### 基础

定义：在constructor里定义state

**何时定义？**

是否可以通过props父组件获取；是否可以通过其他state和props计算得到；是否在render方法中使用。



**如何修改State？**

- 通过this.setState。

- setState是异步的
- State的更新是浅合并，也就是将多次修改合并为一次



**不可变数据**

状态是不可变类型，只能更新覆盖

状态的类型是数组时，用新的数组替换原数组

状态的类型是对象时，用Object.assign()拷贝一个新的对象。有时需要深拷贝

Immuter JS 库



**State和props的区别**

State：

- 可更改的
- 组件内部更改
- 交互或其他逻辑造成的数据更新

Props：

- 在组件内部不可变
- 父组件传入
- 简单的单向数据流

state 可以通过 proos 传入子组件

props 可以初始化 state

#### 进阶 

一般情况下porps、state即使不变仍然会调用render函数，完全没必要。可以通过shouldComponentUpdata来进行判断前后值是否一样，一样的话就不render；或者可以使用更方便的PureComponent



**单一数据源**

同一页面下的组件使用一个数据源

子组件的数据依赖于父组件传递的props，如果子组件存在状态变更，则将其转化为事件传递给父组件处理。这样可以保证父子组件的状态都以父组件为准

单一数据流：仅从父组件流向子组件



**状态提升**

就是将兄弟组件的状态统一提升到他们的父组件进行管理

把状态提升到最上边一层



**无状态&有状态**

有状态组件：容器组件、类组件、处理状态

无状态组件：函数组件、仅展示组件



**总结**

优化渲染：shouldComponentUpdata或者pureComponent

使用不可变数据：方便管理响应式属性

单一数据源及状态提升：保证单一数据流，方便状态管理

无状态组件：使用函数组件书写更加简洁（仅展示）

### 生命周期

三个阶段：创建阶段-更新阶段-卸载阶段

**创建阶段**

初始化：`constructor`

​	初始化内部状态，象形设置和隐形设置，super调用基类的方法，可以直接修改state

挂载：

`componentWillMount`

​	setState不会触发render

`render`

​	render必须有的方法；返回一个顶级的react元素，渲染出来的是类似于DOM树的React对象，也就是虚拟的。

`componentDidMount`

​	渲染完调用；只执行一次；常用来获取一些外部数据资源

**更新阶段**

`shouldComponentUpdate`

​	将旧值与新值进行对比，相等的话就不会重新渲染，可以使用PureComponent代替

`componentDidUpdate`

​	可以比对props的前后变化，发生变化的话可以进行一些操作

`componentWillUnmount`

​	组件即将卸载

### React的组件设计模式

高阶组件和函数作为子组件renderProps

#### 高阶组件HOC

提供可复用性高，健壮性好的通用组件，减少重复代码。

高阶组件是对现有组件的封装，接收组件作为参数返回一个新的组件。

#### 函数作为子组件renderProps

可以理解为子组件拿到父组件传入的props（包含着父组件的渲染函数内容）然后渲染出来

```react
// 作为子组件被调用，使用props传来的render函数渲染
// 状态依旧是在这里管理
render(){
  return (
  	<div>
    	{props.render(this.state)}
    </div>
  )
}
// 作为父组件，给作为HOC的子组件传入props和需要包装的内容
// 这里解构出来状态里的参数，形参
render(){
  return (
  <sonItem render={({s1,s2}) => {
        <div>需要包装渲染的东西</div>
      }}></sonItem>
    )
}
```





## 生态工具（花园）



### React router



### AntDesign



## 高级应用（边廊）

###



### 



### 



### 



## 原理讲解（穹顶）