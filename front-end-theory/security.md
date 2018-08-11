# Web安全

> Web安全对Web开发者来说是要需要掌握的非常重要的理论知识，在编码设计的过程中要考虑到安全问题。

下面我画了一个思维导图，罗列了常见的Web安全问题，在下面的内容中也作出了详细介绍。

![Web安全](./img/web-security-mind.png)

## 一、XSS

首先是最常见的XSS漏洞，XSS(Cross Site Script)，跨站脚本攻击，因为缩写和CSS(cascading Style Sheets)重叠，所以称为XSS。

**原理**：攻击者在页面中插入可执行的恶意脚本代码，其他用户浏览页面时，脚本代码就会执行，从而达到盗取用户信息或其他侵犯用户安全隐私的目的。

**主要危害**：Cookie窃取、会话劫持、钓鱼诈骗、网页挂马等。

**分类**：反射型XSS、存储型XSS、DOM XSS。

---

### 1.反射型XSS
一般通过将恶意脚本代码作为参数放在URL中传递，当URL被奠基石，恶意代码就会被解析执行。

**反射型XSS特征**：
* 不需要经过服务器存储，直接通过HTTP请求完成攻击，获取用户数据。
* 需要被攻击者点击触发。

**防范方法**：
* Web页面渲染的内容和数据都来自服务器。
* 不要从URL、document.referer、document.forms等API中获取数据直接渲染。
* 不要使用eval、document.write\document.writeln等可以执行字符串的方法。
* 对涉及DOM渲染的方法传入的字符串参数进行**转义处理**。

---

### 2.存储型XSS
存储型的XSS常见于一些表单提交功能中，如论坛留言区，攻击者将恶意代码提交，存储在数据库中，其他用户访问页面时，如果获取到这段代码，就会被渲染执行。不需要诱骗被攻击者点击，但是需要同时满足以下几个条件：
* 后端没有对表单提交的内容进行转义处理。
* 后端从数据库中取出数据没有转义直接输出到前端。
* 前端拿到数据没有转义直接渲染。

**存储型XSS特征**：
* 持久性，存储在数据库中。
* 危害面广。

**防范方法**：
* 永远不要相信用户输入，前后端在发送和接收数据时都要对字段进行转义处理。
* 前端在渲染数据前要进行转义处理。
* cookie设置为httpOnly，js不能获取和操作cookie。

---

### 3.DOM型XSS
现在很多浏览器和开源库针对XSS都进行了转义处理，默认防止大多数XSS攻击，但是还是有方式绕过转义规则。JavaScript支持unicode、escapes、十六进制、八进制等编码形式。可利用编码进行XSS攻击。

基于字符集的攻击原因是由于浏览器在meta没有指定cahrset时会自动识别编码。

**防范方法**：
* 指定`<meta charset="utf-8">`
* XML中要指定字符集位utf-8，标签也要闭合。

---

### HTML特殊字符
在HTML中，一些字符串具有特殊的意义，在输出前应该将这些字符串进行转义，让浏览器将他们当成普通字符进行处理。如`<script>`转义成`&lt;script&gt;`，这段字符串就会在浏览器中正常显示。

``` javascript
el.innerHTML = escapeHTML(title.value);

function encodeHTML (a) {
  return String(a)
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;")
    .replace(/'/g, "&#39;");
};
```

常见的需要转义的字符有：

``` javascript
# --> &#35;
$ --> &#36;
& --> &#38;
( --> &#40;
) --> &#41;
; --> &#59;
< --> &#60;
> --> &#62;
```
更详细的列表参见：[html转义字符](https://ascii.cl/htmlcodes.htm)

举一个例子：  
在发送`GET`请求或`formData`格式的`POST`请求时，如果请求参数重带有`&`符号，请求参数就会被错误的分割，所以需要将`&`转义成`&#38;`。

---

### encodeURI和encodeURIComponent
以前服务端不支持`unicod`编码的时候，前端传递请求参数前需要对参数进行转义。

如将`http://suvllian.com?p=测试`转义成`http://suvllian.com?p=%E6%B5%8B%E8%AF%95`。  
这时可以使用浏览器提供的`encodeURI`方法进行转移，但是`encodeURI`方法不会转义`:`, `/`, `?`, `&`, `=`这些在URL中有特殊含义的字符。

``` javascript
encodeURI('http://suvllian.com?link=http://suvllian.com')

"http://suvllian.com?link=http://suvllian.com"
```

如果URL参数中包含这些字符，可以使用`encodeURIComponent`方法进行转义。

``` javascript
const param = encodeURIComponent('http://suvllian.com');

encodeURI('http://suvllian.com/p?name=测试&from=') + param

"http://suvllian.com/p?name=%E6%B5%8B%E8%AF%95&from=http%3A%2F%2Fsuvllian.com"
```

总结来说，`encodeURI`用于对整个URL进行转义，`encodeURIComponent`用于对参数进行转义。

## 二、CSRF

CSRF（Cross Site Request Forgery），跨站请求伪造。

攻击者可以盗用受害者的信息，以受害者的身份模拟各种请求。攻击者借助社会工程学（电子邮件和聊天发送链接）的一些帮助，诱导Web应用程序用户执行攻击者的操作。

例如用户登陆网络银行查看账户余额之后没有退出，然后点击了别人发送过来的恶意链接，那他的银行账户余额就可能被伪造的请求转移到指定的账户中。

完成CSRF攻击需要具备的条件：  
1.用户已经登录了站点A，并在本地记录了cookie。  
2.在A站点cookie依然生效的情况下，访问了恶意攻击者提供的引诱危险站点B。  
3.A站点没有做任何CSRF防御。

### 防范措施
* cookie设置为httpOnly，js不能获取和操作cookie。
* 重要操作使用POST请求，同时要传递csrf_token。
* URL重写。知乎对URL跳转都做了处理，防止外链网站获取用户信息。
* 校验HTTP referer。

## 三、URL跳转

## 四、DNS劫持
