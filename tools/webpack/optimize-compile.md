## 1. webpack优化打包思路

### 1.1 原则

优化打包通常从两方面入手：

* 优化编译打包的速度
* 优化打包后的静态资源

### 1.2 优化编译打包的速度
* 只重新编译修改或新增的文件
* 并发执行编译构建任务。（webpack中可以使用happyback）

### 1.3 优化打包后的静态资源

* 减小打包的整体体积
  * 代码压缩(UglifyJS)
  * 移除未使用的模块(Tree Shaking)
  * 按需引入模块
  * 提取公共依赖，公共依赖使用external或引入CDN
  * 选择可以替代的体积较小的模块（例如替换moment.js）
* Code Splitting: 模块化引入，按需加载，优化页面首次加载体积。如根据路由按需加载，根据是否可见按需加载
  * 使用 import() 动态加载模块
  * 使用 React.lazy() 动态加载组件
  * 使用 lodable-component 动态加载路由，组件或者模块
* Bundle Splitting：分包，根据模块更改频率分层次打包，充分利用缓存

### 1.4 参考
* [前端高级进阶：如何更好地优化打包资源](https://mp.weixin.qq.com/s/Gh4VJSk33OqHKM3FIIkERw)

## 2. 具体优化方法实践

### 2.1 利用webpack external优化打包

**作用** ：防止将某些`import`的包(package)打包到 bundle 中，而是在运行时(runtime)再去从外部获取这些扩展依赖，从而减小打包的bundle体积。

**使用经验**：通过加载CDN或全局JS中暴露全局变量使用常用的基础包，如react和react-dom

#### 参考

* [官方文档-externals](https://www.webpackjs.com/configuration/externals/)
* [webpack externals 深入理解](https://segmentfault.com/a/1190000012113011?utm_source=tag-newest)
* [webpack打包优化之外部扩展externals的实际应用](https://www.cnblogs.com/weiqinl/p/10020773.html)


### 2.2 实现按需加载
以antd组件为例，通常有两种方式实现按需加载。

**分别引入组件对应的js和css**。  
该方法的优点是简单，不需要配置，直接加载即可。但是每次载入一个新的组件都会引入冗余代码。

``` jsx
import Button from 'antd/lib/button';
import 'antd/lib/button/style';
```

**使用babel-plugin-import按需加载**

使用这个插件之后仍然可以用`import { Button } from 'antd';`来引入组件，插件会帮你转换成`antd/lib/xxx`的写法。另外此插件配合 style 属性可以做到模块样式的按需自动加载。