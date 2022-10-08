---
layout: post
title: React VS Vue
subtitle: React VS Vue
date: 2020-02-09
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - React
  - frontend
---

#### 相同点

- 使用 virtual dom
- 提供了响应式和组件化的视图组件
- 将注意力集中在核心库，将其他功能如路由和全局状态管理交给相关的库

---

#### 不同点

- 性能
  - vue 组件的渲染是被系统自动追踪的，精确判断需不需要重新渲染；
  - react 16 之前需要通过 pureComponet 或 shouldComponent， 且需要保证该组件的整个子树的渲染输出都是由该组件的 props 决定; react 16 对此进行了优化；
- html
  - 在 react 中，一切都是 js，html 可以用 jsx 表达（css 也被纳入到 js 中处理） ；
  - 在 vue 中，整体思想还是拥抱传统的前端技术，在其基础上进行扩展；
    - 虽然 vue 也提供了渲染函数，支持 jsx 语法，但是 vue 还是推荐模板的写法；
    - vue 的模板写法学习成本并不高， jsx 也有一定的学习成本；
- 组件作用域内的 css

  - 在 react 中，CSS 作用域是通过 CSS-in-JS 的方案实现的
    - 比如 styled-components、glamorous 和 emotion；
    - 这引入了面向组件的样式开发规范，这与普通的 css 开发是有区别的；
    - 这提供了 js 处理 css 的灵活性，但是却增加了 bundle 的尺寸和性能（虽然可以将 css 代码提取到一个单独的文件中，但是 bundle 里需要一个运行时程序来使得这些 css 生效）
  - Vue 设置样式的默认方法是同一组件文件内的 stle 标签

    ```
    <style scoped>
      @media (min-width: 250px) {
        .list-container:hover {
          background: orange;
        }
      }
    </style>

    //这个可选 scoped 属性会自动添加一个唯一的属性 (比如 data-v-21e5b78) 为组件内 CSS 指定作用域，编译的时候 .list-container:hover 会被编译成类似 .list-container[data-v-21e5b78]:hover。
    ```

- 规模
  - 向上扩展
    - React 和 Vue 都提供了强大的路由和状态管理库。不同的是 React 将此交给社区维护，而 Vue 则是官方维护和核心库同步更新。
    - Vue Cli 貌似比 React Cli （create-react-app）更灵活；
  - 向下扩展

    - React 学习曲线更陡峭，学习 React 需要知道 JSX 、ES6、 构建工具；
    - Vue 向上扩展好比 React 一样，Vue 向下扩展后就类似于 jQuery。你只要把如下标签放到页面就可以运行

    ```
        <script src="https://cdn.jsdelivr.net/npm/vue"></script>

    ```
- 原生渲染
  - React 有 React Native
  - Vue 和 Weex 合作，Weex 已支持 Vue 语法

---

[参考 Vue 官方文档](https://cn.vuejs.org/v2/guide/comparison.html)
