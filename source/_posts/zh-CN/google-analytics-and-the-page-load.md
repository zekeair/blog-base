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

　　发现一篇讲述浏览器页面加载以及Google分析等数据收集工具运行机制的极好的文章，原文先贴出来: **[Google Analytics And The Page Load](http://www.simoahava.com/analytics/google-analytics-page-load/)** ，自己看得有点生涩，现尝试将其翻译出来，加深理解。
　　
——————————————————————————————————————————————————————————————————————　
## Google分析与页面加载

如果你使用过[Google分析](http://www.google.com/analytics/)、[Google标签管理](https://tagmanager.google.com/)，或者任何基于Javascript的数据收集与分析平台的产品，你是否曾经好奇过他们到底是如何工作的？我能理解，你当然更加关心那些收集到的数据，但你是否认为一切关于这些工具的处理和运行都是理所当然？<!-- more -->

这个问题我思考了很长一段时间，因为我并不确定那些在工作中与这些平台息息相关的人们，是否真正的理解浏览器与页面之间是如何交互的。

当然这些都无关紧要，但确实令我有一点点的吃惊。

因为如今关于网页分析的主导范式 (predominant paradigm) 都是围绕 Javascript 的，所以对于理解这一技术的薄弱环节非常的重要。而在 Javascript 参与进来之前，有一些你为了能获得更高质量的数据而必须遵守的关于页面加载过程的部分你需要了解。





