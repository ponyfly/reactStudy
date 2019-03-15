# react知识点
## jsx
jsx是一种 JavaScript 的语法扩展，可以在jsx中使用表达式
```
<ul>
    {
       names.map((name, index) => <li key={index}>{name}</li>)
    }
</ul>
```
## 创建组件的方式
+ 工厂函数创建（适合创建简单、无状态组件）
```
function Component1() {
  return (
     <div>
        <div>创建工厂函数的第一种方式：工厂函数</div>
           <input type="text" />
        </div>
      )
  }
ReactDOM.render(<Component1/>, document.getElementById('example1'))
```
+ es6类创建（适合创建复杂组件）
```
class Component2 extends React.Component{
  render() {
    return (
      <div>
        <div>创建组件的第二种方式</div>
      </div>
    )
  }
}
```
## es6 class补充
+ 在类中创建一个数据对象时，该对象默认是绑定在实例上，而非类的原型上
+ 在类中使用箭头函数创建一个方法时，该方法默认是绑定在实例上，而非类的原型上
```
class Person {
  constructor(age){
    this.state.age = age
  }
  state = { // 在实例上定义数据
    name: '张三'
  }
  sayName(){ // 在原型上定义方法
    console.log(this.state.name)
  }
  sayAge = () => {// 箭头函数用来在实例上定义方法，sayAge在实例上而非原型上
    console.log(this.state.age)
  }
}
```
*所以在定义类是当我们使用组件类时，当需要为某些方法绑定this时，推荐使用`bind`,而非使用箭头函数，虽然这样并没有错，但是违背了继承的出发点*
```
class Person {
  constructor(age){
    // this.state = {}  //定义数据：第一种方法
    this.state.age = age
    this.sayHello = this.sayHello.bind(this) // 推荐：将this指向我们的实例，这样就绑定了this
  }
  state = { // 定义数据：第二种方法，与第一种方法等价
    name: '张三'
  }
  sayName(){ // 在原型上定义方法
    console.log(this.state.name)
  }
  sayAge = () => {// 箭头函数会把该方法注册在实例上而非原型上，这样每个实例都会有一个sayAge,但其实定义在原型上更好，共享一个方法
    console.log(this.state.age)
  }
  sayHello() {
    console.log('hello')
  }
}
```
## state
`state`是组件数据来源之一（另一个是props）
定义state的地方有两个：
1. 在`constructor`中定义
```
class Person {
  constructor(age){
   this.state = {}
  }
}
```
2. 在类中作为一个对象单独定义
```
class Person {
  constructor(){}
  state = {}
}
```
*两种方法等价*
## props
+ props的传递
```
ReactDOM.render(<Person name={ person1.name } />, document.getElementById('example1'))
```
+ props的获取
```
class Person {
  constructor(props){
    // this.props
  }
}
```
+ props的配置项(数据类型、是否必须、默认值)
```
Person.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number.isRequired,
  gender: PropTypes.string
}
Person.defaultProps = {
  gender: 'male',
  age: 26
}
```
## refs
ref的值取决于节点的类型
+ 当ref是一个普通的HTML元素，ref的current指向绑定的dom元素
+ 当ref指向一个类组件时，ref的current指向已挂载的实例
+ 不能在函数式组件使用ref,因为她没有实例
ref的两种定义方式
1. 使用React.createRef() （react>16.3）
```
class LgComponent1 extends React.Component{
  constructor(props){
    super(props)
    this.textInput = React.createRef()
  }

  render() {
    console.log('render')
    return (
      <div className="class1">
        <input type="text" ref={ this.textInput } />
      </div>
    )
  }

  componentDidMount() {
    console.log(this.textInput)
    if (this.textInput) this.textInput.current.focus()
  }
}
ReactDOM.render(<LgComponent1/>, document.getElementById('example'))
```
2. 使用回调定义ref
```
class LgComponent2 extends React.Component{
    constructor(props) {
      super(props)
      this.textInput = null
      this.setTextInput = this.setTextInput.bind(this) // 绑定this的方法之一bind
      this.focusTextInput = this.focusTextInput.bind(this) // 绑定this的方法之一bind
    }
    setTextInput(ele) {
      this.textInput = ele
    }
    focusTextInput() {
      if(this.textInput) this.textInput.focus()
    }

    componentDidMount() {
      this.focusTextInput()
    }
    render() {
      return (
        <div className="textinput">
          <input ref={ this.setTextInput } /> // ref的回调在componentDidMount和componentDidUpdate之前调用
        </div>
      )
    }
}
ReactDOM.render(<LgComponent2/>, document.getElementById('example1'))
```
## 生命周期 (官方的图挺好的)
---
## 请求
1. axios
2. fetch
## diff的实现
使用key来进行标示，下次渲染前对比，最小化渲染
