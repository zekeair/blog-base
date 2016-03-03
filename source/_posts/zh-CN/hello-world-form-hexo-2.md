title: Hello World——从Hexo开始(二)
comments: true
tags:
- Node
- Javascript
- Hexo
categories: 
- Skills
- Javascript
- Node
date: 2015-12-03 22:13:38
---

![](/images/gallery/2015/12/03/cover.jpg)

　　说说如何修改和定制化一个Hexo的主题。对于博客的主题，如果懒得重新设计或者自己对于设计的明锐性不够可以在Hexo的官方主题站中选择一款适合自己的主题进行修改和定制[https://hexo.io/themes/](https://hexo.io/themes/)。除此之外，在github上也可以找到很多非官网收录的主题。

　　我选择了[ppoffice](https://github.com/ppoffice)设计的[minos](http://blog.zhangruipeng.me/hexo-theme-minos/)主题。页面简洁，retro风格，明了纯粹。<!-- more -->

　　进入Minos主题的Github项目首页，fork一份到自己的账户下，进入自己账户下的fork过来的页面。点击setting，给项目改一个名字以区分Minos，因为不太可能pull request到原项目。这里我命名为haughty。
　　

![](/images/gallery/2015/12/03/name-setting.jpg)


　　将haughty主题通过git clone到博客目录的themes目录下

``` bash
$ cd themes
$ git clone https://github.com/zekeair/hexo-theme-haughty.git
```

　　修改主题前。先介绍一下整个主题的结构，详细信息参考[官方文档](https://hexo.io/docs/themes.html)

``` plain
.
├── _config.yml
├── languages
├── layout
├── scripts
└── source
```

### **_config.yml**

　　主题的配置文件，yaml结构，在之后的模板中，可以通过JS的特殊变量和方法调用其中的设置的变量和值。

#### **language**

　　这个文件目录是放置那行支持国际化(i18n)不同语言的显示信息。例如：英文叫做Blog，中文是博客，如果希望这个词实现i18n，只需要在language这个目录下创建两个yaml格式的文件，在两个文件中起一个同样名称的变量display，它的值对应为“博客”和“Blog”，然后在模板中通过display这个变量名就可以实现i18n。

#### **layout**

　　这个目录是存放主题模板的。模板一般就是在整个网站或者大多数页面都会显示的内容，为了避免重复设计，而单独放在一个或者多个文件中可以被多次加载使用。比如导航、文章的大纲显示等。

　　Hexo默认渲染swig格式的模板文件，而在Hexo的Plugin库中，也有EJS, Haml和 Jade格式模板的渲染插件，自行下载安装使用。Minos主题使用的就是EJS的模板文件。（无论是哪一种模板格式，其实都是基于 Javascript 的，熟悉了一个，其他的基本都会使用）

#### **scripts**

　　放在当前目录下的 Javascript 脚步都会在系统初始化的时候自动被加载.

#### **source**

　　与主题有关的所有样式文件，前端脚本和图片等资源都是放在这个目录。

　　除了直接的CSS文件，也可以防止CSS的预处理文件（如stylus和less），Hexo默认安装了渲染stylus的插件，如果使用less，自行下载安装相关插件。

#### **variables**

　　Hexo有很多的系统变量是能够在模板中直接被引用的，详情参考[官方文档](https://hexo.io/docs/variables.html)

### **haughty对原Minos主题修改的几个点：**

1. 导航栏上下padding的对称留白我觉得过于死板，我喜欢上部留白略多于下部，这样显得更加鲜活。

	定位到`haughty/source/css/_portal/header.styl`，修改`#header-outer`
	
	{% codeblock lang:stylus %}
#header-outer
  height: 100%
  position: relative
  margin-top: 15px
	{% endcodeblock %}

2. 文本显示排列不整齐，需要给p标签样式添加`text-align: justigy`，使每行的文字排列整体，避免出现参差不齐的现象。

	定位到`haughty/source/css/style.styl`，修改一个p标签样式；
	
	{% codeblock lang:stylus %}
p
  text-align: justify
	{% endcodeblock %}

3. 现在的pc的屏幕多数已经普及为宽屏，而一般文章的内容不会占满整个页面，尝试添加一个显示简介的panel版面（或者尝试借鉴其他的开源主题的panel页面）。

	在`haughty/layout/_partial/`目录下添加`brief-panel.ejs`，添加panel页面所需要模板	

	{% codeblock lang:html %}
<section id="brief-panel">
  <div class="panel-box">
    <div class="panel-avatar">
      <a href="/"></a>
    </div>
    <h1 class="panel-nickname"><a href="/" class="bottom-white-dotted"><%=(config.author?config.author:'Your Nickname')%></a></h1>
    <h3 class="panel-name"><a href="mailto:<%=(theme.email?theme.email:'someone@email.com')%>" class="bottom-white-dotted">@<%=(theme.name? theme.name:'Your Name')%></a></h3>
    <p class="panel-description">
      <%= __('description.panel_description')%>
    </p>
    <nav class="panel-about">
      【
      <a href="<%- url_for(theme.panel_menu.about) %>" class="panel-about-link"><%= __('gallery.about')%></a>,　
      <a href="<%- url_for(theme.panel_menu.books) %>" class="panel-about-link"><%= __('gallery.books')%></a>
       】
    </nav>
    <nav class="panel-contact">
      <a href="https://github.com/<%= (theme.github? theme.github:'')%>" class="panel-contact-link github"></a>
      <a href="https://twitter.com/<%= (theme.twitter? theme.twitter:'')%>" class="panel-contact-link twitter"></a>
      <a href="/atom.xml" class="panel-contact-link rss"></a>
    </nav>
  </div>
</section>
	{% endcodeblock %}

	在`haughty/source/css/_partial/`目录下添加`brief-panel.styl`，添加panel页面所需要的样式
	
	{% codeblock lang:stylus %}
#brief-panel
  position: fixed
  width: 350px
  left: 0
  top: 0
  bottom: 0
  background-color: #353535
  color: #fff
  z-index 999
  transition ease 0.5s
  @media mq-mobile
    position: relative
    width: 100%
    display none
  @media mq-large
    width 520px
  @media mq-panel-top
    left -350px

.panel-box
  position: absolute
  bottom: 0
  left 0
  right 0
  padding: 50px 10%
  text-align: center


.panel-avatar
  width:80px
  height:80px
  margin: 0 auto 20px
  border: 3px solid #fff
  overflow: hidden
  border-radius: 100%
  background-image:url(images/avatar.jpg)
  background-position: center
  background-size: 100%
  transition ease 0.5s
  @media mq-large
    width 100px
    height 100px
  a
    display:block
    width:100%
    height:100%

.panel-nickname
  font-size: 24px
  // font-weight bolder
  margin 10px 0
  @media mq-large
    font-size 30px
    margin 20px 0px

a.bottom-white-dotted
  text-decoration none
  border-bottom 1px dotted #fff
  color #fff

.panel-name
  font-size 20px
  // font-weight bolder
  margin 10px 0
  @media mq-large
    font-size 24px
    margin 15px 0px

.panel-description
  color #777
  text-align center
  margin 10px 0
  @media mq-large
    margin 15px 0px

.panel-about
  margin 10px 0
  @media mq-large
    margin 15px 0px

.panel-about-link
  font-size 18px
  color #864444
  text-decoration none
  @media mq-large
    font-size 20px

.panel-contact-link
  text-decoration none
  display inline-block
  padding 0 15px
  color #777
  font-size 30px
  font-family: font-icon
  @media mq-large
    font-size 45px
    padding 0 25px

.github::before
  content "\f092"

.twitter::before
  content "\f081"

.rss::before
  content "\f143"
	{% endcodeblock %}

4. 修改显示信息，比如站点的logo，显示等。

	**修改站点logo**

	可以在haughty目录下的`_config.yml`中的logo_text中添加你要的文字logo信息，但通过此法添加的文字没有首字母黑底白字的效果。黑底白字效果可以通过修改`haughty/layout/_partial/header.ejs`模板。对`id="logo"`的a标签修改如下：

	{% codeblock lang:html %}
<a class="logo<%=(theme.logo_text?' logo-text':'')%>" href="<%- url_for('/') %>" id="logo">
  <%
    var logoContent = theme.logo_text?theme.logo_text:'';
    if(logoContent.length){
      logoContent.split(' ').forEach(function(item, index, array){
        %>
    <span class="logo-initial"><%= item.charAt(0) %></span><%= item.substr(1)+' ' %><%
      });
    }
  %>
</a>
	{% endcodeblock %}

	再在`haughty/source/css/_partial/header.styl`修改`#logo`内容：

	{% codeblock lang:stylus %}
#logo
  float: left
  .logo-initial
    background-color: #000
    color: #fff
    padding: 0 5px
    margin-right: 0.5px
  @media mq-mobile
    float: none
    margin: 0 auto
    display: block
    background-position: center
    background-size: 85%
    text-align: center
	{% endcodeblock %}

	现在logo text中的字符每个单词的首字符呈现白底黑字的效果。但是这时的效果并不是最好的，因为在单倍行距中字符上下的间隔很难控制。如果比较挑剔，可以用Photoshop制作一张logo图，替换掉haughty/source/css/images/logo.png。

	**修改导航显示**

	使导航的各个项符合i18n，中文个性化显示。在`haughty/languages/`下的`zh-CN.yml`和`en.yml`添加显示信息。

	{% codeblock lang:yaml %}
nav:
  next: 'Next'
  prev: 'Prev'
article:
  comments: 'No Comments'
  contents: 'Contents'
  more: 'Read More'
menu:
  archives: 'Timeline'
  categories: 'Categories'
  tags: 'Tags'
  about: 'About'
gallery:
  about: 'About'
  books: 'Books'
search: 'Search'
about: 'About Me'
archives: 'Timeline'
category: 'category'
tag: 'tag'
description:
  panel_description: 'a boy becoming a man and trying to make difference'
	{% endcodeblock %}

	通过`__()`方法在`haughty/layout/_partial/`下的`header.ejs`和`brief-panel.ejs`模板中调用

5. 移动首页“阅读全文”和“评论”链接的位置。

	对`haughty/layout/partial/article.ejs`中`class="article-entry"`的div标签修改如下：
	
	{% codeblock lang:html %}
<div class="article-entry" itemprop="articleBody">
  <% if (post.excerpt && index){ %>
    <%- post.excerpt %>
    <p class="article-more-link">
      <a href="<%- url_for(post.path) %>#more"><%= __('article.more') %></a>
      <% if (post.comments && config.disqus_shortname){ %>
        <a href="<%- post.permalink %>#disqus_thread" class="article-comment-link"><%= __('article.comments') %></a>
      <% } %>
    </p>
  <% } else { %>
    <%- post.content %>
  <% } %>
</div>
	{% endcodeblock %}

　　haughty差不多就修改好了。以上就是关于修改Hexo主题的全部内容。如果你对我修改的haughty主题感兴趣，欢迎[fork](https://github.com/zekeair/hexo-theme-haughty)并进一步创造修改。下篇文章将介绍Hexo的发布到github，域名设置，sitemap等内容。





　　
　　