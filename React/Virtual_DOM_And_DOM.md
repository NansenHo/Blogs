# Virtual DOM and DOM

## What’s Virtual DOM

```jsx
<div class='app' id='appRoot'>
  <h1 class='title'>欢迎进入React的世界</h1>
  <p>
    React.js 是一个帮助你构建页面 UI 的库
  </p>
</div>
```

上面这个 HTML 所有的信息我们都可以用一个 **JavaScript 对象** 来表示：

```jsx
{
  tag: 'div',
  attrs: { className: 'app', id: 'appRoot'},
  children: [
    {
      tag: 'h1',
      attrs: { className: 'title' },
      children: ['欢迎进入React的世界']
    },
    {
      tag: 'p',
      attrs: null,
      children: ['React.js 是一个构建页面 UI 的库']
    }
  ]
}
```

这段 JavaScript 对象就是 虚拟 DOM 。

## Virtual DOM vs Real DOM

用 DOM， 只要修改一点点信息，就需要重新渲染页面。

用虚拟 DOM，就可以修改哪部分，它只渲染哪部分的页面，速度很快。

## React 高性能的体现：Virtual DOM

在 Web 开发中我们总需要将变化的数据实时反应到 UI 上，这时就需要对 DOM 进行操作。

复杂或频繁的 DOM 操作通常是性能瓶颈产生的原因。

> 如何进行高性能的复杂DOM操作通常是衡量一个前端开发人员技能的重要指标）。

React为此引入了虚拟DOM（Virtual DOM）的机制：

在浏览器端用 Javascript 实现了一套DOM API。

基于 React 进行开发时所有的 DOM 构造都是通过虚拟 DOM 进行，每当数据变化时，React 都会重新构建整个 DOM 树，然后 React 将当前整个 DOM 树和上一次的 DOM 树进行对比，得到 DOM 结构的区别，然后仅仅将需要变化的部分进行实际的浏览器 DOM 更新。

而且，React 能够批处理虚拟 DOM 的刷新，在一个事件循环（Event Loop）内的两次数据变化会被合并。

例如，你连续的先将节点内容从 A->B, B->A，React 会认为 A 变成 B，然后又从 B 变成 A，UI 不发生任何变化，而如果通过手动控制，这种逻辑通常是极其复杂的。

尽管每一次都需要构造完整的虚拟 DOM 树，但是因为 **虚拟 DOM 是内存数据，性能是极高的**，而对实际 DOM 进行操作的仅仅是 Diff 部分，因而能达到提高性能的目的。

这样，在保证性能的同时，开发者将不再需要关注某个数据的变化如何更新到一个或多个具体的 DOM 元素，而只需要关心在任意一个数据状态下，整个界面是如何 Render 的。

## JSX

但是按 JavaScript 对象这样写，太长且不直观，于是 React 为了兼顾两者，就发明了 JSX 语法糖。

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  render () {
    return (
      <div className='app' id='appRoot'>
        <h1 className='title'>欢迎进入React的世界</h1>
        <p>
          React.js 是一个构建页面 UI 的库
        </p>
      </div>
    )
  }
}

ReactDOM.render(
	<App />,
  document.getElementById('root')
)
```

编译的过程会把左边类似 HTML 的 JSX 结构转换成👇🏻的 JavaScript 的对象结构，

但并不是简单的 JavaScript 代码描述，而是有方法调用。

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  render () {
    return (
      React.createElement(
        "div",
        {
          className: 'app',
          id: 'appRoot'
        },
        React.createElement(
          "h1",
          { className: 'title' },
          "欢迎进入React的世界"
        ),
        React.createElement(
          "p",
          null,
          "React.js 是一个构建页面 UI 的库"
        )
      )
    )
  }
}

ReactDOM.render(
	React.createElement(App),
  document.getElementById('root')
)
```