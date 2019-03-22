## 开发chrome插件

### 一、相关资料

* 文档
	* [chrome插件官方文档](https://developer.chrome.com/extensions)
	* [中文文档](https://crxdoc-zh.appspot.com/extensions/getstarted)
* 参考文章
	* [Chrome扩展程序开发](https://segmentfault.com/a/1190000007182038)

### 二、个人总结

`manifest.json`是插件的入口文件，其中会涵盖扩展程序的基本信息，并指明需要的权限和资源文件。

``` javascript

{
  "manifest_version": 2, // 必须为2，1号版本已弃用
  "name": "LiveReloadTool", // 扩展程序名称
  "version": "1.0.1", // 版本号
  "description": "just live reload tool", 
  "author": "Demon",
  // 根据自己使用的权限填写
  "permissions": [
    "storage",
    "tabs",
    "http://*/*",
    "https://*/*"
  ],
  // content_scripts，在各个浏览器页面里运行的文件，可以获取到当前页面的上下文DOM
  "content_scripts": [
    {
      "matches": [
        "http://*/*",
        "https://*/*"
      ],
      "js": [
        "live-reload-tool.js"
      ]
    }
  ],
  // browser_action，左键点击右上角插件logo时，弹出的popup框。不填此项则点击logo不会有用
  "browser_action": {
    "default_title": "just live reload",
    "default_popup": "popup.html"
  },
   // options_page，指右键点击右上角里的插件logo时，弹出列表中的“选项”是否可点，以及在可以点击时，左键点击后打开的页面
  "options_page": "view/options.html",
  // background，后台执行的文件，一般只需要指定js即可。会在浏览器打开后全局范围内后台运行
  "background": {
    "scripts": ["js/vendor/jquery-3.1.1.min.js", "js/background.js"],
    // persistent代表“是否持久”。如果是一个单纯的全局后台js，需要一直运行，则不需配置persistent（或者为true）。当配置为false时转变为事件js，依旧存在于后台，在需要时加载，空闲时卸载
    "persistent": false
  }
}

```

我们一共有三种资源文件，针对着三个运行环境：

##### browser_action
* 控制logo点击后出现的弹窗，涵盖相关的html/js/css
* 在弹窗中，可以进行登录/注册的操作，并将用户信息保存在本地储存中。

##### background
* 在后台持续运行，或者被事件唤醒后运行
* 右键菜单的点击和异步保存事件将在这里触发

##### content_scripts
* 当前浏览的页面里运行的文件，可以操作DOM，可以在这个文件里监听用户的选择事件

**其他注意点**

* `content_scripts`中如果没有`matches`，则扩展程序无法正常加载，也不能通过“加载未封装的扩展程序”来添加。如果你的content_scripts中有js可以针对所有页面运行，则填写`"matches" : ["http://*/*", "https://*/*"]`即可

* 推荐将`background`中的`persistent`设置为`false`，根据事件来运行后台js

### 三、问题
#### 在插件中获取页面window对象中的数据

chrome插件的content scripts中不能直接获取页面的window对象，但是可以操作页面的document对象，所以可以通过跨域通信的方式去解决这个问题。

可以使用window.postMessage去发送跨域信息。

在content script中写入以下内容：

``` javascript

const script = document.createElement("script");
script.innerHTML = 'window.onload=function(){window.frames.postMessage({windowName: JSON.stringify(window.name)})};'
document.head.appendChild(script)

// get window.windowName
window.addEventListener('message', function(e){
  if(e.source != window.parent || !e.data.windowName) {
    return
  }

  window.windowName = JSON.parse(e.data.windowName)
}, false)
```