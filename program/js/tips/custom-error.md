# 自定义错误对象

## 实现
写一个新的CustomError继承Error类，重写构造函数，继承Error对象的构造函数即可

## 代码示例

``` javascript
class CustomError extends Error {
  constructor(name, message) {
    const errorMessage = name && message ? message : name;
    const errorName = name && message ? name : '';

    super(errorMessage);

    if (errorName) {
      this.name = name;
    }

    Error.captureStackTrace(this, this.constructor);
  }
}
```

## 参考文档
* [Mozilla Error](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)
* [JavaScript Error对象及错误类型](https://itbilu.com/javascript/js/V1oOv4Vv-.html)