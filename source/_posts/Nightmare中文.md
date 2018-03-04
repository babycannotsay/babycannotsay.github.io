---
title: 【测试框架】Nightmare中文版
date: 2017-12-16 18:50:48
categories: "一劳永逸"
tags:
---

英文版地址：<https://github.com/segmentio/nightmare#api>
## API ##
### `Nightmae.version` ###
返回Nightmare的版本号

### `waitTimeout (default: 30s)`  ###
如果在设置的时间段内，.wait()没有return true，就抛出一个异常

	const nightmare = Nightmare({
		waitTimeout: 1000 //in ms
	});

<!--more-->
### `gotoTimeout (default: 30s)` ###
如果在设置的时间段内，.goto()没有结束loading，就抛出一个异常。注意，即时页面的所有资源都load完了，如果dom还没有loaded，还是会抛出异常。

	const nightmare = Nightmare({
		gotoTimeout: 1000 // in ms
	});

### `loadTimeout (default: infinite)` ###
如果某个事件（比如.click()）造成了页面跳转不能再设置时间内结束，强制Nightmare继续。如果loadTimeout比gotoTimeout短，gotoTimeout能够抛出的异常将被抑制。

	const nightmare = Nightmare({
		loadTimeout: 1000 // in ms
	});

### `executionTimeout (default: 30s)` ###
等待.evvaluate()状态完成的最大时间

	const nightmare = Nightmare({
		executionTimeout: 1000 // in ms
	});

### `paths` ###
Electron知道的系统默认路径。以下是可用的路径列表
<https://github.com/atom/electron/blob/master/docs/api/app.md#appgetpathnames>
你能像如下方式重写他们

	const nightmare = Nightmare({
		paths: {
			userData: '/user/data'
		}
	});

### `switches` ###
由Chrome浏览器使用的命令行开关也由Electron支持。 以下是支持的Chrome命令行开关列表
<https://github.com/atom/electron/blob/master/docs/api/chrome-command-line-switches.md>

const nightmare = Nightmare({
  switches: {
    'proxy-server': '1.2.3.4:5678',
    'ignore-certificate-errors': true
  }
});

### `electronPath` ###
预建的Electron二进制文件的路径。 这对在不同版本的Electron上进行测试非常有用。 请注意，Nightmare只支持这个包依赖的版本。 使用此选项需要您自担风险。

	const nightmare = Nightmare({
	  electronPath: require('electron')
	});

### `dock (OS X)` ###
一个布尔值，可选地在停靠栏中显示Electron图标（默认为false）。 这对于测试目的很有用。

	const nightmare = Nightmare({
	  dock: true
	});

### `openDevTools` ###
可以选择使用true在Electron窗口中显示DevTools，或使用包含对象哈希模式：'detach'在单独的窗口中显示。 散列被传递给contents.openDevTools（）来处理。 这对于测试目的也是有用的。 请注意，只有在show设置为true的情况下才能使用此选项。

	const nightmare = Nightmare({
	  openDevTools: {
	    mode: 'detach'
	  },
	  show: true
	});

### `typeInterval (default: 100ms)` ###
使用.type（）时击键之间要等待多长时间。

	const nightmare = Nightmare({
	  typeInterval: 20
	});

### `pollInterval (default: 250ms)` ###
检查.wait（）条件是否成功需要等待多长时间。

	const nightmare = Nightmare({
	  pollInterval: 50 //in ms
	});

### `maxAuthRetries (default: 3)` ###
定义使用.authenticate（）进行设置时重试认证的次数。

	const nightmare = Nightmare({
	  maxAuthRetries: 3
	});

### `.engineVersions()` ###
获取Electron和Chromium的版本。

### `.useragent(useragent)` ###
设置Electron使用的useragent。

### `.authentication(user, password)` ###
使用基本身份验证设置访问网页的用户和密码。 请务必在调用.goto（url）之前进行设置。

