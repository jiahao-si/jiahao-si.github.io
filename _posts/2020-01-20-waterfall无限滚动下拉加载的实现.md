---
layout: post
title: React 长列表优化
subtitle: waterfall无限滚动下拉加载的实现
date: 2020-01-20
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - react
---

#### 思路

- 对于滚动的组件，在 componentDidMount 方法中做判断：

  - 若是初次视口内可见，则直接发请求渲染；
  - 若不可见，则绑定 onScroll 事件，在滚动事件发生后，再次进行视口内可见性判断，若滚动后组件可见，则发请求渲染，并移除绑定的 onScroll 事件

- 对于 onScroll 事件，要做防抖处理；
- 滚动元素在最外层时，要在 window 上添加滚动事件

- 如何判断组件可见？

  - 利用 getClientBoundingReact Api

    -       export const checkVisible = node => {
                if (node) {
                    const { top, bottom, left, right } = node.getBoundingClientRect();
                    return (
                      bottom > 0 &&
                      top < window.innerHeight &&
                      left < window.innerWidth &&
                      right > 0
                    );
                 }
            }

- 如何获取 react 组件的实例？
  - 利用 ref

参考文章：https://juejin.im/post/5d8b54b8e51d45782e603976
