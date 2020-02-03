---
layout: post
title: async await VS Promise
subtitle: async await VS Promise, 实现啰嗦的妻子。
date: 2020-02-04
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - async
  - promise
  - frontend
---

> 场景：啰嗦的妻子<BR/>
  丈夫和妻子去看电影，排队时，妻子却说自己饿了，去便利店买东西；
-   Promise 实现

```
console.log('person1: show ticket')
console.log('person2: show ticket')
console.log('husband: let go in')
console.log('wife:no, i am hungry')
const getBread = new Promise((resolve,reject) => {
    setTimeout(()=>{
        resolve('bread')
    },1000)
})

const getButter = getBread.then((bread)=> {
    console.log(`husband: here is ${bread},let us go in`)
    console.log(`wife: no, i need some butter`)
    return new Promise((resolve,reject) => {
        setTimeout(()=>{
            resolve('butter')
        },1000)
    })
})

const getDrink = getButter.then(butter => {
    console.log(`husband: here is ${butter},let us go in`)
    console.log(`wife: no, i need some drink`)
    return new Promise((resolve,reject) => {
        setTimeout(()=>{
            resolve('drink')
        },1000)
    })
})

getDrink.then(resolve => {
    console.log(resolve);
    console.log('person3: show ticket')
})
console.log('person4: show ticket')
console.log('person5: show ticket')

```
-   运行结果

```
person1: show ticket
VM44:2 person2: show ticket
VM44:3 husband: let go in
VM44:4 wife:no, i am hungry
VM44:29 person4: show ticket
VM44:30 person5: show ticket
VM44:10 husband: here is bread,let us go in
VM44:11 wife: no, i need some butter
VM44:18 husband: here is butter,let us go in
VM44:19 wife: no, i need some drink
VM44:26 drink
VM44:27 person3: show ticket
```

-   async await 实现

```

console.log('person1: show ticket')
console.log('person2: show ticket')


const getBread = new Promise((resolve,reject) => {
    setTimeout(()=>{
        resolve('bread') 
    },1000)
})

// async 函数 return 的为一个 promise 对象
// 异步操作的结果需要放在 promise 对象里
const getButter = async () => {
    return await new Promise((resolve,reject) => {
		setTimeout(()=>{
			resolve('butter')
		},1000)
	})
}


const getDrink = new Promise((resolve,reject) => {
    setTimeout(()=>{
        resolve('drink') 
    },1000)
})


const goIn = async () => {
    console.log('husband: let go in')
    console.log('wife:no, i am hungry')
    
    let bread = await getBread;
    
    console.log(`husband: here is ${bread},let us go in`)
    console.log(`wife: no, i need some butter`)
    
    let butter = await getButter()
    
    console.log(`husband: here is ${butter},let us go in`)
    console.log(`wife: no, i need some drink`)
    
    let drink = await getDrink
    
    console.log(`husband: here is ${drink},let us go`)
    console.log('wife: all right')
}

goIn();
console.log('person4: show ticket')
console.log('person5: show ticket')
```

