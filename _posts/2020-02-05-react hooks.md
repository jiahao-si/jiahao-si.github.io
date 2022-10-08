---
layout: post
title: react hooks
subtitle: useState、useEffect 模拟生命周期、自定义hook实现逻辑共享
date: 2020-02-05
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - FE
---

- React Hooks 的定位
-       1.可以在函数组件使用更多的 React 的特性， return JSX、state 管理、声明周期；
        2.更容易的抽离公共逻辑函数，如请求数据 useHttp、设置订阅 的 Hook；

- React Hook 的定义
-       1.hook 是钩子的意思；
       2.hook 总是以 use 开头；

---

- useState
-        const [count, setCount] = useState(0);
        一般来说，函数退出后变量会消失，但是 state 里的变量会被 React 保留；

- useEffect
-       可以看成是componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合：
       componentDidMount <==> useEffect(() => {},[]);
        componentDidUpdate <==> useEffect(() => {},[dependencies]);
        componentWillUnmount <==> useEffect(() => {return ()=>{}},[]);

-       如果要模拟 shouldComponentUpdate，可以用 React.memo(函数组件，[] )

---

- React 提供的 Hook
-       基础Hook：
        useState
        useEffect
        useContext
-       额外的 Hook：
        useReducer
        useCallback
        useMemo
        useRef
        useImperativeHandle
        useLayoutEffect
        useDebugValue

---

- 自定义一个 Hook
-       //定义
       funcion useHttp(url,dependencies){
            const [isLoading,setIsLoading] = useState(false);
            const [fetchedData,setFetchedData] = useState(null);

            useEffect(() => {
                setIsLoading = true;
                ferch(url)
                    .then(res => {
                        setIsLoading = false;
                        setFetchedData(res.data);
                    })
                    .catch(err => {
                        setIsLoading = false;
                    })
            },dependecies)

            return [isLoading, fetchedData];
        }

        //使用 Hook
        const [isLoading, fetchedData] = useHttp('backend.cn/api',selectedTarget)
