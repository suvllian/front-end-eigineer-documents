## 零散知识点汇总

### 1. 在父组件中如何调用自组件的方法

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