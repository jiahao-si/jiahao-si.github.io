---
layout: post
title: React源码解读之packages_react.md
subtitle: React源码解读之packages_react
date: 2020-03-17
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - js
  - react
---

> React 代码主要逻辑都是在 Packages 这个文件夹下，今天我们来解读下这个文件夹下的 React 目录。

### React

#### 位置

react.js

#### 输出接口

```
export {
  Children,
  createRef,
  Component,
  PureComponent,
  createContext,
  forwardRef,
  lazy,
  memo,
  useCallback,
  useContext,
  useEffect,
  useImperativeHandle,
  useDebugValue,
  useLayoutEffect,
  useMemo,
  useMutableSource,
  useReducer,
  useRef,
  useState,
  REACT_FRAGMENT_TYPE as Fragment,
  REACT_PROFILER_TYPE as Profiler,
  REACT_STRICT_MODE_TYPE as StrictMode,
  REACT_SUSPENSE_TYPE as Suspense,
  createElement,
  cloneElement,
  ReactVersion as version,
  ReactSharedInternals as __SECRET_INTERNALS_DO_NOT_USE_OR_YOU_WILL_BE_FIRED,
  // Deprecated behind disableCreateFactory
  createFactory,
  // Concurrent Mode
  useTransition,
  useDeferredValue,
  REACT_SUSPENSE_LIST_TYPE as SuspenseList,
  withSuspenseConfig as unstable_withSuspenseConfig,
  // enableBlocksAPI
  block,
  // enableDeprecatedFlareAPI
  useResponder as DEPRECATED_useResponder,
  createResponder as DEPRECATED_createResponder,
  // enableFundamentalAPI
  createFundamental as unstable_createFundamental,
  // enableScopeAPI
  createScope as unstable_createScope,
  // enableJSXTransformAPI
  jsx,
  jsxs,
  // TODO: jsxDEV should not be exposed as a name. We might want to move it to a different entry point.
  jsxDEV,
};
```

### react-element

#### 位置

ReactElement.js

#### 输出接口

jsx 方法、createElement 方法

- jsx 方法

```
    export function jsx(type, config, maybeKey) {
      let propName;

      // Reserved names are extracted
      const props = {};

      let key = null;
      let ref = null;


      if (maybeKey !== undefined) {
        key = '' + maybeKey;
      }

      if (hasValidKey(config)) {
        key = '' + config.key;
      }

      if (hasValidRef(config)) {
        ref = config.ref;
      }

      // Remaining properties are added to a new props object
      for (propName in config) {
        if (
          hasOwnProperty.call(config, propName) &&
          !RESERVED_PROPS.hasOwnProperty(propName)
        ) {
          props[propName] = config[propName];
        }
      }

      // Resolve default props
      if (type && type.defaultProps) {
        const defaultProps = type.defaultProps;
        for (propName in defaultProps) {
          if (props[propName] === undefined) {
            props[propName] = defaultProps[propName];
          }
        }
      }

      return ReactElement(
        type,
        key,
        ref,
        undefined,
        undefined,
        ReactCurrentOwner.current,
        props,
      );
    }

```

- createElement
  jsx 方法实质被转化成 createElement 方法

```
export function createElement(type, config, children) {
  let propName;

  // Reserved names are extracted
  const props = {};

  let key = null;
  let ref = null;
  let self = null;
  let source = null;

  if (config != null) {
    if (hasValidRef(config)) {
      ref = config.ref;
    }
    if (hasValidKey(config)) {
      key = '' + config.key;
    }

    self = config.__self === undefined ? null : config.__self;
    source = config.__source === undefined ? null : config.__source;
    // Remaining properties are added to a new props object
    for (propName in config) {
      if (
        hasOwnProperty.call(config, propName) &&
        !RESERVED_PROPS.hasOwnProperty(propName)
      ) {
        props[propName] = config[propName];
      }
    }
  }

  // children 可以是多个参数
  const childrenLength = arguments.length - 2;
  if (childrenLength === 1) {
    props.children = children;
  } else if (childrenLength > 1) {
    const childArray = Array(childrenLength);
    for (let i = 0; i < childrenLength; i++) {
      childArray[i] = arguments[i + 2];
    }

    props.children = childArray;
  }

  // Resolve default props
  if (type && type.defaultProps) {
    const defaultProps = type.defaultProps;
    for (propName in defaultProps) {
      if (props[propName] === undefined) {
        props[propName] = defaultProps[propName];
      }
    }
  }

  return ReactElement(
    type,
    key,
    ref,
    self,
    source,
    ReactCurrentOwner.current,
    props,
  );
}

```

