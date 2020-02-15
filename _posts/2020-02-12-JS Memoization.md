---
layout: post
title: JS Memoization
subtitle: React.memo 的原理
date: 2020-02-12
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - React
  - memo
  - 性能优化
  - frontend
---


#### 源码
```

function memo(func){
  var cache = {};
    return function(){
      var key = JSON.stringify(arguments);
      if (cache[key]){
        console.log(cache)
        return cache[key];
      }
      else{
        val = func.apply(null, arguments);
        cache[key] = val;
        return val; 
      }
  }
}

```

利用 闭包 的原理，维护一个 cache 的闭包变量，缓存函数的计算结果，使得函数执行更快。

####    应用

```

//斐波那契数列
var fib = memo(function(n) {
   if (n < 2){
     return 1;
   }else{
     
     return fib(n-2) + fib(n-1);
   }
});

```

#####   参考

[Understanding JavaScript Memoization In 3 Minutes](https://codeburst.io/understanding-memoization-in-3-minutes-2e58daf33a19)

