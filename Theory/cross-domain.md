## 前端跨域

由于浏览器同源策略的限制，本域的Javascript不能操作其他域的页面对象。

### Jsonp
Javascript标签的引入不受同源策略的限制，所以可以通过script标签引入js文件或者其他后缀形式（php或jsp）的文件。

在本域中定义的函数，可以在其他域中调用，将其他域的代码文件引入即可实现跨域。

缺点：jsonp只能实现GET请求跨域，不能发送POST请求。

Basic Usage
``` html
  index.html
  <script type="text/javascript">
    function test(value) {
      console.log(value);
    }
  </script>
  <script type="text/javascript" src="./1.php"></script>
```

``` php
  1.php
  <?php
    echo 'test("hello");';
```

### Access Control
在服务端开启CORES，可以实现跨域，也可发送POST请求。
服务器设置如下：

Basic Usage
```
// 指定允许其他域名访问  
header('Access-Control-Allow-Origin:*');  
// 允许的请求类型  
header('Access-Control-Allow-Methods:POST');  
// 响应头设置  
header('Access-Control-Allow-Headers:x-requested-with,content-type');
```
指定域名，接收客户端传送的cookie：
```
header("Access-Control-Allow-Origin: http://suvllian.com");
header("Access-Control-Allow-Credentials: true");
header("Access-Control-Allow-Headers: x-requested-with, content-type");
```

### window.name
window对象的name属性，在一个窗口的生命周期内，窗口载入的所有页面都共享window.name，每个页面都有window.name的读写权限，window.name持久存在一个窗口载入过的所有页面中，并不会因为页面的载入和重置。

``` html
a.html
window.name = "test hahah";
setTimeout(function(){
	window.location = 'b.html';
},2000);
	b.html
conssole.log(window.name); // test hahah
```

### document.domain

### postMessage