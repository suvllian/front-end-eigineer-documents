## Redux详解及源码分析

### 背景
随着Javascript单页应用逐渐复杂，Javascript要管理更多的state。这些state可能包括服务器响应、缓存数据、本地生成未保存到数据库的数据，也包括UI状态等。

管理不断变化的state越来越复杂。如果一个model变化会引起另一个model变化，那么当view变化时，就有可能引起对应的model以及另一个model的变化，同时也会引起另一个view的变化。state的变化变得不可预测。

React在视图层禁止异步和直接操作DOM，减少了视图层操作的复杂度，但是没有一个管理数据的机制。

因此有了Redux，**让state的变化变得可预测**。

Redux提供不局限于React的数据状态管理，它是零依赖的，可以配合任何框架或者类库一起使用。

### 设计原理

Redux设计符合MVC模式，其中有三个核心概念，分别是：action、reducer、store。

用户操作View层触发action，action通过dispatch触发reducer函数，更改Store中state，state变化后通知View层。

其数据流转如下图：

<img src="../imgs/redux/redux-data-flow.png" width="400" style="text-align:center;">

Redux有三大核心原则：

- 单一数据源：整个应用的state被储存在一棵object tree中，并且这个object tree只存在于唯一一个store中。
- State是只读的：唯一改变state的方法就是触发action，action是一个用于描述已发生事件的普通对象。
- 使用纯函数执行修改：为了描述action如何改变state tree，你需要编写reducers。

### 源码解析

Redux中一共暴露出五个方法：`createStore`、`combineReducers`、`bindActionCreators`、`applyMiddleware`、`compose`

#### createStore(reducer, preloadedState, enhancer)

createStore用于初始化并创建唯一的store，用来存放应用中所有的state，是redux最核心的部分。

入参：

* redux函数
* preloadedState：state初始状态
* enhancer：store增强器

返回一个对象，对象中有5个方法：

* dispatch：更新state
* subscribe：注册监听器，返回函数，可以调用返回的函数销毁监听器
* getState：获取当前state
* replaceReducer：替换reducer函数

dispatch函数逻辑流程图如下：

<img src="../imgs/redux/redux-dispatch.png" width="300" style="text-align:center;">

#### combineReducers

把一个由多个不同reducer函数作为value的object，合并成一个最终的 reducer函数。

注意点：
* reducer不能返回undefined，未匹配的action也要返回原state

combineReducers源码如下图

<img src="../imgs/redux/combineReducer.png" width="300" style="text-align:center;">

分析源码，我们可以看出每个reducer的写法虽然是独立的，但是在combineReducer每个reducer还是会执行一次，这也是Redux存在的问题。

#### bindActionCreators

把一个value为不同[action creator](https://cn.redux.js.org/docs/Glossary.html#action-creator) 的对象，转成拥有同名key的对象。同时使用`dispatch`对每个action creator进行包装，以便可以直接调用它们。

唯一会使用到 `bindActionCreators` 的场景是当你需要把action creator往下传到一个组件上，却不想让这个组件觉察到Redux的存在，而且不希望把`dispatch`或Redux store传给它。

#### applyMiddleware

#### compose
