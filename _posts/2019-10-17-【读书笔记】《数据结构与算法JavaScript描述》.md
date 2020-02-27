---
layout: post
title: 【读书笔记】《数据结构与算法JavaScript描述》
subtitle: 数据结构：栈、队列、链表、字典、集合、二叉树；算法：二叉树查找、图算法、排序算法
date: 2019-10-17
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - 数据结构
  - 算法
  - 读书笔记
---

## 数组
- JavaScript 是一种特殊的对象，用来表示偏移量的索引是该对象的属性，索引可能是整数，并在内部转换成字符串类型（JavaScrip 对象的属性名必须是字符串）。JavaScript中的数组只是一种特殊对象，所以效率没有其他语言的数据高。
- 字符串生成数组：split 
- 数组生成字符串：join \ toString
- 改变数组：尾部（pop \ push）、头部（shift \ unshift）、排序（sort \ reverse）、从中间插入： splice(开始位置，0，待插入的元素)、从中间删除： splice(开始位置，需要删除的个数)
- 迭代器方法：生成新数组的（map \ filter）不生成新数组的（ forEach \ every \ some \ reduce ）
- 二维数组：按列访问时两层循环，内层循环对应列；按行访问时两层循环，内层循环对应行；

---
## 列表
- 列表是一组有序的数据。每个列表中的数据项成为元素。

```
//List 类的表示
function List(){
    this.listSize = 0;  //列表的元素个数（属性）
    this.pos = 0; //列表的当前位置（属性）
    this.dataStore = []; //初始化一个空数组来保存列表元素
    this.clear = clear;
    this.find = find; //查找元素在列表中的位置，不存在返回 -1
    this.toString = toString;
    this.insert = insert;
    this.append = append;
    this.remove = remove;
    this.front = front;
    this.end = end;
    this.prev = prev;
    this.next = next;
    this.hasNext = hasNext;
    this.hasPrev = hasPrev;
    this.length = length; //返回listSize
    this.moveTo = moveTo;
    this.currPos = currPos;
    this.getElement = getElement; //放回当前元素
    this.contains = contains;
}
```

```
//使用迭代器遍历列表 names
for(names.front(); names.hasNext(); names.next()){
    console.log(names.getElement())
}
```

---
## 栈
- 后入先出 LIFO （last-in-first-out）
- 入栈 push 出栈 pop 
- 返回栈顶元素，而不删除 peek 方法

```
//栈的表示
function Stack(){
    this.dataStore = [];
    this.top = 0; //记录栈顶位置
    this.pop = () =>  this.dataStore[--this.top];
    this.push = (ele) => this.dataStore[this.top++] = ele;
    this.peek = () => this.dataSore[this.top - 1];
    this.clear = () => this.top = 0;
}
```
- 栈的应用一： 回文（判断一个字符串是不是回文）从左往右将每个字母压入栈，连续弹出栈得到新的字符串进行比较；
- 栈的应用二： 递归

```
//递归函数（阶乘）
function factoria(n){
    if(n === 0){
        return 1;
    }
    else{
        return n * factoria(n-1)
    }
}
```

```
//使用栈模拟递归过程
function fact(n){
    let s = new Stack();
    while(n > 1){
        s.push(n--);
    }
    let product = 1;
    while(s.length() > 0 ){
        product *= s.pop(); 
    }
    return product;
}

console.log(factoria(5)) //120
console.log(fact(5)) //120
```

---
## 队列
- 先进先出 （First-In-First-Out, FIFO）
- 入队 enqueue 「push」
- 出队 dequeue  「shift」
- 读取队头元素 front 「this.datastore[0]」
- 读取队尾元素 back 「this.datastore[this.datastore.length - 1 ]」
- 清空队列 clear 「this.datastore = [] 」


```
function Queue() {
    this.datastore = [];
    this.enqueue = (ele) => {
        this.datastore.push(ele)
    };
    this.dequeue = () => {
        this.datastore.shift()
    };
    this.front = () => this.datastore[0];
    this.back = () => this.datastore[this.datastore.length - 1];
    this.empty = () => this.datastore.length == 0;
}
```
##### 应用
- 舞伴分配   
    
    根据舞者的性别分别进入男性队列和女性队列，男队列和女性队列依次出队，搭配成舞伴；知道有一队列为空，则非空队列为等待跳舞的舞者；  

- 基数排序
    
    对于 0～99 的数字，基数排序将数据集扫描两次。第一次按个位上的数字进行排序，第二次按十位上的数字进行排序。    
    需要十个队列，每个对应一个个位数字。将所有的队列保存在一个数组中，使用取余和除法操作决定个位和十位。并将数组加入到相应的队列，根据个位数对其重新排序，然后根据十位上的数值进行排序。


