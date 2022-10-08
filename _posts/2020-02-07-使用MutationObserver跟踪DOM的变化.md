---
layout: post
title: 使用MutationObserver跟踪DOM的变化
subtitle: 使用MutationObserver跟踪DOM的变化
date: 2020-02-07
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - frontend
---

#### 概述

Mutation Observer API 用来监视 DOM 变动。DOM 的任何变动，比如节点的增减、属性的变动、文本内容的变动，这个 API 都可以得到通知。

概念上，它很接近事件，可以理解为 DOM 发生变动就会触发 Mutation Observer 事件。但是，它与事件有一个本质不同：事件是同步触发，也就是说，DOM 的变动立刻会触发相应的事件；Mutation Observer 则是异步触发，DOM 的变动并不会马上触发，而是要等到当前所有 DOM 操作都结束才触发，这样设计是为了应付 DOM 变动频繁的特点；

#### 用法

- 传入一个函数来创建一个 MutationObserver 实例，每当有变化发生，这个函数将会被调用；
- 被创建的实例将会有三个方法：
  - observe 启动监听
  - disconnect 用来停止观察
  - takeRecords 返用来清除变动记录，即不再处理未处理的变动

#### 示例

```
// Select the node that will be observed for mutations
let targetNode = document.querySelector(`#id`);

// Options for the observer (which mutations to observe)
let config = {
    attributes: true,
    childList: true,
    subtree: true
};

// Callback function to execute when mutations are observed
const mutationCallback = (mutationsList) => {
    for(let mutation of mutationsList) {
        let type = mutation.type;
        switch (type) {
            case "childList":
                console.log("A child node has been added or removed.");
                break;
            case "attributes":
                console.log(`The ${mutation.attributeName} attribute was modified.`);
                break;
            case "subtree":
                console.log(`The subtree was modified.`);
                break;
            default:
                break;
        }
    }
};

// Create an observer instance linked to the callback function
let observer = new MutationObserver(mutationCallback);

// Start observing the target node for configured mutations
observer.observe(targetNode, config);

// Later, you can stop observing
observer.disconnect();


```

[参考 MDN MutationObserver WebApi 文档](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)
