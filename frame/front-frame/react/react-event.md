
## React事件机制

react自身实现了一套事件机制，包括事件注册、事件合成、事件冒泡、事件派发等。 
和原生的事件机制不通，但也是基于浏览器的事件机制的。

react的所有事件并没有绑定到具体的dom节点上，而是绑定在了document 上，然后由统一的事件处理程序来处理，同时也是基于事件冒泡机制，所有节点的事件都会在 document 上触发，再由dispatchEvent统一处理。

**意义：**

* 减少内存消耗，提升性能。
* 简化事件处理逻辑，方便统一处理。


### 一、查看运行时的事件

所有时间都注册到最顶层的document上。

**结论：**原生事件阻止冒泡会阻止合成事件的触发，合成事件阻止冒泡不会影响原生事件。


### 二、react事件合成

react事件不仅仅是对事件进行简单的合成和处理，还包括：

* 对原生事件的封装
* 对原生事件的升级
* 不同浏览器下的兼容处理

#### 2.1 对原生事件的封装
事件处理方法中的参数e，是对原生事件的包装。

SyntheticEvent 实例将被传递给你的事件处理函数，它是浏览器的原生事件的跨浏览器包装器。除兼容所有浏览器外，它还拥有和浏览器原生事件相同的接口，包括 stopPropagation() 和 preventDefault()。  
如果因为某些原因，当你需要使用浏览器的底层事件时，只需要使用 nativeEvent 属性来获取即可。

#### 2.2 对原生事件的升级
有些dom事件，react不是只处理代码中声明的事件类型，还会额外增加一些其他事件。

当我们给input添加onChange事件时，react会帮我们添加一些其他的事件处理。
原生的onchange函数，只有在失去焦点时才会触发，这个原生事件的缺陷，react也帮我们弥补了。


#### 2.3 浏览器事件兼容处理
针对IE浏览器的兼容处理

### 三、react事件处理
react事件注册过程主要做了2件事：

* 组件挂载阶段，根据组件内部声明的事件类型，在document上添加事件，并指定统一的事件处理dispatchEvent
* 把react组件内的所有事件都存放到一个对象里，在事件触发时找到对应的方法执

**备注：**事件处理函数的源码在ReactBrowserEventEmitter文件中

#### 3.1 JSX解析

#### 3.2 处理props

react在进行组件加载和更新时，都会调用updateProperties方法更新props。如果是事件，则会将其存入一个事件处理集合中。

* createReactNoop.js createReactNoop
* ReactDOMHostConfig.js finalizeInitialChildren
* ReactDOMComponent.js setInitialProperties->setInitialDOMProperties->ensureListeningTo
* ReactBrowserEventEmitter.js listenTo getListeningSetForElement->listenToTopLevel

事件触发过程总结为主要下面几个步骤：

* 进入统一的事件分发函数：ReactFabricEventEmitter.js dispatchEvent
* 结合原生事件找到当前节点对应的ReactDOMComponent对象：ReactFabricEventResponderSystem.js 
* 合成事件
	* 根据当前事件类型生成指定的合成对象
	* 封装原生事件和冒泡机制
	* 查找当前元素以及他所有父级
	* 在 listenerSet查找事件回调并合成到event
* 批量处理合成事件内的回调事件


### 四、源码分析
#### 4.1 dispatchEvent

备注：源码见ReactFabricEventEmitter.js文件

所有事件经过冒泡，都会传递给document上注册的dispatchEvent处理。

dispatchEventForResponderEventSystem traverseAndHandleEventResponderInstances eventResponderContext processTimers

### Reference
* [一文吃透 React 事件机制原理](https://mp.weixin.qq.com/s/8KrgoeLSuZ5-p-0cDZeb8A)