```
function distribute(nums, queues, n, digit){
    for(var i = 0; i< n; ++i){
        if(digit == 1){
            queues[nums[i] % 10].enqueue(nums[i])
        }else{
            queues[Math.floor(nums[i) / 10].enqueue(nums[i])
        }
    }
}   

function collect(queues, nums) {
    var i = 0;
    for(var digit = 0; digit < 10 ; ++ digit){
        while(!queues[digit].empty()){
            nums[i++] = queues[digit].dequeue();
        }
    }
    
}

//主程序
var queues = [];
for ( var i = 0 ;i<10; ++i){
    queue[i] = new Queue();
}

var nums = [32,91,87,36,22,46];

distribute(nums,queues,10,1);
collect(queues,nums);

distribute(nums,queues,10,10);
collect(queues,nums);

console.log(nums.join(''))

```

- 优先队列    
    
    对dequeue 方法重新定义，时期删除队列中拥有最高优先级的元素


---

## 链表

- 数组的缺点：很多编程语言中，数组的长度是固定的，当数组被数据填满时，就无法插入新的元素。另外，添加和删除数组也很麻烦，因为需要将其他元素向前或向后平移。但是 JavaScript 数组不存在这个问题，使用 splice 方法不需要再访问其他元素。 JavaScript 数组的主要问题是，JavaScript 数组被实现成了对象，效率很低！
- 链表的定义：
- 链表靠相互之间的关系进行引用（数组则是靠位置进行引用）。在链表中，我们可以说一个元素在另一个元素的前面，而不能说在第几个。
- 链表的尾元素指向一个 null 节点，链表的最前面有一个头节点。
- 链表支持插入、删除、查找、遍历。

```
// Node 链表的节点
function Node(ele){
    this.ele = ele;
    this.next = null;
}

//LinkedList 
function LList(){
    this.head = new Node('head');
    this.find = (item) => {
        var current = this.head;
        while(current.ele != item){
            current = current.next;
        }
        
        return current;
    };
    //在 item 后面插入 newEle
    this.insert = (newEle, item) => {
        var newNode = new Node(newEle);
        var current = this.find(newEle);
        newNode.next = current.next;
        current.next = newNode;
    };
    //遍历
    this.display = () => {
        var current = this.head;
        while(current.next != null){
            console.log(current);
            current = current.next
        }
    };
    this.findPrev = (item) => {
        var current = this.head;
        while(current.next != null && current.next.ele != item ){
            current = current.next;
        }
        return current
    };
    //删除
    this.remove = (item) => {
        var prevNode = this.findPrev(item);
        while(prevNode.next != null){
            prevNode.next = prevNode.next.next;
        }
    }
}

```

#### 双向链表
- 需要为 Node 类设置一个 previous 属性
- insert 的时候要设置新节点的 previous，并指向该节点的前驱

#### 循环链表
- head.next = head;
- 这种行为会传到至链表的每一个节点，构成循环链表

---

## 字典

- 字典是一种以键-值对形式存储数据对数据结构
- Dictionary 类的基础是 Array 类，而不是 Object 类。主要是因为，JavaScript 中不能对对象对属性进行排序。
- 例，[key1: 213, key2: 989]
```
function Dictionary(){
    this.dataStore = new Array();
    this.add = (key, value) => {
        this.datastore[key] = value;
    };
    this.find = (key) => this.datastore[key];
    this.remove = (key) => delete this.datastore[key];
    this.showAll = () => {
        Object.keys(this.datastore).forEach(key => {
            console.log(key + ' => ' + this.datastore[key])
        })
    }
    
}
```
- Dictionary 类的辅助方法

```
function count (){
    var n = 0;
    for (var key in Object.keys(this.datastore)){
        ++n
    }
    return n 
}

//为什么不能用 length，因为当键的类型为字符串时，length 就不管用类

```

- 排序

```
  let showAll = () => {
        for (var key in Object.keys(this.datastore).sort()){
            console.log(key + ' => ' + this.datastore[key])
        }
       
    }
```

---
## 散列
- 散列是一种常见的数据存储技术
- 散列表是基于数组进行设计的
- 碰撞的概念：即使用一个高效的散列函数，仍会存在两个键映射成同一个值的可能。
- 散列表的数组应该是多大？1.首先长度应该是一个质数   2.长度应该在100以上，常用 137
- 如何选择散列函数？1.简单：以数组的长度对键取余 2.优化：霍纳算法，每次乘以一个质数（常用 31）

