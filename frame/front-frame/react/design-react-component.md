## 设计一个React组件  
### 一、组件设计要素  
一个大型应用包含许多的功能，如果把所有的功能都放在一个组件中，那这个组件会及其臃肿，并且难以管理。这时候就应该将一个大型应用拆分成许多独立的组件，方便进行管理。这时候就会有一些问题，什么时候该拆分组件，组件的设计原则是什么？  

我理解的组件是指实现特定功能的独立的、可复用的代码。拆分组件最关键的就是确定组件的边界，只在有必要的时候去拆分组件，如果对于不应该拆分的组件也拆成几个小组件，那就会得不偿失了。 

#### 高内聚 
> 高内聚指的是把逻辑紧密相关的内容放在一个组件中  

#### 低耦合 
> 低耦合指的是不同组件之间的依赖关系要尽量弱化，每个组件尽量独立  

每个组件都应该是独立存在的，如果两个组件只是有少量的逻辑联系，而且功能差距比较大，就可以拆分成两个组件。  
如果两个组件逻辑联系太紧密，这两个组件就不应该被拆开，应该是一个独立的组件。否则两个组件就需要频繁的进行数据交互，得不偿失。

### 二、React组件的数据 
**`props和state`**  

React组件的数据来源分为props和state，props是组件对外的接口，是父组件传过来的数据，state是组件内部的数据。props和state改变都有可能导致组件重新渲染。  

#### props
props是从外部传递给组件的数据，每个组件都是独立存在的模块，组件之外的一切都是外部世界，外部世界和组件都是通过props进行通信。  

父组件可以通过props传递任何一种Javascript支持的数据类型到子组件，函数也可以，当props的类型不是字符串时，在JSX中必须用花括号将props值包住，props是一个对象，传递的数据是props的属性，在子组件中可以通过访问props对象属性获取传递的数据。  

**`父组件`**  

``` jsx 
import React, { Component } from 'react' 
import ClickCounter from './../components/click-counter.jsx'

class App extends Component {
  render() {
  return (
      <div>
        <ClickCounter caption="First" initValue={10} />
        <ClickCounter caption="Second" initValue={0} />
        <ClickCounter caption="Third" initValue={5} />
      </div>
  )
  }
}
```  

**`子组件`**

``` jsx
constructor(props) {
  super(props);
  this.state = {
    count: props.ininValue || 0
  }
  this.onClickButton = this.onClickButton.bind(this)
}
```  

子组件接收父组件传递的数据，首先要通过构造函数调用`super(props)`方法，如果没有调用该方法，组件实例被构造之后，类的函数成员就无法通过`this.props`访问父组件传递的props。  

构造函数中还给一个成员函数绑定了当前的this执行环境，因为ES6方法创造的组件类并不会自动绑定this到当前实例类。如果没有手动绑定函数到当前this环境，函数体中如果想通过this获取当前组件实例就会失败。  

**`propTypes检查`**  

props是组件对外的接口，可以通过propTypes声明组件的规范： 
* 组件支持哪些props   
* 每个props应该是什么样的格式

在运行时和静态代码检查时，都可以根据propTypes判断外部是否传递正确的数据类型到组件中。通过propTypes可以检查防止不正确的prop使用方法，但propTypes只是一个辅助开发的功能，并不会改变组件的行为。  

propTypes能够帮助开发者在开发过程中发现问题，但是生产环境就不需要了，所以需要在生成生产环境代码时，有一种自动的方式将propTypes去掉，最终部署到生产环境中，可以使用`babel-react-optimize`这个npm包。


ClickCounter组件中的propTypes定义如下：
``` jsx
ClickCounter.propTypes = {
  caption: PropTypes.string.isRequired,
  initValue: PropTypes.number
}
```  

以上规则要求caption必须时string类型，initValue必须时number类型，而且组件中必须指定caption，而initValue没有也没关系。

#### state
state代表组件内部的状态。

**`初始化state`**  

在构造函数中，对state进行赋值可以完成state的初始化，state是一个对象。

``` jsx
constructor(props) {
    super(props)
    this.state = {
       count: props.initValue || 0   
    }
}
```