### `.end()` ###
完成任何队列操作，断开并关闭Electron进程。 请注意，如果使用的是promise，则必须在.end（）之后调用.then（）才能运行.end（）任务。 还要注意，如果使用.end（）回调，则.end（）调用等同于调用.end（）后跟.then（fn）。 考虑：
	
	nightmare
	  .goto(someUrl)
	  .end(() => "some value")
	  //prints "some value"
	  .then(console.log);

### `.halt(error, done)` ###
清除所有排队的操作，杀死Electron进程，并将错误信息或“Nightmare Halted”传递给未解决的承诺。 完成后将在调用完成后调用。

## 页面交互 ##

### `.goto(url[, headers])` ###
在url中加载页面。 可选地，可以提供头部哈希以在goto请求上设置头部。

当页面加载成功时，goto返回一个包含页面加载元数据的对象，包括：

`url`：已加载的网址
`code`：HTTP状态代码（例如200,404,500）
`method`：使用的HTTP方法（例如“GET”，“POST”）
`referrer`：窗口在此载入之前显示的页面，或者如果这是首页加载，则为空字符串。
headers：表示请求响应头的对象，如{header1-name：header1-value，header2-name：header2-value}

如果页面加载失败，则错误将是具有以下属性的对象：

message：描述错误类型的字符串
code：描述出错的底层错误代码。 请注意，这不是HTTP状态码。 有关可能的值，请参阅<https://code.google.com/p/chromium/codesearch#chromium/src/net/base/net_error_list.h>
details：包含有关错误的其他详细信息的字符串 这可能是null或一个空字符串。
url：未能加载的网址

请注意，来自服务器的任何有效响应都被认为是“成功的”。这意味着404“找不到”错误是goto的成功结果。 只有那些不会在浏览器窗口中出现页面的东西，比如没有服务器在给定的地址响应，服务器挂在响应中，或者URL无效，都是错误的。

你也可以通过在Nightmare构造函数中设置gotoTimeout选项来调整goto等待超时的时间。

### `.back()` ###
回到上一页。

### `.forward()` ###
转到下一页。

### `.refresh()` ###
刷新当前页面。

### `.click(selector)` ###
点击一次选择元素。

### `.mousedown(selector)` ###
按下一次选择元素

### `.mouseup(selector)` ###

### `.mouseover(selector)` ###

### `.mouseout(selector)` ###

### `.type(selector[, text])` ###
输入提供给selector元素的text。 为text提供空值或伪值(Boolean(text) === false)将清空选择器的值。

.type（）模仿用户在文本框中输入并发出正确的键盘事件。

按键也可以使用.type（）的Unicode值进行触发。 例如，如果您想按下回车键，则会写入.type（'body'，'\ u000d'）。
> 如果您不需要键盘事件，请考虑使用.insert（），因为它会更快，更健壮。

### `.insert(selector[, text])` ###
类似于.type（），.insert（）输入提供给选择器元素的文本。 为文本提供的空值或伪值将清除选择器的值。.insert（）比.type（）快，但不会触发键盘事件。

### `.check(selector)` ###
选中selector复选框元素。

### `.uncheck(selector)` ###
取消选中selector复选框元素。

### `.select(selector, option)` ###
将selector选择器下拉元素更改为属性[value = option]的选项

### `.scrollTo(top, left)` ###
将页面滚动到所需的位置。 顶部和左侧总是相对于文档的左上角。

### `.viewport(width, height)` ###
设置视口大小。

### `.inject(type, file)` ###
将本地文件注入当前页面。 文件类型必须是js或css。

### `.evaluate(fn[, arg1, arg2,...])` ###
在页面上用arg1，arg2，...调用fn。所有参数都是可选的。 完成后返回fn的返回值。 用于从页面提取信息。 以下是一个例子：

	const selector = 'h1';
	nightmare
	  .evaluate((selector) => {
	    // now we're executing inside the browser scope.
	    return document.querySelector(selector).innerText;
	   }, selector) // <-- that's how you pass parameters from Node scope to browser scope
	  .then((text) => {
	    // ...
	  })

