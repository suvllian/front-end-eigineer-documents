## 实现一个简单的虚拟DOM  

### 一、引言
Web性能的一个瓶颈就是需要频繁的去操作DOM，而React就通过虚拟DOM的方式解决了这样一个问题。  

虚拟DOM是真实DOM存在于内存中的一个映射，React在进行render时才把虚拟DOM插入到真实的DOM中，这样就会减少对DOM的频繁操作。  

虚拟DOM中还涉及到的两个点：   

* 编译：通过编译JSX代码，生成对象树的过程。  
* diff算法：通过将新的虚拟DOM和原来的虚拟DOM进行比较，只更新发生变化的部分。

### 二、虚拟DOM的工作方式：  
1、先创建一个虚拟DOM树，它是真实DOM树的映射。  
2、只要数据模型发生变化，虚拟DOM就会将UI重新绘成新的虚拟DOM。  
3、然后对比两个虚拟DOM树之间的区别，找出在真实DOM树中需要修改的地方。  
4、最后在真实的DOM树中更新需要更新的部分

### 三、实现一个简单的虚拟DOM 
React中的虚拟DOM，是通过编译JSX代码，生成对象树，然后根据对象树去生成虚拟DOM。

#### 1、我们需要什么样的结果？

虚拟DOM最终还是需要渲染到真实DOM中，所以我们可以通过真实DOM结构来反推虚拟DOM生成的过程。

``` html 
<ul id="list">
  <li class="item">Item1</li>
  <li class="item">Item2</li>
  <li class="item">Item3</li>
  <li class="item">Item4</li>
</ul>
```

假设我们需要这样一个列表结构，我们可以观察到列表的根元素是`ul`标签，`id`是`list`，还有四个`li`标签，他们都有共同的class，每个`li`又有自己的文本内容。

可以观察到一个DOM元素可以分为三块内容：标签类型、标签属性、子元素。

在列表结构中可以看到标签类型可能会为`ul`和`li`，标签属性可能为`id`和`class`，子元素可能是DOM结构，也可能是文本。

列表结构经过编译就会生成这样的结构：

``` js
let ul = ele("ul", {id:'list'}, [
  ele('li', {class: 'item'}, ['Item 1']),
  ele('li', {class: 'item'}, ['Item 2']),
  ele('li', {class: 'item'}, ['Item 3']),
  ele('li', {class: 'item'}, ['Item 4'])
])
```

可以看出我们把生成虚拟DOM的过程抽象成一个函数，函数中包含三项参数
``` js
let ele = function(tagName, props, children) {
  // 进行处理
}
```

#### 2、如何根据对象树生成虚拟DOM

下面要思考的就是如何去具体实现这个生成过程。

考虑到在第1步中的问题点，具体实现如下：

``` js
class Element {
  constructor(tagName, props = {}, children = []) {
    this.tagName = tagName
    this.props = props
    this.children = children
  }
  
  render() {
    let dom = document.createElement(this.tagName)
    let props = this.props
    
    for (let prop in props) {
      let propValue = props[prop]
      dom.setAttribute(prop, propValue)
    }
    
    this.children.forEach((child) => {
      let childEle = (child instanceof Element) ? child.render() :
        document.createTextNode(child)
      dom.appendChild(child)
    })
    return dom
  }
}

let ele = function(tagName, props, children) {
  new Element(tagName, props, children)
}

```

通过这样一个类实现去生成虚拟DOM，最后调用render方法返回DOM结构，最终插入到真实DOM结构中，即可渲染到页面中。

```
let ulRoot = ul.render();
document.body.appendChild(ulRoot);
```

完整示例代码：[链接](https://github.com/suvllian/learning/blob/master/React/realize-virtual-dom/index.html)

### 三、总结

真实的React虚拟DOM实现肯定是比较复杂的，但是通过这样一个简单的示例可以了解从对象树生成虚拟DOM，再到渲染到真实的DOM的过程。