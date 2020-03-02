## 如何在浏览器端直接引用UMD方式打包的react组件？

这个问题的场景是我需要开发一个SDK，通过异步请求去获取react组件的配置及cdn链接，再通过异步加载的方式获取UMD方式打包的组件资源。  
然后将异步请求获取的参数传递到组件中，组件正常渲染。

### 解决方案

1. 组件代码中不是直接抛出组件，而是将render过组件渲染的结果封装在函数中，抛出该函数。
2. 在html代码中，调用该函数，可将组件参数及渲染的锚点作为函数参数传入。

``` jsx
// index.js
import React, { Component } from 'react';
import render from 'react-dom'
import { Modal } from 'antd'

class App extends Component {
  constructor() {
    super();
  }

  componentDidMount() {
   
  }

  handleOk() {

  }

  render() {
    const { visible } = this.props

    return <div>
      <Modal
        title="Basic Modal"
        visible={true}
        onOk={this.handleOk}
        onCancel={this.handleOk}
      >
        <p>Some contents...</p>
        <p>Some contents...</p>
        <p>Some contents...</p>
      </Modal>
    </div>;
  }
}

module.exports = function MyApp(props, anchor) {
  render(<App {...props} />, document.querSelector(anchor || '__react_content__'));
}
```


``` html.js
MyApp({visible: true}, '#body);
```

参考：[nanon](https://github.com/bjarneo/nanon)

### 解决思路

一开始在选用preact作为UI库的时候，在组件打包时，只抛出组件，在SDK内部，也就是浏览器端实现组件渲染。  
渲染过程中使用h函数，也就是react中的createElement函数去解决组件渲染。但是preact中的h函数似乎不能渲染DOM树结构，于是通过递归的方式去渲染DOM结构。
在渲染基础html元素写的组件时，这种方法是可行的。同理在react中使用createElement函数去渲染也是可行的。

但是上述方法在组件复杂的情况下不适用。
后来因为preact没有组件生态，我换成react了，我要在我的组件中要使用antd的组件，antd组件渲染的reactdom的type是function，而不是基础html元素，需要调用react函数进一步处理。  
我查了很多资料，在github中也看了很多umd loader的实现，但是知识我上面说的那种使用createElement的实现，我也尝试了各种奇技淫巧，还是无解。  
最后在github中偶然看到了这个解法，在写组件时直接render，然后抛出函数，我试了下果然可行。  
这应该是最合理的方式！