- jsx 方法和 createElement 方法实质返回到 react 对象

```
const ReactElement = function(type, key, ref, self, source, owner, props) {

  //jsx 和 createElement 方法实质返回的对象
  const element = {
    // This tag allows us to uniquely identify this as a React Element
    $$typeof: REACT_ELEMENT_TYPE,

    // Built-in properties that belong on the element
    type: type,
    key: key,
    ref: ref,
    props: props,

    // Record the component responsible for creating this element.
    _owner: owner,
  };

  return element;
};
```

### react-component

#### 位置

ReactBaseClasses.js

#### 输出接口

Component, PureComponent

- Component

```
function Component(props, context, updater) {
  this.props = props;
  this.context = context;
  // If a component has string refs, we will assign a different object later.
  this.refs = emptyObject;
  // We initialize the default updater but the real one gets injected by the
  // renderer.
  this.updater = updater || ReactNoopUpdateQueue;
}

Component.prototype.isReactComponent = {};

Component.prototype.setState = function(partialState, callback) {

  this.updater.enqueueSetState(this, partialState, callback, 'setState');
};

Component.prototype.forceUpdate = function(callback) {
  this.updater.enqueueForceUpdate(this, callback, 'forceUpdate');
};

```

- PureComponent

```
function ComponentDummy() {}
ComponentDummy.prototype = Component.prototype;


function PureComponent(props, context, updater) {
  this.props = props;
  this.context = context;
  // If a component has string refs, we will assign a different object later.
  this.refs = emptyObject;
  this.updater = updater || ReactNoopUpdateQueue;
}

// 本质是继承 Component
const pureComponentPrototype = (PureComponent.prototype = new ComponentDummy());
pureComponentPrototype.constructor = PureComponent;
// Avoid an extra prototype jump for these methods.
Object.assign(pureComponentPrototype, Component.prototype);
//设置标志
pureComponentPrototype.isPureReactComponent = true;
```

### react-ref

位置： ReactCreateRef.js

```
export function createRef(): RefObject {
  const refObject = {
    current: null,
  };

  return refObject;
}

```

### forward-ref

位置：forwardRef.js

```
export default function forwardRef(render) {

  return {
    $$typeof: REACT_FORWARD_REF_TYPE,
    render,
  };
}
```

render 是一个函数组件，如可以这样使用

```
const FuncCompWithRef = React.forwardRef((props,ref) => {
    return <input {...props} ref={ref} />
})

<FuncCompWithRef ref={this.state.data} />
```

#### context

位置： ReactContext.js

```
export function createContext(defaultValue,calculateChangedBits) {
  if (calculateChangedBits === undefined) {
    calculateChangedBits = null;
  }

  const context = {
    $$typeof: REACT_CONTEXT_TYPE,
    _calculateChangedBits: calculateChangedBits,
    // As a workaround to support multiple concurrent renderers, we categorize
    // some renderers as primary and others as secondary. We only expect
    // there to be two concurrent renderers at most: React Native (primary) and
    // Fabric (secondary); React DOM (primary) and React ART (secondary).
    // Secondary renderers store their context values on separate fields.
    _currentValue: defaultValue,
    _currentValue2: defaultValue,
    // Used to track how many concurrent renderers this context currently
    // supports within in a single renderer. Such as parallel server rendering.
    _threadCount: 0,
    // These are circular
    Provider: null,
    Consumer: null,
  };

  context.Provider = {
    $$typeof: REACT_PROVIDER_TYPE,
    _context: context,
  };

  context.Consumer = context;

  return context;
}

```
