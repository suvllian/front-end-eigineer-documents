## 在父组件中调用自组件方法

### 父组件

``` jsx
class Parent extends Component {
  onRef(child) {
    this.child = child
  }

  onClick() {
    this.child.search()
  }

  render() {
    return (<div>
      <div onClick={this.onClick.bind(this)}></div>
      <Child onRef={this.onRef.bind(this)} />
    </div>)
  }
}
```

### 子组件
``` jsx
class Child extends Component {
  componentDidMount() {
    this.props.onRef(this)
  }
  
  search() {
    console.log('search')
  }

  render() {
    return (<div>
      I am Child
    </div>)
  }
}
```