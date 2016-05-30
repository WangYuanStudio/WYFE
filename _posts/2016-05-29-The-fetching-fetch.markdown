---
layout:     post
title:      "The fetching fetch"
subtitle:   "迷人的Fetch Api"
date:       2016-05-30
author:     "J3n5en"
header-img: "http://ww1.sinaimg.cn/large/6bf00bd8jw1f4dk43kn7ej20xz09hjsw.jpg"
tags:
    - Javascripe
    - Front-end
---

说到`XMLHttpRequest`估计你们都不陌生，通常我们说Ajax技术，就是指基于`XMLHttpRequest`的Ajax。他的一个demo是这样的：

```javascript
// 首先，需要些一些特征检测来做下浏览器兼容
if (window.XMLHttpRequest) {  
  request = new XMLHttpRequest();
} else if (window.ActiveXObject) {
  try {
    request = new ActiveXObject('Msxml2.XMLHTTP');
  } 
  catch (e) {
    try {
      request = new ActiveXObject('Microsoft.XMLHTTP');
    } 
    catch (e) {}
  }
}

// 然后开启一个请求
request.open('GET', 'https://nodejs.org/api/http.json', true);  
// 真正意义上的请求
request.send(null);  
request.addEventListener('load', function() {  
    // 处理返回结果
})
```

就算不写兼容那段代码，光写后面发起请求的那段代码，也够我烦了。虽然说有很多第三方封装好的API就像`jQuery.ajax()`.但是有没有更加简洁、方便的解决方案呢？答案是有的，这篇文章要介绍的内容就是`XMLHttpRequest`的最新替代技术——[Fetch API](https://fetch.spec.whatwg.org/)。

## 浏览器兼容

---

![caniuse](http://ww2.sinaimg.cn/large/6bf00bd8jw1f4dgdu4ghpj20yv09m40j.jpg)

从[CaniUse报告](http://caniuse.com/#search=fetch)中可以看出，意料中的IE全家完美不兼容。

不过可以参考[github的fetch兼容解决方案](https://github.com/github/fetch),他可以解决一些兼容问题

| [![Chrome](https://camo.githubusercontent.com/3bfe3f8c64cf4e968b3d45f587c291853a1b8035/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f6368726f6d652f6368726f6d655f34387834382e706e67)](https://camo.githubusercontent.com/3bfe3f8c64cf4e968b3d45f587c291853a1b8035/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f6368726f6d652f6368726f6d655f34387834382e706e67) | [![Firefox](https://camo.githubusercontent.com/0a3d07e334548501ef5b7c20a75fc1a4e9457566/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f66697265666f782f66697265666f785f34387834382e706e67)](https://camo.githubusercontent.com/0a3d07e334548501ef5b7c20a75fc1a4e9457566/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f66697265666f782f66697265666f785f34387834382e706e67) | [![IE](https://camo.githubusercontent.com/439d1528b7dc0a003ff74eaeb1fe30d24bb6c904/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f696e7465726e65742d6578706c6f7265722f696e7465726e65742d6578706c6f7265725f34387834382e706e67)](https://camo.githubusercontent.com/439d1528b7dc0a003ff74eaeb1fe30d24bb6c904/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f696e7465726e65742d6578706c6f7265722f696e7465726e65742d6578706c6f7265725f34387834382e706e67) | [![Opera](https://camo.githubusercontent.com/ef1c2ea75ec9ec27156ec690f03b8b44e9c0e996/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f6f706572612f6f706572615f34387834382e706e67)](https://camo.githubusercontent.com/ef1c2ea75ec9ec27156ec690f03b8b44e9c0e996/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f6f706572612f6f706572615f34387834382e706e67) | [![Safari](https://camo.githubusercontent.com/7e8c82eab10c4686d5d94a5875ba436750ac33d7/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f7361666172692f7361666172695f34387834382e706e67)](https://camo.githubusercontent.com/7e8c82eab10c4686d5d94a5875ba436750ac33d7/68747470733a2f2f7261772e6769746875622e636f6d2f616c7272612f62726f777365722d6c6f676f732f6d61737465722f7361666172692f7361666172695f34387834382e706e67) |
| :--------------------------------------: | :--------------------------------------: | :--------------------------------------: | :--------------------------------------: | :--------------------------------------: |
|                 Latest ✔                 |                 Latest ✔                 |                  10+ ✔                   |                 Latest ✔                 |                  6.1+ ✔                  |

PS：经过测试，微信的的渣5内核需要使用[github的fetch兼容解决方案](https://github.com/github/fetch)。

## 语法

---

```javascript
fetch(input, init).then(function(response) { ... });  
```

``fetch()``接受两个参数

`input` 参数，即可以直接传入一个 url，也可以传入一个 [`Request`](https://developer.mozilla.org/en-US/docs/Web/API/Request) 对象;

`init` 参数是可选，是一个包含了请求方法，请求头部，请求主体，模式，凭证，缓存模式等配置的对象

返回的是一个 [Promise](https://developer.mozilla.org/en-US/docs/Web/API/Promise)。

### 一、简单的fetch示例

一个上面`XHR`一样的GET请求

```javascript
fetch('https://nodejs.org/api/http.json')
	.then(function(response) {  
   		return // 响应处理
	})
	.catch(function(err) {
    	// 捕获错误
	});
```

提交一个POST请求

```javascript
fetch("http://www.example.org/submit.php", {
  method: "POST",
  headers: {
    "Content-Type": "application/x-www-form-urlencoded"
  },
  body: "firstName=Nikhil&favColor=blue&password=easytoguess"
}).then(function(res) {
   return // 响应处理
}, function(e) {
  // 捕获错误
});
```

提交一个JOSN格式的POST请求

```javascript
fetch('/submit-json', {
	method: 'post',
	body: JSON.stringify({
		email: document.getElementById('email')
		answer: document.getElementById('answer')
	})
});
```

### 二、处理JSON

假如说，你需求发起一个JSON请求 — 返回的数据对象里有一个`json`方法，能将原始数据转化成JavaScript对象：

```javascript
fetch('http://www.webhek.com/demo/arsenal.json')
	.then(function(response) { 
		return response.json();
	}).then(function(j) {
		console.log(j); 
	});
```

其实它就是一个简单的`JSON.parse(jsonString)`调用，但是还是很方便的。

### 三、处理基本的Text/HTML响应

有时候我们需要的不是JSON而仅仅是普通的文本或者HTML,这个时候我们可以改用`.text()`

```javascript
fetch('/next/page')
  .then(function(response) {
    return response.text();
  }).then(function(text) { 
  	// <!DOCTYPE ....
  	console.log(text); 
  });
```

### 四、配置headers

```javascript
// url (required), options (optional)
fetch('/some/url', {
	headers: {
		'Accept': 'application/json',
    		'Content-Type': 'application/json'
	}
});
```



## 最后

---

想要[更详细的文档](https://fetch.spec.whatwg.org/)可以点这。

fetch api 是简单好用，但是还是不稳定的，还在开发中的，所以在使用的时候，要考虑好兼容性。


> Happy Hacking !! && have a nice day!!

\#EOF\#

