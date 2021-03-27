# this对象
`this`对象指的是在某一作用域下的上下文对象

## 作为对象方法调用

函数为对象的方法，使用这种方式时，this对象被自然绑定到该对象。

``` javascript
var obj = {
    x: 0,
    y: 0,
    init: function(x, y) {
        this.x = x
        this.y = y
    }
}

obj.init(1, 1)
```

## 作为函数调用

函数可以直接调用，此时this绑定到全局对象。在浏览器中，全局的this对象指向window对象。

在下面的例子中，this指向全局对象，然后执行赋值语句，相当于隐式声明一个全局变量。

``` javascript
function setValue(x) {
    this.x = x
}
setValue(3)
x // 3
```

对于内部函数，即声明在一个函数体内的函数，内部函数的this对象会指向全局对象，这时可以将this对象赋值给一个变量。
``` javascript
var obj = {
    x: 0,
    y: 0,
    init: function(x, y) {
        var that = this
        var setX = function(x) {
            that.x = x
        }
        var setY = function(y) {
            that.y = y
        }
        setX(x)
        setY(y)
    }
}
obj.init(1, 2)
```

## 作为构造函数调用
this对象指向新创建的对象

``` javascript
function Point(x, y) {
    this.x = x
    this.y = y
}

var instance = new Point(1, 2)
```

## 使用call、apply调用
call、apply可以切换函数执行的上下文环境，即this绑定的对象。

``` javascript
function Point(x, y){ 
   this.x = x; 
   this.y = y; 
   this.moveTo = function(x, y){ 
       this.x = x; 
       this.y = y; 
   } 
} 
 
var p1 = new Point(0, 0); 
var p2 = {x: 0, y: 0}; 
p1.moveTo(1, 1); 
p1.moveTo.apply(p2, [10, 10]);
```
使用apply可以将P1的方法用在P2上，this也被绑定到P2上。

## 箭头函数

es6里面this指向固定化，始终指向外部对象，因为箭头函数没有this，因此它自身不能进行new实例化,同时也不能使用call，apply，bind等方法来改变this的指向