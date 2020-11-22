## 零散知识点汇总

### 1. 在父组件中如何调用子组件的方法

#### 方法一
将自组件的方法存入redux中，父组件通过redux获取子组件方法进行调用。

#### 方法二
父组件调用子组件的方法，一般使用`refs`，子组件调用父组件一般采用`callback`

``` javascript
class Parent extends React.Component {
    render() {
        return(
            <div>
                <Child ref={r => this.child = r}/>
            </div>
        );
    }
    
    myFunction() {
        this.child.childFunction();
    }
}

class Child extends React.Component {
    render() {
        //....
    }
    
    childFunction() {
    
    }
}
```

### 2. 在页面其他DOM元素或者其他react组件加载完成之后再挂载某一个react组件

因为某一个react组件的挂载依赖前置react组件中的DOM节点，所以需要等其他react组件挂载到真实DOM之后再去挂载

#### 方法一

通过监听DOM的加载事件，加载完成之后再去挂载react组件

``` javascript
let reactRoot = null
const timeHandle = setInterval(() => {
  reactRoot = document.getElementById('__react_content')

  if (!!reactRoot) {
    clearInterval(timeHandle)
    const root = document.createElement('div')
    reactRoot.parentNode.appendChild(root)

    render(
      <App />,
      root
    )

  }
}, 1000)

if (document.readyState === 'complete') {
    ReactDOM.render(<App />, document.getElementById('__react_content'))
} else {
    document.addEventListener('DOMContentLoaded', () => {
    ReactDOM.render(<App />, document.getElementById('__react_content'))
    }, false)
}
```

