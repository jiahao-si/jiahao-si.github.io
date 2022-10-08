---
layout: post
title: React Refs
subtitle: refs 的创建和访问方法、应用场景
date: 2019-09-29
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - react
  - frontend
---

#### 概述

React Refs 提供了一种方式， 允许我们访问 Dom 节点或是 render 方法创建的 React 元素。

#### 适合使用 Refs 的场景

- 管理焦点，文本选择或媒体播放
- 触发强制动画
- 集成第三方 DOM 库

#### API

- 创建 Refs

  ```
  class MyComponent extends React.Component {
    constructor(props) {
      super(props);
      //在构造函数中，通常分配给实例属性，以便在整个组件中都可以引用它们
      this.myRef = React.createRef();
    }
    render() {
      return <div ref={this.myRef} />;
    }
  }
  ```

  Refs 通过 React.createRef() 创建，并通过 ref 属性附加到 React 元素上。

- 访问 Refs

  ```
      const node = this.myRef.current
  ```

  当 ref 被传递给 React 元素时，对该节点的引用可以在 ref 的 current 属性中被访问。

  ref 的值根据节点的类型而有所不同：

  - 当 ref 属性用于 Html 元素时，通过 React.createRef 创建的 ref 对象接收底层 dom 作为其 current 属性；
  - 当 ref 属性用**于 class 组件**时，ref 对象接收组件的挂载实例作为其 current 属性；

  React 会在组件挂载时给 current 属性传入 DOM 元素，并在组件卸载时传入 null；ref 会在 componentDidMount 和 componentDidUpdate 触发前更新。

- Ref 与 函数组件

  函数组件没有实例，所以不能在函数组件上使用 ref 属性。

  ```
      function MyFunctionComponent() {
        return <input />;
      }

      class Parent extends React.Component {
        constructor(props) {
          super(props);
          this.textInput = React.createRef();
        }
        render() {
          // This will *not* work!
          return (
            <MyFunctionComponent ref={this.textInput} />
          );
        }
      }
  ```

  //如果想要在函数组件使用 Ref，可以使用 forwardRef。

  ```
          const FancyButton = React.forwardRef((props, ref) => (
            <button ref={ref} className="FancyButton">
              {props.children}
            </button>
          ));

          // You can now get a ref directly to the DOM button:
          const ref = React.createRef();
          <FancyButton ref={ref}>Click me!</FancyButton>;
  ```

- 回调 refs

  React 也支持另一种设置 refs 的方式，称为“回调 refs”。

  ```
  class CustomTextInput extends React.Component {
    constructor(props) {
      super(props);

      this.textInput = null;

      this.setTextInputRef = element => {
        this.textInput = element;
      };

      this.focusTextInput = () => {
        // 使用原生 DOM API 使 text 输入框获得焦点
        if (this.textInput) this.textInput.focus();
      };
    }

    componentDidMount() {
      // 组件挂载后，让文本框自动获得焦点
      this.focusTextInput();
    }

    render() {
      // 使用 `ref` 的回调函数将 text 输入框 DOM 节点的引用存储到 React
      // 实例上（比如 this.textInput）
      return (
        <div>
          <input
            type="text"
            ref={this.setTextInputRef}
          />
          <input
            type="button"
            value="Focus the text input"
            onClick={this.focusTextInput}
          />
        </div>
      );
    }
  }
  ```

  不同于传递 createRef() 创建的 ref 属性，你会传递一个函数。这个函数中接受 React 组件实例或 HTML DOM 元素作为参数，以使它们能在其他地方被存储和访问。

> PS. 过时 API：String 类型的 Refs，已过时并可能会在未来的版本被移除。