上面的示例再构造函数中初始化了state，如果props中指定了initValue，就将其赋值给count，如果没有指定，则初始化为0.上述代码可以进行优化，给props设置默认值，就不需要在初始化state时进行判断。 

``` jsx
Counter.defaultProps = {
    initValue: 0
}

this.state = {
    count: props.initValue
}
```

**`读取和更新state`**  

在组件内的任何位置都可以通过this.state来访问state对象，改变state的值必须使用`this.setState()`方法，不能直接去修改state，直接修改state，只是修改了state中的值，没有驱动组件重新渲染，所以在View层看不出来state的变化。 

``` jsx
this.setState({count: this.state.count + 1})
```   

#### props和state对比 

* props用于定义组件外部接口，state用于记录组件内部状态
 
* props的赋值在组件外部，组件内部不能修改props，state的赋值在组件内部。

### 三、组件的生命周期  

React组件的生命周期可能会经过下面三个过程：

* 装载过程：组件第一次在DOM树中渲染的过程。
 
* 更新过程：组件被重新渲染的过程。
 
* 卸载过程：组件从DOM中删除的过程。

#### 装载过程

```  
constructor

getInitState

getDefaultProps

componentWillMount

render

componentDidMount  
```

**`constructor`** 

1、初始化state  

2、绑定成员函数的this指向

**`getInitState和getDefaultProps`**  

用于初始化state和props，不过这两个方法只有在用React.createClass方法创建组件时才会有作用，使用ES6语法创建组件时，这两个方法无效。使用ES6语法创建组件时，可以用第二部分的方法初始化state和props。  

**`render`**  

render是一个纯函数，返回组件要渲染的内容。

**`componentWillMount和componentDidMount`**  

componentWillMount方法会在组件渲染前被调用，componentDidMount方法在render方法后被调用，此时render函数返回的内容已经渲染到DOM结构上，这时候就可以进行操作DOM了。 

#### 更新过程 

当组件的props和state被修改时，就会引发组件的更新。

```  
componentWillReceiveProps

shouldComponentUpdate

componentWillUpdate

render

componentDidUpdate  
```

**`componentWillReceiveProps`** 

只要父组件的render函数被调用，在render中被渲染的子组件按就回经历更新过程，不管父组件传递给子组件的props有没有改变，都会触发componentWillReceiveProps函数。

在componentWillReceiveProps函数中可以将传入的参数nextProps和this.props进行对比，只在两者有变化时，才更新内部的state。

**`shouldComponentUpdate`**  

shouldComponentUpdate函数决定了组件在什么时候不需要渲染，shouldComponentUpdate返回一个布尔值，告诉React当前组件这次更新过程是否需要继续。如果返回false，立即停止更新过程，不会引发后续的渲染。

shouldComponentUpdate的参数是新的state和props，可以将新的state、props和之前的state、props进行比较，如果发生变化则更新，没有发生变化则不更新。这样可以大大提高应用的性能。

**`componentWillUpdate和componentDidUpdate`**  
类似于componentWillMount和componentDidMount，是在依次更新过程中的render函数的前后调用。

#### 卸载过程  

卸载过程中只有一个函数：  

* componentWillUnmount：可以在函数中进行一些清理工作。


### 四、react16生命周期
React16废弃的三个生命周期函数

* componentWillMount
* componentWillReceiveProps
* componentWillUpdate

新增生命周期函数

* getDerivedStateFromProps
* getSnapshotBeforeUpdate


### 五、总结

本节主要介绍了React组件的数据来源：state和props，还介绍了组件的生命周期，这些都是在开发React应用是肯定会用到的知识。    

考虑组件的设计原则，处理好state、props和组件的生命周期，就可以设计出高质量的组件。 

现在只提到state和props两种数据来源，你可能会发现存在许多问题，例如：  

* 如果存在多层嵌套的组件，根组件想传递数据到子组件，通过props一级一级的往下传就会导致组件强耦合，这并不是我们希望看到的。  

* 兄弟组件想获取彼此的数据，只能通过父组件作为桥梁来传递数据。

* 不同组件中的数据可能不同步。

可以看出state存储数据可能会导致数据的重复和冗余。这就引出了Flux和Redux这些管理应用数据的框架。
