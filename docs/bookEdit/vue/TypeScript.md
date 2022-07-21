





# TypeScript

[TOC]

## 小思考

接口等价于类型类

「接口」是属于「面向对象」

「类型类」属于「函数式编程」

### 类型类

1. 整型可以被比较

2. 数组可以遍历

.......

> 类型是人类的思考方式

映射的思维

ComponentsClass =(实例化)=> Component

Array<ComponentsClass> => Array<Component>

> 类型的演化体现在系统的进步



## 介绍

2012年微软发布的一门编程语言

- Transcompiler - 翻译编译器 - 编译成JS

- ts是js的超集

- Client && ServerSide

- 多范式（面向对象、函数式）

- Duck Typing，Gradual Typing、Strict Typing...

  鸭子类型：像鸭子就是鸭子

  渐进类型：让一部分变量不被检查

  严格类型： 字面意思

TS在编译时会进行类型检查，运行时是没有类型检查的

> TS是一种工具，并作为思考方式
>
> 分清楚理解记忆和死记硬背



## TS环境和Helloworld

Node环境的TS解释执行器（TS-Node）**REPL**（Read eval print loop）

ts需要在有tsconfig的地方执行

翻译编译器**TSC**，编译成js文件，需要编译的文件和编译后文件的位置都在tsconfig中配置好了。

## React/Vue+TS环境实战

### React + TS

webpack

rollup



### Vue + TS

webpack

```javascript
const path = requires("path")
module.exports = {
entry: path.resolive(__dirname,"../src/main.tsx"),
 mode : 'develepment',
   module: {
     rules: [
       {
         test: /\.tsx?$/,
         loader: 'babel-loader',
         options: {
           presets: [
             "@babel/preset-env",
             "@babel/presettypescript"
           ]
         },
         exculed: /node_modules/
       },
       {
         test: /\.vue$/,
         loader: 'vue-loader'
       }
     ]
   },
   resolve: {
     extensions: [".tsx", ".ts",",js",".vue"],
       output:{
         filename: 'bundle.js',
         path: path.resplve(__dirname, "../build")
       }
   }  
}
```



rollup

```javascript
import path from 'path'
import typescript from 'rollup-plugin-typescript2'
import babel from '@rollup/pliugin-babel'
import vue from 'rollup-plugin-vue'
export default [{
  input: path.resolve(__dirname,"../src/main.tsx"),
  output: {
    file: "build/main.js",
    format: 'commonjs',
    name : "main"
  },
  plugins: [
    vue(),
  	typescript(),
    babel({
      exclude: "node_modules/**",
      extensions: [".tsx", ".ts",",js",".jsx"],
      presets:[
        "@babel/preset-env",
        "@babel/preset-typescript"
      ],
      plugins: [
        "@vue/babel-plugin-jsx"
      ]
    })
  ]
}]
```

## TS日常篇（一些语法）

ts是js的一种「标注」

**泛型**

`const a = new Array<string>()`

`const a = new Array<T>()`

数组中的类型统一

----

**规避类型检查**「标注`any` 」

`const a = {x: 1}` 

a属于`{x:1}`这样的一个类型对象，所以` a.y` 是无法访问和创建的（会报错）



`const a : any = { x: 1 }`

any就是为了让ts不再检查这个对象

通过`//@ts-ignore`也可以跳过下一行的检查



`const a : {[key : String] : number} = {x:1}`

但是可以通过这种的方式访问`a.y`

`a.y = 2` 是被允许的

----

标注`unknown`

也可以规避，但是不是不检查，而是ts当作不知道来处理；它可会被当作任意类型，可以被任意赋值，但是不会被赋值给「已标注类型的变量」

```typescript
let a : unknown = {x:1}
a = 'string' //是被允许的
a = 1
// 通常用于JSON和一些实在不知道类型的情况
const x : unknown = JSON.parse(...);
let y: string = x //这样子是不被允许的，因为y是明确类型
```

----

函数中的参数和返回值标注

```typescript
function greet(name: String): String{
  console.log(name)
  return name + 'hello'
}
```

----

匿名函数中的标注

ts会帮助推到数据的类型，下边的`value`没有标注，但是ts推导出他是`string`类型，所以无法赋值给`y`

```typescript
const names = ['xk','zzy']
names.map(value => {
  value.toUpperCase() //可以使用字符串方法
	const y : number = value //哒咩
})
```

这种推导称为「上下文类型推导」Contextual Typing技术

> 上下文是程序间「共享的知识」：上文中提到的东西在下文引用。
>
> 中文就是中国人的上下文。

----

对象类型的标注

```typescript
const pt : {
	x : number,
  y : number,
  z? : number //?是可选项
} = {
  x : 1,
  y : 1,
}
pt.z? // ?检测是否是undefined，而不会报错
// if(a && a.o && a.o.p) == a?.o?.p?
  
  
  
```

----



类型的联合

```typescript
function printId(id : number | sring){
  console.log("id"+ id + '!')
}
printId(101)  //可以
printId('101') //可以
printId({x:101}) //哒咩
```
-----



类型的别名

一种字符串查找替换的方法，全名太长了取个名字

```typescript
type Point = { x: number,y:number}
const pt : Point = {
  x:100,
  y:100
}
```

type和interface类似，但是语义化区分前者就是「别名」，后者是「接口」。

而且接口有合并的能力

```typescript
interface Animal {
  name : string
}
interface Animal {
  type: string
}
// 两者会合并
```

还有一点是type和interface的继承能力

