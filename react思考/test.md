title: react思考
speaker: 曹毅
url: https://github.com/youzaiyouzai666
transition: slide3
files: /js/index.js,/css/demo.css,/js/zoom.js
theme: moon
usemathjax: yes




[slide]
# react思考
## 学习react过程中的思考与总结
### 效率
### 代码可维护性 
<br />
<small style="vertical-align:middle;display:inline-block"><iframe src="http://ghbtns.com/github-btn.html?user=youzaiyouzai666&repo=blog&type=watch&count=true" allowtransparency="true" frameborder="0" scrolling="0" width="110" height="20" style="width:110px;height:20px;  background-color: transparent;"></iframe><iframe src="http://ghbtns.com/github-btn.html?user=youzaiyouzai666&repo=blog&type=fork&count=true" allowtransparency="true" frameborder="0" scrolling="0" width="110" height="20" style="width:110px;height:20px;  background-color: transparent;"></iframe><iframe src="http://ghbtns.com/github-btn.html?user=youzaiyouzai666&repo=blog&type=follow&count=false" allowtransparency="true" frameborder="0" scrolling="0" width="170" height="20" style="width:170px;height:20px;  background-color: transparent;"></iframe></small>




[slide]
## 主要内容
----
* 基本语法
  * JSX——模板语法
  * 组件创建
  * 组件生命周期
  * 事件系统
* 一般开发流程 
* 组件
  * 组件基础深入
  * 组件之间通信
  * 组件抽象与复用
* 状态管理
* react代码分割



[slide]
# 基本语法
* JSX——模板语法
* 组件创建
* 组件的states和props
* 组件生命周期
* 事件系统




[slide]
# JSX——模板语法
* 实际`jsx`语法 最终被`babel`转换为 `createElement`

```
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```
```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
```
//React.createElement()返回
// 注意: 以下示例是简化过的（不代表在 React 源码中是这样）
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```



[slide]
## 思考：
* Vue 模板语法最终转为`render`语法 {:&.rollIn}
* react 与vue 对模板语法处理不同
```vue
<h1>{{ blogTitle }}</h1>
```
```vue
render: function (createElement) {
  return createElement('h1', this.blogTitle)
}
```



[slide]
# [组件创建](https://segmentfault.com/a/1190000008402834)
----
1. function
2. React.createClass
3. extends React.Component
4. [extends React.PureComponent](https://www.zcfy.cc/article/why-and-how-to-use-purecomponent-in-react-js-60devs)
<br />
----
* 思考： {:&.rollIn}
* 对比Vue组件创建 
* 组件创建到底做了哪些事？
  * 状态 模板 事件  {:&.rollIn}




[slide]
# 组件的states和props
1. states vs props
    * 很容易将所有的数据都放在states中，一般只把引起UI变化的数据放在states中
2. setState()——异步过程

* 思考：
* 对比 vue vs react {:&.rollIn}
* setState()实现原理

[slide]
## vue render函数



[slide]
[magic data-transition="earthquake"]
# 生命周期
-----
<div class="columns2">
    <img src="/img/sheng.png" height="650">
    <img src="/img/shengVue.png" height="650">
</div>
[/magic]


[slide]
## 思考：
* vue与react生命周期不同 {:&.rollIn}
* `react`中异步数据是`componentWillMount` vs `componentDidUpdate`
  * `componentWillMount`会在组件render之前执行，并且永远都只执行一次 {:&.rollIn}




[slide]
# 事件（交互）
* 基本语法糖
* 参数传递
* event对象



[slide]
## 事件——基本语法糖

```html
<!--html-->
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

```react
<!-- React:
- React事件绑定属性的命名采用驼峰式写法，而不是小写。
- 如果采用 JSX 的语法你需要传入一个函数作为事件处理函数，而不是一个字符串(DOM元素的写法) -->
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

```vue
<!-- vue -->
<div id="example-2">
  <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>
```



[slide]
# 思考：
  * 对比 原生，react， vue之间区别（思考实现原理，实现相同功能，为什么设计这么不同） {:&.rollIn}
  * 为什么在HTML中监听事件？
   1. react组件和vue模板本质是JavaScript {:&.rollIn}
   2. 最核心是，因为组件系统，将模板文件抽的很小，复杂度很低。




[slide]
## 事件——参数传递
```html
<!--1.必须是event 2.this表示当前节点-->
<button onclick="console.log(event.type)">
    click me
</button>
```

```react
//版本1
  function handleClick(e) {
    console.log(e);
  }
  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );

```

```react
//版本2——注意this
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>//1.里面再封装一层 2.事件对象e要放在最后
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
//对比 react与 vue中this在事件中的指向问题！
```

```vue
<div id="example-3">
  <button v-on:click="say('hi', $event)">Say hi</button>
  <button v-on:click="say('what', $event)">Say what</button>
