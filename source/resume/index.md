title: "resume"
i18n: true
layout: resume
comments: false
---
<p class="resume-language">[In English](en)</p>

<div class="resume-download"><strong>简历下载 -&gt; <a href="/assets/resume.pdf">PDF</a> | <a href="/assets/resume.docx">Docx</a></strong></div>

<h1 class="title">周裕翔</h1>

## 个人信息

<img src="/images/resume/QR.jpg" class="QR"/>

- 性别：男
- 学历：本科 | 重庆邮电大学 软件工程专业
- 工作年限：两年半
- 工作情况：在职
- 电子邮件：<a href="mailto:zeke.zhou@foxmail.com" target="_self">zeke.zhou@foxmail.com</a> 
- 联系电话：18616132839
- 技术博客：[http://track.zekeair.xyz](http://track.zekeair.xyz)
- Github：[https://github.com/zekeair](https://github.com/zekeair)
- 期待职位：web全栈工程师，web前端工程师，见习产品经理

## 工作经历

- #### **上海摩鹰信息科技有限公司** | 软件工程师( 全栈，敏捷开发 ) | 2013.07 - 至今

	##### **CRM系统**

	客户关系管理系统。系统基于Java Spring MVC框架；后台存储数据库MySQL；Hibernate数据连接规范；web前端基于bootstrap框架；jQuery库。项目中我的定位是**设计和开发人员**。目前完成了整个项目需求的分析，原型结构设计和原型界面及流程的设计；角色表、权限表，权限关系树和数据共享策略的设计。权限关系树是一个可拓展权限体系结构，有了这个设计，对于权限种类和操作的管理提升到对象级别，大大提高了系统后期对于不同需求产生的新的权限类别和操作的可拓展性。这个项目仍处于开发之中。

	##### **上海外国语大学附属双语学校调查问卷系统**

	[系统访问](http://www.smemobiletech.com/survey/paper/1)。系统基于Java Spring MVC框架；后台存储数据库MySQL；Hibernate数据连接规范；web前端基于frozenUI框架；Zepto操作库。项目中我的定位是**项目总负责人，设计和开发人员**。完成了网页前端页面布局和动画设计，后台问卷模板关系表设计，数据分析平台系统对接和完成数据的导出等。

	##### **复旦大学管理学院移动教务系统**

	[系统访问](http://m.fdsm.fudan.edu.cn/wx/)。系统基于Java Spring MVC框架；后台存储数据库MySQL；Hibernate数据连接规范；web前端基于frozenUI框架；Zepto操作库。项目中我的定位是**项目总负责人，设计和开发人员**。负责整个项目的规划和整体设计，其他参与人员工作分配和安排；后台映射关系表，人员，角色和权限表的设计；统一认证用户中心的对接等。系统上线正式运行之后，发现在有的有权访问的学生登录完成之后会报无访问权限的错误（登录用户的访问权限是更具账号的角色和所在项目进行区分）。这个错误开始一直无法定位到原因，因为错误并不是能够每次重现。经过对其他同事业务代码的排查，发现了有几段插入的hql插入方法没有添加事物标签，由于移动教务系统的数据会在用户登录的时候根据位标识判断即时数据的有效性，无效时会和管理学院原教务系统同步；但管理学院的服务器并不稳定，访问有时会很慢，从而导致前端页面迟迟不能更新，进而用户发起新的request请求，重复对数据同步，而由于没有对更新操作添加事务，导致出错。此外，还优化了部分sql查询的数据检索。

	##### **上海新东方学校VIP课程销售系统**

	[系统访问](http://jw.sh.xdf.cn/xdfhd)。系统基于Java Spring MVC框架；后台存储数据库MySQL；Hibernate数据连接规范；前端为Hybrid IOS应用。项目中我的定位是**设计和开发人员**。负载后台人员，角色和权限表的设计，管理界面网页的设计，销售和课程数据的检索，前端显示页面的HTML配置等。

	此外还有，**[上海新东方学校企业培训系统](http://qp.sh.xdf.cn/xdfct/)**、**[复旦大学管理学院MBA项目微网站](http://events.fdsm.fudan.edu.cn/microsite/mba/pages/home.html)**、**复旦大学管理学院Android端APP应用**等其他项目。

- #### **上海奥美广告有限公司北京分公司** | freelancer + 实习生 | 2011.08 - 2011.09

	熟悉和学习了广告业务技术实现上的流程；基于Flash应用 ActionScript3.0编程的mini site广告宣传网站的搭建；前沿科技或概念与广告结合的尝试（交互体感广告 Xbox Kinect + Flash）。

- #### **境界（北京）网络技术股份有限公司** | 实习生 | 2011.07 - 2011.08

	游戏官网Flash动画效果和脚本的学习、制作；参与了部分前端代码仓库的搭建等。

## 技术栈清单

- Web开发：Java | NodeJS
- Web框架：Spring MVC
- 前端开发：JavaScript | Bootstrap | HTML5 | CSS Animation
- 前端工具：Bower | Less | Stylus
- 数据库相关：MySQL | Hibernate | Redis | MangoDB
- 版本管理、任务管理、文档和自动化部署工具：Git | Maven | Grunt | Gulp
- 系统工具：Vim | Shell

## 致谢

感谢您花时间阅读我的简历，期待能有机会和您共事。
