
# 本笔记基于以下课程，为我个人学习过程中的笔记，感谢视频课程原作者
[react完整课程，从基础到实战高效通关(有vue基础)]([link](https://www.bilibili.com/video/BV1Mu411p7cV/))

# react文件夹和文件说明 
## public 文件夹

index.html文件 中id为root的div相当于vue中的app节点

## src文件夹
是编写代码的文件夹

### app.js 
function App()是根组件相当于app.vue 

### app.css
app.js 的css文件

### index.js 
相当于main.js 把项目挂载到指定的dom节点

### index.css 
index.js的css文件
App.test.js是对app.js单元测试文件（一般不用）

reportWebVitals.js性能检测报告文件（一般不用）

# 两个核心库

## react 核心库 
提供react的各个功能

## React-dom 
提供dom操作方法用于把react创建出来的react对象挂载到真正的htmlDom中，或者从htmldom中卸载。核心作用类似于vue的mount。
把一个react组件从一个真正的dom里卸载或者渲染真正的dom

```js
root.unmount()//root节点卸载dom
```



## react项目组件关系

App组件只有一个，react-dom把App组件渲染到html中id为root的div，整个项目就显示了
不同page都是作为app的子组件而存在

# 组件化开发

## 组件 三要素

- 组件  html模板
- 数据
- 方法

## react 组件分类
### 函数组件
```javascript
function hello(){
return
<div>hello</div>
}

```
### class组件
```javascript
class Hello extends React.Component {
    render(){
        return <div>hello</div>
    }
}
```

# jsx的特点

## 直接再js中混用
React 项目里哟个babel做了对js的编译，所以我们是可以直接再js里写jsx的

## 写法接近js
Jsx几乎和js一样，不同点就在于，可以更方便的写html在js里，写在js里的html最后会被编译称一个js对象，我们也可以用react自带createElement创建这个对象。

```javascript
function hello(){
    return <div>hello</div>
}

console(React.createElement(<div>hello</div>));
```
等价于
```javascript
function hello(){
    return React.createElement('div',[],'hello')
}

console(React.createElement('div',[],'hello'));
```
- react 和 jsx是独立的

- jsx的目的是方面我们在js中直接写html

- 不借助jsx，就要直接用react.createElement方法创建react-element对象

React解析react-element对象，创建成页面


# React 组件命名 

- 首字母一定要大写，否则不能显示并报错<br/>
-  如果不大写，就相当于普通的方法，为了做区分，因此组件方法首字母要大写


# react 组件在jsx的各种用法
```javascript
  let obj = FnHello();
  let com1 = <FnHello></FnHello>;

  return (
    <div className="App">
      {obj}
      {com1}
      <FnHello></FnHello>
      <ClassHello></ClassHello>
    </div>
  );
```
# jsx里面渲染不同内容的区别
## 字符串，数字 
    直接渲染
## 方法
    无法渲染 报错
## 对象
    直接渲染element对象
## 布尔值
    不渲染任何内容（vue会渲染）
## 数组
    把数组每一项单独渲染
## Undefined,null
    不渲染任何内容 （vue会渲染）
## 表达式
    运行表达式  例如（1+1）/（true?<FnHello></FnHello>:"123"）

# 事件绑定

## 规则模式
- 类似于原生 on+方法名
- 一定要赋值给事件一个方法
## 特别注意问题
- 不作处理的情况下，this会指向undefined
- 给到事件绑定的一定得是一个方法，不要直接调用方法(就是方法名后不要加括号)，调用方法只会在页面初次渲染指向方法
  ```javascript
    render(){
    return <div className="App">
      <div onClick={this.click}>123</div>
    </div>

  }
  
  ```
## 当需要绑定this
- 用bind，this.click.bind(this,arg1,arg2) （常用，方便传参 ，因为不能直接加括号调用函数，因此可以把参数放在bind方法的括号中）
- 在写一个匿名箭头函数
- 方法本身写成箭头函数

## 如果获取事件对象
- 在事件方法传参总数的下一个参数，如果没有参数，那么事件对象在第一个参数，如果自己传有n个参数，那么在n+1个参数，可在方法体设置多一个形参获取
- 获取的事件对象不是js原生事件对象，是合成事件对象，本身也可获取事件坐标，设置冒泡等，原生事件对象在e.nativeEvent
- 用e.stopPropagation()阻止事件冒泡
- e.preventDefault() 阻止事件默认行为，比如阻止链接跳转，表单提交等
- vue是用.stop .prevent等，react则直接在事件绑定方法中调用相应的函数
```javascript
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 阻止默认行为 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 只当事件在该元素本身触发时触发 -->
<div v-on:click.self="doThat"></div>

<!-- 事件只触发一次 -->
<button v-on:click.once="doThis"></button>

<!-- 指示浏览器不要等待 preventDefault 的调用 -->
<a v-on:click.passive="doThat"></a>

```



# React组件中的数据
## 类组件响应式数据的定义
- 响应式数据定义在类的state属性中
```javascript
class ClassState extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            //这里面写state，也就是响应式数据
            //类似于vue的date方法返回的对象
            a:123
        }
    }
}
```
- es7新写法中，constructor可以省略，直接写
```javascript
state = {
    a = 123
}
```
如果state写法和constructor同时存在state，那constructor内的state会完全覆盖外面的state

写在state外的数据可以显示，但不是响应式的

## react响应式体系的原理
- react不能像vue一样直接修改触发更新
- react修改能修改值，但没法触发更新，因为react没有像vue一样监听get和set，而是在调用setState的时候调用react的更新操作
  
```javascript
  addA=()=>{
    this.state.a+=1; //不触发更新
    console.log(this.state.a)
    this.setState({ })//触发更新
     this.state.a+=1; //更改值
    this.setState({ //触发更新
      a:this.state.a
    })
  }
```
this.setState({})中必须传一个对象，执行时，会先跟state对象进行一个浅合并，再更新，因此传入空对象也会更新所有state值

## setState工作流程
- 调用setState
- 传入一个对象
- 给传入的对象和state对象进行浅合并
- 合并后的调用更新方法更新

`1、通过浅合并来修改数据`<br/>
`2、调用setState方法会触发更新，修改state不会触发更新`<br/>
`3、因为是浅合并，因此只会再第一层合并，state中的对象的子对象会直接覆盖，因此修改要把子对象的属性写完全 或 ...this.state.c`


setState的方法是异步的，因此setState方法的第二个参数回调函数中获取更新后的值
```javascript
this.setState({
    a:1
},()=>{
    //在这里获取更新后的值
    console.log(this.state.a);
})
```
## setState的一些特性
- setState方法多次修改，会合并为一次，统一更新
- setState返回会触发更新，不管你是否有修改，这造成了一个问题，重复修改为相同的值也会让组件更新。(解决方法:让组件继承React.PureComponent)
- 一定不要在render中调用setState，会死循环阻塞主线程，页面卡死

### React.PureComponent局限性
- 当state中存在子对象或数组时，对象数组增减了新的属性，state子对象也没有被更新，因为对象和数组的相等判断为地址引用判断。
- 因此当使用这个父类时，更改子对象数组时要重新拷贝赋值一个新的对象或数组才能触发更新
```javascript
this.state.arr.push("item")
this.setState({
    arr:[...this.state.arr] //触发更新
})

```
```javascript
this.state.arr.push("item")
this.setState({
    arr:this.state.arr //不触发更新
})

```
## PureComponent下对于对象和数组的修改

因为PureComponent 会根据state是否改变来决定是否更新，而我们对于对象数组这样的引用类型判断它是否改变的原理时看它的内存地址，而不是内容<br/>
因此我们PureComponent下修改对象和数组，一定要赋予一个新对象，所以一般我们不直接操作原对象，而是先拷贝一份，再进行操作。

# 条件渲染和列表循环

## react没有指令
- react没有vue一样的指令，一切操作本质上都是通过运算生成不同的内容，拿去render函数中渲染，得到不同的页面。
- react 渲染undefined,null,空字符串，false不会渲染成任何内容
- 如果渲染一个jsx编写的html元素，就会渲染成页面上的内容
- 在react中实现条件渲染，就要编写一个逻辑运算，true就返回html元素，false就返回false或空字符串或其他html元素

## 列表循环（v-for）
- 渲染一个数组会把数组里的每一项单独取出渲染
- 那么编写一个里面存放都是html结构的数组，就会渲染成列表
- 首先把数据数组循环生成一个元素数组，然后把html结构的数组放到render中渲染，就会渲染成列表(一般用map，filter)
- react和vue列表循环一样，子元素item都需要key值，否则会有警告


```javascript
  handleList(){
    let listElement = [];
    this.state.arrList.forEach(element => {
      listElement.push(<div key={element}>{element}</div>)
    });
    return listElement
  } 


    render(){
    return <div className="App">     
      调用方法
      {this.handleList()}
      //直接用map
      {this.state.arrList.map((item) => {
        return <div key={item}>{item}</div>
      })}
    </div>
  }
code
```


---
### 复习：react jsx模板代码中，渲染函数要加()表示渲染时运行，事件回调不能加()
### 继承PureComponent时，要改变state中的数组，要新建一个数组重新赋值才能触发更新
---

***react与vue相比而言 react各种效果都要通过逻辑运算产出对应的内容渲染，vue则依赖用指令编写，容易进行简单控制***

## react 的思想
- 更少的封装，更高的自由度，能够让使用者更加高度的去自定义实现，不用像vue一样必须遵守规则，只需要遵守js规则
- 跟vue相比，js比较纯粹，不用背特殊指令。



# 表单绑定

- react中的很多思路都是按原生的操作逻辑做，表单绑定也如此。
- 原生表单获取表单，通过监听input,change事件，获取e.target.value。如果要设置表单的值，通常设置value属性，如果时选择框则时checked属性
- react 可以用value={} 和onInput={}分别实现input的属性到state中属性的两个方向的数据绑定

```javascript
  render(){
    return <div className="App">
      
      <input value={this.state.inputValue} onInput={(e) => {
        console.log(e.target.value)
        this.setState({inputValue:e.target.value})
      }}></input>
    </div>

  }
code
```
***原生的react进行表单操作，需要手写获取和设置checkbox的数组的设置和回显双向绑定，逻辑和步骤与原生js类似***


# Props和组件传值

## props时react中的核心
-   在react中，一切写在组件上的属性(props.xxx)和子节点(props.children)都被规划为了props
-   所以，props时react很多功能的根本,父子传值，插槽全都基于props,不像vue有事件监听，emit，专门的插槽一类东西
- vue中，子组件可以对接受的props设置数据类型验证，react中要自己手动书写props的数据类型等校验逻辑，并抛出异常。(一般可借助proptypes库封装好的验证方法进行校验)

```javascript
//在子组件class书写props数据校验逻辑
Son.propTypes = {
    mes: function(props){
        if(typeof props.mes !== "string"){
            throw new Error("nes must be a string")
        }
    }
    //用proptypes库验证
    mesProptypes:proptypes.string
}
//书写子组件props默认值
Son.defaultProps = {
    mes:"defaultMes"
}
```
## react模拟vue中的slot插槽
- 插槽本质上就是子组件的html内容需要父组件传入，在jsx的加持下，我可以把html像普通的字符串，数字一样传递，所以插槽只需要直接作为props传入就行
- 作用域插槽的实现，props传入一个方法，return html节点，子组件调用这个props作为函数调用并传入参数，即可实现在父组件获取子组件的属性数据加入显示在插槽中


## 子往父传值（通过props）
- 父组件把方法通过props传递给子组件
- 子组件调用传递过来的方法
- 调用时传递参数
- 父组件执行方法，进行操作，比如修改state（注意this,父组件通过props传递时bind(this)）


## 兄弟组件传值
- 子1 =>父组件 =>子2（常用）
- eventbus

# react 样式设置

## class类名设置
- 必须写为className
- 类名和样式写在css文件里
- 必须接受一个字符串
  
## style内联
- 不能像原生一样写成字符串，必须写成对象































