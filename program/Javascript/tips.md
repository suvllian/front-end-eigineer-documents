

### 1、页面刷新后自动定位到顶部

由于chrome浏览器在当前页面刷新后，会记录滚动条的位置，在某些场景下会引发问题，所以需要刷新后自动定位到页面顶部。

``` javascript
window.onbeforeunload = function () {
  //刷新后页面自动回到顶部
  //ie下
  document.documentElement.scrollTop = 0;
  // 非IE
  window.scrollTo(0, 0)
}
```