</div>
```




[slide]
**思考：为什么语法会如此之不同，特别是`vue`,`react`之间？**




[slide]
#事件——event对象
原生Event

```javascript
preventDefault()  //取消默认行为
stopPropagation() //阻止冒泡
```

React Event 对象

> 1. 是一个合成事件。React 根据 [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/) 来定义这些合成事件，所以你不需要担心跨浏览器的兼容性问题。
> 2. e.nativeEvent  //得到原生事件对象

Vue

```vue
// v-on 提供了事件修饰符
.stop
.prevent
.capture
.self
.once
.passive
```
[slide]
# 一般开发流程
## 很大难点是——将prd转换为组件系统
> 可以简单将jquery时代页面理解为一个页面就是一个大组件

<div class="columns1">
    <img src="/img/components.png" height="955">
   
</div>




[slide]
## [一般开发流程](https://react.docschina.org/docs/thinking-in-react.html)

1. 确定原型与JSON接口

2. 按照UI抽组件(边界问题)

3. 用React创建一个静态版本

   只有props，而不适用State

   自底向上（较大）|| 自顶而下（较小）

4. 设置state（最小原则）

   props  vs state

5. 确定State位于哪个组件(一般是父子)

6. 添加子向父的数据流




[slide]
# 组件深入
* 组件之间通信（组件之间关系）
* 组件抽象与复用
* ref及DOM操作


[slide]
## 组件之间通信
1. 父子
2. 兄弟
3. 祖孙
4. 任意组件


[slide]
## 组件之间通信-父子
* [父->子](https://codepen.io/youzaiyouzai666/pen/XoOPda?editors=1010)
* [子—>父]

[slide]
## 组件之间通信-祖孙(Context)
```react
// 创建一个 theme Context,  默认 theme 的值为 light
const ThemeContext = React.createContext('light');

function ThemedButton(props) {
  // ThemedButton 组件从 context 接收 theme
  return (
    <ThemeContext.Consumer>
      {theme => <Button {...props} theme={theme} />}
    </ThemeContext.Consumer>
  );
}
// 中间组件
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}
class App extends React.Component {
  render() {
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

```




[slide]
## 组件之间通信-兄弟
```react
class Container extends Component {
  setValue = value => {
    this.setState({value})
  }
  render() {
    return (
      <div>
        <A setValue={this.setValue}/>
        <B value={this.state.value} />
      </div>
    );
  }
}
// 兄弟A
class A extends Component {
  handleClick = () => {
    const { setValue } = this.props;
    setValue(this.value);
  }
  render() {
    return (
      <div className="card">
        我是Brother A, <input onChange={this.handleChange} />
        <div className="button" onClick={this.handleClick}>通知</div>
      </div>
    )
  }
}
// 兄弟B
const B = props => (...);
export default B;
```




[slide]
## 组件之间通信-任意组件间

* 共同祖先
* 观察者模式
* 第三方状态管理

[slide]
# 组件抽象复用
## 通用 灵活 *高内聚* 低耦合
[note]高内聚：一个程序有50个函数，这个程序执行得非常好；然而一旦你修改其中一个函数，其他49个函数都需要做修改，这就是高耦合的后果[/note]
* 继承 vs 组合
* 高阶组件
* Render Props
* Hooks


[slide]
## 继承 vs 组合
* 继承是`A is B`（子类必须是父类，子类是父类更小的。）
* 组合是`A has B`(拥有)

[note]
难点是，父类不好确认。
卡车和轿车父类是汽车；但人和汽车的父类不好确认。
人和汽车都`拥有`跑的功能
[/note]



[slide]
## 高阶组件
* 创建方式
  * 调用传入的组件
  * 继承传入组件
  * 装饰器模式
 
[slide]
## 高阶组件创建
```react
export default simpleHoc = WrappedComponent => {
  console.log('simpleHoc');
  return class extends Component {
    render() {
      return <WrappedComponent {...this.props}/>
    }
  }
}
//使用
class Usual extends Component{}
export default simpleHoc(Usual);
```

```react
export default simpleHoc2 = WrappedComponent => {
  console.log('simpleHoc');
  return class extends WrappedComponent {
  }
}
//使用
class Usual extends Component{}
export default simpleHoc2(Usual);
```

```react
import React, { Component } from 'react';
import simpleHoc from './simple-hoc';

@simpleHoc
export default class Usual extends Component {
  render() {
    return (
      <div>
        Usual
      </div>
    )
  }
}
```

[slide]
## 高阶组件与mixin 对比
[vue mixin](https://cn.vuejs.org/v2/guide/mixins.html)
```vue
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```
<div class="columns1">
    <img src="/img/mixin.png" height="655">
</div>



[slide]
[magic data-transition="earthquake"]
## [Render Props](https://react.docschina.org/docs/render-props.html)
跟踪鼠标位置-对比
-----
<div class="columns2">
    <img src="/img/render-base.png" height="650">
    <img src="/img/render-props.png" height="650">
</div>
[/magic]
[note]
三个实体，实体之间关系。
[/note]

[slide]
## [Render Props](https://react.docschina.org/docs/render-props.html)
跟踪鼠标位置——高阶组件
```react
// If you really want a HOC for some reason, you can easily
// create one using a regular component with a render prop!
function withMouse(Component) {
  return class extends React.Component {
    render() {
      return (
        <Mouse render={mouse => (
          <Component {...this.props} mouse={mouse} />
        )}/>
      );
    }
  }
}

class Car extends Component{}
exprot withMouse(Car)
```

[slide]
# ref及DOM操作
大多数情况下，不需要直接操作DOM。ref提供了操作DOM的能力
```react
function CustomTextInput(props) {
  // 这里必须声明 textInput，这样 ref 回调才可以引用它
  let textInput = null;

  function handleClick() {
    textInput.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={(input) => { textInput = input; }} />

      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );  
}
```

[slide]
# 后续
* 状态管理
* 代码分割
* 性能优化
* 服务端渲染