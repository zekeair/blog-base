title: Hello World——从Hexo开始(一)
date: 2015-12-02 19:23:18
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

![](/images/gallery/2015/12/02/cover.jpg)

　　先来说说本博客系统的基础引擎Hexo。在写这篇文章的时候，我也是第一次接触Hexo。记录一下其中遇到问题和经验，希望能够帮助到一些初识Hexo的朋友。本文会介绍一些Hexo的基础配置和使用，而比较高阶的比如模板和主题的定制化等，在后续文章中会详细介绍。<!-- more -->
　　
　　首先，是安装Hexo(我假设你对于Node以及npm已经有了初步的了解).对于Hexo的介绍和了解，请问Hexo的官网页面[https://hexo.io/](https://hexo.io/)。

``` bash
$ npm install hexo-cli -g
```

　　初始化一个安放博客文件目录。

``` bash
$ hexo init blog && cd blog 
```

{% note warn 注意 %}
如果你使用的系统是Mac OX S且版本大于10.9，在运行以上命令时可以会报一些模块无法找到错误。请用 `$ hexo init blog --no-optional && cd blog ` 替换，详情参考[issue:#1597](https://github.com/hexojs/hexo/issues/1597)
{% endnote %}

　　此时已经包含了Hexo所需要的所有依赖包的package.json文件，安装必要依赖。

``` bash
$ npm install
```

　　从Hexo 3开始，server模块已经从主模块中分离，但在实际使用情况，还是必要的。单独安装并运行server模块。

``` bash
$ npm install hexo-server --save
$ hexo server
```

　　此时的博客框架已经搭建好了，并运行在[localhost:4000](http://localhost:4000)。关于系统更详细配置，目录更改等内容，请参照Hexo官方文档[https://hexo.io/docs/](https://hexo.io/docs/)。
　　
### Tips:

- Read More/显示详情：对于一些内容较长的博文，往往希望只在展示页显示一部分，通过点击相关链接显示全部内容。那么记得在Post中添加`<!-- more -->`。其后的内容将不显示在展示页。

- 文章中的图片存放位置：在blog根目录下的source目录下创建一个images目录存放图片，不同的文章可以通过时间或者标题进行划分管理。在文章中只需要通过`![](/images/title/picture.jpg)`即可以访问。Hexo提供了两外一种方式，按照不同的Post的title创建一个目录存放图片（需要在_config.yml中配置`post_asset_folder: true
`）,然后通过文件的名字即可访问。个人觉得这种方式是通过相对路径访问，在展示页图片显示异常，同时不利于后期文件的整理和管理。固不推荐第二种方式。

- 对post的layout的配置：默认的post的layout只有很少的内容。建议将评论和目录的占位符一并添加。当然，也可以创建一个新的layout。

	``` yaml
	title: {{ title }}
	date: {{ date }}
	comments: true
	tags:
	categories:
	---
	```

　　现在可以开始写博文了，只要熟悉[Markdown](http://daringfireball.net/projects/markdown/)即可。关于更多的Hexo配置和定制化会在以后的文章中描述。下篇文章将会说说如何定制化一个Hexo的主题。