```
function HashTable() {
    this.table = new Array(137);
    this.simpleHash = (data) => {
        var total = 0;
        for (var i = 0;i < data.length; ++i){
            total += data.charCodeAt(i)
        }
        
        return total % this.table.length;
    };
    this.put = (data) => {
        var pos = this.simpleHash(data);
        this.table[pos] = data;
    };
    this.get = (key) => {
        return this.table[this.simpleHash(key)];
    };
    this.showDistro = () => {
        for(var i = 0;i < this.table.length; ++i){
            if(this.table[i] != undefined){
                console.log(this.table[i])
            }
        }
    };
    
    //霍纳算法
    this.betterHash = (data) => {
        var total = 0 ;
        var H = 37;
        for(var i = 0; i < data.length; ++i){
            total = total * H + data.charCodeAt(i); 
        }
        return total% this.table.length;
    }
}
```
- 碰撞的处理：1. 开链法（每个散列后数组的元素都是一个数组） 2.线性探索法（碰撞后顺序往下查找下一个空的位置）
- 如何选择？当数组的大小是待散列的数据的两倍及以上时，选择线性探测法，其余的选择开链法
---
## 集合
- 无序、互异

```
function Set(){
    this.dataStore = [];
    this.add = (ele) => {
        if(this.dataStore.indexOf(ele) > -1){
            return
        }
        this.dataStore.push(ele);
    };
    this.remove = (ele) => {
        var pos = this.dataStore.indexOf(ele);
        if(pos > -1){
            this.dataStore.splice(pos,1);
            
            return true;
        }
        
        return false
    };
    //并集
    this.union = (set1) => {
        var unionSet = new Set();
        for(var i = 0; i <set1.dataStore.length; ++i){
            unionSet.add(set1.dataStore[i])
        }
        for(var i = 0; i < this.dataStore.length; ++i){
            unionSet.add(this.dataStore[i])
        }
        return unionSet
    };
    //交集
    this.intersect = (set1) => {
        var intersectSet = new Set();
        for(var i = 0; i < this.dataStore.length; ++i){
            if(set1.dataStore.indexOf(this.dataStore[i] > -1)){
                unionSet.add(set1.dataStore[i])
            }
        }
        
        return intersectSet;
    };
    //补集
    this.difference = (set1) => {
        var differenceSet = new Set();
        for(var i = 0; i < this.dataStore.length; ++i){
            if(set1.dataStore.indexOf(this.dataStore[i] === -1)){
                differenceSet.add(set1.dataStore[i])
            }
        }
        
        return differenceSet;
    }
    
}
```

---
## 二叉树与二叉查找树
- 根节点、父节点、子节点、叶子节点
- 层级（根节点为第 0 层）、路径（从一个节点到另一个节点的这一组边）
- 二叉查找树，相对较小的值保存在左节点，较大的值保存在右节点。
    

```
function Node(data,left,right){
    this.data = data;
    this.left = left;
    this.right = right;

    this.show = () => {
        return this.data;
    }
}

function BST() {
    this.root = null;
    this.insert = (data) =>{
        var n = new Node(data, null, null);
        if(this.root == null){
            this.root = n;
        }
        else {
            var current = this.root;
            var parent;
            while(true){
                parent = current;
                if(data < current.data){
                    current = current.left;
                    if(current == null){
                        parent.left = n;
                        break;
                    }
                }
                else {
                    current = current.right;
                    if(current == null){
                        parent.right = n;
                        break;
                    }
                }
            }
        }
    };
    
}

```
- 中序遍历：按节点上的值，以升序访问

```
function inOrder(node){
    if(node !== null ){
        inOrder(node.left);
        console.log(node.show());
        inOrder(node.right);
    }
}
```

- 先序遍历：先访问根节点，再以同样方式访问左子树和右子树

```
function inOrder(node){
    if(node !== null ){
        console.log(node.show());
        inOrder(node.left);
        inOrder(node.right);
    }
}
```

- 后序遍历：先访问叶子节点，从左子树到右子树，再到根节点    

```
function inOrder(node){
    if(node !== null ){
        inOrder(node.left);
        inOrder(node.right);
        console.log(node.show());
    }
}
```

- 查找最小值和最大值


```
function getMin() {
    var current = this.root;
    while(current.left !== null){
        current = current.left
    }
    return current.data;
}
```

- 查找指定值
    

```
function find (data){
    var current = this.root;
    while(current !== null){
        if(current.data == data){
            return current;
        }else if(data < current.data){
            current = current.left;
        }else{
            current = current.right
        }
    }
    return null
}
```
- 二叉查找树的一个用途是记录一组数据集中数据出现的次数。

---
## 图和图算法
-   图由边的集合和顶点的集合组成。
-   图可以对显示中的很多系统建模，如交通流量建模、局域网和广域网建模。
-   表示顶点：
       
```
function Vertex(label, wasVisited){
    this.label = label;
    this.wasVisited = wasVisited;
}
```
-   表示边： 图的边的表示方法称为邻接表或者邻接表数组，将边存储为由顶点的相邻顶点组成的数组，并以此顶点作为索引
-   图

