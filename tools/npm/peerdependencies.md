## npm dependencies

通常我们如果需要在项目中使用一个依赖包，就需要在package.json中添加依赖，如果只是开发时需要使用，则添加到devDependencies中，生产环境的依赖则添加到dependencies中。

但是在一些场景中会设置peerDependencies，同等依赖，用于指定当前包兼容的宿主版本。

例如使用antd的一个插件antd-plugin，但是插件中调用了antd3.x的API，这就需要保证项目中也是使用antd3.x版本，那么在antd-plugin的package.json中就可以写

``` json
{
  "peerDependencies": {
    "antd": "3.x"
  }
}
```

peerDependencies的目的是提示宿主环境去安装满足插件peerDependencies所指定依赖的包，然后在插件import或者require所依赖的包的时候，永远都是引用宿主环境统一安装的npm包，最终解决插件与所依赖包不一致的问题。

在npm2中，PackageA包中peerDependencies所指定的依赖会随着npm install PackageA一起被强制安装，所以不需要在宿主环境的package.json文件中指定对PackageA中peerDependencies内容的依赖。

但是在npm3中，peerDependencies的表现与npm2不同：

npm3中不会再要求peerDependencies所指定的依赖包被强制安装，相反npm3会在安装结束后检查本次安装是否正确，如果不正确会给用户打印警告提示。


### 参考

* [Peer Dependencies](https://nodejs.org/en/blog/npm/peer-dependencies/)