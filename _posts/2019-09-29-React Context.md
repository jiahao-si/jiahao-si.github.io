---
layout: post
title: React Context
subtitle: 一种跨组件状态共享的方法，context api 及 高级用法
date: 2019-09-29
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - FE
---

#### 概述

全局变量管理，react 的思想是向下传递的单向数据流，如果一个组件嵌套很深，必须一层层传递 prop；一种解决方案是用 redux，但是很复杂，没必要搞这么复杂；context 相对轻量级；

#### API

- React.createContext

  ```
  const MyContext = React.createContext(defaultValue);
  ```

  创建一个 Context 对象

- Context.Provider

  ```
  <MyContext.Provider value={/* 某个值 */}>
  ```

  每个 Context 对象都会返回一个 Provider React 组件，它允许消费组件订阅 context 的变化

- Class.contextType

  ```
  class MyClass extends React.Component {
    static contextType = MyContext;
    render() {
      let value = this.context;
      /* 基于这个值进行渲染工作 */
    }
  }
  ```

  挂载在 class 上的 contextType 属性会被重赋值为一个由 React.createContext() 创建的 Context 对象。这能让你使用 this.context 来消费最近 Context 上的那个值。你可以在任何生命周期中访问到它，包括 render 函数中。

- Context.Consumer

  ```
  <MyContext.Consumer>
    {value => /* 基于 context 值进行渲染*/}
  </MyContext.Consumer>
  ```

  这需要函数作为子元素（function as a child）这种做法。这个函数接收当前的 context 值，返回一个 React 节点。

- 函数组件的 context hooks useContext
- 示例代码

  ```
  // Context 可以让我们无须明确地传遍每一个组件，就能将值深入传递进组件树。
  // 为当前的 theme 创建一个 context（“light”为默认值）。
  const ThemeContext = React.createContext('light');

  class App extends React.Component {
    render() {
      // 使用一个 Provider 来将当前的 theme 传递给以下的组件树。
      // 无论多深，任何组件都能读取这个值。
      // 在这个例子中，我们将 “dark” 作为当前的值传递下去。
      return (
        <ThemeContext.Provider value="dark">
          <Toolbar />
        </ThemeContext.Provider>
      );
    }
  }

  // 中间的组件再也不必指明往下传递 theme 了。
  function Toolbar(props) {
    return (
      <div>
        <ThemedButton />
      </div>
    );
  }

  class ThemedButton extends React.Component {
    // 指定 contextType 读取当前的 theme context。
    // React 会往上找到最近的 theme Provider，然后使用它的值。
    // 在这个例子中，当前的 theme 值为 “dark”。
    static contextType = ThemeContext;
    render() {
      return <Button theme={this.context} />;
    }
  }
  ```

#### 高级用法

- 如何在嵌套组件中更新 context？

  ```
  //theme-context.js
  // 确保传递给 createContext 的默认值数据结构是调用的组件（consumers）所能匹配的！
  export const ThemeContext = React.createContext({
    theme: themes.dark,
    toggleTheme: () => {},
  });

  //theme-toggler-button.js

  import {ThemeContext} from './theme-context';

  function ThemeTogglerButton() {
    // Theme Toggler 按钮不仅仅只获取 theme 值，它也从 context 中获取到一个 toggleTheme 函数
    return (
      <ThemeContext.Consumer>
        {({theme, toggleTheme}) => (
          <button
            onClick={toggleTheme}
            style={{backgroundColor: theme.background}}>

            Toggle Theme
          </button>
        )}
      </ThemeContext.Consumer>
    );
  }

  export default ThemeTogglerButton;

  //app.js
  import {ThemeContext, themes} from './theme-context';
  import ThemeTogglerButton from './theme-toggler-button';

  class App extends React.Component {
    constructor(props) {
      super(props);

      this.toggleTheme = () => {
        this.setState(state => ({
          theme:
            state.theme === themes.dark
              ? themes.light
              : themes.dark,
        }));
      };

      // State 也包含了更新函数，因此它会被传递进 context provider。
      this.state = {
        theme: themes.light,
        toggleTheme: this.toggleTheme,
      };
    }

    render() {
      // 整个 state 都被传递进 provider
      return (
        <ThemeContext.Provider value={this.state}>
          <Content />
        </ThemeContext.Provider>
      );
    }
  }

  function Content() {
    return (
      <div>
        <ThemeTogglerButton />
      </div>
    );
  }

  ReactDOM.render(<App />, document.root);
  ```

- 如何消费多个 context？

  ```
  // Theme context，默认的 theme 是 “light” 值
  const ThemeContext = React.createContext('light');

  // 用户登录 context
  const UserContext = React.createContext({
    name: 'Guest',
  });

  class App extends React.Component {
    render() {
      const {signedInUser, theme} = this.props;

      // 提供初始 context 值的 App 组件
      return (
        <ThemeContext.Provider value={theme}>
          <UserContext.Provider value={signedInUser}>
            <Layout />
          </UserContext.Provider>
        </ThemeContext.Provider>
      );
    }
  }

  function Layout() {
    return (
      <div>
        <Sidebar />
        <Content />
      </div>
    );
  }

  // 一个组件可能会消费多个 context
  function Content() {
    return (
      <ThemeContext.Consumer>
        {theme => (
          <UserContext.Consumer>
            {user => (
              <ProfilePage user={user} theme={theme} />
            )}
          </UserContext.Consumer>
        )}
      </ThemeContext.Consumer>
    );
  }
  ```

- 函数组件中的 useContext

```
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);

  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

useContext 的参数必须是 context 对象本身
