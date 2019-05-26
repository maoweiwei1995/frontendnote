wepack问题

npm问题

git常见操作

node问题

常见js设计模式

**实现常见css 轮播图**

## 前端常见的攻击方式及预防方法

#### 1.XSS (Cross Site Script) ，跨站脚本攻击

有句话说

> 所有的输入都是有害的。

跨站脚本是最常见的计算机安全漏洞，**跨站脚本攻击**指的是恶意攻击者往Web页面里插入恶意html代码，当用户浏览该页之时，嵌入的恶意html代码会被执行，对受害用户可能采取Cookie资料窃取、会话劫持、钓鱼欺骗等各种攻击。
xss给人留下的印象：

> "xss? 不就是弹出个对话框给自己看吗？"
> “跨站脚本是在客户端执行，xss漏洞关我什么事！”
> “反正xss不能窃取我的root权限。”

其实，随着web2.0信息分享模式和社交网络的发展，xss衍生出的攻击危害还是很大的。最早的MySpace的xss蠕虫攻击，传染了100多万用户，网站瘫痪，后来的知名网址如微博就爆发了xss蠕虫攻击，持续了16分钟。

**危害**：

- 网络钓鱼，包括窃取用户账号；
- 窃取用户cookies资料，从而获得用户隐私信息，或利用用户身份进一步对网站执行操作；
- 劫持用户会话，进行非法转账、强制发表日志、发送电子邮件等；
- 强制弹出广告、刷流量等；
- 恶意操作，删除文章等；
- 网页挂马；
- 传播跨站脚本蠕虫等
  .....

**举个简单栗子** ：

假如你现在是站点上一个用户，发布信息的功能存在漏洞可以执行js。你在此刻输入一个恶意脚本，那么当前所有看到你新信息的人的浏览器都会执行这个脚本弹出提示框 （很爽吧 弹出广告 ：）），如果你做一些更为激进行为呢？后果难以想象。

##### 反射型XSS

早期，通过url的输入，将恶意脚本附加到URL地址的参数中。能够弹出一个警告框，说明跨站脚本攻击漏洞存在。特点：单击时触发，执行一次。
通常出现在网站的搜索栏、用户登入口等地方。
`http://weibo.com/login.php?'u=1931138954'<script>alert(/ssss/)</script>`



![img](https://upload-images.jianshu.io/upload_images/1180061-f26e723f39ae4afc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/660/format/webp)



- ##### XSS 跨框架钓鱼

怎么构造钓鱼？使用标签iframe.
`http://weibo.com/login.php?'u=1931138954'<iframe src='http://www.baidu.com'/>`



![img](https://upload-images.jianshu.io/upload_images/1180061-817532f9b2b1c615.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/662/format/webp)



假如这是一个银行网银登录的网站，存在这样的漏洞，那我们作为攻击者，我可以构造一个类似的页面，把它原有的登录框覆盖掉。我可以把这个链接发给被攻击者，以邮件或者其他的方式，这里需要用到的技术是社会工程学，诱使被攻击者访问这个链接，最终欺骗他来登录，而攻击者就可以轻易的拿到被攻击者的账号和密码信息。也有人问，发过去的这个链接带有这样的标志或参数，被攻击者一眼就能看出来。在这里，我们可以对这段代码进行编码，不管是url的编码还是其他方式的编码，也就是说，被攻击者一眼看不出来这个异常，所以就会被诱使去访问这个链接，最终造成影响。这个是钓鱼攻击。

现在大部分主流浏览器已经对url攻击做了预防方法。
比如在老问吧输入： `https://bar.focus.cn/s?keywords=<script>alert(1)</script>`

chrome： **直接报错**

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_df53d92be12a485bb838cb34d0871069)

ie：**下面会有弹窗提示阻止跨站点脚本攻击**

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_f99ecda9a22a4cc5ae6fefe0f3582e04)

搜狗：**页面结果过滤插入的代码**，控制台输出提示做了xss防御

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_291b9ded477647f78e785bffc2423fa3)

危险：
网站的用户就有被冒名顶替的风险。也就是我们的cookie信息被获取，我们就存在有被冒名顶替的风险。
我们刚才获取cookie的信息不是让它alter告警方式展示给用户，而是通过脚本程序将我们函数获取的cookie信息，发送至远程的恶意网站，远程的这个恶意网站是专门用来接收当前的cookie信息的，当被攻击者触发这个漏洞以后，它当前用户的cookie信息就会被发送到远端的恶意网站。攻击者登录恶意网站，查看到cookie信息后可以对这个用户的控制权限进行冒名顶替。

- HTML注入式钓鱼
  直接利用XSS漏洞注射HTML/JavaScript代码到页面中，或者是把用户输入的数据”存储“在服务器端。比如登录密码直接设置为<script>alert(1)</script>，存入数据库中。

**常见解决办法**:

设计xss Filter，分析用户提交的输入，并消除潜在的跨站脚本攻击、恶意的HTML等。在需要html输入的地方对html标签及一些特殊字符( ” < > & 等等 )做过滤，将其转化为不被浏览器解释执行的字符。

**再举个栗子**：

用ie在问吧的搜索输入`<script>alert('包小姐')</script>`

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_870109472cad45a9b3b4a1f5f4e087e9)

输入

```
<script>alert(document.cookie)</script>
```

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_818b8516f9f34aa0bc692f5f0c233c10)

在微博同样在搜索输入

```
<script>alert('包小姐')</script>
```

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_72aa9ba2094d492ba0a3c9e637ab0ba1)

在知乎搜索输入

```
<script>alert('hello')</script>
```

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_50189f17c8f64f3daa0a1ade88be9648)



在知乎修改密码为：`<script>alert('包小姐')</script>`
结果：没反应，控制台报错HTTP405: 错误方法 - 不支持使用的 HTTP 谓词。

#### 持久型xss

> 攻击者事先将恶意js代码上传或存储到漏洞服务器中，只要受害者浏览包含此恶意js代码的页面就会执行恶意代码。

通常出现在网站的留言、评论、博客日志等。
危害：不需要单击URL触发，危害比反射型xss大，更严重的是能编写**xss蠕虫**（利用ajax/js 编写的蠕虫病毒，能够在网站中实现病毒的几何数级传播，其感染速度和攻击效果都很可怕）。
(1)寻找xss点
发掘个人档案、日志、留言等地方的xss漏洞
(2)实现蠕虫行为
将xss shellcode写进xss点（例如个人档案），引诱用户查看，攻击者利用ajax修改受害用户的个人档案信息，将恶意的代码复制进去。随后任何查看受害者个人档案的也会被感染，执行重复操作，直到xss蠕虫传播。

绕过**xss** Filter：

1、利用<>标记注射HTML/js

解决：过滤和转义`<>`或`<script>`等字符，从而过滤某些形式的xss`<script>shellcode</script>`

2、利用HTML标签属性值执行xss

<table background="javascript:alert(/xss/)"></table>
![](javascript:alert('xss');)
解决：过滤JavaScript等关键字。

3、空格回车Tab

利用空车、回车和Tab键绕过限制，`![](javas cript:alert(/xss/))`

4、对标签属性值转码

HTML属性值支持ASCII形式，`![](javascrip&#116&#58alert(/xss/);)`
解决：最好也过滤&#\等字符。

5、产生自己的事件

`![](#)`，只要图片不存在就会触发onerror事件

6、利用css跨站剖析

使用css样式表执行javascript具有隐蔽、灵活多变的特点

```
<div style="background-image:url(javascript:alert('xss')">
<style>
  <body {background-image:url("javascript:alert('xss')");}
</style>
```

使用link或import引用css`<link rel="stylesheet" href="http://www.evil.com/attack.css">`
`p {background-image: expression(alert("xss"));}`
`<style type='text/css'>@import url(http://www.evil.com/xss.css);</style>`
解决：禁用style标签，过滤标签时过滤style属性，过滤含expression、import等敏感字符的样式表

7、扰乱过滤规则

大小写混淆，不使用引号`<iMg sRC="jaVasCript:alert(0);">`
全角字符`<div style="{left: ｅｘｐｒｅｓｓｉｏｎ（ａｌｅｒｔ(0)）}">`
/**/会被浏览器忽略 `<div style="wid/****/th: expre/*XSS*/ssion(alert('xss'));">`
\和\0 被浏览器忽略`@\0im\port'\0ja\vasc\ript:alert("xss")`;
e转换成\65 `<p style="xss:\65xpression(alert(/xss/))">`

**字符编码**：

可以让xss代码绕过服务端的过滤，还能更好地隐藏Shellcode
`![](javascript:alert('XSS');)`
进行十进制转码后得到
`![](&#106&#97&#118&#97&#115&#99&#114&#105&#112&#116&#58&#97&#108&#101&#114&#116&#40&#39&#88&#83&#83&#39&#41&#59)`
用eval()执行10进制脚本
`![](javascript:eval(String.fromCharCode(97,108,101,114,116,40,39,88,83,83,39,41)))`
style属性中经过十六进制编码逃避过滤

```
<div style="xss:expression(alert(1));"></div>
<img STYLE="background-image:\75\72\6c\28\6a\61\76\61\73\63\72\69\70\74\3a\61\6c\65\72\74\28\27\58\53\53\27\29\29">
```

等等其他编码方式
还有js支持unicode、escapes、十六进制、八进制等编码形式，如果运用于跨站攻击，能大大加强xss的威力。

**拆分跨站法**

当程序没有过滤xss关键字符，却对输入字符长度有限制时。
比如：使用拆分法连续发表4篇文章

> 标题1：<script>z='<script src=';/*
> 标题2：/*z+='http://www.test.c';/*
> 标题3：*/z+='n/1.js></script>';/*
> 标题4：/*document.write(z)</script>

/* * / 在脚本标签中是注释，/*和*/之间的字符被忽略，最终转成：

> <script>z='<script src=';
> z+='http://www.test.c';
> z+='n/1.js></script>';
> document.write(z)</script>

脚本顺利执行。
等等还有很多攻击方式与调用shellcode的方法，这里就不一一讲述了。。。。

##### 防御

确认客户端生成数据的唯一安全方法就是在服务器端实施保护措施。
（1）输出编码

- < 转成 & lt ;
  *> 转成 & gt;
- & 转成 & amp;
  ...

（2）白名单、黑名单
（3）URL属性

- 规定href和src是以http://开头；
- 规定不能有十进制和十六进制的编码字符
- 规定属性以双引号“界定

**前端防御组件**：[js-xss](https://link.jianshu.com/?t=https://github.com/leizongmin/js-xss/blob/master/README.zh.md)

js-xss是一个用于对用户输入的内容进行过滤，以避免遭受XSS攻击的模块，一般是基于**黑白名单**的安全过滤策略。

- 特性
  （1）白名单控制允许的HTML标签及各标签的属性
  （2）通过自定义处理函数，可对任意标签及其属性进行处理
- 安装

**NPM**

```
$ npm install xss
```

- 使用方法

在**node**.js中使用

```
const xss = require('xss');
$('.btnSure').on('click', function(){
  let result = xss($('.input').val());
  putout.html(result);
});
```

结果：能够显示出输入的代码，而不是出现alert弹窗。

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_f97fed44c97542eabc9cf15f75c275f4)

**自定义过滤规则**

通过 `whiteList` 来指定，格式为：`{'标签名': ['属性1', '属性2']}` 。不在白名单上 的标签将被过滤，不在白名单上的属性也会被过滤。以下是示例：

```
// 只允许a标签，该标签只允许 title这个属性
let options = {
  whiteList:{
    a: ['title']
  }
};
// 使用以上配置后，下面的HTML
// <a href="https://bar.focus.cn" title="问吧">问吧问吧</a>
// 将被过滤为
// <a title="问吧">问吧问吧</a>
```

