## 浏览器不同窗口间通信


### Broadcast Channel

#### page 1
``` javascript
var channel = new BroadcastChannel("channel-BroadcastChannel");
    channel.postMessage('Hello, BroadcastChannel!')
```

#### page 2
``` javascript
var channel = new BroadcastChannel("channel-BroadcastChannel");
    channel.addEventListener("message", function(ev) {
        console.log(ev.data)
    });
```

注意：存在兼容性问题，[can i use BroadcastChannel](https://caniuse.com/?search=BroadcastChannel)


### StorageEvent
StorageEvent：https://developer.mozilla.org/en-US/docs/Web/API/StorageEvent

#### Page 1
``` javascript
localStorage.setItem('message',JSON.stringify({
    message: '消息'，
    from: 'Page 1',
    date: Date.now()
}))
```

#### Page 2
``` javascript
window.addEventListener("storage", function(e) {
    console.log(e.key, e.newValue, e.oldValue)
});
```

### reference
* [两个浏览器窗口间通信](https://www.cnblogs.com/cloud-/p/10713213.html)