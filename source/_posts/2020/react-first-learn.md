---
title: React 入门教程笔记
date: 2020-05-05 20:21:31
categories:
  - 前端
tags:
  - React
---

阅读本文之前，建议先阅读以下教程：

1. [React 教程 - 菜鸟教程](https://www.runoob.com/react/react-tutorial.html)
2. [React 文档 - 中文](https://react.docschina.org/docs/hello-world.html)

<!-- more -->

## 安装 React

1、下载

```bash
wget https://cdn.staticfile.org/react/16.4.0/umd/react.development.js \
https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js \
https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js
```

react.min.js - React 的核心库
react-dom.min.js - 提供与 DOM 相关的功能
babel.min.js - Babel 可以将 ES6 代码转为 ES5 代码
Babel 内嵌了对 JSX 的支持

> 如果我们需要使用 JSX，则 `<script>` 标签的 type 属性需要设置为 text/babel

2、hello world

```html
<script src="./react.development.js"></script>
<script src="./react-dom.development.js"></script>
<script src="./babel.min.js"></script>

<div id="app"></div>

<script type="text/babel">
  ReactDOM.render(<h1>Hello, world!</h1>, document.getElementById("app"));
</script>
```

## React 元素渲染

1、创建元素
元素是构成 React 应用的最小单位，用于描述屏幕上输出的内容

```js
const element = <h1>Hello, world!</h1>;

// 将元素渲染到 DOM 中
ReactDOM.render(element, document.getElementById("app"));
```

2、更新元素
React 元素都是不可变的, 更新界面的唯一办法是创建一个新的元素

计时器示例

```js
function tick() {
  const element = <h2>现在时刻：{new Date().toLocaleString()}</h2>;

  ReactDOM.render(element, document.getElementById("app"));
}

setInterval(tick, 1000);
```

2、封装成函数

```js
function Clock(props) {
  return <h2>现在时刻：{props.date.toLocaleString()}</h2>;
}

function tick() {
  ReactDOM.render(<Clock date={new Date()} />, document.getElementById("app"));
}

setInterval(tick, 1000);
```

4、创建 React.Component 子类

```js
class Clock extends React.Component {
  render() {
    return <h2>现在时刻：{this.props.date.toLocaleString()}</h2>;
  }
}

function tick() {
  ReactDOM.render(<Clock date={new Date()} />, document.getElementById("app"));
}

setInterval(tick, 1000);
```

## JSX

1、使用 JavaScript 表达式

```js
ReactDOM.render(<h1>{1 + 1}</h1>, document.getElementById("app"));
```

可以使用 conditional (三元运算) 表达式

2、样式
React 推荐使用 camelCase 语法设置内联样式

```js
const style = {
  fontSize: 20,
  color: "#FF0000",
};

ReactDOM.render(
  <h1 style={style}>hello world!</h1>,
  document.getElementById("app")
);
```

3、注释

```js
ReactDOM.render(
  <div>
    <h1>hello world!</h1>
    {/*注释*/}
  </div>,
  document.getElementById("app")
);
```

数组

数组会自动展开所有成员

```js
var arr = [<h1>hello</h1>, <h1>world!</h1>];

ReactDOM.render(<div>{arr}</div>, document.getElementById("app"));
```

## 组件

原生 HTML 元素名以小写字母开头，
自定义的 React 类名以大写字母开头

class 属性需要写成 className ，
for 属性需要写成 htmlFor

1、函数定义组件

```js
function MyComponent(props) {
  return <h1>{props.name}</h1>;
}

ReactDOM.render(<MyComponent name="Tom" />, document.getElementById("app"));
```

2、ES6 class 定义组件

```js
class MyComponent extends React.Component {
  render() {
    return <h1>{this.props.name}</h1>;
  }
}

ReactDOM.render(<MyComponent name="Tom" />, document.getElementById("app"));
```

3、复合组件

```js
function Name(props) {
  return <h1>名称：{props.name}</h1>;
}

function Url(props) {
  return <h1>网址：{props.url}</h1>;
}

function App() {
  return (
    <div>
      <Name name="百度" />
      <Url url="http://www.baidu.com" />
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("app"));
```

## State(状态)

计时器每秒更新一次

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() };
  }

  // 挂载
  componentDidMount() {
    this.timerID = setInterval(() => {
      this.tick();
    }, 1000);
  }

  // 卸载
  componentWillUnMount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date(),
    });
  }

  render() {
    return <h1>{this.state.date.toLocaleString()}</h1>;
  }
}