![img](http://avatarimg.bjcnc.img.sohucs.com/bp_0e8e67f5c84e4ef5b4079acf0689c4bb)

自定义CSS**过滤器**

如果配置中允许了标签的 `style`属性，则它的值会通过`cssfilter` 模块处理。`cssfilter`模块包含了一个默认的CSS白名单，你可以通过以下的方式配置：

```
myxss = new xss.FilterXSS({
  css: {
    whiteList: {
      position: /^fixed|relative$/,
      top: true,
      left: true,
    }
  }
});
html = myxss.process('<script>alert("xss");</script>');
```

**去掉不在白名单上的标签**

通过`stripIgnoreTag`来设置：

```
let options = {
  whiteList:{
    'a': ['title']
  },
  stripIgnoreTag: true
};
```

结果：
`code:<script>alert('问吧');</script>`过滤为`code:alert('问吧');`

**去掉不在白名单上的标签及标签体**

通过`stripIgnoreTagBody`来设置：

```
let options = {
  whiteList:{
    'a': ['title']
 },
  stripIgnoreTag: ['script']
};
```

结果：
`code:<script>alert('问吧');</script>`过滤为`code:`

参考资料：
《XSS跨站脚本攻击剖析与防御》 邱永华

### 2.CSRF(Cross Site Request Forgery)，跨站点伪造请求。

攻击者通过各种方法伪造一个请求，模仿用户提交表单的行为，从而达到修改用户的数据，或者执行特定任务的目的。
简单例子：

- 在某个论坛管理页面，管理员可以在list.php页面执行删除帖子操作，根据URL判断删除帖子的id，像这样的一个URL

```
http://localhost/list.php?action=delete&id=12
```

当恶意用户想管理员发送包含CSFR的邮件，骗取管理员访问[http://test.com/csrf.php](https://link.jianshu.com/?t=http://test.com/csrf.php),在这个恶意网页中只要包含这样的html语句就可以利用让管理员在不知情的情况下删除帖子了

```
<img alt="" arc="http://localhost/list.php?action=delete&id=12"/>
```

这个利用了img的src可以跨域请求的特点，这种情况比较少，因为一般网站不会利用get请求修改资源信息。
但是恶意网站这么写一样可以攻击：

```
<!DOCTYPE html>
<html>
　　<body>
　　　　<iframe display="none">
　　　　　　<form method="post" action="http://localhost/list.php">
　　　　　　　　<input type="hidden" name="action" value="delete">
　　　　　　　　<input type="hidden" name="id" value="12">
                <input id="csfr" type="submit"/>
　　　　　　</form>
　　　　</iframe>

        <script type="text/javascript">
      　　　　 document.getElementById('csfr').submit();
　　　　</script>
　　</body>
</html>
```

**解决的思路有**:

1.采用POST请求,增加攻击的难度.用户点击一个链接就可以发起GET类型的请求。而POST请求相对比较难，攻击者往往需要借助javascript才能实现。
2.对请求进行认证，确保该请求确实是用户本人填写表单并提交的，而不是第三者伪造的.具体可以在会话中增加token,确保看到信息和提交信息的是同一个人。（验证码）

### 3.Http Heads攻击

HTTP协议在Response header和content之间，有一个空行，即两组CRLF（0x0D 0A）字符。这个空行标志着headers的结束和content的开始。“聪明”的攻击者可以利用这一点。只要攻击者有办法将任意字符“注入”到headers中，这种攻击就可以发生。

- 以登陆为例:有这样一个url:
  `http://localhost/login?page=http%3A%2F%2Flocalhost%2Findex`
  当登录成功以后，需要重定向回page参数所指定的页面。下面是重定向发生时的response headers.
  `HTTP/1.1 302 Moved Temporarily Date: Tue, 17 Aug 2010 20:00:29 GMT Server: Apache mod_fcgid/2.3.5 mod_auth_passthrough/2.1 mod_bwlimited/1.4 FrontPage/5.0.2.2635 Location: http://localhost/index`

假如把URL修改一下，变成这个样子：
[http://localhost/login?page=http%3A%2F%2Flocalhost%2Fcheckout%0D%0A%0D%0A%3Cscript%3Ealert%28%27hello%27%29%3C%2Fscript%3E](https://link.jianshu.com/?t=http://localhost/login?page=http%3A%2F%2Flocalhost%2Fcheckout%0D%0A%0D%0A%3Cscript%3Ealert%28%27hello%27%29%3C%2Fscript%3E)

那么重定向发生时的reponse会变成下面的样子：
HTTP/1.1 302 Moved Temporarily
Date: Tue, 17 Aug 2010 20:00:29 GMT
Server: Apache mod_fcgid/2.3.5 mod_auth_passthrough/2.1 mod_bwlimited/1.4 FrontPage/5.0.2.2635
Location: [http://localhost/checkout](https://link.jianshu.com/?t=http://localhost/checkout)<CRLF>
<CRLF>
<script>alert('hello')</script>

这个页面可能会意外地执行隐藏在URL中的javascript。类似的情况不仅发生在重定向（Location header）上，也有可能发生在其它headers中，如Set-Cookie header。这种攻击如果成功的话，可以做很多事，例如：执行脚本、设置额外的cookie（<CRLF>Set-Cookie: evil=value）等。
避免这种攻击的方法，就是过滤所有的response headers，除去header中出现的非法字符，尤其是CRLF。

服务器一般会限制request headers的大小。例如Apache server默认限制request header为8K。如果超过8K，Aapche Server将会返回400 Bad Request响应。
对于大多数情况，8K是足够大的。假设应用程序把用户输入的某内容保存在cookie中,就有可能超过8K.攻击者把超过8k的header链接发给受害者,就会被服务器拒绝访问.解决办法就是检查cookie的大小,限制新cookie的总大写，减少因header过大而产生的拒绝访问攻击
测试:
结果控制台输出：SEC7130: “[http://weibo.com/u/1931138954/home?wvr=5&lf=reg%2Fcheckout%0D%0A%0D%0A%3Cscript%3Ealert%28%27hello%27%29%3C%2Fscript%3E](https://link.jianshu.com/?t=http://weibo.com/u/1931138954/home?wvr=5&lf=reg%2Fcheckout%0D%0A%0D%0A%3Cscript%3Ealert%28%27hello%27%29%3C%2Fscript%3E)”中检测到可能的跨站点脚本操作。内容已被 XSS 筛选器修改。

### 一、XSS

【Cross Site Script】跨站脚本攻击 

恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被执行，从而达到恶意攻击用户的目的。

 **1、Reflected XSS（反射型攻击：非持久型，多出现于搜索页面）**

基于反射的XSS攻击，主要依靠站点服务端返回脚本，在客户端触发执行从而发起Web攻击。Web客户端使用Server端脚本生成页面为用户提供数据时，如果未经验证的用户数据被包含在页面中而未经HTML实体编码，客户端代码便能够注入到动态页面中。

 **2、Stored XSS （存储型攻击：持久性；多出现于评论页面和编辑文章页面）**

该类型是应用最为广泛而且有可能影响到Web服务器自身安全的漏洞，骇客将攻击脚本上传到Web服务器上，使得所有访问该页面的用户都面临信息泄漏的可能，其中也包括了Web服务器的管理员。 

**3、DOM-based XSS**

 本地利用漏洞，这种漏洞存在于页面中客户端脚本自身。

**4、防御措施**

1.**对所有用户提交内容进行可靠的输入验证**，包括对URL、查询关键字、HTTP头、POST数据等，仅接受指定长度范围内、采用适当格式、采用所预期的字符的内容提交，对其他的一律过滤。

2.实现**Session标记(session tokens)、CAPTCHA系统**或者HTTP引用头检查，以防功能被第三方网站所执行。

3.确认接收的的内容被妥善的规范化，仅包含最小的、安全的Tag(没有javascript)，去掉任何对远程内容的引用(尤其是样式表和javascript)，使用**HT**TP only的cookie（只有服务器才能访问cookies）。

4**.使用HTTPS**。
5**.后台输入的校验**。

 当然，如上操作将会降低Web业务系统的可用性，用户仅能输入少量的制定字符，人与系统间的交互被降到极致，仅适用于信息发布型站点。

[XSS的原理分析与解剖](http://www.cnblogs.com/lovesong/p/5199623.html) 

[XSS攻击及防御](http://blog.csdn.net/ghsau/article/details/17027893)

### **二、 CSRF** 

【Cross Site Request Forgery】站点伪造请求 

跨站点参考伪造通过在访问用户被认为已经通过身份验证的Web应用程序的页面中包含恶意代码或链接来工作。 如果该Web应用程序的会话没有超时，攻击者可能执行未授权的命令。

**防御措施**

1.验证 HTTP Referer 字段 ;

2.在请求地址中添加 token 并验证 ;

3.在 HTTP 头中自定义属性并验证

4.正确使用GET,POST和Cookie；

5.在非GET请求中增加伪随机数；

[预防CSRF攻击](https://my.oschina.net/biezhi/blog/490249) 

[CSRF攻击介绍及防御](http://blog.csdn.net/u012322925/article/details/51290054)

### 三、SQL注入** 

 防御措施

 采用sql语句预编译和绑定变量，是防御sql注入的最佳方法。采用JDBC的预编译语句集，它内置了处理SQL注入的能力，只要使用它的setXXX方法传值即可。

原理：采用了PreparedStatement，就会将sql语句：”select id, no from user where id=?” 预先编译好，也就是SQL引擎会预先进行语法分析，产生语法树，生成执行计划，也就是说，后面你输入的参数，无论你输入的是什么，都不会影响该sql语句的 语法结构了,只会被当做字符串字面值参数。 

使用正则表达式来过滤一些sql关键字，如or、where等。 

[【Web安全与防御】简析Sql注入与防御措施](http://blog.csdn.net/acmman/article/details/48862841)

 

### **四、cookie窃取和session劫持**

Cookie包含了浏览器客户端的用户凭证，相对较小。Session则维护在服务器，用于维护相对较大的用户信息。cookie被攻击者窃取、session被劫持即攻击者劫持会话，合法登录了你的账户，可以浏览大部分用户资源。 
**用通俗的语言，Cookie是钥匙，Session是锁芯。** 
**最基本的cookie窃取方式：xss漏洞。** 

[cookie窃取和session劫持](http://www.2cto.com/article/201411/355266.html)

 

###  **五、钓鱼攻击【重定向攻击】**

 攻击者会发送给受害者一个合法链接，当链接被点击时，用户被导向一个似是而非的非法网站，从而达到骗取用户信任、窃取用户资料的目的。

防御措施 

对所有的重定向操作进行审核,以避免重定向到一个危险的地方.

 

常见解决方案是白名单,将合法的要重定向的url加到白名单中,非白名单上的域名重定向时拒之;

重定向token,在合法的url上加上token,重定向时进行验证.

 

###  **六、Http Heads攻击**

 HTTP协议在Response header和content之间，有一个空行，即两组CRLF（0x0D 0A）字符。这个空行标志着headers的结束和content的开始。“聪明”的攻击者可以利用这一点。只要攻击者有办法将任意字符“注入”到headers中，这种攻击就可以发生。

防御措施 

过滤所有的response headers，除去header中出现的非法字符，尤其是CRLF。

 **七、拒绝服务攻击【DoS】**

 攻击者想办法让目标机器停止提供服务：一是使用SYN flood迫使服务器的缓冲区满，不接收新的请求；二是使用IP欺骗，迫使服务器把非法用户的连接复位，影响合法用户的连接。

**防御措施** 

**对于SYN flood：启用SYN Cookie、设置SYN最大队列长度以及设置SYN+ACK最大重试次数。**

 

**八、文件上传攻击** 

用户上传一个可执行的脚本文件，并通过此脚本文件获得了执行服务端命令的能力。 

**分类**

**文件名攻击**：上传的文件采用上传之前的文件名,可能造成客户端和服务端字符码不兼容,导致文件名乱码问题;文件名包含脚本,从而造成攻击.

**文件后缀攻击**：上传的文件的后缀可能是exe可执行程序,js脚本等文件,这些程序可能被执行于受害者的客户端,甚至可能执行于服务器上.因此我们必须过滤文件名后缀,排除那些不被许可的文件名后缀.

**文件内容攻击**：IE6有一个很严重的问题 , 它不信任服务器所发送的content type，而是自动根据文件内容来识别文件的类型，并根据所识别的类型来显示或执行文件.如果上传一个gif文件,在文件末尾放一段js攻击脚本,就有可能被执行.这种攻击,它的文件名和content type看起来都是合法的gif图片,然而其内容却包含脚本,这样的攻击无法用文件名过滤来排除，而是必须扫描其文件内容，才能识别。

## 防抖和节流

​       JS中防抖和节流，是前端中最基础的性能优化，在绑定 scroll 、resize 这类事件时，当它发生时，它被触发的频次非常高，间隔很近。如果事件中涉及到大量的位置计算、DOM 操作、元素重绘等工作且这些工作无法在下一个 scroll 事件触发前完成，就会造成浏览器掉帧。加之用户鼠标滚动往往是连续的，就会持续触发 scroll 事件导致掉帧扩大、浏览器 CPU 使用率增加、用户体验受到影响。尤其是在涉及与后端的交互中，前端依赖于某种事件如resize，scroll，发送Http请求，在这个过程中，如果不做防抖处理，那么在事件触发的一瞬间，会有很多个请求发过去，增加了服务端的压力。今天又接触到一个项目，里面的响应式做的非常到位，同时性能优化做的非常好，所以想把JS的防抖和节流做一个总结。

![img](https://images2015.cnblogs.com/blog/608782/201605/608782-20160516205933748-797476534.gif)

### 一、函数防抖（debounce）

​         当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新开始延时。如下图，持续触发scroll事件时，并不执行handle函数，当1000毫秒内没有触发scroll事件时，才会延时触发scroll事件。

下面通过一段代码来演示一个防抖的例子

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            height: 5000px;
        }
    </style>
</head>
<body>
    <script>
        function debounce(fn, wait) {
            var timeout = null;
            return function() {
                if(timeout !== null)
                    clearTimeout(timeout);
                timeout = setTimeout(fn, wait);
            }
        }
        // 处理函数
        function handle() {
            console.log(Math.random());
        }
        // 滚动事件
        window.addEventListener('scroll', debounce(handle, 1000));
    </script>
</body>
</html>
```

大概的效果就是这个样子： 

![img](https://img-blog.csdn.net/20180917111553767?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDAwODkx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

这是我自己的运行结果：

![img](https://img-blog.csdn.net/20180917105458358?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDAwODkx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



### 二、函数节流（throttle）

​        当持续触发事件时，保证一定时间段内只调用一次事件处理函数。节流通俗解释就比如我们水龙头放水，阀门一打开，水哗哗的往下流，秉着勤俭节约的优良传统美德，我们要把水龙头关小点，最好是如我们心意按照一定规律在某个时间间隔内一滴一滴的往下滴。如下图，持续触发scroll事件时，并不立即执行handle函数，每隔1000毫秒才会执行一次handle函数。

#### 时间戳方案 

```javascript
  var throttle = function(func, delay) {
            var prev = Date.now();
            return function() {
                var context = this;
                var args = arguments;
                var now = Date.now();
                if (now - prev >= delay) {
                    func.apply(context, args);
                    prev = Date.now();
                }
            }
        }
        function handle() {
            console.log(Math.random());
        }
        window.addEventListener('scroll', throttle(handle, 1000));
```

#### 定时器方案 

```javascript
var throttle = function(func, delay) {
            var timer = null;
            return function() {
                var context = this;
                var args = arguments;
                if (!timer) {
                    timer = setTimeout(function() {
                        func.apply(context, args);
                        timer = null;
                    }, delay);
                }
            }
        }
        function handle() {
            console.log(Math.random());
        }
        window.addEventListener('scroll', throttle(handle, 1000));
```

####  时间戳+定时器

```javascript
var throttle = function(func, delay) {
     var timer = null;
     var startTime = Date.now();
     return function() {
             var curTime = Date.now();
             var remaining = delay - (curTime - startTime);
             var context = this;
             var args = arguments;
             clearTimeout(timer);
              if (remaining <= 0) {
                    func.apply(context, args);
                    startTime = Date.now();
              } else {
                    timer = setTimeout(func, remaining);
              }
      }
}
function handle() {
      console.log(Math.random());
}

 window.addEventListener('scroll', throttle(handle, 1000));
```

效果如下：

![img](https://img-blog.csdn.net/20180917111627196?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDAwODkx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 三、总结

**函数防抖：**将几次操作合并为一此操作进行。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。

**函数节流：**使得一定时间内只触发一次函数。原理是通过判断是否到达一定时间来触发函数。

**区别：** 函数节流不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数，而函数防抖只是在最后一次事件后才触发一次函数。 比如在页面的无限加载场景下，我们需要用户在滚动页面时，每隔一段时间发一次 Ajax 请求，而不是在用户停下滚动页面操作时才去请求数据。这样的场景，就适合用节流技术来实现。

## 前端性能优化常用总结

参考：<https://www.jianshu.com/p/fe32ef31deed>

前端优化层出不穷，移动端大行其道的现在，我们可以说优化好移动端，PC端也将会更好。所以，我们可以综合以下图片进行一些分析，如图：

![img](https:////upload-images.jianshu.io/upload_images/948614-1752f5c8993cc1a0.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/600/format/webp)

优化

图中已经对前端性能做了一些概括。但其实，我觉得我们可以将这个概括更加精准，扼要，丰富。所以，接下来我会从三个方面就前端性能进行总结：网络方面、DOM操作及渲染方面、数据方面。

### 网络方面

web应用，总是会有一部分的时间浪费在网络连接和资源下载方面。**往往建立一次网络连接是需要时间成本的。而且浏览器同一时间所发送的网络请求数是有限的**。所以，这个层面的优化可以从「减少请求数目」开始：

1. **减少http请求**：在YUI35规则中也有提到，**主要是优化js、css和图片资源三个方面**，因为html是没有办法避免的。因此，我们可以做一下的几项操作：

   1. 合并js文件

   2. 合并css文件

   3. 雪碧图的使用(css sprite)

   4. 使用base64表示简单的图片

   上述四个方法，前面两者我们可以使用webpack之类的打包工具进行打包；

   雪碧图的话，也有专门的制作工具；

   图片的编码是使用base64的，所以，对于一些简单的图片，例如空白图等，可以使用base64直接写入html中。

回到之前网络层面的问题，除了减少请求数量来加快网络加载速度，往往整个资源的体积也是，平时我们会关注的方面。

2. **减小资源体积**：可以通过以下几个方面进行实施：

- gzip压缩
- js混淆
- css压缩
- 图片压缩

gzip压缩主要是针对html文件来说的，它**可以将html中重复的部分进行一个打包，多次复用的过程**。

js的混淆可以有**简单的压缩(将空白字符删除)、丑化(丑化的方法，就是将一些变量缩小)、或者可以使用php对js进行混淆加密。**

css压缩，就是进行简单的压缩。

图片的压缩，**在不影响观感的前提下，尽量压缩图片，使用png等图片格式，减少矢量图、高清图等的使用**。这样子的做法不仅可以加快网页显示，也能减少流量的损耗。

除了以上两部分的操作之外，在网络层面我们还需要做好缓存工作。真正的性能优化来说，缓存是效率最高的一种，往往缩短的加载时间也是最大的。

3.**缓存**：可以通过以下几个方面来描述：

- DNS缓存
- CDN部署与缓存
- http缓存

由于浏览器会在DNS解析步骤中消耗一定的时间，所以，对于一些高访问量网站来说，做好DNS的缓存工作，就会一定程度上提升网站效率。

利用CDN缓存，CDN作为静态资源文件的分发网络，本身就已经提升了网站静态资源的获取速度，同时也给静态资源做好缓存工作，有效的利用已缓存的静态资源，加快获取速度。

http缓存，给资源设定缓存时间，防止在有效的缓存时间内对资源进行重复的下载，从而提升整体网页的加载速度。

其实，网络层面的优化还有很多，特别是针对于移动端页面来说。众所周知，移动端对于网络的敏感度更加的高，除了目前的4G和WIFI之外，其他的移动端网络相当于弱网环境，在这种环境下，资源的缓存利用是相当重要的。而且，减少http的请求次数，也是至关重要的，移动端弱网环境下，对于http请求的时间也会增加。所以，我们可以看一下我们在移动端网络方面可以做的优化：

4. **移动端优化**：使用以下几种方式来加快移动端网络方面的优化：

1. 使用长cache，减少重定向

2. 首屏优化，保证首屏加载数据小于14kb

3. 不滥用web字体

**「使用长cache」，可以使得移动端的部分资源设定长期缓存，这样可以保证资源不用向服务器发送请求，来比较资源是否更新，从而避免304的情况。**304重定向，在PC端或许并不会影响网页的加载速度，但是，在移动端网络不稳定的前提下，多一次请求，就多了一部分加载时间。

**「首屏优化」，对于移动端来说是至关重要的。2s时间是用户的最佳体验，一旦超出这个时间，将会导致用户的流失。所以，针对移动端的网络情况，不可能在这么短时间内加载完成所有的网页资源，所以我们必须保证首屏中的内容被优先显示出来，而且基于TCP的慢启动和拥塞控制，第一个14kb的数据是非常重要的，所以需要保证首页加载数据能够小于14kb。**

「不滥用web字体」，web字体的好处就是，可以代替某些图片资源，但是，**在移动端过多的web字体的使用，会导致页面资源加载的繁重**，所以，慎用web字体

### 渲染和DOM操作方面

首先，简单的聊一下优化渲染的重要性。在网页初步加载时，获取到HTML文件之后，最初的工作是**构建DOM和构建CSSOM两个树**，之后将他们合并形成渲染树，最后对其进行打印。我们可以通过图片来看一下，简单的过程：

![img](https:////upload-images.jianshu.io/upload_images/5797628-b2cd3a2d463b05fd?imageMogr2/auto-orient/strip%7CimageView2/2/w/888/format/webp)

DOM渲染

这里整个过程拉出来写，具体可以再写一篇文章，恕我偷下懒，推荐一篇比较好的文章给大家吧。[浏览器渲染过程与性能优化](https://link.jianshu.com?t=https://juejin.im/post/59d489156fb9a00a571d6509)

继续我们的话题，我们可以如何去缩短这个过程呢？可以从以下几个操作进行优化。

1. **优化网页渲染**：

   1. css的文件放在头部，js文件放在**尾部或者异步**

   2. 尽量避免內联样式

   css文件放在「头部加载」，**可以保证解析DOM的同时，解析css文件**。**因为，CSS（外链或内联）会阻塞整个DOM的渲染，然而DOM解析会正常进行，所以将css文件放在头部进行解析，可以加快网页的构建速度。**假设将其放在尾部，那时DOM树几乎构建，这时就得等到CSSOM树构建完成，才能够继续下面的步骤。

   「js放在尾部」：js文件不同，将js文件放在尾部或者异步加载的原因是**JS（外链或内联）会阻塞后续DOM的解析**，后续DOM的渲染也将被阻塞，而且一旦js中遇到DOM元素的操作，很可能会影响。这方面可以推荐一篇文章——[异步脚本载入提高页面性能](https://link.jianshu.com?t=http://harttle.com/2016/05/18/async-javascript-loading.html)。

   **「避免使用内联样式」，可以有效的减少html的体积**，一般考虑内联样式的时候，往往是样式本身体积比较小，往往加载网络资源的时间会大于它的时候。

除了页面渲染层面的优化，当然最重要的就是DOM操作方面的优化，这部分的优化应该是最多的，而且也是平时开发可以注意的地方。如果开发前期明白这些原理，同时付诸实践的话，就可以在后期的性能完善上面少下很多功夫。那么，接下来我们可以来看一下具体的操作：

2. **DOM操作优化**：

- 避免在document上直接进行频繁的DOM操作

- 使用classname代替大量的内联样式修改

- 对于复杂的UI元素，设置position为absolute或fixed

  

- 尽量使用css动画

- 使用requestAnimationFrame代替setInterval操作

- 适当使用canvas

- 尽量减少css表达式的使用

- 使用事件代理

前面三个操作，其实都是希望『减少回流和重绘』。其实，进行一次DOM操作的代价是非常之大的，以前可以通过网页操作是否卡顿来进行判断，但是，现代浏览器的进步已经大大减少了这方面的影响。但是，我们还是需要清楚，如何去减少回流和重绘的问题。因为这里不想细说这方面的知识，想要了解的话，可以看这篇文章——[回流与重绘：CSS性能让JavaScript变慢？](https://link.jianshu.com?t=http://www.zhangxinxu.com/wordpress/2010/01/%E5%9B%9E%E6%B5%81%E4%B8%8E%E9%87%8D%E7%BB%98%EF%BC%9Acss%E6%80%A7%E8%83%BD%E8%AE%A9javascript%E5%8F%98%E6%85%A2%EF%BC%9F/)。

「尽量使用css动画」，是因为相较于js的复杂动画，css动画比较简单，浏览器本身对其进行了优化，使用上面不会出现卡顿等问题。

「使用requestAnimationFrame代替setInterval操作」，相信大家都有所耳闻，**setInterval定时器会有一定的延时**，对于变动性高的动画来说，会出现卡顿现象。而requestAnimationFrame正好解决的整个问题。

「适当使用canvas」，不得不说canvas是前端的一个进步，出现了它之后，前端界面的复杂性也随之提升了。一些难以完成的动画，都可以使用canvas进行辅助完成。但是，**canvas使用频繁的话，会加重浏览器渲染的压力**，**同时导致性能的下降**。所以，适当时候使用canvas是一个不错的建议。

「尽量减少css表达式的使用」，这个在YUI规则中也被提到过，往往css的表达式在设计之初都是美好的，但在使用过程中，**由于其频繁触发的特性，会拖累网页的性能，出现卡顿**。因此在使用过程中尽量减少css表达式的使用，可以改换成js进行操作。

「使用事件代理」：往往对于具备冒泡性质的事件来说，使用事件代理不失为一种好的方法。举个例子：一段列表都需要设定点击事件，这时如果你给列表中的每一项设定监听，往往会导致整体的性能下降，但是如果你给整个列表设置一个事件，然后通过点击定位目标来触发相应的操作，往往性能就会得到改善。

DOM操作的优化，还有很多，当然也包括移动端的。这个会在之后移动端优化部分被提及，此处先卖个关子。上面我们概述了开始渲染的时候和DOM操作的时候的一些注意事项。接下来要讲的是一些小细节的注意，这些细节可能对于页面影响不大，但是一旦堆积多了，性能也会有所影响。

1. **操作细节注意**：

   - 避免图片或者frame使用空src
   - 在css属性为0时，去掉单位
   - 禁止图像缩放
   - 正确的css前缀的使用
   - 移除空的css规则
   - 对于css中可继承的属性，如font-size，尽量使用继承，少一点设置
   - 缩短css选择器，多使用伪元素等帮助定位

   上述的一些操作细节，是平时在开发中被要求的，更可以理解为开发规范。(基本操作，坐下^_^)

列举完基本操作之后，我们再来聊一下移动端在DOM操作方面的一些优化。

1. **移动端优化**：

   - 长列表滚动优化
   - 函数防抖和函数节流
   - 使用touchstart、touchend代替click
   - HTML的viewport设置
   - 开启GPU渲染加速

   首先，长列表滚动问题，是移动端需要面对的，**IOS尽量使用局部滚动，android尽量使用全局滚动。同时，需要给body添加上-webkit-overflow-scrolling: touch来优化移动段的滚动**。如果有兴趣的同学，可以去了解一下ios和android滚动操作上的区别以及优化。

   「防抖和节流」，**设计到滚动等会被频繁触发的DOM事件，需要做好防抖和节流的工作。它们都是为了限制函数的执行频次**，以优化函数触发频率过高导致的响应速度跟不上触发频率，出现延迟，假死或卡顿的现象。

   > 介绍：**函数防抖**，当调用动作过n毫秒后，才会执行该动作，若在这n毫秒内又调用此动作则将重新计算执行时间；**函数节流**，预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期。

   「touchstart、touchend代替click」，也是移动端比较常用的操作。click在移动端会有300ms延时，这应该是一个常识呗。(不知道的小伙伴该收藏一下呦)。这种方法会影响用户的体验。所以做优化时，最简单的方法就是使用touchstart或者touchend代替click。因为它们事件执行顺序是touchstart->touchmove->touchend->click。或者，使用fastclick或者zepto的tap事件代替click事件。

   「HTML的viewport设置」，可以防止页面的缩放，来优化性能。

   「开启GPU渲染加速」，小伙伴们一定听过CPU吧，但是这里的GPU不能和CPU混为一谈呦。GPU的全名是Graphics Processing Unit，是一种硬件加速方式。一般的css渲染，浏览器的渲染引擎都不会使用到它。但是，在3D渲染时，计算量较大，繁重，浏览器会开启显卡的硬件加速来帮助完成这些操作。所以，我们这里可以**使用css中的translateZ设定，来欺骗浏览器，让其帮忙开启GPU加速，加快渲染进程**。

### 数据方面

数据，也可以说是前端优化方面比较重要的一块内容。页面与用户的交互响应，往往伴随着数据交互，处理，以及ajax的异步请求等内容。所以，我们也可以来聊聊这一块的知识。首先是对于图片数据的处理：

1. **图片加载处理**：

   - 图片预加载
   - 图片懒加载
   - 首屏加载时进度条的显示

   「图片预加载」，预加载的寓意就是提前加载内容。而图片的预加载往往会被用在图片资源比较大，即时加载时会导致很长的等待过程时，才会被使用的。常见场景：图片漫画展示时。往往会预加载一张到两张的图片。

   「图片懒加载」，懒加载或许你是第一次听说，但是，这种方式在开发中会被经常使用。首先，我们需要明白一个道理：**往往只有看到的资源是必须的，其他资源是可以随着用户的滚动，随即显示的**。所以，特别是对于图片资源特别多的网站来说，做好图片的懒加载是可以大大提升网页的载入速度的。

   > 常见的图片懒加载的方式就是：在最初给图片的src设置一个比较简单的图片，然后将图片的真实地址设置给自定义的属性，做一个占位，然后给图片设置监听事件，一旦图片到达视口范围，从图片的自定义属性中获取出真是地址，然后赋值给src，让其进行加载。

   「首屏进度条的显示」：往往对于首屏优化后的数据量并不满意的话，同时也不能进一步缩短首屏包的长度了，就可以使用进度条的方式，来提醒用户进行等待。

讲完了图片这一块数据资源的处理，往往我们需要去优化一下异步请求这一部分的内容。因为，异步的数据获取也是前端不可分割的。这一部分我们也可以做一定的处理：

2.**异步请求的优化**：

- 使用正常的json数据格式进行交互
- 部分常用数据的缓存
- 数据埋点和统计

「JSON交互」，JSON的数据**格式轻巧，结构简单**，往往可以大大优化前后端的数据通信。

「常用数据的缓存」，**可以将一些用户的基本信息等常用的信息做一个缓存，这样可以保证ajax请求的减少**。同时，HTML5新增的storage的内容，也不用怕cookie暴露，引起的信息泄漏问题。

「**数据埋点和统计」**，对于资深的程序员来说，比较了解。而且目前的大部分公司也会做这方面的处理。有心的小伙伴可以自行查阅。

最后，还有就是大量数据的运算。对于javascript语言来说，本身的单线程就限制了它并不能计算大量的数据，往往会造成页面的卡顿。而可能业务中有些复杂的UI需要去运行大量的运算，所以，**webWorker的使用**是至关重要的。或许，前端标准普及的落后，会导致大家对于这些新生事物的短暂缺失吧。

## 封装一个promise

用css实现轮播图



## 正则表达式

> 正则表达式(regular expression)描述了一种字符串匹配的模式，可以用来检查一个字符串是否含有某种子串、将匹配的子串做替换或者从某个字符串中取出符合某个条件的子串等。

> **说白了正则表达式就是处理字符串的,我们可以用它来处理一些复杂的字符串**。

### 为什么要学习正则表达式

我们直接用一个例子来说明

```js
//找出这个字符串中的所有数字
var str = 'abc123de45fgh6789qqq111';
//方法1
     function findNum(str) {
        var tmp = '',
            arr = [];
        for (var i = 0; i < str.length; i++) {
            var cur = str[i];
            if (!isNaN(cur)) {
                tmp += cur;
            } else {
                if (tmp) {
                    arr.push(tmp);
                    tmp = '';
                }
            }
        }
        if (tmp) {
            arr.push(tmp)
        }
        return arr;
    }
    console.log(findNum(str))
    //["123", "45", "6789", "111"]
    
//方法2 使用正则表达式
    var reg = /\d+/g;
    console.log(str.match(reg))
   // ["123", "45", "6789", "111"]
```

通过比较2种方法我们明显看出在对字符串进行处理时，使用正则表达式会简单许多，所以虽然正则表达式看起来像是火星文一样的一堆乱码的东西，但我们还是有必要去学习它的。

### 正则表达式的创建方式

- 字面量创建方式
- 实例创建方式

```js
    var reg = /pattern/flags
    // 字面量创建方式
    var reg = new RegExp(pattern,flags);
    //实例创建方式
    
    pattern:正则表达式  
    flags:标识(修饰符)
    标识主要包括：
    1. i 忽略大小写匹配
    2. m 多行匹配，即在到达一行文本末尾时还会继续寻常下一行中是否与正则匹配的项
    3. g 全局匹配 模式应用于所有字符串，而非在找到第一个匹配项时停止
```

**字面量创建方式和构造函数创建方式的区别**

1. 字面量创建方式不能进行字符串拼接，实例创建方式可以

```js
var regParam = 'cm';
var reg1 = new RegExp(regParam+'1');
var reg2 = /regParam/;
console.log(reg1);  //   /cm1/
console.log(reg2);  //  /regParam/
```

1. 字面量创建方式特殊含义的字符不需要转义，实例创建方式需要转义

```js
var reg1 = new RegExp('\d');  //    /d/ 
var reg2 = new RegExp('\\d')  //   /\d/
var reg3 = /\d/;              //  /\d/
```

### 元字符

代表特殊含义的元字符

```js
\d : 0-9之间的任意一个数字  \d只占一个位置
\w : 数字，字母 ，下划线 0-9 a-z A-Z _
\s : 空格或者空白等 space
\D : 除了\d    
\W : 除了\w
\S : 除了\s
 . : 除了\n之外的任意一个字符
 \ : 转义字符
 | : 或者
() : 分组
\n : 匹配换行符
\b : 匹配边界 字符串的开头和结尾 空格的两边都是边界 => 不占用字符串位数
 ^ : 限定开始位置 => 本身不占位置
 $ : 限定结束位置 => 本身不占位置
[a-z] : 任意字母 []中的表示任意一个都可以
[^a-z] : 非字母 []中^代表除了
[abc] : abc三个字母中的任何一个 [^abc]除了这三个字母中的任何一个字符
```

代表次数的量词元字符

```js
* : 0到多个 >=0
+ : 1到多个 >=1
? : 0次或1次 可有可无
{n} : 正好n次；
{n,} : n到多次
{n,m} : n次到m次
```

量词出现在元字符后面 如\d+，限定出现在前面的元字符的次数

```js
var str = '1223334444';
var reg = /\d{2}/g;
var res = str.match(reg);
console.log(res)  //["12", "23", "33", "44", "44"]

var str ='  我是空格君  ';
var reg = /^\s+|\s+$/g; //匹配开头结尾空格
var res = str.replace(reg,'');
console.log('('+res+')')  //(我是空格君)
```

正则中的()和[]和重复子项 //拿出来单独说一下

- **一般[]中的字符没有特殊含义 如+就表示+但是像\w这样的还是有特殊含义的**

```js
var str1 = 'abc';
var str2 = 'dbc';
var str3 = '.bc';
var reg = /[ab.]bc/; //此时的.就表示.
reg.test(str1)  //true
reg.test(str2)  //false
reg.test(str3)  //true
```

- **[]中，不会出现两位数**

```js
[12]表示1或者2 不过[0-9]这样的表示0到9 [a-z]表示a到z
例如:匹配从18到65年龄段所有的人
var reg = /[18-65]/; // 这样写对么
reg.test('50')
 //Uncaught SyntaxError: Invalid regular expression: /[18-65]/: Range out of order in character class
//聪明的你想可能是8-6这里不对，于是改成[16-85]似乎可以匹配16到85的年龄段的，但实际上发现这也是不靠谱的

实际上我们匹配这个18-65年龄段的正则我们要拆开来匹配
我们拆成3部分来匹配 18-19  20-59 60-65 
reg = /(18|19)|([2-5]\d)|(6[0-5])/;
```

- **()的提高优先级功能:凡是有|出现的时候，我们一定要注意是否有必要加上()来提高优先级；**
- ()的分组 重复子项 (两个放到一起说)

```js
分组：
只要正则中出现了小括号那么就会形成一份分组
只要有分组，exec(match)和replace中的结果就会发生改变(后边的正则方法中再说)

分组的引用(重复子项) :
只要在正则中出现了括号就会形成一个分组，我们可以通过\n (n是数字代表的是第几个分组)来引用这个分组，第一个小分组我们可以用\1来表示

例如：求出这个字符串'abAAbcBCCccdaACBDDabcccddddaab'中出现最多的字母,并求出出现多少次(忽略大小写)。
var str = 'abbbbAAbcBCCccdaACBDDabcccddddaab';
    str = str.toLowerCase().split('').sort(function(a,b){return a.localeCompare(b)}).join('');

    var reg = /(\w)\1+/ig;
    var maxStr = '';
    var maxLen = 0;
    str.replace(reg,function($0,$1){
        var regLen = $0.length;
        if(regLen>maxLen){
            maxLen = regLen;
            maxStr = $1;
        }else if(maxLen == regLen){
            maxStr += $1;
        }
    })
    console.log(`出现最多的字母是${maxStr},共出现了${maxLen}次`)
```

- **当我们加()只是为了提高优先级而不想捕获小分组时，可以在()中加?:来取消分组的捕获**

```js
var str = 'aaabbb';
var reg = /(a+)(?:b+)/;
var res =reg.exec(str);
console.log(res)
//["aaabbb", "aaa", index: 0, input: "aaabbb"]
//只捕获第一个小分组的内容
```

### 正则运算符的优先级

1. 正则表达式从左到右进行计算，并遵循优先级顺序，这与算术表达式非常类似。
2. 相同优先级的会从左到右进行运算，不同优先级的运算先高后低。

```js
下面是常见的运算符的优先级排列
依次从最高到最低说明各种正则表达式运算符的优先级顺序：

\ : 转义符
(), (?:), (?=), []  => 圆括号和方括号
*, +, ?, {n}, {n,}, {n,m}   => 量词限定符
^, $, \任何元字符、任何字符 
|       => 替换，"或"操作

字符具有高于替换运算符的优先级，一般用 | 的时候，为了提高 | 的优先级，我们常用()来提高优先级
如： 匹配 food或者foot的时候 reg = /foo(t|d)/ 这样来匹配
```

### 正则的特性

- 贪婪性

  > 所谓的贪婪性就是正则在捕获时，每一次会尽可能多的去捕获符合条件的内容。
  > 如果我们想尽可能的少的去捕获符合条件的字符串的话，**可以在量词元字符后加?**

- 懒惰性

  > 懒惰性则是正则在成功捕获一次后不管后边的字符串有没有符合条件的都不再捕获。
  > 如果想捕获目标中所有符合条件的字符串的话，**我们可以用标识符g来标明是全局捕获**

```js
var str = '123aaa456';
var reg = /\d+/;  //只捕获一次,一次尽可能多的捕获
var res = str.match(reg)
console.log(res)
// ["123", index: 0, input: "123aaa456"]
reg = /\d+?/g; //解决贪婪性、懒惰性
res = str.match(reg)
console.log(res)
// ["1", "2", "3", "4", "5", "6"]
```

### 和正则相关的一些方法

这里我们只介绍test、exec、match和replace这四个方法

- **reg.test(str)** 用来验证字符串是否符合正则 符合返回true 否则返回false

```js
var str = 'abc';
var reg = /\w+/;
console.log(reg.test(str));  //true
```

- **reg.exec()** 用来捕获符合规则的字符串

```js
var str = 'abc123cba456aaa789';
var reg = /\d+/;
console.log(reg.exec(str))
//  ["123", index: 3, input: "abc123cba456aaa789"];
console.log(reg.lastIndex)
// lastIndex : 0 

reg.exec捕获的数组中 
// [0:"123",index:3,input:"abc123cba456aaa789"]
0:"123" 表示我们捕获到的字符串
index:3 表示捕获开始位置的索引
input 表示原有的字符串
```

当我们用exec进行捕获时，如果正则没有加'g'标识符，则exec捕获的每次都是同一个，当正则中有'g'标识符时 捕获的结果就不一样了,我们还是来看刚刚的例子

```js
var str = 'abc123cba456aaa789';
var reg = /\d+/g;  //此时加了标识符g
console.log(reg.lastIndex)
// lastIndex : 0 

console.log(reg.exec(str))
//  ["123", index: 3, input: "abc123cba456aaa789"]
console.log(reg.lastIndex)
// lastIndex : 6

console.log(reg.exec(str))
// ["456", index: 9, input: "abc123cba456aaa789"]
console.log(reg.lastIndex)
// lastIndex : 12

console.log(reg.exec(str))
// ["789", index: 15, input: "abc123cba456aaa789"]
console.log(reg.lastIndex)
// lastIndex : 18

console.log(reg.exec(str))
// null
console.log(reg.lastIndex)
// lastIndex : 0

每次调用exec方法时,捕获到的字符串都不相同
lastIndex ：这个属性记录的就是下一次捕获从哪个索引开始。
当未开始捕获时，这个值为0。          
如果当前次捕获结果为null。那么lastIndex的值会被修改为0.下次从头开始捕获。
而且这个lastIndex属性还支持人为赋值。
```

**exec的捕获还受分组()的影响**

```js
var str = '2017-01-05';
var reg = /-(\d+)/g
// ["-01", "01", index: 4, input: "2017-01-05"]
"-01" : 正则捕获到的内容
"01"  : 捕获到的字符串中的小分组中的内容
```

- **str.match(reg)** 如果匹配成功，就返回匹配成功的数组，如果匹配不成功，就返回null

```js
//match和exec的用法差不多
var str = 'abc123cba456aaa789';
var reg = /\d+/;
console.log(reg.exec(str));
//["123", index: 3, input: "abc123cba456aaa789"]
console.log(str.match(reg));
//["123", index: 3, input: "abc123cba456aaa789"]
```

上边两个方法console的结果有什么不同呢？二个字符串是一样滴。
当我们进行全局匹配时，二者的不同就会显现出来了.

```js
var str = 'abc123cba456aaa789';
var reg = /\d+/g;
console.log(reg.exec(str));
// ["123", index: 3, input: "abc123cba456aaa789"]
console.log(str.match(reg));
// ["123", "456", "789"]
```

当全局匹配时，match方法会一次性把符合匹配条件的字符串全部捕获到数组中,
如果想用exec来达到同样的效果需要执行多次exec方法。

> 我们可以尝试着用exec来简单模拟下match方法的实现。

```js
 String.prototype.myMatch = function (reg) {
    var arr = [];
    var res = reg.exec(this);
    if (reg.global) {
        while (res) {
            arr.push(res[0]);
            res = reg.exec(this)
        }
    }else{
        arr.push(res[0]);
    }
    return arr;
}

var str = 'abc123cba456aaa789';
var reg = /\d+/;
console.log(str.myMatch(reg))
// ["123"]

var str = 'abc123cba456aaa789';
var reg = /\d+/g;
console.log(str.myMatch(reg))
// ["123", "456", "789"]
```

此外，match和exec都可以受到分组()的影响，不过match只在没有标识符g的情况下才显示小分组的内容，如果有全局g，则match会一次性全部捕获放到数组中

```js
var str = 'abc';
var reg = /(a)(b)(c)/;

console.log( str.match(reg) );
// ["abc", "a", "b", "c", index: 0, input: "abc"]
console.log( reg.exec(str) );
// ["abc", "a", "b", "c", index: 0, input: "abc"]


当有全局g的情况下
var str = 'abc';
var reg = /(a)(b)(c)/g;
console.log( str.match(reg) );
// ["abc"]
console.log( reg.exec(str) );
// ["abc", "a", "b", "c", index: 0, input: "abc"]
```

- **str.replace()** 这个方法大家肯定不陌生，现在我们要说的就是和这个方法和正则相关的东西了。

```js
正则去匹配字符串，匹配成功的字符去替换成新的字符串
写法：str.replace(reg,newStr);

var str = 'a111bc222de';
var res = str.replace(/\d/g,'Q')
console.log(res)
// "aQQQbcQQQde"

replace的第二个参数也可以是一个函数
str.replace(reg,fn);

var str = '2017-01-06';
str = str.replace(/-\d+/g,function(){
    console.log(arguments)
})

控制台打印结果：
["-01", 4, "2017-01-06"]
["-06", 7, "2017-01-06"]
"2017undefinedundefined"
从打印结果我们发现每一次输出的值似乎跟exec捕获时很相似，既然与exec似乎很相似，那么似乎也可以打印出小分组中的内容喽 

var str = '2017-01-06';
str = str.replace(/-(\d+)/g,function(){
    console.log(arguments)
})
["-01", "01", 4, "2017-01-06"]
["-06", "06", 7, "2017-01-06"]
"2017undefinedundefined"
从结果看来我们的猜测没问题。

此外，我们需要注意的是，如果我们需要替换replace中正则找到的字符串，函数中需要一个返回值去替换正则捕获的内容。
```

通过replace方法获取url中的参数的方法

```js
(function(pro){
    function queryString(){
        var obj = {},
            reg = /([^?&#+]+)=([^?&#+]+)/g;
        this.replace(reg,function($0,$1,$2){
            obj[$1] = $2;
        })
        return obj;
    }
    pro.queryString = queryString;
}(String.prototype));

// 例如 url为 https://www.baidu.com?a=1&b=2
// window.location.href.queryString();
// {a:1,b:2}
```

### 零宽断言

用于查找在某些内容(但并不包括这些内容)之前或之后的东西，如\b,^,$那样用于指定一个位置，这个位置应该满足一定的条件(即断言)，因此它们也被称为零宽断言。

> 在使用正则表达式时，捕获的内容前后必须是特定的内容，而我们又不想捕获这些特定内容的时候，零宽断言就可以派上用场了。

- 零宽度正预测先行断言 (?=exp)
- 零宽度负预测先行断言 (?!exp)
- 零宽度正回顾后发断言 (?<=exp)
- 零宽度负回顾后发断言 (?<!exp)

这四胞胎看着名字好长，给人一种好复杂好难的感觉，我们还是挨个来看看它们究竟是干什么的吧。

- (?=exp) 这个简单理解就是说字符出现的**位置**的右边必须匹配到exp这个表达式。

```js
var str = "i'm singing and dancing";
var reg = /\b(\w+(?=ing\b))/g
var res = str.match(reg);
console.log(res)
// ["sing", "danc"]


注意一点，这里说到的是位置，不是字符。
var str = 'abc';
var reg = /a(?=b)c/;
console.log(res.test(str));  // false

// 这个看起来似乎是正确的，实际上结果是false
reg中a(?=b)匹配字符串'abc' 字符串a的右边是b这个匹配没问题,接下来reg中a(?=b)后边的c匹配字符串时是从字符串'abc'中a的后边b的前边的这个位置开始匹配的，
这个相当于/ac/匹配'abc',显然结果是false了
```

- (?!exp) 这个就是说字符出现的**位置**的右边不能是exp这个表达式。

```js
var str = 'nodejs';
var reg = /node(?!js)/;
console.log(reg.test(str)) // false
```

- (?<=exp) 这个就是说字符出现的**位置**的前边是exp这个表达式。

```js
var str = '￥998$888';
var reg = /(?<=\$)\d+/;
console.log(reg.exec(str)) //888
```

- (?<!exp) 这个就是说字符出现的**位置**的前边不能是exp这个表达式。

```js
var str = '￥998$888';
var reg = /(?<!\$)\d+/;
console.log(reg.exec(str)) //998
```

最后，来一张思维导图

![Paste_Image.png](https://upload-images.jianshu.io/upload_images/2791152-a45fef5bcfabcf2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 事件

### 一、事件

**事件是文档或者浏览器窗口中发生的，特定的交互瞬间。**

事件是用户或浏览器自身执行的某种动作，如click,load和mouseover都是事件的名字。

事件是javaScript和DOM之间交互的桥梁。

你若触发，我便执行——事件发生，调用它的处理函数执行相应的JavaScript代码给出响应。

典型的例子有：页面加载完毕触发load事件；用户单击元素，触发click事件。

### 二、事件流

**事件流描述的是从页面中接收事件的顺序。**

### 1、事件流感性认识

问题：单击页面元素，什么样的元素能感应到这样一个事件？

答案：单击元素的同时，也单击了元素的容器元素，甚至整个页面。

例子：有三个同心圆， 给每个圆添加对应的事件处理函数，弹出对应的文字。单击最里面的圆，同时也单击了外面的圆，所以外面圆的click事件也会被触发。

 效果如下：

[![img](https://images0.cnblogs.com/blog/315302/201411/010913358782848.png)](http://www.cnblogs.com/starof/p/4066381.html)

[![img](https://images0.cnblogs.com/blog/315302/201411/010913543944892.png)](http://www.cnblogs.com/starof/p/4066381.html)[ ![img](https://images0.cnblogs.com/blog/315302/201411/010915517849460.png)](http://www.cnblogs.com/starof/p/4066381.html)[ ![img](https://images0.cnblogs.com/blog/315302/201411/010916084409806.png)](http://www.cnblogs.com/starof/p/4066381.html)

### 2、事件流

**事件发生时会在元素节点与根节点之间按照特定的顺序传播**，**路径所经过的所有节点都会收到该事件，这个传播过程即DOM事件流。**

#### 1、两种事件流模型

事件传播的顺序对应浏览器的两种事件流模型：**捕获型事件流和冒泡型事件流**。

**冒泡型事件流**：事件的传播是从**最特定**的**事件目标**到最不特定的**事件目标**。即从DOM树的**叶子到根**。**【推荐】**

**捕获型事件流**：事件的传播是从**最不特定**的**事件目标**到最特定的**事件目标**。即从DOM树的**根到叶子**。

事件捕获的思想就是不太具体的节点应该更早接收到事件，而最具体的节点最后接收到事件。

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
<div id="myDiv">Click me!</div>
</body>
</html>
```

上面这段html代码中，单击了页面中的<div>元素，

在冒泡型事件流中click事件传播顺序为**<div>**—》**<body>**—》**<html>**—》**document**

在捕获型事件流中click事件传播顺序为**document**—》**<html>**—》**<body>**—》**<div>**

 ![img](https://images0.cnblogs.com/blog/315302/201411/010945436598199.png)![img](https://images0.cnblogs.com/blog/315302/201411/010945579257474.png)

**note**:

1）、**所有现代浏览器都支持事件冒泡**，但在具体实现中略有差别：

IE5.5及更早版本中事件冒泡会跳过<html>元素(从body直接跳到document)。

IE9、Firefox、Chrome、和Safari则将事件一直冒泡到window对象。

2）、IE9、Firefox、Chrome、Opera、和Safari都支持事件捕获。尽管DOM标准要求事件应该从document对象开始传播，但这些浏览器都是从window对象开始捕获事件的。

3）、由于老版本浏览器不支持，很少有人使用事件捕获。**建议使用事件冒泡**。

#### **2、DOM事件流**

**DOM标准采用捕获+冒泡**。两种事件流都会触发DOM的所有对象，从document对象开始，也在document对象结束。

![img](https://images2015.cnblogs.com/blog/315302/201606/315302-20160621155328756-279009443.png)

DOM标准规定事件流包括三个阶段：**事件捕获阶段、处于目标阶段和事件冒泡阶段**。

- **事件捕获阶段**：**实际目标**（<div>）在捕获阶段**不会接收事件**。也就是在捕获阶段，事件从document到<html>再到<body>就停止了。上图中为1~3.
- **处于目标阶段**：事件在<div>上发生并处理。**但是事件处理会被看成是冒泡阶段的一部分**。
- **冒泡阶段**：事件又传播回文档。

**note**:

1）、尽管“DOM2级事件”标准规范明确规定事件捕获阶段不会涉及事件目标，但是在IE9、Safari、Chrome、Firefox和Opera9.5及更高版本都会在捕获阶段触发事件对象上的事件。结果，就是有两次机会在目标对象上面操作事件。

2）、**并非所有的事件都会经过冒泡阶段** 。所有的事件都要经过捕获阶段和处于目标阶段，但是有些事件会跳过冒泡阶段：如，获得输入焦点的focus事件和失去输入焦点的blur事件。

两次机会在目标对象上面操作事件例子：

运行效果就是会陆续弹出6个框，为说明原理我整合成了一个图：

[![img](https://images0.cnblogs.com/blog/315302/201411/052135036896502.png)](http://www.cnblogs.com/starof/p/4066381.html)

### 3、事件流的典型应用事件代理

传统的事件处理中，需要为**每个元素**添加事件处理器。js事件代理则是一种简单有效的技巧，通过它可以把事件处理器添加到**一个父级元素**上，从而避免把事件处理器添加到**多个子级元素**上。

#### 1、事件代理

事件代理的原理用到的就是**事件冒泡和目标元素**，**把事件处理器添加到父元素，等待子元素事件冒泡**，并且父元素能够通过target（IE为srcElement）判断是哪个子元素，从而做相应处理。

事件代理的处理方式，代码如下：

```html
<body>
<ul id="color-list">
<li>red</li>
<li>orange</li>
<li>yellow</li>
<li>green</li>
<li>blue</li>
<li>indigo</li>
<li>purple</li>
</ul>
<script>
(function(){
    var colorList=document.getElementById("color-list");
    colorList.addEventListener('click',showColor,false);
    function showColor(e)
    {
        e=e||window.event;
        var targetElement=e.target||e.srcElement;
        if(targetElement.nodeName.toLowerCase()==="li"){
        alert(targetElement.innerHTML);
        }
    }
})();
</script>
</body>
```

#### 2、事件代理的好处***

1. 将多个事件处理器减少到一个，因为事件处理器要驻留内存，这样就提高了性能。
2. DOM更新无需重新绑定事件处理器，因为事件代理对不同子元素可采用不同处理方法。如果新增其他子元素（a,span,div等），直接修改事件代理的事件处理函数即可。

#### 3、事件代理的问题：

代码如下：事件代理同时绑定了li和span，当点击span的时候，li和span都会冒泡。

```html
<li><span>li中的span的内容</span></li>

<script>
    $(document).on('click', 'li', function(e){
        alert('li li');
    });

    $(document).on('click', 'span', function(e){
        alert('li span');
    })
</script>
```

解决办法：

方法一：span的事件处理程序中阻止冒泡

```js
 $(document).on('click', 'span', function(e){
        alert('li span');
        e.stopPropagation();
    })
```

方法二：li的事件处理程序中检测target元素

```js
 $(document).on('click', 'li', function (e) {
        if (e.target.nodeName == 'SPAN') {
            e.stopPropagation();
            return;
        }
        alert('li li');
    });
```

#### 4、事件代理的一个有趣应用

点击一个列表时，输出对应的索引

```html
<script>
    var ul=document.querySelector('ul');
    var lis=ul.querySelectorAll('ul li');
    ul.addEventListener('click', function (e) {
        var target= e.target;
        if(target.nodeName.toUpperCase()==='LI'){
            alert([].indexOf.call(lis,target));
        }
    },false)
</script>
```

### 三、事件处理程序

前面提到，事件是用户或浏览器自身执行的某种动作，如click,load和mouseover都是事件的名字。响应某个事件的函数就叫**事件处理程序**（也叫**事件处理函数**、**事件句柄**）。事件处理程序的名字以"on"开头，因此click事件的事件处理程序就是onclick,load事件的事件处理程序就是onload。

为事件指定事件处理程序的方法主要有3种。

#### 1、html事件处理程序

**事件直接加在html元素上。**

首先，这种方法已经过时了。因为动作(javascript代码)和内容(html代码)紧密耦合，修改时即要修改html也要修改js。但是写个小demo的时候还是可以使用的。

这种方式也有两种方法，都很简单：

第一种：直接在html中定义事件处理程序及包含的动作。

```html
<input type="button" value="click me!" onclick="alert('clicked!')"/>
```

第二种：html中定义事件处理程序，执行的动作则调用其他地方定义的脚本。

```html
<input type="button" value="click me!" onclick="showMessage()"/>
<script>
function showMessage(){
    alert("clicked!");
}
</script>
```

**note**:

1）通过event变量可以直接访问事件本身，比如onclick="alert(event.type)"会弹出click事件。

2）this值等于事件的目标元素，这里目标元素是input。比如 onclick="alert(this.value)"可以得到input元素的value值。

#### 2、DOM0级事件处理程序

**把一个函数赋值给一个事件处理程序属性。**

这种方法简单而且跨浏览器，但是只能为一个元素添加一个事件处理函数。

因为这种方法为元素添加多个事件处理函数，则后面的会覆盖前面的。

**添加事件处理程序**：

```html
<input id="myBtn" type="button" value="click me!"/>
<script>
    var myBtn=document.getElementById("myBtn");
    myBtn.onclick=function(){
        alert("clicked!");
    }
</script>
```

分析：

```html
/*
第一步：myBtn=document.getElementById("myBtn");取得btn对象
第二步：myBtn.onclick其实相当于给myBtn添加了一个onclick的属性。
第三步：myBtn.onclick=function(){
    alert("clicked!");
}
把函数赋值给onclick事件处理程序属性。
*/
```

**删除事件处理程序**：

```js
    myBtn.onclick=null;
```

#### 3、DOM2级事件处理程序

DOM2级事件处理程序可以为一个元素添加多个事件处理程序。其定义了两个方法用于添加和删除事件处理程序：**addEventListener()和removeEventListener()**。

**所有的DOM节点都包含这2个方法。**

这两个方法都需要3个参数：事件名，事件处理函数，布尔值。

这个布尔值为true,在捕获阶段处理事件，为false，在冒泡阶段处理事件，**默认为false**。

**添加事件处理程序**：现在为按钮添加两个事件处理函数，一个弹出“hello”,一个弹出“world”。

```html
<input id="myBtn" type="button" value="click me!"/>
<script>
    var myBtn=document.getElementById("myBtn");
    myBtn.addEventListener("click",function(){
        alert("hello");
    },false);
    myBtn.addEventListener("click",function(){
        alert("world");
    },false);
</script>
```

**删除事件处理程序**：通过addEventListener添加的事件处理程序必须通过removeEventListener删除，且参数一致。

**note**:**通过addEventListener添加的匿名函数将无法删除。**下面这段代码将不起作用！

```html
    myBtn.removeEventListener("click",function(){
        alert("world");
    },false);
```

看似该removeEventListener与上面的addEventListener参数一致，实则第二个参数中匿名函数是完全不同的。

所以为了能删除事件处理程序，代码可以这样写：

```html
<input id="myBtn" type="button" value="click me!"/>
<script>
    var myBtn=document.getElementById("myBtn");
    var handler=function(){
        alert("hello");
    }
    myBtn.addEventListener("click",handler,false);
    myBtn.removeEventListener("click",handler,false);
</script>
```

### 四、IE事件处理程序

#### 1、实际应用场景

IE8及以下浏览器不支持addEventListener，在实际开发中如果要兼容到IE8及以下浏览器。如果用原生的绑定事件，需要做兼容处理，可利用jquery的bind代替。

![img](https://images2015.cnblogs.com/blog/315302/201603/315302-20160323104308636-366856872.jpg)

#### 2、IE8事件绑定

IE8及以下版本浏览器实现了与DOM中类似的两个方法：attachEvent()和detachEvent()。

这两个方法都需要两个参数：**事件处理程序名称**和**事件处理程序函数**。由于IE8及更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段。注意是事件处理程序名称而不是事件名称，所以要加上on，是onclick而不是click。

**note**:

**IE11只支持addEventListener！**

**IE9，IE10对attachEvent和addEventListener都支持！**

**TE8及以下版本只支持attachEvent！**

可以拿下面代码在IE各个版本浏览器中进行测试。

```html
<input id="myBtn" type="button" value="click me!"/>
<script>
    var myBtn=document.getElementById("myBtn");
    var handlerIE=function(){
        alert("helloIE");
    }
    var handlerDOM= function () {
        alert("helloDOM");
    }
    myBtn.addEventListener("click",handlerDOM,false);
    myBtn.attachEvent("onclick",handlerIE);
</script>
```

**添加事件处理程序**：现在为按钮添加两个事件处理函数，一个弹出“hello”,一个弹出“world”。

```html
<script>
    var myBtn=document.getElementById("myBtn");
    myBtn.attachEvent("onclick",function(){
        alert("hello");
    });
    myBtn.attachEvent("onclick",function(){
        alert("world");
    });
</script>
```

**note**:这里运行效果值得注意一下：

IE8以下浏览器中先弹出“world”，再弹出“hello”。和DOM中事件触发顺序相反。

IE9及以上浏览器先弹出“hello”，再弹出“world”。和DOM中事件触发顺序相同了。

可见IE浏览器慢慢也走上正轨了。。。

**删除事件处理程序**:通过attachEvent添加的事件处理程序必须通过detachEvent方法删除，且参数一致。

和DOM事件一样，添加的匿名函数将无法删除。

所以为了能删除事件处理程序，代码可以这样写：

```html
<input id="myBtn" type="button" value="click me!"/>
<script>
    var myBtn=document.getElementById("myBtn");
    var handler= function () {
        alert("hello");
    }
    myBtn.attachEvent("onclick",handler);
    myBtn.detachEvent("onclick",handler);
</script>
```

**note**:IE事件处理程序中还有一个地方需要注意：**作用域**。

使用attachEvent()方法，事件处理程序会在全局作用域中运行，因此this等于window。

而dom2或dom0级的方法作用域都是在元素内部，this值为目标元素。

下面例子会弹出true。

```html
<input id="myBtn" type="button" value="click me!"/>
<script>
    var myBtn=document.getElementById("myBtn");
    myBtn.attachEvent("onclick",function(){
        alert(this===window);
    });
</script>
```

在编写跨浏览器的代码时，需牢记这点。

IE7\8检测：

```js
 //判断IE7\8 兼容性检测
            var isIE=!!window.ActiveXObject;
            var isIE6=isIE&&!window.XMLHttpRequest;
            var isIE8=isIE&&!!document.documentMode;
            var isIE7=isIE&&!isIE6&&!isIE8;
             
            if(isIE8 || isIE7){
                li.attachEvent("onclick",function(){
                    _marker.openInfoWindow(_iw);
                })    
            }else{
                li.addEventListener("click",function(){
                    _marker.openInfoWindow(_iw);
                })
            }
```

### 五、事件对象

**什么是事件对象？在触发DOM上的事件时都会产生一个对象。**

#### 1、认识事件对象

事件在浏览器中是以对象的形式存在的，即event。触发一个事件，就会产生一个事件对象event，该对象**包含着所有与事件有关的信息**。包括导致事件的元素、事件的类型以及其他与特定事件相关的信息。

例如：鼠标操作产生的event中会包含鼠标位置的信息；键盘操作产生的event中会包含与按下的键有关的信息。

所有浏览器都支持event对象，但支持方式不同，在DOM中event对象必须作为唯一的参数传给事件处理函数，在IE中event是window对象的一个属性。

#### 2、html事件处理程序中event

```html
<input id="btn" type="button" value="click" onclick=" console.log('html事件处理程序'+event.type)"/>
```

这样会创建一个包含局部变量event的函数。可通过event直接访问事件对象。

3、DOM中的事件对象

DOM0级和DOM2级事件处理程序都会把event作为参数传入。

**根据习惯来：可以用e，或者ev或者event。**

```html
<body>
<input id="btn" type="button" value="click"/>
<script>
    var btn=document.getElementById("btn");
    btn.onclick=function(event){
        console.log("DOM0 & click");
        console.log(event.type);    //click
    }
    btn.addEventListener("click", function (event) {
        console.log("DOM2 & click");
        console.log(event.type);    //click
    },false);
</script>
</body>
```

DOM中事件对象重要**属性和方法**。

**属性**：

- type属性，用于获取事件类型
- target属性 用户获取事件目标 事件加在哪个元素上。（更具体target.nodeName）

**方法**：

- stopPropagation()方法 用于阻止事件冒泡
- preventDefault()方法 阻止事件的默认行为 **移动端用的多**

### 4、IE中的事件对象

第一种情况： 通过DOM0级方法添加事件处理程序时，event对象作为window对象的一个属性存在。

```html
<body>
<input id="btn" type="button" value="click"/>
<script>
    var btn=document.getElementById("btn");
    btn.onclick= function () {
        var event=window.event;
       console.log(event.type); //click
    }

</script>
</body>
```

第二种情况：通过attachEvent()添加的事件处理程序，event对象作为参数传入。

```html
<body>
<input id="btn" type="button" value="click"/>
<script>
    var btn=document.getElementById("btn");
    btn.attachEvent("onclick", function (type) {
        console.log(event.type);    //click
    })
</script>
</body>
```

 IE中事件对象重要**属性和方法**。

属性：

- type属性，用于获取事件类型(一样)
- srcElement属性 用户获取事件目标 事件加在哪个元素上。（更具体target.nodeName）

```js
//兼容性处理
function showMsg(event){
    event=event||window.event;  //IE8以前必须是通过window获取event，DOM中就是个简单的传参
    var ele=event.target || event.srcElement; //获取目标元素，DOM中用target,IE中用srcElement
    alert(ele);
 }
e.stopPropagation()
preventDefault()
cancelBubble = true
returnValue = false
```

- cancelBubble**属性** 用于阻止事件冒泡 IE中cancelBubble为属性而不是方法，true表示阻止冒泡。

- returnValue**属性** 阻止事件的默认行为 false表示阻止事件的默认行为

但是我有两个地方不懂。

1、通过DOM0级方法添加的事件处理程序中同样可以传入一个event参数，它的type和window.event.type一样，但是传入的event参数却和window.event不一样，为什么？

```js
    btn.onclick= function (event) {
        var event1=window.event;
        console.log('event1.type='+event1.type);  //event1.type=click
        console.log('event.type='+event.type);    //event.type=click
        console.log('event1==event?'+(event==event1));  //event1==event?false
    }
```

2、通过attachEvent添加的事件处理程序中传入的event和window.event是不一样的，为什么？

```html
<body>
<input id="btn" type="button" value="click"/>
<script>
    var btn=document.getElementById("btn");
    btn.attachEvent("onclick", function (type) {
        console.log(event.type);    //click
        console.log("event==window.event?"+(event==window.event)); //event==window.event?false
    })
</script>
</body>
```

### 六、事件对象的公共成员

#### 1、DOM中的event的公共成员

event对象包含与创建它的特定事件有关的属性和方法。触发的事件类型不一样，可用的属性和方法不一样。但是，DOM中所有事件都有以下公共成员。【注意bubbles属性和cancelable属性】

| 属性/方法                  | 类型         | 读/写 | 说明                                                         |
| -------------------------- | ------------ | ----- | ------------------------------------------------------------ |
| bubbles                    | Boolean      | 只读  | 表明事件是否冒泡                                             |
| **stopPropagation()**      | Function     | 只读  | 取消事件的进一步捕获或冒泡。如果bubbles为true,则可以使用这个方法 |
| stopImmediatePropagation() | Function     | 只读  | 取消事件的进一步捕获或冒泡**，同时阻止任何事件处理程序被调用**（DOM3级事件中新增） |
| cancelable                 | Boolean      | 只读  | 表明是否可以取消事件的默认行为                               |
| **preventDefault()**       | Function     | 只读  | 取消事件的默认行为。如果cancelable是true，则可以使用这个方法 |
| defaultPrevented           | Boolean      | 只读  | 为true表示已经调用了preventDefault()(DOM3级事件中新增)       |
| **currentTarget**          | Element      | 只读  | 其事件处理程序当前正在处理事件的那个元素（**currentTarget始终===this,即处理事件的元素**） |
| **target**                 | Element      | 只读  | 直接事件目标，**真正触发事件的目标**                         |
| detail                     | Integer      | 只读  | 与事件相关的细节信息                                         |
| **eventPhase**             | Integer      | 只读  | 调用事件处理程序的阶段：1表示捕获阶段，2表示处于目标阶段，3表示冒泡阶段 |
| trusted                    | Boolean      | 只读  | 为true表示事件是由浏览器生成的。为false表示事件是由开发人员通过JavaScript创建的（DOM3级事件中新增） |
| **type**                   | String       | 只读  | 被触发的事件的类型                                           |
| view                       | AbstractView | 只读  | 与事件关联的抽象视图。等同于发生事件的window对象             |

##### 1、对比currentTarget和target

在事件处理程序内部，**对象this始终等于currentTarget的值**，而target则只是包含事件的实际目标。

举例：页面有个按钮，在body（按钮的父节点）中注册click事件，点按钮时click事件会冒泡到body进行处理。

```html
<body>
<input id="btn" type="button" value="click"/>
<script>
    document.body.onclick=function(event){
        console.log("body中注册的click事件");
        console.log("this===event.currentTarget? "+(this===event.currentTarget)); //true
        console.log("currentTarget===document.body?"+(event.currentTarget===document.body)); //true
        console.log('event.target===document.getElementById("btn")? '+(event.target===document.getElementById("btn"))); //true
    }
</script>
</body>
```

运行结果为：

[![img](https://images0.cnblogs.com/blog/315302/201411/132236460227118.jpg)](http://www.cnblogs.com/starof/p/4096198.html)

##### 2、通过type属性，可以在一个函数中处理多个事件。

原理：通过检测event.type属性，对不同事件进行不同处理。

举例：定义一个handler函数用来处理3种事件：click,mouseover,mouseout。

```html
<body>
<input id="btn" type="button" value="click"/>
<script>
var handler=function(event){
    switch (event.type){
        case "click":
            alert("clicked");
            break;
        case "mouseover":
            event.target.style.backgroundColor="pink";
            break;
        case "mouseout":
            event.target.style.backgroundColor="";
    }
};
    var btn=document.getElementById("btn");
    btn.onclick=handler;
    btn.onmouseover=handler;
    btn.onmouseout=handler;
</script>
</body>
```

运行效果：点击按钮，弹出框。鼠标经过按钮，按钮背景色变为粉色；鼠标离开按钮，背景色恢复默认。

##### 3、stopPropagation()和stopImmediatePropagation()对比

同：stopPropagation()和 stopImmediatePropagation()都可以用来取消事件的进一步捕获或冒泡。

异：二者的区别在于当一个事件有多个事件处理程序时，stopImmediatePropagation()可以阻止之后事件处理程序被调用。

举例：

运行效果：

[![img](https://images0.cnblogs.com/blog/315302/201411/142318286782479.jpg)](http://www.cnblogs.com/starof/p/4096198.html)

##### 4、eventPhase

eventPhase值在捕获阶段为1，处于目标阶段为2，冒泡阶段为3。

| 常量                  | 值   |
| --------------------- | ---- |
| Event.CAPTURING_PHASE | 1    |
| Event.AT_TARGET       | 2    |
| Event.BUBBLING_PHASE  | 3    |

可以通过下面代码查看：

```js
var btn=document.getElementById("btn");
btn.onclick= function (event) {
console.log(event.CAPTURING_PHASE); //1
console.log(event.AT_TARGET); //2
console.log(event.BUBBLING_PHASE); //3
}
```

 

例子：

运行效果：

[![img](https://images0.cnblogs.com/blog/315302/201411/142343081006950.jpg)](http://www.cnblogs.com/starof/p/4096198.html)

#### 2、IE中event的公共成员

IE中的event的属性和方法和DOM一样会随着事件类型的不同而不同，但是也有一些是所有对象都有的公共成员，且这些成员大部分有对应的DOM属性或方法。

| 属性/方法    | 类型    | 读/写 | 说明                                                         |
| ------------ | ------- | ----- | ------------------------------------------------------------ |
| cancelBubble | Boolean | 读/写 | 默认为false,但将其设置为true就可以取消事件冒泡（**与DOM中stopPropagation()方法的作用相同**） |
| returnValue  | Boolean | 读/写 | 默认为true，但将其设置为false就可以取消事件的默认行为（**与DOM中的preventDefault()方法的作用相同**） |
| srcElement   | Element | 只读  | 事件的目标（**与DOM中的target属性相同**）                    |
| type         | String  | 只读  | 被触发的事件的类型                                           |

### 七、鼠标事件

DOM3级事件中定义了9个鼠标事件。

- mousedown:鼠标按钮被按下（左键或者右键）时触发。不能通过键盘触发。
- mouseup:鼠标按钮被释放弹起时触发。不能通过键盘触发。
- click:单击鼠标**左键**或者按下回车键时触发。这点对确保易访问性很重要，意味着onclick事件处理程序既可以通过键盘也可以通过鼠标执行。
- dblclick:双击鼠标**左键**时触发。
- mouseover:鼠标移入目标元素上方。鼠标移到其后代元素上时会触发。
- mouseout:鼠标移出目标元素上方。
- mouseenter:鼠标移入元素范围内触发，**该事件不冒泡**，即鼠标移到其后代元素上时不会触发。
- mouseleave:鼠标移出元素范围时触发，**该事件不冒泡**，即鼠标移到其后代元素时不会触发。
- mousemove:鼠标在元素内部移到时不断触发。不能通过键盘触发。

**note**:

在一个元素上相继触发mousedown和mouseup事件，才会触发click事件。两次click事件相继触发才会触发dblclick事件。

如果取消 了mousedown或mouseup中的一个，click事件就不会被触发。直接或间接取消了click事件，dblclick事件就不会被触发了。

#### 1、事件触发的顺序

举例：通过双击按钮，看一下上面触发的事件。

在触摸屏幕上的元素时，事件（包括鼠标事件）发生的顺序如下 

(1) touchstart

(2) mouseover

(3) mousemove（一次）

(4) mousedown

(5) mouseup

(6) click

(7) touchend

click在移动端有300ms延迟。。。

[![img](https://images0.cnblogs.com/blog/315302/201411/182134449882507.jpg)](http://www.cnblogs.com/starof/p/4106904.html)

#### 2、mouseenter和mouseover的区别

 区别：

mouseover事件会冒泡，这意味着，鼠标移到其后代元素上时会触发。

mouseenter事件不冒泡，这意味着，鼠标移到其后代元素上时不会触发。

举例：

[![img](https://images0.cnblogs.com/blog/315302/201411/192222025467766.png)](http://www.cnblogs.com/starof/p/4106904.html)[![img](https://images0.cnblogs.com/blog/315302/201411/192226088599084.png)](http://www.cnblogs.com/starof/p/4106904.html)

**note**:

mouseover对应mouseout,mouseenter对应mouseleave。效果可以取消上面代码的注释来看。

 jquery中hover API是把mouseenter 和mouseleave组合在一起来用的。

#### 3、鼠标左键和右键

```html
<script type="text/javascript">
document.onmousedown=function (ev)
{
    var oEvent=ev||event; //IE浏览器直接使用event或者window.event得到事件本身。
    alert(oEvent.button);// IE下鼠标的 左键是1 ，  右键是2   ff和chrome下 鼠标左键是0  右键是2
};
</script>
```

#### 4、mouseover和mousemove的区别

一般情况下mouseover即可，特殊情况才用mousemove，mousemove更耗资源，比如要监控鼠标坐标的变化等。

### Event 对象

Event 对象代表事件的状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态。

事件通常与函数结合使用，函数不会在事件发生前被执行！

事件句柄　(Event Handlers)

HTML 4.0 的新特性之一是能够使 HTML 事件触发浏览器中的行为，比如当用户点击某个 HTML 元素时启动一段 JavaScript。下面是一个属性列表，可将之插入 HTML 标签以定义事件的行为。

| 属性                                                         | 此事件发生在何时...                  |
| :----------------------------------------------------------- | :----------------------------------- |
| [onabort](http://www.w3school.com.cn/jsref/event_onabort.asp) | 图像的加载被中断。                   |
| [onblur](http://www.w3school.com.cn/jsref/event_onblur.asp)  | 元素失去焦点。                       |
| [onchange](http://www.w3school.com.cn/jsref/event_onchange.asp) | 域的内容被改变。                     |
| [onclick](http://www.w3school.com.cn/jsref/event_onclick.asp) | 当用户点击某个对象时调用的事件句柄。 |
| [ondblclick](http://www.w3school.com.cn/jsref/event_ondblclick.asp) | 当用户双击某个对象时调用的事件句柄。 |
| [onerror](http://www.w3school.com.cn/jsref/event_onerror.asp) | 在加载文档或图像时发生错误。         |
| [onfocus](http://www.w3school.com.cn/jsref/event_onfocus.asp) | 元素获得焦点。                       |
| [onkeydown](http://www.w3school.com.cn/jsref/event_onkeydown.asp) | 某个键盘按键被按下。                 |
| [onkeypress](http://www.w3school.com.cn/jsref/event_onkeypress.asp) | 某个键盘按键被按下并松开。           |
| [onkeyup](http://www.w3school.com.cn/jsref/event_onkeyup.asp) | 某个键盘按键被松开。                 |
| [onload](http://www.w3school.com.cn/jsref/event_onload.asp)  | 一张页面或一幅图像完成加载。         |
| [onmousedown](http://www.w3school.com.cn/jsref/event_onmousedown.asp) | 鼠标按钮被按下。                     |
| [onmousemove](http://www.w3school.com.cn/jsref/event_onmousemove.asp) | 鼠标被移动。                         |
| [onmouseout](http://www.w3school.com.cn/jsref/event_onmouseout.asp) | 鼠标从某元素移开。                   |
| [onmouseover](http://www.w3school.com.cn/jsref/event_onmouseover.asp) | 鼠标移到某元素之上。                 |
| [onmouseup](http://www.w3school.com.cn/jsref/event_onmouseup.asp) | 鼠标按键被松开。                     |
| [onreset](http://www.w3school.com.cn/jsref/event_onreset.asp) | 重置按钮被点击。                     |
| [onresize](http://www.w3school.com.cn/jsref/event_onresize.asp) | 窗口或框架被重新调整大小。           |
| [onselect](http://www.w3school.com.cn/jsref/event_onselect.asp) | 文本被选中。                         |
| [onsubmit](http://www.w3school.com.cn/jsref/event_onsubmit.asp) | 确认按钮被点击。                     |
| [onunload](http://www.w3school.com.cn/jsref/event_onunload.asp) | 用户退出页面。                       |

#### 鼠标 / 键盘属性

| 属性                                                         | 描述                                         |
| :----------------------------------------------------------- | :------------------------------------------- |
| [altKey](http://www.w3school.com.cn/jsref/event_altkey.asp)  | 返回当事件被触发时，"ALT" 是否被按下。       |
| [button](http://www.w3school.com.cn/jsref/event_button.asp)  | 返回当事件被触发时，哪个鼠标按钮被点击。     |
| [clientX](http://www.w3school.com.cn/jsref/event_clientx.asp) | 返回当事件被触发时，鼠标指针的水平坐标。     |
| [clientY](http://www.w3school.com.cn/jsref/event_clienty.asp) | 返回当事件被触发时，鼠标指针的垂直坐标。     |
| [ctrlKey](http://www.w3school.com.cn/jsref/event_ctrlkey.asp) | 返回当事件被触发时，"CTRL" 键是否被按下。    |
| [metaKey](http://www.w3school.com.cn/jsref/event_metakey.asp) | 返回当事件被触发时，"meta" 键是否被按下。    |
| [relatedTarget](http://www.w3school.com.cn/jsref/event_relatedtarget.asp) | 返回与事件的目标节点相关的节点。             |
| [screenX](http://www.w3school.com.cn/jsref/event_screenx.asp) | 返回当某个事件被触发时，鼠标指针的水平坐标。 |
| [screenY](http://www.w3school.com.cn/jsref/event_screeny.asp) | 返回当某个事件被触发时，鼠标指针的垂直坐标。 |
| [shiftKey](http://www.w3school.com.cn/jsref/event_shiftkey.asp) | 返回当事件被触发时，"SHIFT" 键是否被按下。   |

#### IE 属性

除了上面的鼠标/事件属性，IE 浏览器还支持下面的属性：

| 属性            | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| cancelBubble    | 如果事件句柄想阻止事件传播到包容对象，必须把该属性设为 true。 |
| fromElement     | 对于 mouseover 和 mouseout 事件，fromElement 引用移出鼠标的元素。 |
| keyCode         | 对于 keypress 事件，该属性声明了被敲击的键生成的 Unicode 字符码。对于 keydown 和 keyup 事件，它指定了被敲击的键的虚拟键盘码。虚拟键盘码可能和使用的键盘的布局相关。 |
| offsetX,offsetY | 发生事件的地点在事件源元素的坐标系统中的 x 坐标和 y 坐标。   |
| returnValue     | 如果设置了该属性，它的值比事件句柄的返回值优先级高。把这个属性设置为 fasle，可以取消发生事件的源元素的默认动作。 |
| srcElement      | 对于生成事件的 Window 对象、Document 对象或 Element 对象的引用。 |
| toElement       | 对于 mouseover 和 mouseout 事件，该属性引用移入鼠标的元素。  |
| x,y             | 事件发生的位置的 x 坐标和 y 坐标，它们相对于用CSS动态定位的最内层包容元素。 |

### 标准 Event 属性

下面列出了 2 级 DOM 事件标准定义的属性。

| 属性                                                         | 描述                                           |
| :----------------------------------------------------------- | :--------------------------------------------- |
| [bubbles](http://www.w3school.com.cn/jsref/event_bubbles.asp) | 返回布尔值，指示事件是否是起泡事件类型。       |
| [cancelable](http://www.w3school.com.cn/jsref/event_cancelable.asp) | 返回布尔值，指示事件是否可拥可取消的默认动作。 |
| [currentTarget](http://www.w3school.com.cn/jsref/event_currenttarget.asp) | 返回其事件监听器触发该事件的元素。             |
| [eventPhase](http://www.w3school.com.cn/jsref/event_eventphase.asp) | 返回事件传播的当前阶段。                       |
| [target](http://www.w3school.com.cn/jsref/event_target.asp)  | 返回触发此事件的元素（事件的目标节点）。       |
| [timeStamp](http://www.w3school.com.cn/jsref/event_timestamp.asp) | 返回事件生成的日期和时间。                     |
| [type](http://www.w3school.com.cn/jsref/event_type.asp)      | 返回当前 Event 对象表示的事件的名称。          |

#### 标准 Event 方法

下面列出了 2 级 DOM 事件标准定义的方法。IE 的事件模型不支持这些方法：

| 方法                                                         | 描述                                     |
| :----------------------------------------------------------- | :--------------------------------------- |
| [initEvent()](http://www.w3school.com.cn/jsref/event_initevent.asp) | 初始化新创建的 Event 对象的属性。        |
| [preventDefault()](http://www.w3school.com.cn/jsref/event_preventdefault.asp) | 通知浏览器不要执行与事件关联的默认动作。 |
| [stopPropagation()](http://www.w3school.com.cn/jsref/event_stoppropagation.asp) | 不再派发事件。                           |





### HTTP 响应状态码（重点分析）

#### 1. 状态码概述

- HTTP 状态码负责表示客户端 HTTP 请求的返回结果、标记服务器端的处理是否正常、通知出现的错误等工作。
- HTTP 状态码如 `200 OK` ，以 3 位数字和原因短语组成。数字中的第一位指定了响应类别，后两位无分类。
- 不少返回的响应状态码都是错误的，但是用户可能察觉不到这点。比如 Web 应用程序内部发生错误，状态码依然返回 `200 OK`。

#### 2. 状态码类别

|      |              类别              |          原因短语          |
| :--: | :----------------------------: | :------------------------: |
| 1xx  |  Informational(信息性状态码)   |     接收的请求正在处理     |
| 2xx  |      Success(成功状态码)       |      请求正常处理完毕      |
| 3xx  |   Redirection(重定向状态码)    | 需要进行附加操作以完成请求 |
| 4xx  | Client Error(客户端错误状态码) |     服务器无法处理请求     |
| 5xx  | Server Error(服务器错误状态码) |     服务器处理请求出错     |

我们可以自行改变 RFC2616 中定义的状态码或者服务器端自行创建状态码，只要遵守状态码的类别定义就可以了。

#### 3. 常用状态码解析

HTTP 状态码种类繁多，数量达几十种。其中最常用的有以下 14 种，一起来看看。

##### 3.1 200 OK

表示从客户端发来的请求在服务器端被正常处理了。

##### 3.2 204 No Content

- 代表服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分。另外，也不允许返回任何实体的主体。
- 一般在只需要从客户端向服务器端发送消息，而服务器端不需要向客户端发送新消息内容的情况下使用。

##### 3.3 206 Partial Content

表示客户端进行了范围请求，而服务器成功执行了这部分的 GET 请求。响应报文中包含由 Content-Range 首部字段指定范围的实体内容。

##### 3.4 301 Moved Permanently

**永久性重定向**。表示请求的资源已被分配了新的 URI。以后应使用资源现在所指的 URI。也就是说，如果已经把资源对应的 URI 保存为书签了，这时应该按 Location 首部字段提示的 URI 重新保存。

##### 3.5 302 Found

- **临时性重定向**。表示请求的资源已被分配了新的 URI，**希望用户（本次）能使用新的 URI 访问**。
- 和 `301 Moved Permanently` 状态码相似，但 `302 Found` 状态码代表资源不是被永久移动，只是临时性质的。换句话说，已移动的资源对应的 URI 将来还有可能发生改变。

##### 3.6 303 See Other

- 表示由于请求的资源存在着另一个 URI，应使用 GET 方法定向获取请求的资源。
- `303 See Othe`r 和 `302 Found` 状态码有着相同的功能，但 `303 See Other` 状态码明确表示客户端应采用 GET 方法获取资源，这点与 `302 Found` 状态码有区别。

##### 3.7 304 Not Modified

- 表示客户端发送附带条件的请求时，服务器端允许请求访问的资源，但未满足条件的情况。
- `304 Not Modified` 状态码返回时，不包含任何响应的主体部分。
- `304 Not Modified` 虽然被划分到 3xx 类别中，但和重定向没有关系。

##### 3.8 307 Temporary Redirect

临时重定向。该状态码与 `302 Found` 有着相同的含义。

##### 3.9 400 Bad Request

- 表示请求报文中存在语法错误。当错误发生时，需修改请求的内容后再次发送请求。
- 另外，浏览器会像 `200 OK` 一样对待该状态码。

##### 3.10 401 Unauthorized

- 表示发送的请求需要有通过 HTTP 认证（BASIC 认证、DIGEST 认证）的认证信息。
- 另外，若之前已进行过 1 次请求，则表示用户认证失败。
- 返回含有 `401 Unauthorized` 的响应必须包含一个适用于被请求资源的 WWW-Authenticate 首部用以质询（challenge）用户信息。

##### 3.11 403 Forbidden

表明对请求资源的访问被服务器拒绝了。服务器端没有必要给出详细的拒绝理由，当然也可以在响应报文的实体主体部分对原因进行描述。

##### 3.12 404 Not Found

**表明服务器上无法找到请求的资源。除此之外，也可以在服务器端拒绝请求且不想说明理由的时候使用**。

##### 3.13 500 Internal Server Error

**表明服务器端在执行请求时发生了错误。也可能是 Web 应用存在的 bug 或某些临时的故障**。

##### 3.14 503 Service Unavailable

表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。如果事先得知解除以上状况需要的时间，最好写入 Retry-After 首部字段再返回给客户端。

浏览器的缓存机制提供了可以将用户数据存储在客户端上的方式，可以利用cookie,session等跟服务端进行数据交互。

## cookie和session

cookie和session都是用来跟踪浏览器用户身份的会话方式。

**区别**：

### 1、保持状态：

cookie保存在浏览器端，session保存在服务器端

### 2、使用方式：

（1）**cookie机制**：如果不在浏览器中设置过期时间，cookie被保存在内存中，生命周期随浏览器的关闭而结束，这种cookie简称会话cookie。如果在浏览器中设置了cookie的过期时间，cookie被保存在硬盘中，关闭浏览器后，cookie数据仍然存在，直到过期时间结束才消失。

​     Cookie是服务器发给客户端的特殊信息，cookie是以文本的方式保存在客户端，每次请求时都带上它

（2）**session机制**：当服务器收到请求需要创建session对象时，首先会检查客户端请求中是否包含sessionid。如果有sessionid，服务器将根据该id返回对应session对象。如果客户端请求中没有sessionid，服务器会创建新的session对象，并把sessionid在本次响应中返回给客户端。通常使用cookie方式存储sessionid到客户端，在交互中浏览器按照规则将sessionid发送给服务器。如果用户禁用cookie，则要使用URL重写，可以通过response.encodeURL(url) 进行实现；API对encodeURL的结束为，当浏览器支持Cookie时，url不做任何处理；当浏览器不支持Cookie的时候，将会重写URL将SessionID拼接到访问地址后。

3、存储内容：

cookie只能保存字符串类型，以文本的方式；session通过类似与Hashtable的数据结构来保存，能支持任何类型的对象(session中可含有多个对象)

4、存储的大小：

cookie：单个cookie保存的数据不能超过4kb；session大小没有限制。

5、安全性：

cookie：

针对cookie所存在的攻击：Cookie欺骗，Cookie截获；session的安全性大于cookie。

原因如下：

（1）sessionID存储在cookie中，若要攻破session首先要攻破cookie；

（2）sessionID是要有人登录，或者启动session_start才会有，所以攻破cookie也不一定能得到sessionID；

（3）第二次启动session_start后，前一次的sessionID就是失效了，session过期后，sessionID也随之失效。

   (4）sessionID是加密的

（5）综上所述，攻击者必须在短时间内攻破加密的sessionID，这很难。

### 6、应用场景：

cookie：

（1）判断用户是否登陆过网站，以便下次登录时能够实现自动登录（或者记住密码）。如果我们删除cookie，则每次登录必须从新填写登录的相关信息。

（2）保存上次登录的时间等信息。

（3）保存上次查看的页面

（4）浏览计数

![img](https://images2017.cnblogs.com/blog/1209205/201709/1209205-20170928000104559-1868896430.png)

session：Session用于保存每个用户的专用信息，变量的值保存在服务器端，通过SessionID来区分不同的客户。

　　（1）网上商城中的购物车

　　（2）保存用户登录信息

　　（3）将某些数据放入session中，供同一用户的不同页面使用

　　（4）防止用户非法登录

###  7、缺点：

**cookie：**（1）大小受限

　　　　（2）用户可以操作（禁用）cookie，使功能受限

　　　　（3）安全性较低

　　　    （4）有些状态不可能保存在客户端。

　　　　　5）每次访问都要传送cookie给服务器，浪费带宽。

　　　　　6）cookie数据有路径（path）的概念，可以限制cookie只属于某个路径下。

 **session：**（1）Session保存的东西越多，就越占用服务器内存，对于用户在线人数较多的网站，服务器的内存压力会比较大。

　　　　　  (2）依赖于cookie（sessionID保存在cookie），如果禁用cookie，则要使用URL重写，不安全

　　　　　（3）创建Session变量有很大的随意性，可随时调用，不需要开发者做精确地处理，所以，过度使用session变量将会导致代码不可读而且不好维护。

### 8、WebStorage

WebStorage的目的是克服由cookie所带来的一些限制，当数据需要被严格控制在客户端时，不需要持续的将数据发回服务器。

WebStorage两个主要目标：

（1）提供一种在cookie之外存储会话数据的路径。

（2）提供一种存储大量可以跨会话存在的数据的机制。

HTML5的WebStorage提供了两种API：localStorage（本地存储）和sessionStorage（会话存储）。

1、生命周期：localStorage:localStorage的生命周期是永久的，关闭页面或浏览器之后localStorage中的数据也不会消失。localStorage除非主动删除数据，否则数据永远不会消失。sessionStorage的生命周期是在仅在当前会话下有效。sessionStorage引入了一个“浏览器窗口”的概念，sessionStorage是在同源的窗口中始终存在的数据。只要这个浏览器窗口没有关闭，即使刷新页面或者进入同源另一个页面，数据依然存在。但是sessionStorage在关闭了浏览器窗口后就会被销毁。同时独立的打开同一个窗口同一个页面，sessionStorage也是不一样的。

2、存储大小：localStorage和sessionStorage的存储数据大小一般都是：5MB

3、存储位置：localStorage和sessionStorage都保存在客户端，不与服务器进行交互通信。

4、存储内容类型：localStorage和sessionStorage只能存储字符串类型，对于复杂的对象可以使用ECMAScript提供的JSON对象的stringify和parse来处理

5、获取方式：localStorage：window.localStorage;；sessionStorage：window.sessionStorage;。

6、应用场景：localStoragese：常用于长期登录（+判断用户是否已登录），适合长期保存在本地的数据。sessionStorage：敏感账号一次性登录；

**WebStorage的优点：**

（1）存储空间更大：cookie为4KB，而WebStorage是5MB；

（2）节省网络流量：WebStorage不会传送到服务器，存储在本地的数据可以直接获取，也不会像cookie一样美词请求都会传送到服务器，所以减少了客户端和服务器端的交互，节省了网络流量；

（3）对于那种只需要在用户浏览一组页面期间保存而关闭浏览器后就可以丢弃的数据，sessionStorage会非常方便；

（4）快速显示：有的数据存储在WebStorage上，再加上浏览器本身的缓存。获取数据时可以从本地获取会比从服务器端获取快得多，所以速度更快；

（5）安全性：WebStorage不会随着HTTP header发送到服务器端，所以安全性相对于cookie来说比较高一些，不会担心截获，但是仍然存在伪造问题；

（6）WebStorage提供了一些方法，数据操作比cookie方便；

　　　　setItem (key, value) ——  保存数据，以键值对的方式储存信息。

​      　　 getItem (key) ——  获取数据，将键值传入，即可获取到对应的value值。

​        　　removeItem (key) ——  删除单个数据，根据键值移除对应的信息。

​        　　clear () ——  删除所有的数据

​        　　key (index) —— 获取某个索引的key

### 三者的异同

|      特性      |                            Cookie                            |                        localStorage                         |                       sessionStorage                        |
| :------------: | :----------------------------------------------------------: | :---------------------------------------------------------: | :---------------------------------------------------------: |
|  数据的生命期  | 一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效 |                  除非被清除，否则永久保存                   |        仅在当前会话下有效，关闭页面或浏览器后被清除         |
|  存放数据大小  |                            4K左右                            |                          一般为5MB                          |                          一般为5MB                          |
| 与服务器端通信 | 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 |     仅在客户端（即浏览器）中保存，不参与和服务器的通信      |     仅在客户端（即浏览器）中保存，不参与和服务器的通信      |
|     易用性     |          需要程序员自己封装，源生的Cookie接口不友好          | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 |

#### 应用场景

有了对上面这些差别的直观理解，我们就可以讨论三者的应用场景了。

因为考虑到每个 HTTP 请求都会带着 Cookie 的信息，所以 Cookie 当然是能精简就精简啦，比较常用的一个应用场景就是判断用户是否登录。针对登录过的用户，服务器端会在他登录时往 Cookie 中插入一段加密过的唯一辨识单一用户的辨识码，下次只要读取这个值就可以判断当前用户是否登录啦。

而另一方面 localStorage 接替了 Cookie 管理购物车的工作，同时也能胜任其他一些工作。比如HTML5游戏通常会产生一些本地数据，localStorage 也是非常适用的。如果遇到一些内容特别多的表单，为了优化用户体验，我们可能要把表单页面拆分成多个子页面，然后按步骤引导用户填写。这时候 sessionStorage 的作用就发挥出来了。

#### 安全性的考虑

需要注意的是，不是什么数据都适合放在 Cookie、localStorage 和 sessionStorage 中的。使用它们的时候，需要时刻注意是否有代码存在 XSS 注入的风险。因为只要打开控制台，你就随意修改它们的值，也就是说如果你的网站中有 XSS 的风险，它们就能对你的 localStorage 肆意妄为。所以千万不要用它们存储你系统中的敏感数据。

#### localStorage和sessionStorage操作

localStorage和sessionStorage都具有相同的操作方法，例如setItem、getItem和removeItem等

setItem存储value

用途：将value存储到key字段

```js
sessionStorage.setItem("key", "value");     localStorage.setItem("site", "js8.in");
```

getItem获取value

用途：获取指定key本地存储的值

```js
var value = sessionStorage.getItem("key");     var site = localStorage.getItem("site");
```

removeItem删除key

用途：删除指定key本地存储的值

```js
sessionStorage.removeItem("key");     localStorage.removeItem("site");
```

clear清除所有的key/value

用途：清除所有的key/value

```js
sessionStorage.clear();     localStorage.clear();
```

其他操作方法：点操作和[ ]

web Storage不但可以用自身的setItem,getItem等方便存取，也可以像普通对象一样用点(.)操作符，及[]的方式进行数据存储，像如下的代码：

```js
var storage = window.localStorage; storage.key1 = "hello"; storage["key2"] = "world"; console.log(storage.key1); console.log(storage["key2"]);
```

localStorage和sessionStorage的key和length属性实现遍历

sessionStorage和localStorage提供的key()和length可以方便的实现存储的数据遍历，例如下面的代码：

```js
var storage = window.localStorage;
for(var i=0, len=storage.length; i<len;i++){
    var key = storage.key(i);     
    var value = storage.getItem(key);     
    console.log(key + "=" + value); 
}
```



# TCP三次握手简介


 [简书地址：http://www.jianshu.com/p/448f37ed29fe](https://www.jianshu.com/p/448f37ed29fe)

![img](https:////upload-images.jianshu.io/upload_images/748004-932648c97280ddce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

TCP三次握手（原创图片）

### TCP首部简介

TCP三次握手涉及到TCP首部的一些知识，所有有必要先介绍下TCP首部的相关知识。如果嫌TCP首部内容太多，那么只要看下`ACK`和`SYN`这两个标志比特就行了（因为TCP三次握手过程主要用到这两个标志比特）。



![img](https:////upload-images.jianshu.io/upload_images/748004-b0bf3d609705bbf7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/655/format/webp)

TCP首部（图片来自网络）

> - 源端口(Source Port)和目的端口(Destination Port): 分别占用16位，表示源端口号和目的端口号。用于区别主机中的不同进程，而IP地址是用来区分不同的主机的，源端口号和目的端口号配合上IP首部中的源IP地址和目的IP地址就能唯一的确定一个TCP连接。
> - 序号(Sequence Number): 用来标识从TCP发端向TCP收端发送的数据字节流，它表示在这个报文段中的的第一个数据字节在数据流中的序号。主要用来解决网络报乱序的问题。
> - 确认号(Acknowledgment Number): 32位确认序列号包含发送确认的一端所期望收到的下一个序号，因此，确认序号应当是上次已成功收到数据字节序号加1。不过，只有当标志位中的ACK标志（下面介绍）为1时该确认序列号的字段才有效。主要用来解决不丢包的问题。
> - 数据偏移(Offset): 给出首部中32 bit字的数目，需要这个值是因为任选字段的长度是可变的。这个字段占4bit（最多能表示15个32bit的的字，即4*15=60个字节的首部长度），因此TCP最多有60字节的首部。然而，没有选项字段，正常的长度是20字节。
> - 保留: 占6位。保留为今后使用，目前置为0。
> - 标志比特(TCP Flags): TCP首部中有6个标志比特，它们中的多个可同时被设置为1，主要是用于操控TCP的状态机的，依次为URG，ACK，PSH，RST，SYN，FIN。每个标志位的意思如下:

```
- URG: 此标志表示TCP包的紧急指针域（后面马上就要说到）有效，用来保证TCP连接不被中断，并且督促中间层设备要尽快处理这些数据。
- ACK: 此标志表示应答域有效，就是说前面所说的TCP应答号将会包含在TCP数据包中。有两个取值: 0和1，为1的时候表示应答域有效，反之为0。
- PSH: 这个标志位表示Push操作。所谓Push操作就是指在数据包到达接收端以后，立即传送给应用程序，而不是在缓冲区中排队。
- RST: 这个标志表示连接复位请求。用来复位那些产生错误的连接，也被用来拒绝错误和非法的数据包。
- SYN: 表示同步序号，用来建立连接。SYN标志位和ACK标志位搭配使用，当连接请求的时候，SYN=1，ACK=0。连接被响应的时候，SYN=1，ACK=1。这个标志的数据包经常被用来进行端口扫描。扫描者发送一个只有SYN的数据包，如果对方主机响应了一个数据包回来 ，就表明这台主机存在这个端口。但是由于这种扫描方式只是进行TCP三次握手的第一次握手，因此这种扫描的成功表示被扫描的机器不很安全，一台安全的主机将会强制要求一个连接严格的进行TCP的三次握手。
- FIN: 表示发送端已经达到数据末尾，也就是说双方的数据传送完成，没有数据可以传送了，发送FIN标志位的TCP数据包后，连接将被断开。这个标志的数据包也经常被用于进行端口扫描。
```

> - 窗口(Window): 也就是有名的滑动窗口，用来进行流量控制。这是一个复杂的问题，这篇博文中并不会进行总结。
> - 校验和: 占2字节。该字段检验的范围包括首部和数据这两部分。由发端计算和存储，并由收端进行验证。
> - 紧急指针: 占2个字节，紧急指针仅在URG=1时才有意义，它指出本报文段中的紧急数据的字节数。当所有紧急数据处理完毕时，TCP就告诉应用程序恢复到正常操作。值得注意的是，即使窗口为0时也可发送紧急数据。
> - 选项: 长度可变，最长可达40字节，当没有选项时，TCP的首部长度是20字节。最大报文段长度MSS，MSS是指每一个TCP报文段中的数据字段的最大长度。

### TCP三次握手过程

其实以下这张图片就能说明TCP三次握手的过程以及握手两端状态的变化。



![img](https:////upload-images.jianshu.io/upload_images/748004-9a5e2c5794d7d62b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

TCP三次握手

- 第一次握手: 建立连接。客户端发送连接请求报文段，将SYN位置为1，Seq(Sequence Number)为X(由操作系统动态随机选取一个32位长的序列号)。然后，客户端进入SYN_SEND状态，等待服务器的确认。
- 第二次握手: 服务器收到客户端的SYN报文段。需要对这个SYN报文段进行确认，设置Ack(Acknowledgment Number)设置为X(第一次握手中的Seq的值)+1。同时，自己还要发送SYN请求信息，将SYN位置为1，Seq(Sequence Number)为Y(由操作系统动态随机选取一个32位长的序列号)。服务器端将上述所有信息一并发送给客户端，此时服务器进入SYN_RECV状态。
- 第三次握手: 客户端收到服务器的报文段。然后将Ack(Acknowledgment Number)设置为Y(第二次握手中的Seq的值)+1，Seq(Sequence Number)设置为X+1`第二次握手中的Ack(Acknowledgment Number)值`，向服务器发送ACK报文段，这个报文段发送完毕以后，客户端和服务器端都进入ESTABLISHED状态，完成TCP三次握手。

TCP的SYN同步标志位被设计成占用一个字节的编号，既然是一个字节的数据，按照TCP对有数据的TCP Segment必须确认的原则，所以可看到一端发送SYN，则另一端用ACK进行响应。

### 握手中断

- 第一次握手中断: A发送给B的SYN中断，A会周期性超时重传，直到A收到B的确认响应。
- 第二次握手中断: B发送给A的SYN、ACK中断，B会周期性超时重传，直到B收到A的确认响应。
- 第三次握手中断: A发送给B的ACK中断，A不会重传。超时后，B会重传SYN信号(即回到第二次握手)，直到B收到A的确认响应。

### 为什么要三次握手

以下是两种比较权威说法：

> - 在谢希仁著《计算机网络》第四版中讲“三次握手”的目的是“为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误”。在另一部经典的《计算机网络》一书中讲“三次握手”的目的是为了解决“网络中存在延迟的重复分组”的问题。这两种不用的表述其实阐明的是同一个问题。谢希仁版《计算机网络》中的例子是这样的，“已失效的连接请求报文段”的产生在这样一种情况下：client发出的第一个连接请求报文段并没有丢失，而是在某个网络结点长时间的滞留了，以致延误到连接释放以后的某个时间才到达server。本来这是一个早已失效的报文段。但server收到此失效的连接请求报文段后，就误认为是client再次发出的一个新的连接请求。于是就向client发出确认报文段，同意建立连接。假设不采用“三次握手”，那么只要server发出确认，新的连接就建立了。由于现在client并没有发出建立连接的请求，因此不会理睬server的确认，也不会向server发送数据。但server却以为新的运输连接已经建立，并一直等待client发来数据。这样，server的很多资源就白白浪费掉了。采用“三次握手”的办法可以防止上述现象发生。例如刚才那种情况，client不会向server的确认发出确认。server由于收不到确认，就知道client并没有要求建立连接。”

> - 在Google Groups的[TopLanguage](https://link.jianshu.com?t=https://groups.google.com/forum/#!forum/pongba)中看到一帖讨论TCP“三次握手”觉得很有意思。贴主提出“[TCP建立连接为什么是三次握手？](https://link.jianshu.com?t=https://groups.google.com/forum/#!topic/pongba/kF6O7-MFxM0/discussion)”的问题，在众多回复中，有[一条回复](https://link.jianshu.com?t=https://groups.google.com/forum/#!msg/pongba/kF6O7-MFxM0/5S7zIJ4yqKUJ)写道：“这个问题的本质是, 信道不可靠, 但是通信双发需要就某个问题达成一致. 而要解决这个问题,  无论你在消息中包含什么信息, 三次通信是理论上的最小值. 所以三次握手不是TCP本身的要求, 而是为了满足"在不可靠信道上可靠地传输信息"这一需求所导致的. 请注意这里的本质需求,信道不可靠, 数据传输要可靠. 三次达到了, 那后面你想接着握手也好, 发数据也好, 跟进行可靠信息传输的需求就没关系了. 因此,如果信道是可靠的, 即无论什么时候发出消息, 对方一定能收到, 或者你不关心是否要保证对方收到你的消息, 那就能像UDP那样直接发送消息就可以了.”。

是不是觉得好难懂的样子，那么可以先看下下面我画的对“为什么要三次握手”的图解，再回头看上面的讲解。



![img](https:////upload-images.jianshu.io/upload_images/748004-187b9a15c5a7cb2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

为什么是三次握手？（原创图片）

由图可以得出，三次握手的本质是：将“四次握手”中的第二次、第三次握手合为一次，因为“四次握手”中的第二次、第三次握手都是由B向A传递报文，而且这两次发送报文的目的允许这两次报文合并为一次。

### 实践(抓包分析)

接下来我们通过网络抓包的方式来了解TCP的三次握手。我这里使用的抓包软件是`Wireshark`。

- 打开`Wireshark`，选择需要捕获的网络。
   

  

  ![img](https:////upload-images.jianshu.io/upload_images/748004-27755a21ef246e0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

  wireshark_welcome

  

- 进入到主界面

  

  ![img](https:////upload-images.jianshu.io/upload_images/748004-c4f08f58e1b84dfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

  wireshark_main

- 找到TCP三次握手

  

  ![img](https:////upload-images.jianshu.io/upload_images/748004-8f5460fcafc3fdbe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

  wireshark_tcp_shake

观察`Wireshark`上部已经捕获的网络数据包列表部分，看`Info`部分，能找到相对连续的三列(分别显示`A -> B [SYN]...`、`B -> A [SYN, ACK]...`、`A -> B [ACK]...`)，便是TCP的三次握手，在找的时候，注意`Source`栏和`Destination`栏中的ip地址的相对应，以及`Info`栏中的端口的对应。

- 查看第一次握手的详情

  

  ![img](https:////upload-images.jianshu.io/upload_images/748004-dd5461a079968965.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

  wireshark_tcp_shake_first

- 查看第二次握手的详情

  

  ![img](https:////upload-images.jianshu.io/upload_images/748004-5616256ba254bb39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

  wireshark_tcp_shake_second

- 查看第三次握手的详情

  

  ![img](https:////upload-images.jianshu.io/upload_images/748004-2be39d080391ba03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

  wireshark_tcp_shake_third

选中每次一的握手数据包，点击下方的`Transmission Control Protocol(TCP)`，即可显示每次TCP握手的详情。在详情中，我们展开`Flags`，可以看到比特标志位是否有被设置的情况。
 我们能发现，实践中的TCP状态情况，跟上面提到的理论是一致的。

- 第一次握手: [SYN] Seq=0
- 第二次握手: [SYN, ACK] Seq=0 Ack=1
- 第三次握手: [ACK] Seq=1 Ack=1

TCP 通信流程TCP 的通信流程
![在这里插入图片描述](https://img-blog.csdn.net/20180208112533496)

上图中的每一个箭头都代表着一次 TCP数据包的发送

需要注意的是， 上图中出现的 ACK = x +1 的写法很容易让人误以为数据包中的 ACK 域的数据值被填成了 y+1 。 ACK = x+1 的实际含义是：
TCP 包的 ACK 标志位（1 bit） 被置成了 1
TCP 包的确认号（acknowledgement number ） 的值为 x+1

**类似的， TCP 数据包中的 SYN 标志位， 也容易与序号（sequence number） 混淆， 这点需要读者注意**

### 过程

第一次握手：建立连接时，客户端发送syn包（syn=j）到服务器，并进入SYN_SENT状态，等待服务器确认；SYN：同步序列编号（Synchronize Sequence Numbers）。

第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=k+1），此包发送完毕，客户端和服务器进入ESTABLISHED（TCP连接成功）状态，完成三次握手。

完成三次握手，客户端与服务器开始传送数据，在上述过程中，还有一些重要的概念：

未连接队列

在三次握手协议中，服务器维护一个未连接队列，该队列为每个客户端的SYN包（syn=j）开设一个条目，该条目表明服务器已收到SYN包，并向客户发出确认，正在等待客户的确认包。这些条目所标识的连接在服务器处于SYN_RECV状态，当服务器收到客户的确认包时，删除该条目，服务器进入ESTABLISHED状态。

 

关闭TCP连接：改进的三次握手

对于一个已经建立的连接，TCP使用改进的三次握手来释放连接（使用一个带有FIN附加标记的报文段）。TCP关闭连接的步骤如下：

第一步，当主机A的应用程序通知TCP数据已经发送完毕时，TCP向主机B发送一个带有FIN附加标记的报文段（FIN表示英文finish）。

第二步，主机B收到这个FIN报文段之后，并不立即用FIN报文段回复主机A，而是先向主机A发送一个确认序号ACK，同时通知自己相应的应用程序：对方要求关闭连接（先发送ACK的目的是为了防止在这段时间内，对方重传FIN报文段）。

第三步，主机B的应用程序告诉TCP：我要彻底的关闭连接，TCP向主机A送一个FIN报文段。

第四步，主机A收到这个FIN报文段后，向主机B发送一个ACK表示连接彻底释放。

**为什么不是2次握手：防止失效的连接请求报文段突然又传送到主机B** 

①A首先发送一个连接请求，但是该请求在网络节点上滞留了，没有收到确认。于是A重传了一次请求，并且收到了B的确认，于是连接建立，数据传输完成后，释放连接，假定A发出的第一个请求报文段并没有丢失，而是在某些网络节点上滞留，本来是一个失效的请求，但B收到后误认为是A再次发出一个新请求，于是向A发送确认，同意建立连接。

②假定采用两次握手，那么只要B发出确认，则新的连接就建立了。由于A并没有发出请求，因此不理会B的确认，也不会向B发送数据，但B却以为新的连接已经建立，并一直等待A的数据，B的许多资源就这样白白浪费了。

③假定采用三次握手，则B发出确认，但A因为并没有发请求，所以不理会B的确认，B没有收到A的确认，则连接建立失败，B知道连接建立失败。会回收资源。

**为什么不采用4次握手：** 
①握手握的是序列号，四次握手的过程是：（1）A发送给B 同步序号SYN+A的seq为x；2，B收到后确认收到ACK发给A，然后令ack=x+1，确认A的seq按序到达；3，B向A发送SYN+自己的序列号seq为y；4, A收到B的序列号，存储到本地，发送ack=y+1,确认收到了，发送ACK+ack和自己的新序号seq为x+1。

**②其中2,3两步是完全可以归为一次发送的，所以不需要四次握手。**