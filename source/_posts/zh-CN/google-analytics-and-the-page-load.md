title: Google分析与页面加载
comments: true
categories:
  - 翻译
  - Javascript
  - Front-end
tags:
  - 翻译
  - Front-end
  - Google
date: 2015-12-25 13:56:50
---

　　一篇关于浏览器页面加载以及Google分析等数据收集工具运行机制的文章**[Google Analytics And The Page Load](http://www.simoahava.com/analytics/google-analytics-page-load/)** ，翻译翻译，加深理解。
　　
——————————————————————————————————————————————————————————————————————　


如果你使用过[Google分析](http://www.google.com/analytics/)、[Google标签管理](https://tagmanager.google.com/)，或者任何基于Javascript的数据收集与分析平台的产品，你是否曾经好奇过他们到底是如何工作的？我能理解，你当然更加关心那些收集到的数据，但你是否认为一切关于这些工具的处理和运行都是理所当然？<!-- more -->

这个问题我思考了很长一段时间，因为我并不确定那些在工作中与这些平台息息相关的人们，是否真正的理解浏览器与页面之间是如何交互的。

当然这些都无关紧要，但确实令我有一点点的吃惊。

因为如今关于网页分析的主导范式 (predominant paradigm) 都是围绕 Javascript 的，所以对于理解这一技术的薄弱环节非常的重要。而在 Javascript 参与进来之前，有一些你为了能获得更高质量的数据而必须遵守的关于页面加载过程的部分你需要了解。

幸运的是，这些认识是能够被解释清楚的。“不，这些工具没有能覆盖到全部的站点访问者；不，浏览量，会话量和用户量不能够只是从字面进行理解；还有，不，在追踪你的站点失败时，大多数时候错不在Google Tag Manager或Google Analytics”。通过在你的站点打开JavaScript的控制台，你会找到真正导致错误的原因。当你真正发现这些问题所在时，不要忘记及时的处理解决掉他们。

![](/images/gallery/2015/12/25/js-console.png)

本文的核心简单来说就是一句话：

> 唯一能够评估你使用的数据的质量的方法是去理解这些数据都是被如何收集的。

记住这句话。我将会描述页面在用户浏览器中的家在过程和揭示Google Analytics是如何对不同过程进行追踪的。

## **事件在页面中的加载序列**

![](/images/gallery/2015/12/25/the-process.png)

本文将分成几个部分来说明。在用户访问你的站点时，每一个部分代表整个错综复杂的处理过程的一个阶段。这些部分揭示了，当用户在和你站点的内容交互时，你应该如何追踪和追踪些什么。

1. [The Request](#request) —— 当一个用户在浏览器的地址栏输入一个地址后都发生了些什么，还有网站追踪的压力点是什么。
2. [The Render](#render) —— 浏览器是如何将源码转换成鲜活的页面的。
3. [The Race](#race) —— 异步JavaScript是如何工作的，竞争条件是什么和你应该注意些什么。
4. [The Interaction](#interaction) —— 一旦页面加载完成，用户的交互式如何被度量的，此时最大的陷阱是什么。

一如往常，先做了一个要点的总结。

### <a name="request"></a>**The Request**

一切到开始于请求。

当一个用户在地址栏中输入了你网站的地址后，希望出现的结果通常是一个网页。为了使这个网页能够产生，浏览器使用HTTP协议向隐藏在地址背后的服务器发起了一次请求。

如果在远端存在一个网站服务器，并且远端已经将请求地址映射到了某个资源，同时发起的请求是有效的（假设不存在额外的认证，防火墙没有拦截此次请求，没有违反其他的安全策略等），网站服务器会发送一个“OK”的回复，大多数情况下包含一个200的状态码，并且通过地址请求的资源文档已经包含在了回复的body体中。

这个文档通常是一个HTML模板文件（页面的源代码），而浏览器的工作就是把这份文档翻译成动态的网页。

为了Google Analytics能够正常的收集到相关数据，这里有一些事是你需要注意的。

#### **1.404——资源未找到**

如果用户请求的地址没有映射到任何的资源，网站服务器将会返回一个“资源未找到”的错误。对于这种情况，你应该配置你的网站服务器，当资源未找到时，发送一个“页面没有找到”的页面模板给你的用户。

**GA应该以一个标签在这个页面上运行，用以追踪浏览量**。[404页面追踪](http://www.lunametrics.com/blog/2014/08/19/404-errors-google-analytics-google-tag-manager/)是一个很棒的额外工具，站长常用以进行斜线分析。

如果你没有设置一个模板，用户将会看到由服务器提供的默认错误页面（它们通常体验很糟糕），如果连默认的错误页面也没有，浏览器将会自动生成一个错误信息（糟糕透了）。重点在于如果你没有一个包含GA追踪代码的自定义模板页面，你将无法在Google Analytics中追踪这些信息。

#### **2. 重定向**

如果你在服务器端添加了一个重定向，一定记得在重定向中维护**查询字符参数（query string parameters）**！如果你忽略了，你将有可能会丢失重要的广告系列追踪信息。

这是维护获取数据质量的基础。

#### **3. 单页面应用**

如果你的站点本质上是一个单页面应用（不同于以往的HTTP GET请求，用户浏览器使用POST请求），这些传统的页面追踪规则需要有一点点变化

你不需要在信任发生在每次页面加载时的重载，实际上在第一个页面被加载完之后，不存在其他的页面加载。你将不得不为追踪你的访问者提出一个新的术语。比如“[滚动追踪（scroll tracking）](http://cutroni.com/blog/2014/02/12/advanced-content-tracking-with-universal-analytics/)”，通过事件和虚拟浏览量；或者追踪用户与页面的交互，比如在选页卡上的点击或者采取某些动作的按钮；或者是使用GTM的[历史监听（history listener）](http://www.simoahava.com/analytics/google-tag-manager-history-listener/)，在浏览器状态中对于变化做出放映。

如果你正在使用Google Tag Manager，一个关于单页面应用很重要的部分就是当页面刷新的时候，`datalayer`[对象](http://www.simoahava.com/analytics/data-layer/)不会重置。这是因为在一个网页环境中，每次页面刷新时，已经被详细渲染的包含了所有相关的变量、对象、元素、文本、链接、图片和其他细节的网页将会被重新创建。换句话说，当页面变换之后，原有页面中存在的对象不会持续存在。而对于一个单页面应用，是不存在页面刷新的，所以也就不存在原有对象的清除。数据层会一直存在用户所浏览的页面中，这就意味着很可能出现存储在`dataLayer`中的数据在传送给GA之后，后续才操作又被重复传送，出现重复追踪。

对于单页面应用，在整个周期中你需要对`dataLayer`持续关注，如果必要，你需要通过删除一些你不希望存储的对象或者值以模仿页面刷新：

``` javascript
dataLayer.push({
  'key_to_be_deleted' : undefined
});
```

#### **4. 别做傻事，搞特立独行**

最后，一个很重要部分关于在与网站服务器交互中，大多数的数据分析平台都希望从你的服务器中能够得到标准的行为。所以，不要定制自定义像“200 OK”这样的回复。如果你的服务器没有提供标准的回复，很可能会导致回调没有执行，点击没有反应，价值数据的又一次丢失等。

Request过程的关键在于：

> 在模板处理和服务器响应中遵守最佳实践 —— 你的分析工具将会给你想要的数据。

### <a name="render"></a>**The Render**

当浏览器接受到源代码之后，它做了一些在自以为是的事。它给了站点一个懈怠的点。正如你看到的，JavaScript被诋毁，浏览器战争越演越烈的众多原因之一就是由于网站本身的错误被遮盖掉了。你的站点可以使用最糟糕的源代码，但在被传送到浏览器之后，它将变成一个令人惊奇的，精心设计的动态网页文档。

这很像修音设备对于那些蹩脚的歌手那样，音乐产业使用修音设备使那些不学无术者变成流行明星。同样的道理，网页浏览器将那些令人厌恶，违反编码的部分转变成了正常网页，而没有直言不讳的告诉开发人员他们的工作很糟糕。

现在，我们可以说修音设备是一个好东西，就像我们可以说浏览器为你做的这样也是不错的。但使用机器就能提高演唱并不是你唱功停滞不前的借口，正如浏览器修正了你的源码错误并不能成为你无法提升自我编码规范和能力的借口一样。

对于分析，浏览器的行为包含一些ß启示。

因为每一个浏览器都希望能够比其他的浏览器更加优秀，他们往往都有自己一套对于源码的解析方式。有时，这些特性足够精妙以至于不露痕迹，但有时它们会全面爆发兼容性问题，大量的兼容代码和满腹牢骚的开发人员。这个悲剧的核心则是那些只是想知道有多少人在提交一个联系表单时会不填写某个特殊字段的数据分析人士。

``` javascript
// Classic example of browser compatibility checking
if (document.addEventListener) {
  // For real browsers
  document.addEventListener('click', handleClick);
} else {
  // For IE8 and earlier
  document.attachEvent('onclick', handleClick);
}
```

为了和这些在渲染过程中存在的问题做斗争，常规比较明智的做法是采用一个框架。传统处理做法，你可以会想到用[jQuery](http://jquery.com/).但如果你正在使用Google Tag Manager，你就已经在使用一个JavaScript框架了。通过借助GTM内在的特性，比如[自动时间追踪（auto-event tracking）](http://www.simoahava.com/analytics/auto-event-tracking-gtm-2-0/)和[标签模板（tag templates）](https://support.google.com/tagmanager/answer/2574372)，你正在将这些跨浏览器困境移交给Google Tag Manager。关注那些你希望使用到的能够横跨多种浏览器和设备的特性，这是这些框架所需要做的。

在有一些情况中，即使是最智能的浏览器也无法帮到你。这些情况被称作单点故障。这些问题的本质是当一个错误发生时，当前的执行环境被破坏了。如果你运行了一个全局分析追踪JavaScript代码段，一个单点句法错误将会破坏当前的脚步执行环境，代码将会执行失败。如果这样的情况发生了，而你的浏览器通常为了保持页面的友好而不会抛出任何的警告信息。所以，你需要设置一些调试代码，使JavaScript控制台能够及时的抛出信息，进行调试。

如果由于错打了一个引号或者遗忘了一个分号而导致对一个整个网站的追踪计划失败会是一件令人羞愧的事。同样，记得小心格式化引号。如果你以一种不正确的方式从一个文档（Microsoft Word）中进行复制粘贴，你可以会在JavaScript编译的时候遇到麻烦，因为在代码中所希望使用的是分格式化的引号。

Render过程的重点在于：

> 确保你的[标签是正确的](http://validator.w3.org/)，你的[JavaScript代码是无误的](http://jslint.com/).如果你的JavaScript因变得越来越复杂而难于管理，尝试使用一个可持续发展的框架，虽然这会带了一些小小的额外开支。

### <a name="race"></a>**The Race**

JavaScript是基于单线程的。也就是说任何时候只有一行JavaScript可以被执行。因此，JavaScript是会导致阻塞的。任何一行在页面模板中的JavaScript再被执行时，都会阻塞模板直到这一行代码执行完毕。

在异步JavaScript成为标准之前，我们倾向于在存在潜在风险的JavaScript放在文档的末尾，很底部的地方。这样就降低了由于JavaScript与浏览器的不一致而导致破坏整个页面的风险。同样的，对于很大的外部加载JS文件，也是放在页面的末尾。因为这些文件的加载和执行时按序列进行的，着就意味着一些严重的页面阻塞可能会发生。

现在好了，我们可以使用异步的JavaScript。异步JavaScript的核心是在于回调，一些方法会在一些动作被处理时调用，通常是一次网络请求。例如：如果你的脚步执行了一个HTTP请求，异步JavaScript会开始请求，但立刻会执行页面模板中的下一行代码。请求被处理之后，会在事件队列中插入和回调方法相关的一个状态，在下一次事件队列被检查的时候，回调方法会被立刻执行。这种方式代码不会阻塞页面的加载，如同在后台运行一样。只有被原方法调用，回调方法才会被执行。

另一个你将会看到异步请求的地方是对外部JS库的加载。Google Analytics 和 Google Tag Manager 都是以异步的方式被加载的。这就意味着请求加载这些JavaScript的请求是异步发送的，当这些文件被加载好并可用后，文件中的代码才会被执行（执行状态才会出现在事件队列中）。通过这种方式，你可以在页面的顶部加载这些外部库，确保它们的家在过程能够竟可能快的开始，但却不用担心存在阻塞其他代码的风险。

而异步JavaScript的缺点就在于无法预测一个给予脚本什么时候执行结束。只有当回调方法被调用的时候，我们才知道它执行结束了，我们无法精确这个时刻在页面加载序列之前。

这样的特性导致了一些叫做竞争条件的情况。竞争条件发生在两个脚本都在进行异步加载，而后一个脚本执行需要等待前一个脚本结束。在GA中，有一个事件追踪的例子：你不希望在浏览量匹配完成之前发送某个事件匹配，因为否则就会以异常会话结束。

![](/images/gallery/2015/12/25/landing-page-not-set.png)

如果竞争条件存在，它不会管事件匹配是否在浏览量匹配之后执行，它只管这个事件能够首先完成，因为一旦异步的执行开始之后，我们将无法控制。

对于GA和GTM来说，认识到在你页面中潜在的竞争条件是重要的。如果想提升浏览量匹配其他后续匹配的概率，你可以尝试例如将所有的其他匹配至至少在页面的DOM加载完成之后被触发。这种安全的方式通常会增加一点点的额外的执行代码去等待DOMComplete的触发，这样往往会使浏览量匹配会有更多时间先于其他后续匹配触发之前完成。

确保异步脚本能够按照顺序加载，避免竞争条件的方法是使用一个成功匹配的回调方法作为后续匹配的触发器。所以在你的浏览量匹配中，一旦对应的标签触发成功，你可以使用[hitCallback](https://developers.google.com/analytics/devguides/collection/analyticsjs/advanced#hitCallback)参数定义将发生什么。在这个回调方法中，你可以让其他标签触发。Google Tag Manager通过[用hitCallback](http://www.simoahava.com/analytics/macro-magic-google-tag-manager/#8)和`dataLayer`使定制序列变得简单。

理解竞争条件对于电子商务同样非常重要，特别是当你在客户端通过HTTP请求获取事务数据时，通常是一个事务标签在事务数据可用之前被执行。

Race的重点在于：

> 定制好代码块之间的依赖，消除竞争条件通过使用回调，并且竟可能多的使用异步加载。

### <a name="interaction"></a>**The Interaction**

页面加载具有少量的趣味性，但却并不是你通过网站分析所关心的，尽管你应该有所了解。当你真正感兴趣的是如何追踪在站点中的交互。

交互是一个加载术语，因为通常很难去精确定位我们想要的意思。一个点击当然是一个交互，但一次滚动呢，鼠标的移动呢，这些事交互吗？用户的视线移动到网站的不同部分呢？能够确信这不是交互吗？当然，它可能也是。

这取决于你想要追踪什么。在实现一个数据收集的系统中，你也许正面临着一大堆的问题。如果你没有意识到可能存在与其他JavaScript监听存在冲突，一些好比点击事件的监听都会变得难以实现。

这个冲突的解决方案覆盖了75%与Google Tag Manager有关的最常文档问题。详情参见：

[为什么我的GTM监听不工作](http://www.simoahava.com/analytics/dont-gtm-listeners-work/)

此处的关键是理解文档对象模型是如何工作的，特别是在事件委派的情况中。最简洁的做法当然是在页面中只用一个监听器，然后分派事件通过事件委托。然而，如果页面中存在有冲突的脚步，这样的做法可能不起作用，你需要采用其他的解决方案。下面是一个简单的问题情况：

![](/images/gallery/2015/12/25/event-propagation.gif)

上图展示了`submit()`事件是如何开始在文档树中向上传递。一旦在`document`节点中到达GTM的监听器，[自动事件追踪](http://www.simoahava.com/analytics/auto-event-tracking-gtm-2-0/)参与进来。同样也展示了当在事件对象中一个侵犯式的`stopPropagation()`被调用时的情况。

但这是关于GTM。当你用传统的GA进行追踪时，出于简单考虑，你也可以使用jQuery进行跨浏览器的事件处理。问题是，你越利用不同的框架，更可能有一些事情出差错。

对于交互追踪，与你的前端开发人员建立讨论是非常重要的。如果你开始向一个运行的不好已有脚步基础上进行新代码的注入，你可能会遇到比数据污染更加严重的问题。你的表单可能不在工作，应该只是用于改变动态内容的页卡项重定向到一个完全不同的页面，页面元素烦人的闪烁，等等。

Interaction的重点在于：

> 只追踪你认为是值得追踪的，时常彻底的测试冲突。

## **总结**

我并不喜欢只是在文章中泛泛而谈，但到目前为止已经超过了3000个字，如果你看到现在，那真是十分感谢。

分析方法是包罗万象的。它可以渗透到每一个你使用的每一个平台中，从各式各样的信息源中进行数据汇总，集中定位。然而，为了使你的分析最大化，我决定相信对于页面加载过程有所理解是很重要的，即使只是在很上层。这篇文章只是抛装引玉，如果你想要了解更多，你可以访问以下资源：

- The Request: [How Browsers Work](http://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)
- The Request: [Google Analytics And 301/302 Redirects](http://www.atlantaanalytics.com/practicing-web-analytics/how-does-google-analytics-handle-301-and-302-redirects/)
- The Render: [Optimizing Content For Different Browsers](https://www.w3.org/community/webed/wiki/Optimizing_content_for_different_browsers:_the_RIGHT_way)
- The Render: [QuirksMode: Compatibility overview](http://www.quirksmode.org/compatibility.html)
- The Race: [Asynchronous JavaScript Programming](http://www.html5rocks.com/en/tutorials/async/deferred/)
- The Race: [#GTMTips: hitCallback And eventCallback](http://www.simoahava.com/gtm-tips/hitcallback-eventcallback/)
- The Interaction: [Custom Event Listener for GTM](http://www.simoahava.com/analytics/custom-event-listeners-gtm/)
- The Interaction: [Google Tag Manager Events Using HTML5 Data Attributes](http://www.swellpath.com/2014/08/google-tag-manager-events-using-html5-data-attributes/)

相关引用文章：


1. [Page Load Time In Universal Analytics](http://www.simoahava.com/analytics/page-load-time-universal-analytics/)
2. [Check If Google Analytics Is In The Page Template](http://www.simoahava.com/analytics/check-if-google-analytics-is-in-page-template/)
3. [Leverage useBeacon And beforeunload In Google Analytics](http://www.simoahava.com/analytics/leverage-usebeacon-beforeunload-google-analytics/)
4. [Send Weather Data To Google Analytics In GTM V2](http://www.simoahava.com/analytics/send-weather-data-to-google-analytics-in-gtm-v2/)