ReactDOM.render(<MyComponent />, document.getElementById("app"));
```

## Props

props 不可变, 用来传递数据

state 用来更新和修改数据

```js
function Name(props) {
  return <h1>{props.name}</h1>;
}

ReactDOM.render(<Name name="Tom" />, document.getElementById("app"));
```

## 事件处理

事件绑定属性的命名采用驼峰式写法

```js
function Link() {
  function handleClick() {
    console.log("按钮被点击了");
  }

  return <button onClick={handleClick}>按钮</button>;
}

ReactDOM.render(<Link />, document.getElementById("app"));
```

按钮点击开启和关闭

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isToggleOn: true,
    };
  }

  handleClick(e) {
    this.setState((prevState) => ({
      isToggleOn: !prevState.isToggleOn,
    }));
  }

  render() {
    return (
      <button onClick={(e) => this.handleClick(e)}>
        {this.state.isToggleOn ? "开启" : "关闭"}
      </button>
    );
  }
}

ReactDOM.render(<Toggle />, document.getElementById("app"));
```

## 条件渲染

```js
function User(props) {
  return <h1>User</h1>;
}

function Guest(props) {
  return <h1>Guest</h1>;
}

function App(props) {
  if (props.role == "user") {
    return <User />;
  } else {
    return <Guest />;
  }
}

ReactDOM.render(<App role="user" />, document.getElementById("app"));
```

## 列表 & Keys

每个列表元素需要分配一个兄弟元素之间的唯一 key

```js
function App(props) {
  const numbers = [1, 2, 3];
  const items = numbers.map((item, index) => {
    return <li key={index}>{item}</li>;
  });
  return items;
}

ReactDOM.render(<App />, document.getElementById("app"));
```

## 组件 API

```js
// 合并状态
setState(object nextState[, function callback])

// 替换状态
replaceState(object nextState[, function callback])

// 合并属性
setProps(object nextProps[, function callback])

// 替换属性
replaceProps(object nextProps[, function callback])

// 强制更新
forceUpdate([function callback])

// 获取DOM节点
DOMElement findDOMNode()

// 组件挂载状态
bool isMounted()
```

点击计数示例

```js
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };

    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState((state) => {
      return { count: state.count + 1 };
    });
  }

  render() {
    return <h1 onClick={this.handleClick}>点击计数：{this.state.count}</h1>;
  }
}

ReactDOM.render(<App />, document.getElementById("app"));
```

## 组件生命周期

三个状态

1. Mounting：已插入真实 DOM
2. Updating：正在被重新渲染
3. Unmounting：已移出真实 DOM

生命周期的方法

1. componentWillMount 在渲染前调用
2. componentDidMount 在第一次渲染后调用
3. componentWillReceiveProps 在组件接收到一个新的 prop (更新后)时被调用
4. shouldComponentUpdate 在组件接收到新的 props 或者 state 时被调用
5. componentWillUpdate 在组件接收到新的 props 或者 state 但还没有 render 时被调用
6. componentDidUpdate 在组件完成更新后立即调用
7. componentWillUnmount 在组件从 DOM 中移除之前立刻被调用

## AJAX

服务端 server.js

```bash
cnpm i express cors
```

```js
const express = require("express");
const cors = require("cors");

const app = express();

app.use(cors());

app.use("/", function (req, res) {
  return res.send({ data: { name: "Tom" } });
});

app.listen(8080, function () {
  console.log("listening: http://127.0.0.1:8080");
});
```

下载 axios 并引入

```
wget https://cdn.bootcdn.net/ajax/libs/axios/0.19.2/axios.min.js
```

```js
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      name: "",
    };
  }

  componentDidMount() {
    axios.get("http://127.0.0.1:8080/").then((res) => {
      this.setState({
        name: res.data.data.name,
      });
    });
  }

  render() {
    return <h1>{this.state.name}</h1>;
  }
}

ReactDOM.render(<App />, document.getElementById("app"));
```

## 表单与事件

```js
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: "",
    };
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(event) {
    this.setState({ value: event.target.value });
  }

  render() {
    return (
      <div>
        <input
          type="text"
          value={this.state.value}
          onChange={this.handleChange}
        ></input>
        <h2>{this.state.value}</h2>
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("app"));
```

## Refs

```js
class App extends React.Component {
  constructor(props) {
    super(props);

    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.refs.input.focus();
  }

  render() {
    return (
      <div>
        <input type="text" ref="input"></input>
        <button onClick={this.handleClick}>focus</button>
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById("app"));
```