作为evaluate（）的一部分支持Error-first回调。 如果传递的参数少于预期的`evaluate`函数的参数，`evaluate`将作为函数的最后一个参数传递回调。 例如：

	const selector = 'h1';
	nightmare
	  .evaluate((selector, done) => {
	    // now we're executing inside the browser scope.
	    setTimeout(() => done(null, document.querySelector(selector).innerText), 2000);
	   }, selector)
	  .then((text) => {
	    // ...
	  })

请注意，回调只支持一个值参数（例如`function（err，value）`）。 最终，回调将被包装在原生Promise中，并且只能解析单个值。

作为evaluate（）的一部分，Promise也被支持。 如果函数的返回值有一个`then`成员，则.evaluate（）会假定它正在等待一个Promise。 例如：

	const selector = 'h1';
	nightmare
	  .evaluate((selector) => (
	    new Promise((resolve, reject) => {
	      setTimeout(() => resolve(document.querySelector(selector).innerText), 2000);
	    )}, selector)
	  )
	  .then((text) => {
	    // ...
	  })

### `.wait(ms)` ###
等待ms毫秒

### `.wait(selector)` ###
一直等到selector元素存在

### `.wait(fn[, arg1, arg2,...])` ###
一直等到用arg1，arg2，...返回true的页面上评估的fn。 所有的参数都是可选的。 请参阅.evaluate（）以了解使用情况。

### `.header(header, value)` ###
为所有HTTP请求添加一个头覆盖。 如果header未定义，头部覆盖将被重置。

## 提取页面 ##

### `.exists(selector)` ###
返回选择器是否存在或不在页面上。

### `.visible(selector)` ###
返回选择器是否可见。

