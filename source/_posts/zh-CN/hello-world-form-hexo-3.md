title: Hello World——从Hexo开始(三)
date: 2015-12-07 22:19:40
comments: true
tags:
- Node
- Javascript
- Hexo
categories: 
- Skills
- Javascript
- Node
---

![](/images/gallery/2015/12/07/cover.jpg)

　　在完成了主题的修改之后，基本上的博客功能和形式差不多就完成了。剩下的都是一些比较琐碎的事，比如部署和发布等。本站是部署在github上的。

### **添加RSS**

　　Hexo的Plugins库中就有RSS的生成插件,安装到本地

``` bash
npm install hexo-generator-feed --save
```

　　在`_config.yml`中配置<!-- more -->

``` yaml
# Feed
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
```
　　在`haughty/layout/_partial/brief-panel.ejs`中添加一个获取`/atom.xml`的链接。

### **添加sitemap**
　　
　　为了方便相关的搜索引擎能够爬到你的站点并收录，添加完整的sitemap文件会比较有效，当然你最好是能够自己去向其提交sitemap，加速站点被搜索引擎收录。

　　Hexo的Plugins库中就有sitemap的生成插件,安装到本地

``` bash
npm install hexo-generator-sitemap --save
```

　　在`_config.yml`中配置

``` yaml
# Sitemap
sitemap: 
  path: sitemap.xml
```

### **添加分析统计引擎**

　　这里说明如何添加Google Analytics。注册使用账号，创建媒体资源。根据你网站的域名（后面会说明如何绑定域名），生成一个媒体资源的UID。将这个媒体资源添UID添加到`haughty/_config.yml`的`google_analytics`字段中。

　　设置好之后，可能需要过一段时间之后才会有数据出现。

### **添加评论系统**

　　haughty主题默认支持disqus，[注册](https://disqus.com/)一个disqus的账号在`haughty/_config.yml`中打开

``` ymal
comment_provider: disqus
```

### **Hexo的生成和部署**

　　通过Hexo生成从所有的模板和预处理文件生成静态文件

``` bash
$ hexo generate
```

　　Hexo将静态文件部署到远程服务器中

``` bash
$ hexo deploy
```

　　Hexo支持将静态文件部署到几个主力的远程服务器上，详情参考[官网文档](https://hexo.io/docs/deployment.html)。只需要在`_config.yml`中进行配置即可。

``` yaml
deploy:
  type: git
  repo: 
  branch: master
  message:
```

{% note warn 注意 %}
如果是使用github作为博客的托管服务站点，请先参考[github page](https://pages.github.com/)做好配置
{% endnote %}

### **域名配置**

　　在域名提供商管理页面做要域名与服务器ip的映射，或者跳转（如果托管在github上的网站，需要定向域名跳转到对应的github域名地址）。

![](/images/gallery/2015/12/07/domain-setting.png)

　　以阿里云域名为例进行说明。在域名解析页面中，如果是创建域名对ip的映射，创建A记录；如果是创建对域名的重定向和跳转，创建CNAME记录。主机记录是在你当前域名下的主机分类。比如我现在的域名是track.zekeair.xyz；track就是主机记录；www也是主机记录。也可以理解为子级域名。

　　如果是使用github托管，在博客目录的source目录下创建CNAME文件，记录你的域名地址。