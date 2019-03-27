title: react状态管理
speaker: 曹毅
url: https://github.com/youzaiyouzai666
transition: slide3
files: /js/index.js,/css/demo.css,/js/zoom.js
theme: moon
usemathjax: yes




[slide]
# react状态管理
## 学习过程中的思考与总结
### 不同的状态管理方案解决什么问题？
<br />
<small style="vertical-align:middle;display:inline-block"><iframe src="http://ghbtns.com/github-btn.html?user=youzaiyouzai666&repo=blog&type=watch&count=true" allowtransparency="true" frameborder="0" scrolling="0" width="110" height="20" style="width:110px;height:20px;  background-color: transparent;"></iframe><iframe src="http://ghbtns.com/github-btn.html?user=youzaiyouzai666&repo=blog&type=fork&count=true" allowtransparency="true" frameborder="0" scrolling="0" width="110" height="20" style="width:110px;height:20px;  background-color: transparent;"></iframe><iframe src="http://ghbtns.com/github-btn.html?user=youzaiyouzai666&repo=blog&type=follow&count=false" allowtransparency="true" frameborder="0" scrolling="0" width="170" height="20" style="width:170px;height:20px;  background-color: transparent;"></iframe></small>




[slide]
## 主要内容
----
* 几个概念
  * 单向数据流动  vs 双向数据流动(props)
  * [单向数据绑定 vs 双向数据绑定](https://www.zhihu.com/question/49964363)
  * [states不可变性](https://react.docschina.org/tutorial/tutorial.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E5%8F%AF%E5%8F%98%E6%80%A7%E5%9C%A8react%E5%BD%93%E4%B8%AD%E9%9D%9E%E5%B8%B8%E9%87%8D%E8%A6%81)
  
* 基本语法
  * 组件内状态管理
    * props vs states
    * 受控组件 vs 非受控组件(表单)
  * setState()
    * 使用及API
    * setState()到底做了什么？
  * 组件间通信——单向数据流
* 第三方状态管理（以mobx为例）
  * 为什么需要第三方状态管理
  * 第三方管理做了什么事
  * 第三方状态管理如何与react结合
  * mobx

[slide]
## 几个概念-单向数据流动
<div class="columns2">
<img src="/img/dan.jpg" height="500">
<img src="/img/dan2.png" height="500">
</div>
* 单向数据流动视角——组件间数据流动
* 单向数据流，并不是说，数据不能从子流向父，只是传递有些限制，比较麻烦

[slide]
## 几个概念-数据绑定
<div class="columns2">
  <img src="/img/bang-dan.jpeg" hheight="500">
  <img src="/img/bang-shuang.jpeg" height="500">
</div>
* [react单向绑定](https://codepen.io/gaearon/pen/VmmPgp?editors=0010);[vue双向绑定](https://codepen.io/youzaiyouzai666/pen/PRgROv)
* v-model本身就是一个语法糖

[slide]
## [几个概念-states不可变性](https://react.docschina.org/tutorial/tutorial.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E5%8F%AF%E5%8F%98%E6%80%A7%E5%9C%A8react%E5%BD%93%E4%B8%AD%E9%9D%9E%E5%B8%B8%E9%87%8D%E8%A6%81)

```javascript
//直接改数据
var player = {score: 1, name: 'Jeff'};
player.score = 2;

functin a(){
  /**  **/
  player.score = 6;
  /**  **/

}
functin b(){
  /**  **/
  player.score = 6;
  /**  **/
}

function action(num){
  player.score = num;
}
```
```javascript
//替换改数据
var player = {score: 1, name: 'Jeff'};
var newPlayer = Object.assign({}, player, {score: 2});
```
* 两种方式的结果是一样的，但是第二种并没有改变之前已有的数据。
* 通过这样的方式，我们可以得到以下几点好处：
  * 很轻松地实现 撤销/重做以及时间旅行
  * 记录变化
  * 在 React 当中判定何时重新渲染

[slide]
## 现在 明白这几个概念了吗？
  * 单向数据流动  vs 双向数据流动(props)
  * [单向数据绑定 vs 双向数据绑定](https://www.zhihu.com/question/49964363)
  * [states不可变性](https://react.docschina.org/tutorial/tutorial.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E4%B8%8D%E5%8F%AF%E5%8F%98%E6%80%A7%E5%9C%A8react%E5%BD%93%E4%B8%AD%E9%9D%9E%E5%B8%B8%E9%87%8D%E8%A6%81)

[slide]
# 基本语法
----
* 组件内状态管理
    * props vs states
    * 受控组件 vs 非受控组件(表单)
* setState()
    * 使用及API
    * setState()到底做了什么？
* 组件间通信

[slide]
----
# 受控组件 vs 非受控组件(表单)
* HTML表单元素与React中的其他DOM元素有所不同,因为表单元素生来就保留一些内部状态



[slide]
----
[不受控](https://react.docschina.org/docs/uncontrolled-components.html)
```react
//无法实时的响应式的拿到value
class Form extends Component {
  handleSubmitClick = () => {
    const name = this._name.value;
  }
  render() {
    return (
      <div>
        <input type="text" ref={input => this._name = input} />
        <button onClick={this.handleSubmitClick}>Sign up</button>
      </div>
    );
  }
}
```
[受控](https://codepen.io/youzaiyouzai666/pen/maNeYr?editors=0010)
```react
class Form extends Component {
  constructor() {
    super();
    this.state = { name: '',};
  }
  handleNameChange = (event) => {
    this.setState({ name: event.target.value });
  };
  render() {
    return (
      <div>
        <input type="text" value={this.state.name}
          onChange={this.handleNameChange} />
      </div>
    );
  }
}
```
* 对比区别

[slide]
[magic data-transition="earthquake"]
----
<img src="/img/constroll.png" height="500">

* 每个表单元素都有一个不同的prop用于设置该值:

| Element                     | Value property         | Change callback | New value in the callback |
| --------------------------- | ---------------------- | --------------- | :------------------------ |
| `<input type="text" />`     | `value="string"`       | `onChange`      | `event.target.value`      |
| `<input type="checkbox" />` | `checked={boolean}`    | `onChange`      | `event.target.checked`    |
| `<input type="radio" />`    | `checked={boolean}`    | `onChange`      | `event.target.checked`    |
| `<textarea />`              | `value="string"`       | `onChange`      | `event.target.value`      |
| `<select />`                | `value="option value"` | `onChange`      | `event.target.value`      |


[/magic]

[slide]
----
## setState——实践1
* 基本 {:&.rollIn}
```react
//输出是什么
() => {
    this.setState({ count: this.state.count + 1 })
    this.setState({ count: this.state.count + 1 })
    this.setState({ count: this.state.count + 1 })
}
this.state.count
```
[参见](https://codepen.io/youzaiyouzai666/pen/gqwpZV?editors=0010)

* 实际等同
```react
Object.assign(  
    {},
    { count: this.state.count + 1 },
    { count: this.state.count + 1 },
    { count: this.state.count + 1 },
)
```
* 要实现输出为3 如何做？

[slide]
----
## setState——实践1
> setState(stateChange|updater, [calback])
/*
*第一个参数  
*  stateChange 一个参数——仅是将stateChange浅合并到新状态中
*  updater 一个function  (prevState, props) => stateChange //prevState表示最新state
*第二个参数
*  [calback]——表示DOM批处理完成后再执行,类似vue $nextTick( [callback] )
*/

```react
//方案一
setState((prevState, props)=>{
    count: prevState.count+1
})
```

```react
//方案二
this.setState(({ count: this.state.count + 1 },()=>{
    this.setState(({ count: this.state.count + 1 },()=>{
    	this.setState(({ count: this.state.count + 1 })
	})
})
```

[slide]
----
## [setState——实践2](https://codepen.io/youzaiyouzai666/pen/zeKvZJ)
```react
//输出是什么？
class Example extends React.Component {
  constructor() {
    super();
    this.state = {
      val: 0
    };
  }
  componentDidMount() {
    this.setState({val: this.state.val + 1});
    console.log(this.state.val);    // 第 1 次 log

    this.setState({val: this.state.val + 1});
    console.log(this.state.val);    // 第 2 次 log

    setTimeout(() => {
      this.setState({val: this.state.val + 1});
      console.log(this.state.val);  // 第 3 次 log

      this.setState({val: this.state.val + 1});
      console.log(this.state.val);  // 第 4 次 log
    }, 0);
  }
};
```
* 解释(执行上下文):  {:&.rollIn}
* 当不在react掌控之内（setTimeout/addEventListener）中，则是一个同步过程。

[slide]
----
## setState——实现功能
1. 批处理
2. 更新如何触发
3. 异步

[slide]
----
## setState Promise 化思考
因为 `setState()`本身是`callback`回调，会引起**回调地狱** 

* 实现  {:&.rollIn}
```react
setStateAsync(state){
    return new Promise((resolve)=>{
        this.setState(state,resolve)
    })
}
```

[slide]
# 第三方状态管理（以mobx为例）
----
  * 为什么需要第三方状态管理
  * 第三方管理做了什么事
  * 第三方状态管理如何与react结合
  * mobx

[slide]
  [magic data-transition="earthquake"]
  ## 为什么需要第三方状态管理
  -----
  <div class="columns2">
      <img src="/img/no-status2.webp" height="650">
      <img src="/img/status2.webp" height="650">
  </div>
  [/magic]

[slide]
## 第三方管理需要做什麼？
### react 本身的 `data--->DOM` 过程
  -----
  <figure><img  src="/img/xuanran.jpeg" height="400" /></figure>

[slide]
  ## mobx
  <figure><img src="/img/mobx.png" class="aligncenter" /></figure>
  * 当应用状态更新时，所有依赖于这些应用状态的监听者（包括UI、服务端数据同步函数等），都应该自动得到细粒度地更新。
  * 通过使用@observer，将react组件转换成一个监听者(Reactions)，这样在被监听的应用状态变量(Observable)有更新时，react组件就会重新渲染

