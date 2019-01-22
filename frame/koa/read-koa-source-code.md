## Koa源码阅读笔记

### Koa2简介
提供封装好的Http上下文、请求、响应，以及基于async/await的中间件容器。

### 源码目录
```
├── lib
│   ├── application.js
│   ├── context.js
│   ├── request.js
│   └── response.js
└── package.json
```
上面是Koa框架的源码结构，核心代码就是lib目录下的四个文件。

* application.js：Koa入口文件，核心的中间件处理流程
* context.js：应用上下文；处理Cookie，错误处理onerror函数
* request.js：处理http请求，包括获取http请求的`header、body、host、url、query、method`等信息
* response.js：处理http响应，包括设置http请求的`header、body、status、message、type、eTag、lastModified`等信息

下面主要分析`application.js`，`context.js`、`request.js`和`response.js`的内容比较简单，此处不作分析。

### Basic usage
``` javascript
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

在上面的基础示例中，框架做了三件事：**初始化Koa对象；添加中间件；创建HTTP服务器**，这也是`application.js`中主要做的事情。

#### app.use(...)

Koa的中间件实际上就是一个**函数数组**，函数数组在构造函数中进行初始化，在调用`app.use(...)`方法时，将函数作为参数传入并添加到数组中。

``` javascript
constructor() {
    super();

    // code...
    
    this.middleware = [];
    
    // code...
}
```

在Koa2中，中间件只支持async函数，如果要使用generator函数，需要通过`koa-convert`进行封装转换才能使用，最后在`listen`方法中调用中间件。

``` javascript
const isGeneratorFunction = require('is-generator-function');
const convert = require('koa-convert');

use(fn) {
    if (typeof fn !== 'function') throw new TypeError('middleware must be a function!');
    if (isGeneratorFunction(fn)) {
      deprecate('Support for generators will be removed in v3. ' +
                'See the documentation for examples of how to convert old middleware ' +
                'https://github.com/koajs/koa/blob/master/docs/migration.md');
      fn = convert(fn);
    }
    debug('use %s', fn._name || fn.name || '-');
    this.middleware.push(fn);
    return this;
  }
```

#### app.listen(...)

`listen`方法创建了HTTP服务器，同时进行了http上下文、请求、响应的初始化，处理中间件等操作。

```javascript
listen(...args) {
    const server = http.createServer(this.callback());
    return server.listen(...args);
}
```

在`callback`函数中通过[koa-compose](https://github.com/koajs/compose/blob/master/index.js)所有处理中间件函数，然后初始化http上下文、请求、响应，最后返回`handleRequest `函数。

```javascript
const onFinished = require('on-finished');
const compose = require('koa-compose');

callback() {
    const fn = compose(this.middleware);

    if (!this.listenerCount('error')) this.on('error', this.onerror);

    const handleRequest = (req, res) => {
      const ctx = this.createContext(req, res);
      return this.handleRequest(ctx, fn);
    };

    return handleRequest;
}

// 初始化http上下文、请求、响应
createContext(req, res) {
    const context = Object.create(this.context);
    const request = context.request = Object.create(this.request);
    const response = context.response = Object.create(this.response);
    context.app = request.app = response.app = this;
    context.req = request.req = response.req = req;
    context.res = request.res = response.res = res;
    request.ctx = response.ctx = context;
    request.response = response;
    response.request = request;
    context.originalUrl = request.originalUrl = req.url;
    context.state = {};
    return context;
  }

handleRequest(ctx, fnMiddleware) {
    const res = ctx.res;
    res.statusCode = 404;
    const onerror = err => ctx.onerror(err);
    const handleResponse = () => respond(ctx);
    onFinished(res, onerror);
    return fnMiddleware(ctx).then(handleResponse).catch(onerror);
}
```

