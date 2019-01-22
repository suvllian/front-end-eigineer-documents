JS存在的问题：

* 弱类型语言导致项目维护存在问题
* 性能不能满足一些场景的需要

webAssembly优点：

* 体积小：浏览器加载编译后的字节码运行。
* 加载快：无需解释的过程。
* 兼容性问题少。


```

emcc hello.c -Os -s WASM=1 -s SIDE_MODULE=1 -o hello.wasm

emcc hello.c -s -o hello.js

```

https://webassembly.js.org/docs/index.html