```typescript
// interface
interface Animal {
  name : string
}
interface Tiger extends Animal {
  meat : boolean
}
const bear = getBear()
bear.name
bear.meat

// type
type Animal {
  name : string
}
type Tiger = Animal & {
  meat : boolean
}
const tiger = getBear()
bear.name
bear.meat
```

-----

类型断言Type Assertion

as 我断言这个值是某个类型

⚠️断言必须是靠谱的⚠️

```typescript
const ele =
      document.getElemenById('canvas') as HTMLCanvasElement
//原本是HTMLElement对象，我将它断言成HTMLCanvasElement类型
```

-----

字面类型Literal

```typescript
//当使用const时
const foo = '123'
//foo的类型将被当作 字符串‘123’
let fdd = '123'
//而fdd的类型会被当作 字符串
```

因为const是声明常量的，所以类型就是字面类型

```typescript
function compare(a:string,b:string) :1|0|-1{
  return a > b? 1 : (a===b) ? 0 : -1
}
const y = compare('abc','dee')
// y的类型就是字面类型「1｜0｜-1」
```

字面类型的补充例子

![补充](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/typescript字面类型补充.png)

----

枚举类型（默认是Int）

```typescript
enum Direction {
  UP = 1,
  DOWN
}
//类似于 联合
type Dir = "UP"|"DOWN"

// 主要运用于switch
function foo(s:Direction) {
  switch(s){
    case Direction.DOWN:
      break
    case Direction.UP:
      break
  }
}
// enum还可以反向枚举
console.log(Direction[Direction.DOWN])
// DOWN
```

在进行枚举的时候还是enum性能更好一些，因为他是枚举整型

> 减少any和unknown的使用，尽可能的窄化类型



## TS的窄化

类型的窄化Narrowing Conversion

> TS是通过控制流分析实现的类型窄化

联合和窄化 

增加typeof判断来窄化

```typescript
function printId(id : number | sring){
  if(typeof id === 'number'){
    id.toFixed() //这里边就可以确定类型为number
    return
  }
  id.toUpperCase()// 而外边就可以确定为string
}
```

narrowing的现象

- 类型在`if+typeof`之后的块（block）变窄了

- TS认为` typeof padding ===“number” `是一种类型守卫（Type Guard）

- TS看到类型守卫后，发生窄化

narrowing的原理：TS根据Type Guard重新定义语句的类型

----

真值窄化

可以帮我们窄化掉null、undefined

if(!a)



相等性窄化

```typescript
function foo (x:number|string ,y:boolean|string){
	if(x === y) {
    /// x \ y 只能是string类型了 
  }  
}

```

`in`操作符窄化

`if('swam' in animal)`



`instanceof`窄化

`if(x instanceof Date)`

----

类型断言(Type Assertion & Type Predicate)

- Assertion ：判断一个表达式值是True还是False
- Predicate：一个返回True或False的函数

```typescript
//as 类型断言
const ele =
      document.getElemenById('canvas') as HTMLCanvasElement
//is 窄化断言
class bird{
  fly(){}
}
class fish{
  swim(){}
}
//这里的isFish并不能做到类型窄化，如果调用isFish
//那么返回值仍然是bird｜fish
function isFish(animal: bird|fish) {
  return animal instanceof bird
}
// 而加上了窄化断言is就可以了，在其他block调用的时候
// 就会返回窄化过的断言fish 或者是bird
function isFish(animal: bird|fish) animal is Fish{
  return animal instanceof bird
}
```

----

Never类型

`const x:never = 1`是不可以的

never 表示不可能发生的事，谁也不能对其赋值

一般用于抛出异常

> as、is是TS特有的关键字



## 泛型

interface 接口（Aspect）是各种能力接口组合在一起

<>泛型（Generics）是对共性的提取。

DeskMaker<T> 木头和石头都可以坐桌子，把这个特性提取出来，便是泛型

DeskMaker<wood> 可以根据传入的参数来实现方法

DeskMaker<stone> 

***泛型可以对共性的抽象，可以将类型作为参数，更好的分离关注点***



当我们想约束一个类型的成员的时候用接口

当我们想提取一些类型的共性的时候去用泛型

## 函数

构造函数的表达

```typescript
type SomeConstructor = {
  new (s:int) :Sring
}
function fn(ctor: SomeConstructor){
	return new ctor("hello")
}
cosnt str = fn(String) //hello
```

-----

泛型函数

「返回数组的第一个数」：直接标注不太好约束类型，所以采取泛型去约束类型

```typescript
function firstElement<T>(arr:T[]):T{
  return arr[0]
}
```

----

函数的重载

函数可以多写几次，从而让泛型模版的类型被提前确定。

让一切都尽可能明确就是重载的意义



## Class

继承是非常重的耦合，尽可能通过接口的组合或者泛型的标注，来实现行为的定义。

### public、private、protected

public公开

private除了这个block中都不可以访问

protected可以通过子类去访问 

### 抽象类

abstract 抽象类可以定义抽象方法，类似于接口，没有具体的实现

抽象类不可以被new但是可以继承他，并且必须要实现抽象方法。

### 类型一致

ts会根据成员来判断两者类型是否一致，或者是否是继承关系。



## TS的模块

模块是一个程序的集合（包括函数、变量、类等）

模块是可见域的分离：模块内的成员仅仅在包内可见，包外成员无法访问

模块是可以主动Export成员：外部就可以访问了

TS中可以import type 只引入类型。 

### resolution

import模块的查找方法Classic，从当前位置一个个去找模块

另一个方法Node，他会去找module.ts/.tsx/.d.ts或者package.json

## TS的实战

![模型层](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/花田的模型层.png)



服务端设计

![服务端](https://raw.githubusercontent.com/jasonk0/myImage-blog/master/imgs/花田服务端设计.png)