```
function Graph(v){
    this.vertices = v;
    this.edges = 0;
    this.adj = [];
    for(var i = 0;i < this.vertices; ++i){
        this.adj[i] = [];
        this.adj[i].push('');
    }
    this.addEdge = (v,w) => {
        this.adj[v].push(w);
        this.adj[w].push(v);
        this.edges ++;
    };
    this.showGraph = () => {
        for(var i = 0; i < this.vertices; i++){
            putstr(i + '=>');
            for(var j = 0;j < this.vertices.length; ++j){
                if(this.adj[i][j] != undefined){
                    putstr(this.adj[i][j] + ' ');
                }
                print();
            }
        }
    }
}
```
- 图的搜索：深度优先搜索、广度优先搜索
- 查找最短路径
- 拓扑排序

---

## 排序算法
### 基本算法
- 冒泡排序
- 选择排序
- 插入排序


### 高级算法
- 希尔排序
- 归并排序
- 快速排序：分而治之，递归。
-       1.选择一个基准元素，将列表分成两个子序列；
        2.对列表重新排序，所有小于基准值对元素放在基准值前面，反之后面；
        3.分别对较小元素的子序列和较大元素的子序列重复步骤1和2；

```
function qSort(list){
    var lesser = [];
    var greater = [];
    var pivot = list[0];
    
    for(var i = 1; i < list.length; ++i){
        if(list[i] < pivot){
            lesser.push(list[i])
        }else{
            greater.push(list[i])
        }
    }
    return qSort(lesser).concat(pivot,qSort(greater))
}
```

---
## 检索算法

-   顺序查找
-   二分查找：只适应于有序数据集
-       function binSearch(arr, data){
            let upperBound = arr.length - 1; //上边界
            let lowerBound = 0; //下边界
            
            while(lowerBound <= upperBound){
                let midPoint = (lowerBound + upperBound) / 2;
                if(arr[midPoint] > data){
                    upperBound = midPoint;
                }else if(arr[midPoint] < data){
                    lowerBound = midPoint;
                }else{
                    return midPoint;
                }
            }
        
        }



---
## 高级算法
-   动态规划
-       使用递归虽然简洁，但是效率不高。动态规划是一种与递归相反的技术。递归从顶部开始将问题分解，来解决整个问题。动态规划从底部解决问题，
       将所有小问题解决掉，然后合并成一个整体解决方案。
-       动态规划实例：计算斐波那契数列
        
        //递归函数版       
        function recurFib(n){
            if(n < 2){
                return n;
            }else {
                return recurFib(n-1) + recurFib(n-2)
            }
        }
        //动态规划版       
        function dynFib(n){
            var val = [];
            for(var i = 0; i<= n; ++i){
                val[i] = 0;
            }
            if( n == 1 || n === 2){
                return 1;
            }else {
                val[1] = 1;
                val[2] = 2;
                for(var i = 3; i <= n; ++i){
                    val(i) = val[i-1] + val[i-2];
                }
                return val(n-1) 
            }
        }
-   贪心算法
-       贪心算法总会选择当下的最优解，而不考虑这一次选择会不会对未来的选择造成影响。实现者希望作出的这一系列局部最优能带来最终的整体‘最优选择’





- 背包问题
-       保险箱有 5 件物品，尺寸分别是 3、4、7、8、9，价值分别是 4、5、10、11、13，且背包容积是 16。怎么选择能达到最大价值？
-       //递归函数版
        max(a,b) => a > b ? a : b;
        function knapsack(capacity, size, value, n){
            if(n == 0 || capacity == 0){
                return 0;
            }
            if(size[n-1] > capacity){
                return knapsack(capacity, size, value, n-1);
            }else {
                return max(value[n-1] + knapsack(capacity -size[n-1],size,value,n-1), knapsack(capacity,size,value,n-1) );
            }
        }
        
        var value = [4,5,10,11,13];
        var size = [3,4,7,8,9];
        var capacity = 16;
        var n = 5;
        console.log(knapsack(capacity,size,value,n));

-       //动态规划版
        function dKnapsack(){
            var K = [];
            for(var i = 0; i <= capacity + 1 ; i++){
                K[i] = [];
            }
            for(var i = 0; i <= n;i++){
                for(var w = 0; w < capacity; w++){
                    if(i = 0 || w == 0){
                        K[i][w] = 0;
                    }else if(size[n-1] <= w){
                        K[i][w] = max(value(i-1) + K[i-1][w-size[i-1]], K[i-1][w])
                    }else{
                        K[i][w] = K[i-1][w]
                    }
                    console.log(K[i][w]);
                }
                
            }
            return K[n][capacity];
        }



-       //贪心算法版
        
