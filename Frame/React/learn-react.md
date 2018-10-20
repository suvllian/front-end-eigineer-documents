## 了解React
### 一、React是什么？
React是Facebook的2013年推出的开源项目，是一个前端MVC框架，准确来说，React并不是一个完整的MVC框架，它更专注于VIEW（视图）层，需要配合社区中的一些库，如React-router、Redux等等才算是一个完整的MVC架构。  

### 二、初始化一个React项目
学任何一门语言和框架，往往是从hello world开始，不过只展示一句hello world就没有体现出React的优点，所以现在尝试写一个有交互Demo开始学习React。

### Example1[源码地址](./example1)

这个Demo是一个计数器，每次点击按钮计数加1。  

![before](../imgs/react/react-1.png)
![after](../imgs/react/react-2.png)  
  
如果像以前一样用`jQuery`进行开发的话，首先要设置一个变量，然后获取Button的DOM节点，每次点击将变量的值加1，然后替换页面上的Counter值。  

**jQuery实现**
``` html
<div>
  <button id="button">Click Me</button>
  <p>Click Count: <span id="counter">0</span></p>
</div>

<script type="text/javascript">
let counter = 0,
    counterDom = $("#counter")

$("#button").click(function() {
  counter++;
  counterDom.text(counter);
})
</script>
```
通过`jQuery`实现一个计数器，首先要获取Button的DOM节点，在DOM节点上添加一个匿名事件处理函数，在事件处理函数中，选中需要被修改的span标签，修改其文本值。  

**React实现**
``` jsx
import React, { Component } from 'react'

class ClickCounter extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    }
    this.onClickButton = this.onClickButton.bind(this)
  }

  onClickButton() {
    this.setState({
      count: this.state.count + 1
    })
  }

  render() {
    const { count } = this.state

    return (
      <div>
        <button onClick={this.onClickButton}>Click Me</button>
        <p>Click Count: {count}</p>
      </div>
    )
  }
}

export default ClickCounter
```  

通过React实现一个计数器，不用关注如何获取DOM节点，如何更新DOM节点，而是专注于“界面应该显示成什么样子”，只用关注每次点击之后counter值应该变为多少，获取DOM更新DOM的过程React会帮我们完成。  

**jQuery和React对比**
> 举个例子来说：  

政府需要建一栋大楼，开发者是建筑设计师，设计好了图纸，`jQuery`和`React`是两个建筑工人。如果交给`jQuery`做的话，设计师得事无巨细的告诉他“如何去做”，首先要把一面墙拆掉重建，然后再墙上开一个窗户......。如果交给`React`做的话，设计师只需要告诉他“需要什么样的建筑”，就可以了，`React`会搞定一切。  

使用`React`，开发者只需要关注需要什么样的结果，而无需关注怎样去做。

### 三、React的优点
对react有了一个初步的认识，现在系统的总结一下使用React的优点。  

#### 组件化 
> React的首要思想就是通过组件开发应用，组件就是指能完成某个特定的功能的独立的、可重用的代码。   
基于组件的开发模式，可以将一个大型应用拆分成许多小的组件，每个组件只关注特定范围内的特定功能，这个更方便进行管理，而且许多组件也可以进行复用。  

在上面的示例中，ClickCounter就是一个组件，他也可以在多个场景中被引用，React组件推荐使用ES6的写法，虽然React支持使用createClass函数来创建组件，但是现在官方逐渐废弃这个方法，使用ES6的写法也更加直观。

#### 虚拟DOM  
在我们以前的Web开发中，需要频繁的获取DOM节点，然后对DOM进行操作，而DOM操作往往是页面性能的一个瓶颈。  
React因此引入了虚拟DOM机制。虚拟DOM是一个真实DOM的一个快速的、仅存在于内存的映射，它是抽象的。   

>虚拟DOM工作模式：  
1、先创建一个虚拟DOM树，它是真实DOM树的映射。  
2、只要数据模型发生变化，虚拟DOM就会将变化后的数据模型绘成新的虚拟DOM。  
3、然后对比两个虚拟DOM树之间的区别，找出在真实DOM树中需要修改的地方。  
4、最后在真实的DOM树中更新需要更新的部分。  

#### 函数式编程
> 纯函数指的是相同的输入，永远得到相同的输出，而且没有任何可观察的副作用。   

每一个React组件都可以看成是一个纯函数，只用关心数据的映射，render中接收数据作为参数，不依赖其他状态。开发者只需关心如何去操作数据，而不用关注如何操作用户界面。只要数据发生改变，界面就会做出响应。  
React组件中render函数会返回一个DOM结构，将每个组件返回的DOM结构组装起来就可以组成一个大型应用。

#### JSX
React中采用jsx语法，jsx可以将HTML、CSS都写在Js中。JSX有一些基本的语法规则：  

##### 1、定义标签时，只允许被一个标签包裹
``` jsx
ReactDOM.render(
  <section>
    <h1>这是正确的例子</h1>
    <span>jsx</span>
  </section>,
  document.getElementById("example")
);

ReactDOM.render(
  <h1>这是错误的例子</h1>
  <span>jsx</span>,
  document.getElementById("example")
);
```

##### 2、标签一定要闭合  
``` jsx
<img src="http://img.suvllian.com/1.png" /> // 标签一定要闭合，img、br标签最后加上斜杠进行闭合
```

##### 3、class属性改为"className",for属性改为"htmlFor"
``` jsx
<div>
  <p className="text">class属性改为"className"</p>
  <label htmlFor="input"><input id="input" /></label>
</div>
```

##### 4、表达式使用{}包起来
``` jsx
<p className={isShow ? "text" : "text-hidden"}>表达式使用{}包起来</p>
```

##### 5、内联样式
``` jsx
const fontStyle = {
    fontSize: 18,
    color: '#000'
}
<p style={fontStyle}>内联样式</p>
```

##### 6、绑定事件
``` jsx
<button onClick={this.onClickButton}>Click Me</button>
```

##### 7、注释
``` jsx
{/* 节点注释 */}
/* 多行
   注释 */
```

##### 8、转义
``` jsx
var content='<strong>content</strong>';    
React.render(
    <div dangerouslySetInnerHTML={{__html: content}}></div>,
    document.body
);
```

### 四、问题 
#### 为什么`React`在组件中没有用到还要引入？  
在每个组件中都会引入`React`，如果删除了会报错，因为在使用JSX的代码文件中，JSX最终会被转译成依赖于`React`的表达式。  

#### JSX中的onClick和原生HTML中的onClick有什么区别？ 
> 原生HTML中onClick函数有以下几个缺点  

* onClick添加的事件处理函数是在全局环境下执行，污染了全局环境  
* 给许多DOM元素添加事件，影响了网页的性能  
* 对于使用onClick添加事件处理函数的DOM元素，如果要动态的删除，需要把对应的时间处理器注销。  

> JSX中的事件处理函数的优点  

JSX中的事件处理函数都使用了事件委托的方式进行处理，无论页面中有多少个onClick函数，最后都只在DOM树上添加了一个事件处理函数，挂载最顶层的DOM节点，点击事件发生时，根据具体组件分配给特定的函数进行处理。


### 五、总结 
第0节课：《了解React》。通过一个实例简单的介绍一下React，了解React的优点和以往的开发模式的区别。