### `on(event, callback)` ###
用回调捕获页面事件。 在调用.goto（）之前，您必须调用.on（）。 支持的事件记录在[这里](https://electronjs.org/docs/api/web-contents#class-webcontents)。

### 可选的页面事件 ###
#### `.on('page', function(type="error", message, stack))` ####

如果在页面上抛出任何javascript异常，则触发此事件。 但是，如果注入的JavaScript代码（例如通过.evaluate（））抛出异常，则不会触发此事件。

### 页面事件 ###
监听`window.addEventListener('error')`, `alert(...)`, `prompt(...)` & `confirm(...)`.

#### `.on('page', function(type="error", message, stack))` ####
侦听最高级页面错误。 这会在页面上抛出错误时触发。

#### `.on('page', function(type="alert", message))` ####
默认情况下Nightmare禁用window.alert弹出，但你仍然可以监听警报对话框的内容。

#### `.on('page', function(type="prompt", message, response))` ####
默认情况下Nightmare禁用window.prompt弹出，但你仍然可以监听消息来。 如果您需要以不同方式处理确认，则需要使用自己的预加载脚本。

#### `.on('page', function(type="confirm", message, response))` ####
默认情况下Nightmare禁用window.confirm弹出，但你仍然可以监听消息来。 如果您需要以不同方式处理确认，则需要使用自己的预加载脚本。

#### `.on('console', function(type [, arguments, ...]))` ####

type将是log，log或error，参数是从控制台传递的。 如果注入的JavaScript代码（例如，通过.evaluate（））使用console.log，则不会触发此事件。

### `.once(event, callback)` ###
与.on（）类似，但只捕获一次具有回调的页面事件。

### `.removeListener(event, callback)` ###
删除给定事件的侦听器回调。

### `.screenshot([path][, clip])` ###
截取当前页面的屏幕截图。 用于调试。 输出总是一个PNG。 两个参数都是可选的。 如果提供了路径，它将图像保存到磁盘。 否则它返回图像数据的缓冲区。 如果提供剪辑（如[此处](https://github.com/electron/electron/blob/master/docs/api/browser-window.md#wincapturepagerect-callback)所述），图像将被剪裁到矩形。

### `.html(path, saveType)` ###
将当前页面保存为html，将文件保存到指定路径的磁盘上。 保存类型选项在[这里](https://github.com/electron/electron/blob/master/docs/api/web-contents.md#webcontentssavepagefullpath-savetype-callback)。

### `.pdf(path, options)` ###
将PDF保存到指定的路径。 选项在[这里](https://github.com/electron/electron/blob/v1.4.4/docs/api/web-contents.md#contentsprinttopdfoptions-callback)。

### `.title()` ###
返回当前页面的title

### `.url()` ###
返回当前页面的url

### `.path()` ###
返回当前页面的路径名

## Cookies ##

### `.cookies.get(name)` ###
通过name获取一个cookie。 该网址将是当前的网址。

### k.cookies.get(query) ###
使用查询对象查询多个cookie。 如果设置了query.name，它将返回它找到的第一个cookie，否则它将查询一个cookie数组。 如果没有设置query.url，它将使用当前的URL。 这是一个例子：

	// get all google cookies that are secure
	// and have the path `/query`
	nightmare
	  .goto('http://google.com')
	  .cookies.get({
	    path: '/query',
	    secure: true
	  })
	  .then((cookies) => {
	    // do something with the cookies
	  })

可用的属性记录在[这里](https://github.com/atom/electron/blob/master/docs/api/session.md#sescookiesgetdetails-callback)

### `.cookies.get()` ###
获取当前网址的所有Cookie。 如果您想为所有网址获取所有Cookie，请使用：.get（{url：null}）。

### `.cookies.set(name, value)` ###
设置cookie的名称和值。 这是最基本的形式，url将是当前的网址。

### `.cookies.set(cookie)` ###
设置一个cookie。 如果cookie.url没有设置，它会在当前的url上设置cookie。 这是一个例子：

	nightmare
	  .goto('http://google.com')
	  .cookies.set({
	    name: 'token',
	    value: 'some token',
	    path: '/query',
	    secure: true
	  })
	  // ... other actions ...
	  .then(() => {
	    // ...
	  })

可用的属性记录在[这里](https://github.com/atom/electron/blob/master/docs/api/session.md#sescookiesgetdetails-callback)

### `.cookies.set(cookies)` ###
一次设置多个Cookie。 cookie是一个cookie对象的数组。 看看上面的.cookies.set（cookie）文档，以更好地了解cookie应该是什么样的。

### `.cookies.clear([name])` ###
清除当前网域的Cookie。 如果未指定名称，则当前域的所有Cookie将被清除。

	nightmare
	  .goto('http://google.com')
	  .cookies.clear('SomeCookieName')
	  // ... other actions ...
	  .then(() => {
	    // ...
	  })

### `.cookies.clearAll()` ###
删除所有域名下的所有cookies

	nightmare
	  .goto('http://google.com')
	  .cookies.clearAll()
	  // ... other actions ...
	  .then(() => {
	    //...
	  });

## 代理 ##
259/5000
Nightmare通过`switches`支持代理。

如果您的代理需要认证，您还需要`authentication`呼叫。

下面的示例不仅演示如何使用代理，但您可以运行它来测试您的代理连接是否正常工作：

	import Nightmare from 'nightmare';
	
	const proxyNightmare = Nightmare({
	  switches: {
	    'proxy-server': 'my_proxy_server.example.com:8080' // set the proxy server here ...
	  },
	  show: true
	});
	
	proxyNightmare
	  .authentication('proxyUsername', 'proxyPassword') // ... and authenticate here before `goto`
	  .goto('http://www.ipchicken.com')
	  .evaluate(() => {
	    return document.querySelector('b').innerText.replace(/[^\d\.]/g, '');
	  })
	  .end()
	  .then((ip) => { // This will log the Proxy's IP
	    console.log('proxy IP:', ip);
	  });
	
	// The rest is just normal Nightmare to get your local IP
	const regularNightmare = Nightmare({ show: true });
	
	regularNightmare
	  .goto('http://www.ipchicken.com')
	  .evaluate(() =>
	    document.querySelector('b').innerText.replace(/[^\d\.]/g, '');
	  )
	  .end()
	  .then((ip) => { // This will log the your local IP
	    console.log('local IP:', ip);
	  });

## Promises ##

默认情况下，Nightmare使用默认的本地ES6 promises。 你可以插入你最喜欢的[ES6风格 promise 库](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)，如[bluebird](https://www.npmjs.com/package/bluebird)或[q](https://www.npmjs.com/package/q)为方便！这是一个例子：

	var Nightmare = require('nightmare');
	
	Nightmare.Promise = require('bluebird');
	// OR:
	Nightmare.Promise = require('q').Promise;

您还可以使用Promise构造函数选项为每个实例指定一个自定义的Promise库，如下所示：

	var Nightmare = require('nightmare');
	
	var es6Nightmare = Nightmare();
	var bluebirdNightmare = Nightmare({
	    Promise: require('bluebird')
	});
	
	var es6Promise = es6Nightmare.goto('https://github.com/segmentio/nightmare').then();
	var bluebirdPromise = bluebirdNightmare.goto('https://github.com/segmentio/nightmare').then();
	
	es6Promise.isFulfilled() // throws: `TypeError: es6EndPromise.isFulfilled is not a function`
	bluebirdPromise.isFulfilled() // returns: `true | false`

## Nightmare 扩展 ##

### `Nightmare.action(name, [electronAction|electronNamespace], action|namespace)` ###

您可以将自己的自定义操作添加到Nightmare原型。 这是一个例子：

	Nightmare.action('size', function(done) {
	  this.evaluate_now(() => {
	    const w = Math.max(document.documentElement.clientWidth, window.innerWidth || 0)
	    const h = Math.max(document.documentElement.clientHeight, window.innerHeight || 0)
	    return {
	      height: h,
	      width: w
	    }
	  }, done)
	})
	
	Nightmare()
	  .goto('http://cnn.com')
	  .size()
	  .then((size) => {
	    //... do something with the size information
	  });

> 请记住，这是附加到静态类Nightmare，而不是实例。

你会注意到我们使用了一个内部函数evaluate_now。 这个函数与nightmare.evaluate不同，因为它立即运行，而nightmare.evaluate是排队的。一个简单的方法来记住：当有疑问时，使用evaluate。 如果您正在创建自定义操作，请使用evaluate_now。 技术原因是因为我们的行动已经排队了，现在我们正在运行它，所以我们不应该重新排列评估函数。我们也可以创建自定义的命名空间。 我们在内部为nightmare.cookies.get和nightmare.cookies.set执行此操作。 如果你有一堆你想要暴露的动作，这些就很有用，但是它会把主要的Nightmare对象弄得乱七八糟。 这是一个例子：

	Nightmare.action('style', {
	  background(done) {
	    this.evaluate_now(
	      () => window.getComputedStyle(document.body, null).backgroundColor, done
	    )
	  }
	})
	
	Nightmare()
	  .goto('http://google.com')
	  .style.background()
	  .then((background) => {
	    // ... do something interesting with background  
	  })

您还可以添加自定义Electron行动。 额外的Electron动作或名称空间动作包括name，option，parent，win，renderer和done。 请注意，Electron动作首先映射.evaluate（）如何工作。 例如：

	Nightmare.action('clearCache', (name, options, parent, win, renderer, done) => {
	    parent.respondTo('clearCache', (done) => {
	      win.webContents.session.clearCache(done);
	    });
	    done();
	  },
	  function (done) {
	    this.child.call('clearCache', done);
	  });
	
	Nightmare()
	  .clearCache()
	  .goto('http://example.org')
	  //... more actions ...
	  .then(() => {
	    // ...
	  });

在导航到example.org之前，会清除浏览器的缓存。

有关创建自定义操作的更多细节，请参阅此[文档](https://github.com/rosshinkley/nightmare-examples/blob/master/docs/beginner/action.md)。

### `.use(plugin)` ###
nightmare.use用于重用实例上的一组任务。 查看[nightmare-swiftly](https://github.com/segmentio/nightmare-swiftly)的一些例子。

## 自定义预加载脚本 ##
如果您在第一次加载窗口环境时需要自定义某些内容，则可以指定自定义预加载脚本。 以下是你如何做到这一点：

	import path from 'path';
	
	const nightmare = Nightmare({
	  webPreferences: {
	    preload: path.resolve("custom-script.js")
	    //alternative: preload: "absolute/path/to/custom-script.js"
	  }
	})


该脚本的唯一要求是您需要以下前奏：

	window.__nightmare = {};
	__nightmare.ipc = require('electron').ipcRenderer;

为了从浏览器中获得所有Nightmare的反馈，可以改为复制Nightmare的[预加载脚本](https://github.com/segmentio/nightmare/blob/master/lib/preload.js)的内容。

在Nightmare实例之间的存储持久性
默认情况下，Nightmare将为每个实例创建一个内存分区。 这意味着当Nightmare结束时，任何localStorage或cookie或任何其他形式的持久状态都将被销毁。 如果你想在实例之间保持状态，你可以在Electron中使用[webPreferences.partition](https://electronjs.org/docs/api/browser-window#new-browserwindowoptions) api。

	import Nightmare from 'nightmare';
	
	nightmare = Nightmare(); // non persistent paritition by default
	yield nightmare
	  .evaluate(() => {
	    window.localStorage.setItem('testing', 'This will not be persisted');
	  })
	  .end();
	
	nightmare = Nightmare({
	  webPreferences: {
	    partition: 'persist: testing'
	  }
	});
	yield nightmare
	  .evaluate(() => {
	    window.localStorage.setItem('testing', 'This is persisted for other instances with the same paritition name');
	  })
	  .end();

如果你指定一个空分区，那么它将使用Electron默认行为（持久化），或者以“persist：”开头的任何字符串将保留在该分区名称下，任何其他字符串将导致仅存储在内存中。

## 用法 ##

### 执行 ###
Nightmare是一个Node模块，可以在Node.js脚本或模块中使用。 这是一个简单的脚本来打开一个网页：

	import Nightmare from 'nightmare',
	
	const nightmare = Nightmare();
	
	nightmare.goto('http://cnn.com')
	  .evaluate(() => {
	    return document.title;
	  })
	  .end()
	  .then((title) => {
	    console.log(title);
	  })

如果将其保存为cnn.js，则可以在命令行上像这样运行它：
	
	npm install --save nightmare
	node cnn.js

### 常见执行问题 ###
Nightmare严重依赖于Electron。 而Electron依赖于几个UI的依赖（如libgtk +），这些往往都会在服务器发行版中丢失的。

为了帮助您在服务器上运行Nightmare检查如何在[Amazon Linux和CentOS](https://gist.github.com/dimkir/f4afde77366ff041b66d2252b45a13db)指南上运行Nightmare。

### Debugging ###
有三种好方法可以获得关于无头浏览器内部发生的事情的更多信息：
1.使用下面描述的DEBUG = *标志。
2.将{show：true}传递给Nightmare构造函数，让它创建一个可见的渲染窗口，在那里你可以看到发生了什么。
3.监听特定的事件。
要运行调试输出相同的文件，像这样运行`DEBUG = nightmare node cnn.js`（在Windows上用 `set DEBUG = nightmare＆node cnn.js`）。这将打印出一些关于发生的事情的附加信息：

	nightmare queueing action "goto" +0ms
	nightmare queueing action "evaluate" +4ms
	Breaking News, U.S., World, Weather, Entertainment & Video News - CNN.com

### Debug Flags ###
所有Nightmare信息

`DEBUG=nightmare*`

Only actions

`DEBUG=nightmare:actions*`

Only logs

`DEBUG=nightmare:log*`

### Tests ###
Nightmare本身的自动测试使用Mocha和Chai来运行，两者都将通过npm install进行安装。 运行恶梦的测试，运行make test。测试完成后，您会看到如下所示的内容：

	make test
	  ․․․․․․․․․․․․․․․․․․
	  18 passing (1m)

请注意，如果您使用的是xvfb，则make test会自动运行xvfb-run包装下的测试。 如果您打算在不运行xvfb的情况下无头运行测试，请将HEADLESS环境变量设置为0。