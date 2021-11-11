# TODO List

总结思路

做了什么事情
遇到什么问题
通过什么方式解决
得到了什么结果
接下来要做的

数据说明

数据说话，性能监控体系的搭建

上班前 10 分钟例行查看核心数据，下班前 15 分钟查看用户反馈。

开发APP的通用问题和对应的解决方案
1. 流程
2. 通用组件
3. 监控（内存，启动速度，卡顿）
4. 性能优化

进步，不外乎是一个解决一个接一个问题

应用开发规约

1. 开发环境工具统一
2. Gladle版本统一
3. 引用库统一
4. 代码下沉统一
5. 

执行adb命令收集信息

多少个进程
多少个线程
多少个服务
多少个模块

终端智能，在终端有限的资源条件下，内存，CPU，电量，网络，如何调度和分配资源，根据用户习惯制定策略，甚至预测用户行为，从而更好的响应和服务用户，提高用户体验。

最大的自由是限制下的自由
整体团队的约束，统一遵守规则下的自由发挥

每一个方法决定有快慢

适用性，可维护性

### NDK 开发

### 相机应用

各种方式优缺点

## 一个最优雅的抢红包App
### 基本功能：
1. 打开后可以监听当前群聊天进行抢红包；
2. 抢完可以自动关闭；
3. 可以监听通知栏抢红包；
4. 可以自动回复；
5. 可以自定义回复；

### 需要素材：
1. App Name 
2. App Icon
3. App PackageName

### 发布市场
1. Google Play
2. 小米市场
3. 华为市场
4. Vivo市场
5. PP市场
6. 豌豆荚市场

### 灰度用户
1. 微信、朋友圈
2. QQ群
3. 技术圈

### 用到的技术
1. 自动抢红包AccessiblyService
2. 实现微信自动向附近的人打招呼 http://blog.csdn.net/dovar_66/article/details/52640929

波利亚用三本书：《How To Solve It》、《数学的发现》、《数学与猜想》）来试图阐明人类解决问题的一般性的思维方法，总结起来主要有以下几种：

* 时刻不忘未知量。即时刻别忘记你到底想要求什么，问题是什么。（动态规划中问题状态的设定）
* 试错。对题目这里捅捅那里捣捣，用上所有的已知量，或使用所有你想到的操作手法，尝试着看看能不能得到有用的结论，能不能离答案近一步（回溯算法中走不通就回退）。
* 求解一个类似的题目。类似的题目也许有类似的结构，类似的性质，类似的解方案。通过考察或回忆一个类似的题目是如何解决的，也许就能够借用一些重要的点子（比较 Ugly Number 的三个题目：263. Ugly Number， 264. Ugly Number II， 313. Super Ugly Number）。
* 用特例启发思考。通过考虑一个合适的特例，可以方便我们快速寻找出一般问题的解。
* 反过来推导。对于许多题目而言，其要求的结论本身就隐藏了推论，不管这个推论是充分的还是必要的，都很可能对解题有帮助。


## Think

如果不是你想要的，就代理一层。

持续交互可执行软件。

设计模式解决代码重复，降低负责度，提高可读性，便于维护

函数的调用设计多个参数的传递，解决的方式可以重载方法，可以通过建造者模式链式构造。可读性和使用性大大增强。


截屏：
本发明涉及一种实现应用截屏的方法及装置，其方法包括：接收截屏操作指令，获取截屏操作指令对应的目标应用程序，在目标应用程序的进程中注入截屏代码；调用截屏代码，对目标应用程序的当前进程进行截屏操作。本发明通过使用跨进程注入截屏代码的方式(截屏代码未使用系统api进行截屏)，可以有效避免android碎片化造成移植适配手机中截屏差异化问题，保证截屏功能在移植适配手机中能够正确的使用，避免出现截屏内容是黑屏，或者是手机的开机画面，且截屏画面包含通知栏和底部操作栏的情形，从而提高了移动终端应用使用过程中截屏操作的成功率和系统测试有效性。

查询网站：
http://so.baiten.cn/

http://www2.soopat.com/Home/


### More 


自由的创作，肆意的挥洒

文字的国度里面自由翱翔。

简洁高效的写作和文字编辑能力可以带来简洁高效的代码，设计，邮件，沟通，即时信息以及更多。

条理清晰的头脑。

读Volley的源码时就感觉酣畅淋漓，并且对Volley的架构设计和代码质量深感佩服。读Glide的源码时却让我相当痛苦，代码极其难懂。当然这里我并不是说Glide的代码写得不好，只是因为Glide和复杂程度和Volley完全不是在一个量级上的。

简单概括就是八个字：抽丝剥茧、点到即止。应该认准一个功能点，然后去分析这个功能点是如何实现的。但只要去追寻主体的实现逻辑即可，千万不要试图去搞懂每一行代码都是什么意思，那样很容易会陷入到思维黑洞当中，而且越陷越深。因为这些庞大的系统都不是由一个人写出来的，每一行代码都想搞明白，就会感觉自己是在盲人摸象，永远也研究不透。如果只是去分析主体的实现逻辑，那么就有比较明确的目的性，这样阅读源码会更加轻松，也更加有成效

通用客户端基础技术的研发和优化，包括但不限于插件热修、监控体系、性能优化、打包平台、跨平台技术，底层技术，基础叫架构以及各类基础库，中间件。

自发组织，解决实际问题，参与讨论，改善现状


免费的编程中文书籍索引
============================

[![](https://img.shields.io/github/issues/justjavac/free-programming-books-zh_CN.svg)](https://github.com/justjavac/free-programming-books-zh_CN/issues) 
[![](https://img.shields.io/github/forks/justjavac/free-programming-books-zh_CN.svg)](https://github.com/justjavac/free-programming-books-zh_CN/network) [![](https://img.shields.io/github/stars/justjavac/free-programming-books-zh_CN.svg)](https://github.com/justjavac/free-programming-books-zh_CN/stargazers) [![](https://travis-ci.org/justjavac/free-programming-books-zh_CN.svg?branch=master)](https://travis-ci.org/justjavac/free-programming-books-zh_CN) [![](https://img.shields.io/github/release/justjavac/free-programming-books-zh_CN.svg)](https://github.com/justjavac/free-programming-books-zh_CN/releases)

免费的编程中文书籍索引，欢迎投稿。

- 国外程序员在 [stackoverflow](http://stackoverflow.com/questions/1711/what-is-the-single-most-influential-book-every-programmer-should-read/1713%231713) 推荐的程序员必读书籍，[中文版](http://justjavac.com/other/2012/05/15/qualified-programmer-should-read-what-books.html "一个合格的程序员应该读过哪些书")。
- [stackoverflow](http://stackoverflow.com/questions/38210/what-non-programming-books-should-programmers-read) 上的程序员应该阅读的非编程类书籍有哪些？ [中文版](what-non-programming-books-should-programmers-read.md)
- [github](https://github.com/vhf/free-programming-books) 上的一个流行的编程书籍索引  [中文版](https://github.com/vhf/free-programming-books/blob/master/free-programming-books-zh.md)

如果这个仓库对你有帮助，欢迎 star。如果这个仓库帮你提升了技能找到了工作，可以请我喝杯咖啡：

<p align="center"><img src="https://cdn.devtips.cn/buy-me-a-coffee-wechat.png?imageView2/2/w/640/interlace/1" width="320" height="320" alt="" /></p>

## 参与交流

欢迎大家将珍藏已久的经典免费书籍共享出来，您可以：

* 使用 [Pull Request](https://github.com/justjavac/free-programming-books-zh_CN/pulls) 提交

如果你发现了不能访问的链接，也可以提 PR，在无法访问链接的后面增加 `:worried:`。

贡献者名单: https://github.com/justjavac/free-programming-books-zh_CN/graphs/contributors

## 目录

* 语言无关类
  * [操作系统](#操作系统)
  * [智能系统](#智能系统)
  * [分布式系统](#分布式系统)
  * [编译原理](#编译原理)
  * [函数式概念](#函数式概念)
  * [计算机图形学](#计算机图形学)
  * [WEB服务器](#web服务器)
  * [版本控制](#版本控制)
  * [编辑器](#编辑器)
  * [NoSQL](#nosql)
  * [PostgreSQL](#postgresql)
  * [MySQL](#mysql)
  * [管理和监控](#管理和监控)
  * [项目相关](#项目相关)
  * [设计模式](#设计模式)
  * [Web](#web)
  * [大数据](#大数据)
  * [编程艺术](#编程艺术)
  * [其它](#其它)

* 语言相关类
  * [Android](#android)
  * [APP](#app)
  * [AWK](#awk)
  * [C/C++](#cc)
  * [C#](#c)
  * [Clojure](#clojure)
  * [CSS/HTML](#csshtml)
  * [Dart](#dart)
  * [Elixir](#elixir)
  * [Erlang](#erlang)
  * [Fortran](#fortran)
  * [Go](#go)
  * [Groovy](#groovy)
  * [Haskell](#haskell)
  * [iOS](#ios)
  * [Java](#java)
  * [JavaScript](#javascript)
  * [Kotlin](#kotlin)
  * [LaTeX](#latex)
  * [LISP](#lisp)
  * [Lua](#lua)
  * [OCaml](#OCaml)
  * [Perl](#perl)
  * [PHP](#php)
  * [Prolog](#prolog)
  * [Python](#python)
  * [R](#r)
  * [Ruby](#ruby)
  * [Rust](#rust)
  * [Scala](#scala)
  * [Shell](#shell)
  * [Swift](#swift)

* [读书笔记及其它](#读书笔记及其它)
* [测试相关](#测试相关)

## 置顶

- [[笔记]前端工程师的入门与进阶](https://shenbao.github.io/2017/04/22/justjavac-live/) :100:
- [[全文]如何正确的学习 Node.js](https://github.com/i5ting/How-to-learn-node-correctly) :100:

## 操作系统

* [开源世界旅行手册](http://i.linuxtoy.org/docs/guide/index.html)
* [鸟哥的Linux私房菜](http://linux.vbird.org/)
* [The Linux Command Line](http://billie66.github.io/TLCL/index.html) (中英文版)
* [Linux 设备驱动](http://oss.org.cn/kernel-book/ldd3/index.html) (第三版)
* [深入分析Linux内核源码](http://www.kerneltravel.net/kernel-book/%E6%B7%B1%E5%85%A5%E5%88%86%E6%9E%90Linux%E5%86%85%E6%A0%B8%E6%BA%90%E7%A0%81.html) :worried:
* [UNIX TOOLBOX](http://cb.vu/unixtoolbox_zh_CN.xhtml)
* [Docker中文指南](https://github.com/widuu/chinese_docker)
* [Docker —— 从入门到实践](https://github.com/yeasy/docker_practice)
* [Docker入门实战](http://yuedu.baidu.com/ebook/d817967416fc700abb68fca1)
* [Docker Cheat Sheet](https://github.com/wsargent/docker-cheat-sheet/tree/master/zh-cn#docker-cheat-sheet)
* [FreeRADIUS新手入门](http://freeradius.akagi201.org) :worried:
* [Mac 开发配置手册](https://aaaaaashu.gitbooks.io/mac-dev-setup/content/)
* [FreeBSD 使用手册](https://www.freebsd.org/doc/zh_CN/books/handbook/index.html)
* [Linux 命令行(中文版)](http://billie66.github.io/TLCL/book/)
* [Linux 构建指南](http://works.jinbuguo.com/lfs/lfs62/index.html)
* [Linux工具快速教程](https://github.com/me115/linuxtools_rst)
* [Linux Documentation (中文版)](https://www.gitbook.com/book/tinylab/linux-doc/details)
* [嵌入式 Linux 知识库 (eLinux.org 中文版)](https://www.gitbook.com/book/tinylab/elinux/details)
* [理解Linux进程](https://github.com/tobegit3hub/understand_linux_process)
* [命令行的艺术](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)
* [SystemTap新手指南](https://spacewander.gitbooks.io/systemtapbeginnersguide_zh/content/index.html)
* [操作系统思考](https://github.com/wizardforcel/think-os-zh)

[返回目录](#目录)

## 智能系统
* [一步步搭建物联网系统](https://github.com/phodal/designiot)

[返回目录](#目录)

## 分布式系统
* [走向分布式](http://dcaoyuan.github.io/papers/pdfs/Scalability.pdf)

[返回目录](#目录)

## 编译原理
* [《计算机程序的结构和解释》公开课 翻译项目](https://github.com/DeathKing/Learning-SICP)

[返回目录](#目录)

## 函数式概念
* [傻瓜函数编程](https://github.com/justinyhuang/Functional-Programming-For-The-Rest-of-Us-Cn)

[返回目录](#目录)

## 计算机图形学
* [OpenGL 教程](https://github.com/zilongshanren/opengl-tutorials)
* [WebGL自学网](http://html5.iii.org.tw/course/webgl/) :worried:
* [《Real-Time Rendering 3rd》提炼总结](https://github.com/QianMo/Real-Time-Rendering-3rd-Summary-Ebook)

[返回目录](#目录)

## WEB服务器

* [Nginx开发从入门到精通](http://tengine.taobao.org/book/index.html) (淘宝团队出品)
* [Nginx教程从入门到精通](http://www.ttlsa.com/nginx/nginx-stu-pdf/)(PDF版本，运维生存时间出品)
* [OpenResty最佳实践](https://www.gitbook.com/book/moonbingbing/openresty-best-practices/details)
* [Apache 中文手册](http://works.jinbuguo.com/apache/menu22/index.html)

[返回目录](#目录)

## 版本控制

* [Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) （本文由 [廖雪峰](http://www.liaoxuefeng.com) 创作，如果觉得本教程对您有帮助，可以去 [iTunes](https://itunes.apple.com/cn/app/git-jiao-cheng/id876420437) 购买）
* [git - 简易指南](http://rogerdudler.github.io/git-guide/index.zh.html)
* [猴子都能懂的GIT入门](http://backlogtool.com/git-guide/cn/)
* [Git 参考手册](http://gitref.justjavac.com)
* [Pro Git](http://git-scm.com/book/zh/v2)
* [Pro Git 中文版](https://www.gitbook.com/book/0532/progit/details) (整理在gitbook上)
* [Git Magic](http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/)
* [GotGitHub](http://www.worldhello.net/gotgithub/index.html)
* [Git权威指南](http://www.worldhello.net/gotgit/)
* [Git Community Book 中文版](http://gitbook.liuhui998.com/index.html)
* [Mercurial 使用教程](https://www.mercurial-scm.org/wiki/ChineseTutorial)
* [HgInit (中文版)](http://bucunzai.net/hginit/)
* [沉浸式学 Git](http://igit.linuxtoy.org)
* [Git-Cheat-Sheet](https://github.com/flyhigher139/Git-Cheat-Sheet) （感谢 @flyhigher139 翻译了中文版）
* [GitHub秘籍](https://snowdream86.gitbooks.io/github-cheat-sheet/content/zh/index.html)
* [GitHub帮助文档](https://github.com/waylau/github-help)
* [git-flow 备忘清单](http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
* [svn 手册](http://svnbook.red-bean.com/nightly/zh/index.html)
* [GitHub漫游指南](https://github.com/phodal/github-roam)

[返回目录](#目录)

## 编辑器

* [exvim--vim 改良成IDE项目](http://exvim.github.io/docs-zh/intro/)
* [笨方法学Vimscript 中译本](http://learnvimscriptthehardway.onefloweroneworld.com/) :worried:
* [Vim中文文档](https://github.com/vimcn/vimcdoc)
* [所需即所获：像 IDE 一样使用 vim](https://github.com/yangyangwithgnu/use_vim_as_ide)
* [vim 实操教程](https://github.com/dofy/learn-vim)
* [Atom飞行手册中文版](https://github.com/wizardforcel/atom-flight-manual-zh-cn)
* [Markdown·简单的世界](https://github.com/wizardforcel/markdown-simple-world)
* [一年成为 Emacs 高手](https://github.com/redguardtoo/mastering-emacs-in-one-year-guide/blob/master/guide-zh.org)
* [Emacs 生存指南](http://lifegoo.pluskid.org/upload/blog/152/Survive.in.Emacs.pdf)
* [Atom官方手册](https://atom-china.org/t/atom/62)

[返回目录](#目录)

## NoSQL

* [NoSQL数据库笔谈](http://old.sebug.net/paper/databases/nosql/Nosql.html)
* [Redis 设计与实现](http://redisbook.com/)
* [Redis 命令参考](http://redisdoc.com/)
* [带有详细注释的 Redis 3.0 代码](https://github.com/huangz1990/redis-3.0-annotated)
* [带有详细注释的 Redis 2.6 代码](https://github.com/huangz1990/annotated_redis_source)
* [The Little MongoDB Book](https://github.com/justinyhuang/the-little-mongodb-book-cn/blob/master/mongodb.md)
* [The Little Redis Book](https://github.com/JasonLai256/the-little-redis-book/blob/master/cn/redis.md)
* [Neo4j 简体中文手册 v1.8](http://docs.neo4j.org.cn/)
* [Neo4j .rb 中文資源](http://neo4j.tw/)
* [Disque 使用教程](http://disquebook.com)
* [Apache Spark 设计与实现](https://github.com/JerryLead/SparkInternals/tree/master/markdown)

[返回目录](#目录)

## PostgreSQL

* [PostgreSQL 8.2.3 中文文档](http://works.jinbuguo.com/postgresql/menu823/index.html)
* [PostgreSQL 9.3.1 中文文档](http://www.postgres.cn/docs/9.3/index.html)
* [PostgreSQL 9.5.3 中文文档](http://www.postgres.cn/docs/9.5/index.html)

[返回目录](#目录)

## MySQL

* [MySQL索引背后的数据结构及算法原理](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)
* [21分钟MySQL入门教程](http://www.cnblogs.com/mr-wid/archive/2013/05/09/3068229.html)

[返回目录](#目录)

## 管理和监控

* [ELKstack 中文指南](http://kibana.logstash.es)
* [Mastering Elasticsearch(中文版)](http://udn.yyuap.com/doc/mastering-elasticsearch/)
* [ElasticSearch 权威指南](https://www.gitbook.com/book/fuxiaopang/learnelasticsearch/details)
* [Elasticsearch 权威指南（中文版）](http://es.xiaoleilu.com)
* [Logstash 最佳实践](https://github.com/chenryn/logstash-best-practice-cn)
* [Puppet 2.7 Cookbook 中文版](http://bbs.konotes.org/workdoc/puppet-27/)

[返回目录](#目录)

## 项目相关

* [持续集成（第二版）](http://article.yeeyan.org/view/2251/94882) (译言网)
* [让开发自动化系列专栏](http://www.ibm.com/developerworks/cn/java/j-ap/)
* [追求代码质量](http://www.ibm.com/developerworks/cn/java/j-cq/)
* [selenium 中文文档](https://github.com/fool2fish/selenium-doc)
* [Selenium Webdriver 简易教程](http://it-ebooks.flygon.net/selenium-simple-tutorial/)
* [Joel谈软件](http://local.joelonsoftware.com/wiki/Chinese_\(Simplified\))
* [約耳談軟體(Joel on Software)](http://local.joelonsoftware.com/wiki/%E9%A6%96%E9%A0%81)
* [Gradle 2 用户指南](https://github.com/waylau/Gradle-2-User-Guide)
* [Gradle 中文使用文档](http://yuedu.baidu.com/ebook/f23af265998fcc22bcd10da2)
* [编码规范](https://github.com/ecomfe/spec)
* [开源软件架构](http://www.ituring.com.cn/book/1143)
* [GNU make 指南](http://docs.huihoo.com/gnu/linux/gmake.html)
* [GNU make 中文手册](http://www.yayu.org/book/gnu_make/)
* [The Twelve-Factor App](http://12factor.net/zh_cn/)

[返回目录](#目录)

## 设计模式

* [图说设计模式](https://github.com/me115/design_patterns)
* [史上最全设计模式导学目录](http://blog.csdn.net/lovelion/article/details/17517213)
* [design pattern 包教不包会](https://github.com/AlfredTheBest/Design-Pattern)
* [设计模式 Java 版](https://quanke.gitbooks.io/design-pattern-java/content/)

[返回目录](#目录)

## Web

* [关于浏览器和网络的 20 项须知](http://www.20thingsilearned.com/zh-CN/home)
* [浏览器开发工具的秘密](http://jinlong.github.io/2013/08/29/devtoolsecrets/)
* [Chrome 开发者工具中文手册](https://github.com/CN-Chrome-DevTools/CN-Chrome-DevTools)
* [Chrome扩展开发文档](http://open.chrome.360.cn/extension_dev/overview.html)
* [Grunt中文文档](http://www.gruntjs.net/)
* [gulp中文文档](http://www.gulpjs.com.cn/docs/)
* [Gulp 入门指南](https://github.com/nimojs/gulp-book)
* [移动Web前端知识库](https://github.com/AlloyTeam/Mars)
* [正则表达式30分钟入门教程](http://deerchao.net/tutorials/regex/regex.htm)
* [前端开发体系建设日记](https://github.com/fouber/blog/issues/2)
* [移动前端开发收藏夹](https://github.com/hoosin/mobile-web-favorites)
* [JSON风格指南](https://github.com/darcyliu/google-styleguide/blob/master/JSONStyleGuide.md)
* [HTTP 接口设计指北](https://github.com/bolasblack/http-api-guide)
* [前端资源分享（一）](https://github.com/hacke2/hacke2.github.io/issues/1)
* [前端资源分享（二）](https://github.com/hacke2/hacke2.github.io/issues/3)
* [前端代码规范 及 最佳实践](http://coderlmn.github.io/code-standards/)
* [前端开发者手册](https://www.gitbook.com/book/dwqs/frontenddevhandbook/details)
* [前端工程师手册](https://www.gitbook.com/book/leohxj/front-end-database/details)
* [w3school教程整理](https://github.com/wizardforcel/w3school)
* [Wireshark用户手册](http://man.lupaworld.com/content/network/wireshark/index.html)
* [一站式学习Wireshark](https://community.emc.com/thread/194901)
* [HTTP 下午茶](https://ccbikai.gitbooks.io/http-book/content/)
* [HTTP/2.0 中文翻译](http://yuedu.baidu.com/ebook/478d1a62376baf1ffc4fad99?pn=1)
* [RFC 7540 - HTTP/2 中文翻译版](https://github.com/abbshr/rfc7540-translation-zh_cn)
* [http2讲解](https://www.gitbook.com/book/ye11ow/http2-explained/details)
* [3 Web Designs in 3 Weeks](https://www.gitbook.com/book/juntao/3-web-designs-in-3-weeks/details)
* [站点可靠性工程](https://github.com/hellorocky/Site-Reliability-Engineering)
* [Web安全学习笔记](https://websec.readthedocs.io)
* [Serverless 架构应用开发指南](https://github.com/phodal/serverless)

[返回目录](#目录)

## 大数据

* [大数据/数据挖掘/推荐系统/机器学习相关资源](https://github.com/Flowerowl/Big-Data-Resources)
* [面向程序员的数据挖掘指南](https://github.com/egrcc/guidetodatamining)
* [大型集群上的快速和通用数据处理架构](https://code.csdn.net/CODE_Translation/spark_matei_phd)
* [数据挖掘中经典的算法实现和详细的注释](https://github.com/linyiqun/DataMiningAlgorithm)
* [Spark 编程指南简体中文版](https://aiyanbo.gitbooks.io/spark-programming-guide-zh-cn/content/)

[返回目录](#目录)

## 编程艺术

* [程序员编程艺术](https://github.com/julycoding/The-Art-Of-Programming-by-July)
* [每个程序员都应该了解的内存知识(译)](http://www.oschina.net/translate/what-every-programmer-should-know-about-memory-part1?print)【第一部分】
* [取悦的工序：如何理解游戏](http://read.douban.com/ebook/4972883/) (豆瓣阅读，免费书籍)

[返回目录](#目录)

## 其它

* [OpenWrt智能、自动、透明翻墙路由器教程](https://www.gitbook.com/book/softwaredownload/openwrt-fanqiang/details)
* [SAN 管理入门系列](https://community.emc.com/docs/DOC-16067)
* [Sketch 中文手册](http://sketchcn.com/sketch-chinese-user-manual.html#introduce)
* [深入理解并行编程](http://ifeve.com/perfbook/)
* [程序员的自我修养](http://www.kancloud.cn/kancloud/a-programmer-prepares)
* [Growth: 全栈增长工程师指南](https://github.com/phodal/growth-ebook)
* [系统重构与迁移指南](https://github.com/phodal/migration)

[返回目录](#目录)

## Android

* [Android Design(中文版)](http://www.apkbus.com/design/index.html)
* Google Material Design 正體中文版 ([译本一](https://wcc723.gitbooks.io/google_design_translate/content/style-icons.html) [译本二](https://github.com/1sters/material_design_zh))
* [Material Design 中文版](http://wiki.jikexueyuan.com/project/material-design/)
* [Google Android官方培训课程中文版](http://hukai.me/android-training-course-in-chinese/index.html)
* [Android学习之路](http://www.stormzhang.com/android/2014/07/07/learn-android-from-rookie/)
* [Android开发技术前线(android-tech-frontier)](https://github.com/bboyfeiyu/android-tech-frontier)
* [Point-of-Android](https://github.com/FX-Max/Point-of-Android) Android 一些重要知识点解析整理
* [Android6.0新特性详解](http://leanote.com/blog/post/561658f938f41126b2000298)

[返回目录](#目录)

## APP

* [Apache Cordova 开发指南](https://github.com/waylau/cordova-dev-guide)

[返回目录](#目录)

## AWK

* [awk程序设计语言](https://github.com/wuzhouhui/awk)
* [awk中文指南](http://awk.readthedocs.org/en/latest/index.html)
* [awk实战指南](https://book.saubcy.com/AwkInAction/)

[返回目录](#目录)

## C/C++

* [C/C++ 中文参考手册](http://zh.cppreference.com/) (欢迎大家参与在线翻译和校对)
* [C 语言编程透视](https://www.gitbook.com/book/tinylab/cbook/details)
* [C++ 并发编程指南](https://github.com/forhappy/Cplusplus-Concurrency-In-Practice)
* [Linux C编程一站式学习](http://akaedu.github.io/book/) (宋劲杉, 北京亚嵌教育研究中心)
* [CGDB中文手册](https://github.com/leeyiw/cgdb-manual-in-chinese)
* [100个gdb小技巧](https://github.com/hellogcc/100-gdb-tips/blob/master/src/index.md)
* [100个gcc小技巧](https://github.com/hellogcc/100-gcc-tips/blob/master/src/index.md)
* [ZMQ 指南](https://github.com/anjuke/zguide-cn)
* [How to Think Like a Computer Scientist](http://www.ituring.com.cn/book/1203) (中英文版)
* [跟我一起写 Makefile](https://github.com/seisman/how-to-write-makefile)
* [GNU make中文手册](https://free-online-ebooks.appspot.com/tools/gnu-make-cn/) (需科学上网) ([PDF](https://hacker-yhj.github.io/resources/gun_make.pdf))
* [GNU make 指南](http://docs.huihoo.com/gnu/linux/gmake.html)
* [Google C++ 风格指南](http://zh-google-styleguide.readthedocs.org/en/latest/google-cpp-styleguide/contents/)
* [C/C++ Primer](https://github.com/andycai/cprimer) (by @andycai)
* [简单易懂的C魔法](http://www.nowamagic.net/librarys/books/contents/c)
* [C++ FAQ LITE(中文版)](http://www.sunistudio.com/cppfaq/)
* [C++ Primer 5th Answers](https://github.com/Mooophy/Cpp-Primer)
* [C++ 并发编程(基于C++11)](https://www.gitbook.com/book/chenxiaowei/cpp_concurrency_in_action/details)
* [QT 教程](http://www.kuqin.com/qtdocument/tutorial.html)
* [DevBean的《Qt学习之路2》(Qt5)](http://www.devbean.net/category/qt-study-road-2/)
* [中文版《QmlBook》](https://github.com/cwc1987/QmlBook-In-Chinese)
* [C++ Template 进阶指南](https://github.com/wuye9036/CppTemplateTutorial)
* [libuv中文教程](https://github.com/luohaha/Chinese-uvbook)
* [Boost 库中文教程](http://zh.highscore.de/cpp/boost/)
* [笨办法学C](https://github.com/wizardforcel/lcthw-zh)
* [高速上手 C++11/14/17](https://github.com/changkun/modern-cpp-tutorial)

[返回目录](#目录)

## C&#35;

* [Microsoft Docs C# 官方文档](https://docs.microsoft.com/zh-cn/dotnet/csharp/)
* [ASP.NET MVC 5 入门指南](http://www.cnblogs.com/powertoolsteam/p/aspnetmvc5-tutorials-grapecity.html)
* [超全面的 .NET GDI+ 图形图像编程教程](http://www.cnblogs.com/geeksss/p/4162318.html)
* [.NET控件开发基础](https://github.com/JackWangCUMT/customcontrol)
* [.NET开发要点精讲（初稿）](https://github.com/sherlockchou86/-free-ebook-.NET-)

[返回目录](#目录)

## Clojure

* [Clojure入门教程](https://wizardforcel.gitbooks.io/clojure-fpftj/)

[返回目录](#目录)

<h2 id="csshtml">CSS/HTML</h2>

* [学习CSS布局](http://zh.learnlayout.com/)
* [通用 CSS 笔记、建议与指导](https://github.com/chadluo/CSS-Guidelines/blob/master/README.md)
* [CSS参考手册](http://css.doyoe.com/)
* [Emmet 文档](http://yanxyz.github.io/emmet-docs/)
* [前端代码规范](http://alloyteam.github.io/CodeGuide/) (腾讯 AlloyTeam 团队)
* [HTML和CSS编码规范](http://codeguide.bootcss.com/)
* [Sass Guidelines 中文](http://sass-guidelin.es/zh/)
* [CSS3 Tutorial 《CSS3 教程》](https://github.com/waylau/css3-tutorial)
* [MDN HTML 中文文档](https://developer.mozilla.org/zh-CN/docs/Web/HTML)
* [MDN CSS 中文文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS)

[返回目录](#目录)

## Dart

* [Dart 语言导览](http://dart.lidian.info/wiki/Language_Tour)

[返回目录](#目录)

## Elixir

* [Elixir编程入门](https://github.com/straightdave/programming_elixir)

[返回目录](#目录)

## Erlang

* [21天学通Erlang](http://xn--21erlang-p00o82pmp3o.github.io/)

[返回目录](#目录)

## Fortran

* [Fortran77和90/95编程入门](http://micro.ustc.edu.cn/Fortran/ZJDing/)

[返回目录](#目录)

## Go

* [Go编程基础](https://github.com/Unknwon/go-fundamental-programming)
* [Go入门指南](https://github.com/Unknwon/the-way-to-go_ZH_CN)
* [学习Go语言](http://mikespook.com/learning-go/)
* [Go Web 编程](https://github.com/astaxie/build-web-application-with-golang) (此书已经出版，希望开发者们去购买，支持作者的创作)
* [Go实战开发](https://github.com/astaxie/Go-in-Action) (当我收录此项目时，作者已经写完第三章，如果读完前面章节觉得有帮助，可以给作者[捐赠](https://me.alipay.com/astaxie)，以鼓励作者的继续创作)
* [Network programming with Go 中文翻译版本](https://github.com/astaxie/NPWG_zh)
* [Effective Go](http://www.hellogcc.org/effective_go.html)
* [Go 语言标准库](https://github.com/polaris1119/The-Golang-Standard-Library-by-Example)
* [Golang标准库文档](http://godoc.ml/)
* [Revel 框架手册](http://gorevel.cn/docs/manual/index.html)
* [Java程序员的Golang入门指南](http://blog.csdn.net/dc_726/article/details/46565241)
* [Go命令教程](https://github.com/hyper-carrot/go_command_tutorial)
* [Go语言博客实践](https://github.com/achun/Go-Blog-In-Action)
* [Go 官方文档翻译](https://github.com/golang-china/golangdoc.translations)
* [深入解析Go](https://github.com/tiancaiamao/go-internals)
* [Go语言圣经(中文版)](https://bitbucket.org/golang-china/gopl-zh/wiki/Home) ([GitBook](https://www.gitbook.com/book/wizardforcel/gopl-zh/details))
* [golang runtime源码分析](https://github.com/sheepbao/golang_runtime_reading)
* [Go语言实战: 编写可维护Go语言代码建议](https://github.com/llitfkitfk/go-best-practice)
* [Golang 系列教程(译)](https://github.com/Tinywan/golang-tutorial)   
* [Go RPC 开发指南](https://github.com/smallnest/go-rpc-programming-guide)[GitBook](https://smallnest.gitbooks.io/go-rpc-programming-guide/)
* [Go语言高级编程](https://books.studygolang.com/advanced-go-programming-book/)   
* [Go2编程指南](https://chai2010.cn/go2-book/)   
* [Go语言设计模式](https://github.com/senghoo/golang-design-pattern)   
* [Go语言四十二章经](https://github.com/ffhelicopter/Go42)   

[返回目录](#目录)

## Groovy

* [实战 Groovy 系列](http://www.ibm.com/developerworks/cn/java/j-pg/)

[返回目录](#目录)

## Haskell

* [Real World Haskell 中文版](http://rwh.readthedocs.org/en/latest/)
* [Haskell趣学指南](https://learnyoua.haskell.sg/content/zh-cn/)

[返回目录](#目录)

## iOS

* [iOS开发60分钟入门](https://github.com/qinjx/30min_guides/blob/master/ios.md)
* [iOS7人机界面指南](http://isux.tencent.com/ios-human-interface-guidelines-ui-design-basics-ios7.html)
* [Google Objective-C Style Guide 中文版](http://zh-google-styleguide.readthedocs.org/en/latest/google-objc-styleguide/)
* [iPhone 6 屏幕揭秘](http://wileam.com/iphone-6-screen-cn/)
* [Apple Watch开发初探](http://nilsun.github.io/apple-watch/)
* [马上着手开发 iOS 应用程序](https://developer.apple.com/library/ios/referencelibrary/GettingStarted/RoadMapiOSCh/index.html)
* [网易斯坦福大学公开课：iOS 7应用开发字幕文件](https://github.com/jkyin/Subtitle)

[返回目录](#目录)

## Java

* [Apache Shiro 用户指南](https://github.com/waylau/apache-shiro-1.2.x-reference)
* [Jersey 2.x 用户指南](https://github.com/waylau/Jersey-2.x-User-Guide)
* [Spring Framework 4.x参考文档](https://github.com/waylau/spring-framework-4-reference)
* [Spring Boot参考指南](https://github.com/qibaoguang/Spring-Boot-Reference-Guide) (翻译中)
* [MyBatis中文文档](http://mybatis.org/mybatis-3/zh/index.html)
* [MyBatis Generator 中文文档](http://mbg.cndocs.tk/)
* [用jersey构建REST服务](https://github.com/waylau/RestDemo)
* [Activiti 5.x 用户指南](https://github.com/waylau/activiti-5.x-user-guide)
* [Google Java编程风格指南](http://www.hawstein.com/posts/google-java-style.html)
* [Netty 4.x 用户指南](https://github.com/waylau/netty-4-user-guide)
* [Netty 实战(精髓)](https://github.com/waylau/essential-netty-in-action)
* [REST 实战](https://github.com/waylau/rest-in-action)
* [Java 编码规范](https://github.com/waylau/java-code-conventions)
* [Apache MINA 2 用户指南](https://github.com/waylau/apache-mina-2.x-user-guide)
* [H2 Database 教程](https://github.com/waylau/h2-database-doc)
* [Java Servlet 3.1 规范](https://github.com/waylau/servlet-3.1-specification)
* [JSSE 参考指南](https://github.com/waylau/jsse-reference-guide)
* [Java开源实现及最佳实践](https://github.com/biezhi/jb)
* [Java 编程要点](https://github.com/waylau/essential-java)
* [Think Java](http://www.ituring.com.cn/minibook/69)
* [Java 8 简明教程](https://github.com/wizardforcel/modern-java-zh)
* [On Java 8 中文版](https://github.com/LingCoder/OnJava8) (翻译中)
* [Effective Java 第3版中文版](https://github.com/sjsdfg/effective-java-3rd-chinese) 

[返回目录](#目录)

## JavaScript

* [现代 Javascript 教程](https://zh.javascript.info/)
* [Google JavaScript 代码风格指南](http://bq69.com/blog/articles/script/868/google-javascript-style-guide.html)
* [Google JSON 风格指南](https://github.com/darcyliu/google-styleguide/blob/master/JSONStyleGuide.md)
* [Airbnb JavaScript 规范](https://github.com/adamlu/javascript-style-guide)
* [JavaScript 标准参考教程（alpha）](http://javascript.ruanyifeng.com/)
* [Javascript编程指南](http://pij.robinqu.me/) ([源码](https://github.com/RobinQu/Programing-In-Javascript))
* [javascript 的 12 个怪癖](https://github.com/justjavac/12-javascript-quirks)
* [JavaScript 秘密花园](http://bonsaiden.github.io/JavaScript-Garden/zh/)
* [JavaScript核心概念及实践](http://icodeit.org/jsccp/) (PDF) (此书已由人民邮电出版社出版发行，但作者依然免费提供PDF版本，希望开发者们去购买，支持作者)
* [《JavaScript 模式》](https://github.com/jayli/javascript-patterns) “JavaScript patterns”中译本
* [JavaScript语言精粹](https://github.com/qibaoguang/Study-Step-by-Step/blob/master/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/javascript_the_good_parts.md)
* [命名函数表达式探秘](http://justjavac.com/named-function-expressions-demystified.html)  (注:原文由[为之漫笔](http://www.cn-cuckoo.com)翻译，原始地址无法打开，所以此处地址为我博客上的备份)
* [学用 JavaScript 设计模式](http://www.oschina.net/translate/learning-javascript-design-patterns) (开源中国)
* [深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
* [ECMAScript 5.1 中文版](http://yanhaijing.com/es5)
* [ECMAScript 6 入门](http://es6.ruanyifeng.com/) (作者：阮一峰)
* [JavaScript Promise迷你书](http://liubin.github.io/promises-book/)
* [You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS) (深入JavaScript语言核心机制的系列图书)
* [JavaScript 教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000) 廖雪峰
* [MDN JavaScript 中文文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
* jQuery
    * [jQuery 解构](http://www.cn-cuckoo.com/deconstructed/jquery.html)
    * [简单易懂的JQuery魔法](http://www.nowamagic.net/librarys/books/contents/jquery)
    * [How to write jQuery plugin](http://i5ting.github.io/How-to-write-jQuery-plugin/build/jquery.plugin.html)
    * [You Don't Need jQuery](https://github.com/oneuijs/You-Dont-Need-jQuery/blob/master/README.zh-CN.md)
    * [如何实现一个类jQuery？](https://github.com/MeCKodo/forchange)
* Node.js
    * [Node入门](http://www.nodebeginner.org/index-zh-cn.html)
    * [七天学会NodeJS](http://nqdeng.github.io/7-days-nodejs/)
    * [Nodejs Wiki Book](https://github.com/nodejs-tw/nodejs-wiki-book) (繁体中文)
    * [express.js 中文文档](http://expressjs.jser.us/)
    * [koa 中文文档](https://github.com/guo-yu/koa-guide)
    * [一起学koa](http://base-n.github.io/koa-generator-examples/)
    * [使用 Express + MongoDB 搭建多人博客](https://github.com/nswbmw/N-blog)
    * [Express框架](http://javascript.ruanyifeng.com/nodejs/express.html)
    * [Node.js 包教不包会](https://github.com/alsotang/node-lessons)
    * [Learn You The Node.js For Much Win! (中文版)](https://www.npmjs.com/package/learnyounode-zh-cn)
    * [Node debug 三法三例](http://i5ting.github.io/node-debug-tutorial/)
    * [nodejs中文文档](https://www.gitbook.com/book/0532/nodejs/details)
    * [orm2 中文文档](https://github.com/wizardforcel/orm2-doc-zh-cn)
    * [一起学 Node.js](https://github.com/nswbmw/N-blog)
* underscore.js
    * [Underscore.js中文文档](http://learningcn.com/underscore/)
* backbone.js
    * [backbone.js中文文档](http://www.css88.com/doc/backbone/)
    * [backbone.js入门教程](http://www.the5fire.com/backbone-js-tutorials-pdf-download.html) (PDF)
    * [Backbone.js入门教程第二版](https://github.com/the5fire/backbonejs-learning-note)
    * [Developing Backbone.js Applications(中文版)](http://feliving.github.io/developing-backbone-applications/)
* AngularJS
    * [AngularJS最佳实践和风格指南](https://github.com/mgechev/angularjs-style-guide/blob/master/README-zh-cn.md)
    * [AngularJS中译本](https://github.com/peiransun/angularjs-cn)
    * [AngularJS入门教程](https://github.com/zensh/AngularjsTutorial_cn)
    * [构建自己的AngularJS](https://github.com/xufei/Make-Your-Own-AngularJS/blob/master/01.md)
    * [在Windows环境下用Yeoman构建AngularJS项目](http://www.waylau.com/build-angularjs-app-with-yeoman-in-windows/)
* Zepto.js
    * [Zepto.js 中文文档](http://mweb.baidu.com/zeptoapi/)
* Sea.js
    * [Hello Sea.js](http://island205.github.io/HelloSea.js/)
* React.js
    * [React 学习之道](https://github.com/the-road-to-learn-react/the-road-to-learn-react-chinese)
    * [React.js 小书](https://github.com/huzidaha/react-naive-book)
    * [React.js 中文文档](https://doc.react-china.org/)
    * [React webpack-cookbook](https://github.com/fakefish/react-webpack-cookbook)
    * [React 入门教程](http://fraserxu.me/intro-to-react/)
    * [React 入门教程](https://hulufei.gitbooks.io/react-tutorial/content/) (作者：hulufei, 与上行不同作者)
    * [React Native 中文文档(含最新Android内容)](http://wiki.jikexueyuan.com/project/react-native/)
    * [Learn React & Webpack by building the Hacker News front page](https://github.com/theJian/build-a-hn-front-page)
* impress.js
    * [impress.js的中文教程](https://github.com/kokdemo/impress.js-tutorial-in-Chinese)
* CoffeeScript
    * [CoffeeScript Cookbook](http://island205.com/coffeescript-cookbook.github.com/)
    * [The Little Book on CoffeeScript中文版](http://island205.com/tlboc/)
    * [CoffeeScript 编码风格指南](https://github.com/geekplux/coffeescript-style-guide)
* TypeScipt
    * [TypeScript Handbook](https://zhongsp.gitbooks.io/typescript-handbook/content/)
* ExtJS
    * [Ext4.1.0 中文文档](http://extjs-doc-cn.github.io/ext4api/)
* Meteor
    * [Discover Meteor](http://zh.discovermeteor.com/)
    * [Meteor 中文文档](http://docs.meteorhub.org/#/basic/)
    * [Angular-Meteor 中文教程](http://angular.meteorhub.org/)
* VueJS
    * [逐行剖析 Vue.js 源码](https://nlrx-wjc.github.io/Learn-Vue-Source-Code/)
* [Chrome扩展及应用开发](http://www.ituring.com.cn/minibook/950)

## Kotlin
* [Kotlin 官方参考文档 中文版](https://hltj.gitbooks.io/kotlin-reference-chinese/content/) 
* [Kotlin 中文文档](https://huanglizhuo.gitbooks.io/kotlin-in-chinese/) [GitHub](https://github.com/huanglizhuo/kotlin-in-chinese)   
* [Kotlin 参考文档](http://www.liying-cn.net/kotlin/docs/reference/)    
* [《Kotlin for android developers》中文版](https://wangjiegulu.gitbooks.io/kotlin-for-android-developers-zh/content/)  

[返回目录](#目录)


## LaTeX

* [一份其实很短的 LaTeX 入门文档](http://liam0205.me/2014/09/08/latex-introduction/)
* [一份不太简短的 LATEX 2ε 介绍](http://www.mohu.org/info/lshort-cn.pdf) （PDF版）

[返回目录](#目录)

## LISP
* Common Lisp
    * [ANSI Common Lisp 中文翻譯版](http://acl.readthedocs.org/en/latest/)
    * [On Lisp 中文翻译版本](http://www.ituring.com.cn/minibook/862)
* Scheme
    * [Yet Another Scheme Tutorial Scheme入门教程](http://deathking.github.io/yast-cn/)
    * [Scheme语言简明教程](http://songjinghe.github.io/TYS-zh-translation/)
    * Racket
        * [Racket book](https://github.com/tyrchen/racket-book)
        

[返回目录](#目录)

## Lua

* [Lua编程入门](https://github.com/andycai/luaprimer)
* [Lua 5.1 参考手册 中文翻译](http://www.codingnow.com/2000/download/lua_manual.html)
* [Lua 5.3 参考手册 中文翻译](http://cloudwu.github.io/lua53doc/)
* [Lua源码欣赏](http://www.codingnow.com/temp/readinglua.pdf)

[返回目录](#目录)

## OCaml

* [Real World OCaml](https://github.com/zforget/translation/tree/master/real_world_ocaml)

[返回目录](#目录)

## Perl

* [Modern Perl 中文版](https://github.com/horus/modern_perl_book)
* [Perl 程序员应该知道的事](http://perl.linuxtoy.org/)

[返回目录](#目录)

## PHP

* [PHP 官方手册](http://php.net/manual/zh/)
* [PHP调试技术手册](http://www.laruence.com/2010/06/21/1608.html)(PDF)
* PHP之道：php-the-right-way ([@wulijun版](http://wulijun.github.io/php-the-right-way/) [PHPHub版](http://laravel-china.github.io/php-the-right-way/))
* [PHP 最佳实践](https://github.com/justjavac/PHP-Best-Practices-zh_CN)
* [PHP 开发者实践](https://ryancao.gitbooks.io/php-developer-prepares/content/)
* [深入理解PHP内核](https://github.com/reeze/tipi)
* [PHP扩展开发及内核应用](http://www.walu.cc/phpbook/)
* [Laravel5.1 中文文档](http://laravel-china.org/docs/5.1)
* [Laravel 5.1 LTS 速查表](https://cs.phphub.org/)
* [Symfony2 Cookbook 中文版](http://wiki.jikexueyuan.com/project/symfony-cookbook/)(版本 2.7.0 LTS)
* [Symfony2中文文档](http://symfony-docs-chs.readthedocs.org/en/latest/) (未译完)
* [YiiBook几本Yii框架的在线教程](http://yiibook.com//doc)
* [深入理解 Yii 2.0](http://www.digpage.com/)
* [Yii 框架中文官网](http://www.yiichina.com/)
* [简单易懂的PHP魔法](http://www.nowamagic.net/librarys/books/contents/php)
* [swoole文档及入门教程](https://github.com/LinkedDestiny/swoole-doc)
* [Composer 中文网](http://www.phpcomposer.com)
* [Slim 中文文档](http://ww1.minimee.org/php/slim)
* [Lumen 中文文档](http://lumen.laravel-china.org/)
* [PHPUnit 中文文档](https://phpunit.de/manual/current/zh_cn/installation.html)
* [PHP-LeetCode](https://github.com/wuqinqiang/leetcode-php)

[返回目录](#目录)

## Prolog

* [笨办法学Prolog](http://fengdidi.github.io/blog/2011/11/15/qian-yan/)

[返回目录](#目录)

## Python

* [廖雪峰 Python 2.7 中文教程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)
* [廖雪峰 Python 3 中文教程](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)
* [简明Python教程](http://www.kuqin.com/abyteofpython_cn/)
* [简明 Python 教程(Python 3)](https://legacy.gitbook.com/book/lenkimo/byte-of-python-chinese-edition/details)
* [零基础学 Python 第一版](http://www.kancloud.cn/kancloud/python-basic)
* [零基础学 Python 第二版](http://www.kancloud.cn/kancloud/starter-learning-python)
* [可爱的 Python](http://lovelypython.readthedocs.org/en/latest/)
* [Python 2.7 官方教程中文版](http://www.pythondoc.com/pythontutorial27/index.html)
* [Python 3.3 官方教程中文版](http://www.pythondoc.com/pythontutorial3/index.html)
* [Python Cookbook 中文版](http://www.kancloud.cn/thinkphp/python-cookbook)
* [Python3 Cookbook 中文版](https://github.com/yidao620c/python3-cookbook)
* [深入 Python](http://www.kuqin.com/docs/diveintopythonzh-cn-5.4b/html/toc/)
* [深入 Python 3](http://old.sebug.net/paper/books/dive-into-python3/)
* [PEP8 Python代码风格规范](https://code.google.com/p/zhong-wiki/wiki/PEP8)
* [Google Python 风格指南 中文版](http://zh-google-styleguide.readthedocs.org/en/latest/google-python-styleguide/)
* [Python入门教程](http://liam0205.me/2013/11/02/Python-tutorial-zh_cn/) ([PDF](http://liam0205.me/attachment/Python/The_Python_Tutorial_zh-cn.pdf))
* [笨办法学 Python](http://old.sebug.net/paper/books/LearnPythonTheHardWay/) ([PDF](http://liam0205.me/attachment/Python/PyHardWay/Learn_Python_The_Hard_Way_zh-cn.pdf) [EPUB](https://www.gitbook.com/download/epub/book/wizardforcel/lpthw))
* [Python自然语言处理中文版](http://pan.baidu.com/s/1qW4pvnY) （感谢陈涛同学的翻译，也谢谢 [@shwley](https://github.com/shwley) 联系了作者）
* [Python 绘图库 matplotlib 官方指南中文翻译](http://liam0205.me/2014/09/11/matplotlib-tutorial-zh-cn/)
* [Scrapy 0.25 文档](http://scrapy-chs.readthedocs.org/zh_CN/latest/)
* [ThinkPython](https://github.com/carfly/thinkpython-cn)
* [ThinkPython 2ed](https://github.com/bingjin/ThinkPython2-CN)
* [Python快速教程](http://www.cnblogs.com/vamei/archive/2012/09/13/2682778.html)
* [Python 正则表达式操作指南](http://wiki.ubuntu.org.cn/Python正则表达式操作指南)
* [python初级教程：入门详解](http://www.crifan.com/files/doc/docbook/python_beginner_tutorial/release/html/python_beginner_tutorial.html)
* [Twisted 与异步编程入门](https://www.gitbook.com/book/likebeta/twisted-intro-cn/details)
* [TextGrocery 中文 API](http://textgrocery.readthedocs.org/zh/latest/index.html) ( 基于svm算法的一个短文本分类 Python 库 )
* [Requests: HTTP for Humans](http://requests-docs-cn.readthedocs.org/zh_CN/latest/)
* [Pillow 中文文档](http://pillow-cn.readthedocs.org/en/latest/#)
* [PyMOTW 中文版](http://pymotwcn.readthedocs.org/en/latest/index.html)
* [Python 官方文档中文版](http://data.digitser.net/zh-CN/python_index.html)
* [Fabric 中文文档](http://fabric-chs.readthedocs.org)
* [Beautiful Soup 4.2.0 中文文档](http://beautifulsoup.readthedocs.org/zh_CN/latest/)
* [Python 中的 Socket 编程](https://legacy.gitbook.com/book/keelii/socket-programming-in-python-cn/details)
* [用Python做科学计算](http://old.sebug.net/paper/books/scipydoc)
* [Sphinx 中文文档](http://www.pythondoc.com/sphinx/index.html)
* [精通 Python 设计模式](https://github.com/cundi/Mastering.Python.Design.Patterns)
* [python 安全编程教程](https://github.com/smartFlash/pySecurity)
* [程序设计思想与方法](https://www.gitbook.com/book/wizardforcel/sjtu-cs902-courseware/details)
* [知乎周刊·编程小白学Python](https://read.douban.com/ebook/16691849/)
* [Scipy 讲义](https://github.com/cloga/scipy-lecture-notes_cn)
* [Python 学习笔记 基础篇](http://www.kuqin.com/docs/pythonbasic.html)
* [Python 学习笔记 模块篇](http://www.kuqin.com/docs/pythonmodule.html)
* [Python 标准库 中文版](http://old.sebug.net/paper/books/python/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B%E4%B8%AD%E6%96%87%E7%89%88.pdf)
* [Python进阶](https://www.gitbook.com/book/eastlakeside/interpy-zh/details)
* [Python 核心编程 第二版](https://wizardforcel.gitbooks.io/core-python-2e/content/) CPyUG译
* [Python最佳实践指南](http://pythonguidecn.readthedocs.io/zh/latest/)
* [Python 精要教程](https://www.gitbook.com/book/wizardforcel/python-essential-tutorial/details)
* [Python 量化交易教程](https://www.gitbook.com/book/wizardforcel/python-quant-uqer/details)
* Django
    * [Django 1.5 文档中文版](http://django-chinese-docs.readthedocs.org/en/latest/) 正在翻译中
    * [Django 2.0 文档中文版](https://docs.djangoproject.com/zh-hans/2.0/)
    * [Django 最佳实践](https://github.com/yangyubo/zh-django-best-practices)
    * [Django 2.1 搭建个人博客教程](https://www.dusaiphoto.com/article/detail/2/) ( 编写中 )
    * [Django搭建简易博客教程](https://www.gitbook.com/book/andrew-liu/django-blog/details)
    * [The Django Book 中文版](http://djangobook.py3k.cn/2.0/)
    * [Django 设计模式与最佳实践](https://github.com/cundi/Django-Design-Patterns-and-Best-Practices)
    * [Django 网站开发 Cookbook](https://github.com/cundi/Web.Development.with.Django.Cookbook)
    * [Django Girls 學習指南](https://www.gitbook.com/book/djangogirlstaipei/django-girls-taipei-tutorial/details)
* Flask
    * [Flask 文档中文版](http://docs.jinkan.org/docs/flask/)
    * [Jinja2 文档中文版](http://docs.jinkan.org/docs/jinja2/)
    * [Werkzeug 文档中文版](http://werkzeug-docs-cn.readthedocs.org/zh_CN/latest/)
    * [Flask之旅](http://spacewander.github.io/explore-flask-zh/)
    * [Flask 扩展文档汇总](https://www.gitbook.com/book/wizardforcel/flask-extension-docs/details)
    * [Flask 大型教程](http://www.pythondoc.com/flask-mega-tutorial/index.html)
    * [SQLAlchemy 中文文档](http://docs.jinkan.org/docs/flask-sqlalchemy/)
    * [Flask 入门教程](https://read.helloflask.com)
* web.py
    * [web.py 0.3 新手指南](http://webpy.org/tutorial3.zh-cn)
    * [Web.py Cookbook 简体中文版](http://webpy.org/cookbook/index.zh-cn)
* Tornado
    * [Introduction to Tornado 中文翻译](http://demo.pythoner.com/itt2zh/index.html)
    * [Tornado源码解析](http://www.nowamagic.net/academy/detail/13321002)
    * [Tornado 4.3 文档中文版](https://tornado-zh.readthedocs.org/zh/latest/)

[返回目录](#目录)

## R

* [R语言忍者秘笈](https://github.com/yihui/r-ninja)

[返回目录](#目录)

## Ruby

* [Ruby 风格指南](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [Rails 风格指南](https://github.com/JuanitoFatas/rails-style-guide/blob/master/README-zhCN.md)
* [笨方法學 Ruby](http://lrthw.github.io/)
* [Ruby on Rails 指南](http://guides.ruby-china.org/)
* [Ruby on Rails 實戰聖經](https://ihower.tw/rails4/index.html)
* [Ruby on Rails Tutorial 原书第 3 版](http://railstutorial-china.org/) (本书网页版免费提供，电子版以 PDF、EPub 和 Mobi 格式提供购买，仅售 9.9 美元)
* [Rails 实践](http://rails-practice.com/content/index.html)
* [Rails 5 开发进阶(Beta)](https://www.gitbook.com/book/kelby/rails-beginner-s-guide/details)
* [Rails 102](https://www.gitbook.com/book/rocodev/rails-102/details)
* [编写Ruby的C拓展](https://wusuopu.gitbooks.io/write-ruby-extension-with-c/content/)
* [Ruby 源码解读](https://ruby-china.org/topics/22386)
* [Ruby中的元编程](http://deathking.github.io/metaprogramming-in-ruby/)

[返回目录](#目录)

## Rust

* [Rust编程语言 中文翻译](https://kaisery.github.io/trpl-zh-cn/)
* [Rust Primer](https://github.com/rustcc/RustPrimer)

[返回目录](#目录)

## Scala

* [Scala课堂](http://twitter.github.io/scala_school/zh_cn/index.html) (Twitter的Scala中文教程)
* [Effective Scala](http://twitter.github.io/effectivescala/index-cn.html)(Twitter的Scala最佳实践的中文翻译)
* [Scala指南](http://zh.scala-tour.com/)

[返回目录](#目录)

## Shell

* [Shell脚本编程30分钟入门](https://github.com/qinjx/30min_guides/blob/master/shell.md)
* [Bash脚本15分钟进阶教程](http://blog.sae.sina.com.cn/archives/3606)
* [Linux工具快速教程](https://github.com/me115/linuxtools_rst)
* [shell十三问](https://github.com/wzb56/13_questions_of_shell)
* [Shell编程范例](https://www.gitbook.com/book/tinylab/shellbook/details)
* [Linux命令搜索引擎](https://wangchujiang.com/linux-command/)

[返回目录](#目录)

## Swift

* [The Swift Programming Language 中文版](http://numbbbbb.github.io/the-swift-programming-language-in-chinese/)
* [Swift 语言指南](http://dev.swiftguide.cn)
* [Stanford 公开课，Developing iOS 8 Apps with Swift 字幕翻译文件](https://github.com/x140yu/Developing_iOS_8_Apps_With_Swift)
* [C4iOS - COSMOS](http://c4ios.swift.gg)

[返回目录](#目录)

## 读书笔记及其它

* [编译原理（紫龙书）中文第2版习题答案](https://github.com/fool2fish/dragon-book-exercise-answers)
* [把《编程珠玑》读薄](http://hawstein.com/2013/08/11/make-thiner-programming-pearls/)
* [Effective C++读书笔记](https://github.com/XiaolongJason/ReadingNote/blob/master/Effective%20C%2B%2B/Effective%20C%2B%2B.md)
* [Golang 学习笔记、Python 学习笔记、C 学习笔记](https://github.com/qyuhen/book) (PDF)
* [Jsoup 学习笔记](https://github.com/code4craft/jsoup-learning)
* [学习笔记: Vim、Python、memcached](https://github.com/lzjun567/note)
* [图灵开放书翻译计划--C++、Python、Java等](http://www.ituring.com.cn/activity/details/2004)
* [蒂姆·奥莱利随笔](http://g.yeeyan.org/books/2095) （由译言网翻译，电子版免费）
* [SICP 解题集](http://sicp.readthedocs.org/en/latest/)
* [精彩博客集合](https://github.com/hacke2/hacke2.github.io/issues/2)
* [中文文案排版指北](https://github.com/sparanoid/chinese-copywriting-guidelines)
* [Standard C 语言标准函数库速查 (Cheat Sheet)](http://ganquan.info/standard-c/)
* [Git Cheatsheet Chs](http://gh.amio.us/git-cheatsheet-chs/)
* [GitBook简明教程](http://www.chengweiyang.cn/gitbook/index.html)
* [制造开源软件](http://producingoss.com/zh/)
* [提问的智慧](http://www.dianbo.org/9238/stone/tiwendezhihui.htm)
* [Markdown 入门参考](https://github.com/LearnShare/Learning-Markdown)
* [AsciiDoc简明指南](https://github.com/stanzgy/wiki/blob/master/markup/asciidoc-guide.asciidoc)
* [背包问题九讲](http://love-oriented.com/pack/)
* [老齐的技术资料](https://github.com/qiwsir/ITArticles)
* [前端技能汇总](https://github.com/JacksonTian/fks)
* [借助开源项目，学习软件开发](https://github.com/zhuangbiaowei/learn-with-open-source)
* [前端工作面试问题](https://github.com/h5bp/Front-end-Developer-Interview-Questions/tree/master/Translations/Chinese)
* [leetcode/lintcode题解/算法学习笔记](https://www.gitbook.com/book/yuanbin/algorithm/details)
* [前端开发笔记本](https://github.com/li-xinyang/FEND_Note)
* [LeetCode题解](https://siddontang.gitbooks.io/leetcode-solution/content/)
* [《不可替代的团队领袖培养计划》](https://leader.js.cool/#/)

[返回目录](#目录)

### 测试相关


&emsp;&emsp;阅读文章前请看这篇文章：[Android优质技术资源合集](Android优质技术资源合集.md)

<p>
	<strong><span style="font-size:18px;">一、精选工具：</span></strong>
</p>
<ul>
<li>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.androiddevtools.cn/">AndroidDevTools</a><br />
	</span>
</p>
</li>
<li>
<p>
	<span style="font-size:18px;"><a target="_blank" href="https://github.com/inferjay/AndroidDevTools/">AndroidDevTools(Github)</a><br />
	</span>
</p>
</li>
</ul>
<p>
	<span style="font-size:18px;"><br />
	</span>
</p>
<p>
	<strong><span style="font-size:18px;">二、官方文档：</span></strong>
</p>
<ul>
<li>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://hukai.me/android-training-course-in-chinese/index.html">AndroidTraning中文版</a><br />
	</span>
</p>
</li>
</ul>
<p>
	<span style="font-size:18px;"><br />
	</span>
</p>
<p>
	<strong><span style="font-size:18px;">三、优质博客：</span></strong>
</p>
<ul>
<li>
<p><a href="http://blog.csdn.net/guolin_blog">郭霖</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/Innost">邓凡平</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/singwhatiwanna">singwhatiwanna</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/Luoshengyang">Luoshengyang</a></p>
</li>
<li>
<p><a href="http://www.trinea.cn/">Trinea</a></p>
</li>
<li>
<p><a href="http://blog.daimajia.com/">daimajia</a></p>
</li>
<li>
<p><a href="http://droidyue.com/">技术小黑屋</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/lmj623565791">Hongyang</a></p>
</li>
<li>
<p><a href="http://stormzhang.github.io/">stormzhang</a></p>
</li>
<li>
<p><a href="http://tech.meituan.com/">美团点评技术团队</a></p>
</li>
<li>
<p><a href="http://pedant.cn/">书呆子精神院</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/lzyzsd/">大头鬼Bruce</a></p>
</li>
<li>
<p><a href="http://blog.zhaiyifan.cn/">markzhai的博客</a></p>
</li>
<li>
<p><a href="http://androidperformance.com/">Gracker</a></p>
</li>
<li>
<p><a href="http://android-developers.blogspot.com/">Android官方博客</a></p>
</li>
<li>
<p><a href="http://jakewharton.com/">JakeWharton</a></p>
</li>
<li>
<p><a href="http://square.github.io/">Square</a></p>
</li>
<li>
<p><a href="http://chris.banes.me/">Chris Banes</a></p>
</li>
<li>
<p><a href="http://jeremyfeinstein.com/">Jeremy Feinstein</a></p>
</li>
<li>
<p><a href="http://nostra13android.blogspot.com/">Sergey Tarasevich</a></p>
</li>
<li>
<p><a href="http://koush.com/">Koushik Dutta</a></p>
</li>
<li>
<p><a href="http://simonvt.net/">Simon Vig</a></p>
</li>
<li>
<p><a href="http://cyrilmottier.com/">Cyril Mottier</a></p>
</li>
<li>
<p><a href="http://emilsjolander.se/">Emil Sjolander</a></p>
</li>
<li>
<p><a href="http://loopj.com">James Smith</a></p>
</li>
<li>
<p><a href="https://github.com/ManuelPeinado">Manuel Peinado</a></p>
</li>
<li>
<p><a href="http://greenrobot.de/">greenrobot</a></p>
</li>
<li>
<p><a href="http://jeffgilfelt.com">Jeff Gilfelt</a></p>
</li>
<li>
<p><a href="http://roman.nurik.net/">Roman Nurik</a></p>
</li>
<li>
<p><a href="http://www.flavienlaurent.com">Flavien Laurent</a></p>
</li>
<li>
<p><a href="http://gmariotti.blogspot.it">Gabriele Mariotti</a></p>
</li>
<li>
<p><a href="http://www.sephiroth.it/">sephiroth74</a></p>
</li>
<li>
<p><a href="http://www.curious-creature.org">Romain Guy</a></p>
</li>
<li>
<p><a href="https://twitter.com/kevinsawicki">Kevin Sawicki</a></p>
</li>
<li>
<p><a href="http://about.me/chris.jenkins">Christopher Jenkins</a></p>
</li>
<li>
<p><a href="http://jpardogo.com">Javier Pardo</a></p>
</li>
<li>
<p><a href="http://graphics-geek.blogspot.com/">Chet Haase</a></p>
</li>
<li>
<p><a href="http://mttkay.github.io/">Matthias Käppler</a></p>
</li>
<li>
<p><a href="http://blog.danlew.net/">Daniel Lew</a></p>
</li>
<li>
<p><a href="https://code.facebook.com/mobile/">FaceBook</a></p>
</li>
<li>
<p><a href="https://blog.stylingandroid.com/">Styling Android</a></p>
</li>
<li>
<p><a href="http://www.cnblogs.com/hanyonglu">hanyonglu</a></p>
</li>
<li>
<p><a href="http://www.fookwood.com">fookwood.com</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/lilu_leo">lilu_leo</a> </p>
</li>
<li>
<p><a href="https://github.com/youxiachai">youxiachai</a></p>
</li>
<li>
<p><a href="https://github.com/dodola">dodola </a></p>
</li>
<li>
<p><a href="http://imid.me">imid.me</a></p>
</li>
<li>
<p><a href="https://github.com/mcxiaoke">mcxiaoke</a></p>
</li>
<li>
<p><a href="https://github.com/soarcn">soarcn</a></p>
</li>
<li>
<p><a href="http://www.cnblogs.com/qianxudetianxia">qianxudetianxia</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/xiaominghimi">xiaominghimi</a></p>
</li>
<li>
<p><a href="https://github.com/yangfuhai">yangfuhai</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/hellogv">hellogv</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/yiyaaixuexi">yiyaaixuexi</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/wangjinyu501">wangjinyu501</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/asce1885">asce1885</a></p>
</li>
<li>
<p><a href="http://blog.csdn.net/qinjuning">qinjuning</a> </p>
</li>
<li>
<p><a href="http://blog.csdn.net/tangcheng_ok">tangcheng_ok</a></p>
</li>
<li>
<p><a href="http://over140.cnblogs.com">over140</a></p>
</li>
<li>
<p><a href="http://www.paper3d.net">paper3d.net</a></p>
</li>
<li>
<p><a href="http://www.cnblogs.com/daizhj">daizhj</a></p>
</li>
<li>
<p><a href="http://www.cnblogs.com/sunzn">sunzn</a></p>
</li>
</ul>
<p>
	<span style="font-size:18px;"><br />
	</span>
</p>
<p>
	<strong><span style="font-size:18px;">四、精选视频：</span></strong>
</p>
<ul>
<li>
<p>
	<a href="http://i.youku.com/u/UMjczOTc0NDkzNg==/playlists"><span style="font-size:18px;">Analytics Academy 四套中文课程视频</span></a><br />
	
</p>
</li>
<li>
<p>
	<a href="http://www.infoq.com/cn/news/2015/01/google-android-performance?utm_reader=feedly#0-tsina-1-92503-397232819ff9a47a7b7e80a40613cfe1"><span style="font-size:18px;">Google探讨Android性能模式的16个视频总结</span></a><br />
	
</p>
</li>
<li>
<p>
	<a href="http://www.infoq.com/cn/presentations/from-360-development-see-big-mobile-application-development"><span style="font-size:18px;">从360手机卫士的开发历程看如何实施大型移动应用开发</span></a><br />
	
</p>
</li>
<li>
<p>
	<a href="http://v.youku.com/v_show/id_XODQ1MjI2MDQ0.html?f=23088492"><span style="font-size:18px;">Mastering the Android Touch System</span></a><br />
	
</p>
</li>
<li>
<p>
	<a href="http://www.infoq.com/cn/presentations/taobao-mobile-architecture-evolution-practice"><span style="font-size:18px;">手机淘宝架构演化实践</span></a><br />
	
</p>
</li>
<li>
<p>
	<a href="http://www.infoq.com/cn/presentations/write-high-quality-gradle-script"><span style="font-size:18px;">Gradle脚本的整洁之道——编写高质量的Gradle脚本</span></a><br />
	
</p>
</li>
<li>
<p>
	<a href="http://www.infoq.com/cn/presentations/the-road-of-mobile-qq-mobile-network-practice"><span style="font-size:18px;">手机QQ的移动网络实践之路</span></a><br />
	
</p>
</li>
<li>
<p>
	<a href="http://www.infoq.com/cn/presentations/static-detection-method-for-android-app-vulnerabilities"><span style="font-size:18px;">安卓APP漏洞的静态检测方法</span></a><br />
	
</p>
</li>
</ul>
<p>
	<span style="font-size:18px;"><br />
	</span>
</p>
<p>
	<strong><span style="font-size:18px;">五、技术周报：</span></strong>
</p>
<ul>
<li>
<p><a href="http://www.androidweekly.cn/">Android开发技术周报</a></p>
</li>
<li>
<p><a href="http://www.androidblog.cn/">Android博客周刊</a></p>
</li>
<li>
<p><a href="http://mobilefrontier.github.io/archives/">移动开发前线</a></p>
</li>
<li>
<p><a href="http://androidweekly.net/">Android Weekly</a></p>
</li>
<li>
<p><a href="https://github.com/PaicHyperionDev/MobileDevWeekly">平安金融科技移动开发周报</a></p>
</li>
<li>
<p><a href="http://gank.io/">干货集中营</a></p>
</li>
</ul>
<p>
	<span style="font-size:18px;"><br />
	</span>
</p>
<p>
	<strong><span style="font-size:18px;">六、技术类APP：</span></strong>
</p>
<ul>
<li>
<p><a href="https://gold.xitu.io">掘金</a></p>
</li>
<li>
<p><a href="http://toutiao.io/">开发者头条</a></p>
</li>
<li>
<p><a href="http://www.tuicool.com/">推酷</a></p>
</li>
</ul>
<p>
	<span style="font-size:18px;"><br />
	</span>
</p>
<p>
	<strong><span style="font-size:18px;">七、精华文章：</span></strong>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/android_tutor/article/details/5772285">两分钟彻底让你明白Android Activity生命周期(图文)!</a><br />
	</span>
</p>
<p>
         <span style="font-size:18px;"><a target="_blank"   href="http://www.stormzhang.com/android/2014/07/07/learn-android-from-rookie/"> android的学习之路</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.cnblogs.com/bravestarrhu/archive/2012/05/02/2479461.html">Android四大基本组件介绍与生命周期</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.cnblogs.com/noTice520/archive/2011/12/05/2276379.html">android应用开发全程实录-你有多熟悉listview？</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/liuhe688/article/details/6715983">Android中SQLite应用详解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/8881711">Android Fragment完全解析，关于碎片你所需知道的一切</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/android_tutor/article/details/4952960">Android应用程序的生命周期(一定要理解,面试会问的哦!)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://hukai.me/android-notes-process-and-thread/">Android进程线程讲解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/12921889">Android LayoutInflater原理分析，带你一步步深入了解View(一)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/16330267">Android视图绘制流程完全解析，带你一步步深入了解View(二)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/17045157">Android视图状态及重绘流程分析，带你一步步深入了解View(三)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/17357967">Android自定义View的实现方法，带你一步步深入了解View(四)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/11952435">Android Service完全解析，关于服务你所需知道的一切(上)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/9797169">Android Service完全解析，关于服务你所需知道的一切(下)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://stormzhang.github.io/android/2014/04/10/android-optimize-layout/">Android 布局优化</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/android_tutor/article/details/5740845">Android中Intent传递对象的两种方法(Serializable,Parcelable)!</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/9991569">Android异步消息处理机制完全解析，带你从源码的角度彻底理解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/11711405">Android AsyncTask完全解析，带你从源码的角度彻底理解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.trinea.cn/android/android-performance-demo/">Android性能调优</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/17482095">Android Volley完全解析(一)，初识Volley的基本用法</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/17482165">Android Volley完全解析(二)，使用Volley加载网络图片</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/17612763">Android Volley完全解析(三)，定制自己的Request</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/guolin_blog/article/details/17656437">Android Volley完全解析(四)，带你从源码的角度理解Volley</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.cnblogs.com/plokmju/p/android_Handler.html">Android--多线程之Handler</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/hmg25/article/details/8100896">浅析android应用增量升级</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.trinea.cn/android/android-listview-display-error-image-when-scroll/">Android ListView滑动过程中图片显示重复错位闪烁问题解决</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/innost/article/details/9008691">Android系统性能调优工具介绍</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/innost/article/details/8474683">Android Wi-Fi Display（Miracast）介绍</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/innost/article/details/6055793">Android Looper和Handler分析</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/innost/article/details/6083467">Android MediaScanner 详尽分析</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/innost/article/details/6124685">Android深入浅出之Binder机制</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/android_tutor/article/details/5508615">Android 中自定义属性(attr.xml,TypedArray)的使用!</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://stormzhang.github.io/android/2014/05/16/android-screen-adaptation/">Android 屏幕适配</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/chenshijun0101/article/details/7045394">android WebView总结</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/shiqx429/article/details/3865581">构建自定义组件</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.importnew.com/8038.html">Android NDK介绍（上）</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.importnew.com/8052.html">Android NDK介绍（下）</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.importnew.com/9019.html">Android Service教程</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.importnew.com/895.html">通过多线程技术提高Android应用性能</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.importnew.com/1324.html">SQLite在Android中的使用</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.importnew.com/3784.html">Android性能优化案例研究(上)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://www.importnew.com/4065.html">Android性能优化案例研究（下）</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a target="_blank" href="http://blog.csdn.net/stonecao/article/details/6425019">Android AIDL使用详解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/singwhatiwanna/article/details/22597587">Android apk动态加载机制的研究</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/singwhatiwanna/article/details/17841165">Android属性动画深入分析：让你成为动画牛人</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://stormzhang.github.io/android/2013/07/30/android-custome-attribute-format/">Android中自定义属性格式详解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://stormzhang.github.io/androidtips/2014/08/24/android-viewfinder/">Android ViewFinder</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://stormzhang.github.io/android/2014/08/08/activity-fragment-lifecycle/">Android Activity/Fragment Lifecycle</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.cnblogs.com/slider/archive/2012/02/14/2351702.html">Android中内容观察者的使用---- ContentObserver类详解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://smallwoniu.blog.51cto.com/3911954/1248643">Android应用程序完全退出</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://gundumw100.iteye.com/blog/1338645">WebView实现文件下载功能</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/xieqibao/article/details/6682128">Android 之 远程图片获取和本地缓存</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/xieqibao/article/details/6567814">Android 之 Window、WindowManager 与窗口管理</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/ichliebephone/article/details/5916320">Android的设置界面及Preference使用</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/ekeuy/article/details/10971609">Android中获取View缩略图</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.cnblogs.com/xiaoxuetu/p/3411214.html">adb常用命令</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.jianshu.com/p/860bc2bf1a6a">
	Android ADB命令大全(通过ADB命令查看wifi密码、MAC地址、设备信息、操作文件、查看文件、日志信息、卸载、启动和安装APK等)</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/liuhe688/article/details/6679879">使用ANT打包Android应用</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.cnblogs.com/youxilua/archive/2013/05/20/3087935.html">用Gradle 构建你的android程序</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/xyz_lmn/article/details/16826669">Android 触摸及手势操作GestureDetector</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.tutorialspoint.com/android/android_gestures.htm">Android Gestures Tutorial</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.trinea.cn/android/performance/">性能优化系列总篇</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/stonecao/article/details/6425019">Android AIDL使用详解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/android_tutor/article/details/6427680">Android 中的AIDL!!!</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/way_ping_li/article/details/7927273">Android将程序崩溃信息保存本地文件</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/liuhe688/article/details/6584143">Android中处理崩溃异常</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/guolin_blog/article/details/8830286">Android官方提供的支持不同屏幕大小的全部方法</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/shulianghan/article/details/19698511">Android屏幕适配解析</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.trinea.cn/android/android-plugin/">Android 插件化</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://qiang106.iteye.com/blog/1830416">android NDK开发、编译、调试环境搭建与操作入门</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.importnew.com/8038.html">Android NDK介绍（上） </a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.importnew.com/8052.html">Android NDK介绍（下） </a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.importnew.com/3784.html">Android性能优化案例研究(上) </a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.importnew.com/4065.html">Android性能优化案例研究（下） </a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.importnew.com/895.html">通过多线程技术提高Android应用性能</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.cnblogs.com/hibraincol/archive/2011/05/30/2063847.html">Android: NDK编程入门笔记</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.cnblogs.com/vanezkw/archive/2012/07/19/2599092.html">.9 图片讲解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://blog.csdn.net/nokiaguy/article/details/5509638">Android中的长度单位详解（dp、sp、px、in、pt、mm）</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.cnblogs.com/hanyonglu/archive/2012/03/28/2420515.html">Android开发之InstanceState详解</a><br />
	</span>
</p>
<p>
	<span style="font-size:18px;"><a href="http://www.cnblogs.com/hanyonglu/archive/2012/03/26/2417278.html">Android开发之Intent.Action</a><br />
	</span>
</p>
<p>
	<a href="http://www.cnblogs.com/TerryBlog/archive/2010/07/09/1774475.html"><span style="font-size:18px;">Android OpenGL 学习笔记</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/ithomer/article/details/6727581"><span style="font-size:18px;">Android APK反编译详解（附图）</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/ithomer/article/details/6727640"><span style="font-size:18px;">Android如何防止apk程序被反编译</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/xn4545945/article/details/9001097"><span style="font-size:18px;">Android开发---摇一摇功能</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/singwhatiwanna/article/details/9054001"><span style="font-size:18px;">android中图片的三级cache策略（内存、文件、网络）</span></a><br />
	
</p>
<p>
	<a href="http://sunbofu.blog.51cto.com/6431507/1280441"><span style="font-size:18px;">整理一下Android中的ListView</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/newcj/archive/2011/05/30/2061370.html"><span style="font-size:18px;">Android 中的 Service 全面总结</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/ekeuy/article/details/39252199"><span style="font-size:18px;">Android：通过SpannableString为TextView设置丰富的显示效果</span></a><br />
	
</p>
<p>
	<a href="http://lanyan-lan.iteye.com/blog/1561500"><span style="font-size:18px;">android strings.xml转义字符</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/34093441"><span style="font-size:18px;">Android照片墙完整版，完美结合LruCache和DiskLruCache</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/28863651"><span style="font-size:18px;">Android DiskLruCache完全解析，硬盘缓存的最佳方案</span></a><br />
	
</p>
<p>
	<a href="http://www.itzhai.com/android-to-parse-json-data-using-gson.html"><span style="font-size:18px;">Android中使用Gson解析JSON数据</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/raymond19840709/archive/2008/06/26/1230289.html"><span style="font-size:18px;">JXL操作Excel</span></a><br />
	
</p>
<p>
	<a href="http://lanhuidong.iteye.com/blog/1553532"><span style="font-size:18px;">java操作excel——jxl和poi比较</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/liuhe688/article/details/6532519"><span style="font-size:18px;">详解Android中AsyncTask的使用</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/liuhe688/article/details/6415593"><span style="font-size:18px;">Android中解析XML</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/liujian885/article/details/5404834"><span style="font-size:18px;">Android中如何修改系统时间（应用程序获得系统权限）</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/joshua_yu/article/details/6563587"><span style="font-size:18px;">Android推送通知指南</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/38083103"><span style="font-size:18px;">Android数据库高手秘籍(零)——前言</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/38461239"><span style="font-size:18px;">Android数据库高手秘籍(一)——SQLite命令</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/38556989"><span style="font-size:18px;">Android数据库高手秘籍(二)——创建表和LitePal的基本用法</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/39151617"><span style="font-size:18px;">Android数据库高手秘籍(三)——使用LitePal升级表</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/39207945"><span style="font-size:18px;">Android数据库高手秘籍(四)——使用LitePal建立表关联</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/39345833"><span style="font-size:18px;">Android数据库高手秘籍(五)——LitePal的存储操作</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/40083685"><span style="font-size:18px;">Android数据库高手秘籍(六)——LitePal的修改和删除操作</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/40153833"><span style="font-size:18px;">Android数据库高手秘籍(七)——体验LitePal的查询艺术</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/guolin_blog/article/details/40614197"><span style="font-size:18px;">Android数据库高手秘籍(八)——使用LitePal的聚合函数</span></a><br />
	
</p>
<p>
	<a href="http://bxbxbai.github.io/2014/09/14/android-working-with-volley/"><span style="font-size:18px;">Android库Volley的使用介绍</span></a><br />
	
</p>
<p>
	<a href="http://blog.isming.me/2014/08/01/android-push/"><span style="font-size:18px;">android推送技术方案与原理</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/wangkuifeng0118/article/details/7368157"><span style="font-size:18px;">android弹出窗口的实现（PopupWindow）</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/wangjinyu501/article/details/8169924"><span style="font-size:18px;">Android ViewPager使用详解</span></a><br />
	
</p>
<p>
	<a href="http://stormzhang.github.io/android/2014/09/22/onsaveinstancestate-and-onrestoreinstancestate/"><span style="font-size:18px;">onSaveInstanceState & onRestoreInstanceState</span></a><br />
	
</p>
<p>
	<a href="http://www.trinea.cn/android/android-boot_completed-not-work/"><span style="font-size:18px;">Android应用如何开机自启动、自启动失败原因</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/sunzn/archive/2013/02/13/2910906.html"><span style="font-size:18px;">Android 编程下监视应用程序的启动</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/plokmju/p/android_Matrix.html"><span style="font-size:18px;">Android--Matrix图片变换处理</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/plokmju/p/android_SoundPool.html"><span style="font-size:18px;">Android--SoundPool</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/sunzn/p/3854009.html"><span style="font-size:18px;">Android 编程下设置 Activity 切换动画</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/shulianghan/article/details/19913755"><span style="font-size:18px;">【Android 应用开发】Android资源文件 - 使用资源存储字符串 颜色 尺寸 整型 布尔值 数组</span></a><br />
	
</p>
<p>
	<a href="http://www.androidlearner.net/use-strictmode.html"><span style="font-size:18px;">android 使用StrictMode诊断程序性能</span></a><br />
	
</p>
<p>
	<a href="http://www.robotium.cn"><span style="font-size:18px;">Android自动化测试框架Robotium</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/angeldevil/archive/2012/04/08/2437747.html"><span style="font-size:18px;">android中layout_weight的理解</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/hdhd588/article/details/6739281"><span style="font-size:18px;">APK安装过程及原理详解</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/yuanzeyao/article/details/38174537"><span style="font-size:18px;">Android解耦库EventBus的使用和源码分析</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/wirelessqa/article/details/14222041"><span style="font-size:18px;">Android Studio入门指南 (历上最全，全球首发)</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/esrichina/p/3347036.html"><span style="font-size:18px;">Android中使用SQLiteOpenHelper管理SD卡中的数据库</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/ryantang03/article/details/7831826"><span style="font-size:18px;">二维码、条形码扫描——使用Google ZXing</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/xxdddail/article/details/20522233"><span style="font-size:18px;">Android之查看Wifi密码</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/linmiansheng/article/details/18763987"><span style="font-size:18px;">（Android动画曲线）关于Property Animation的TimeInpolator和TypeEvaluator</span></a><br />
	
</p>
<p>
	<a href="http://ikinglai.blog.51cto.com/6220785/1225309"><span style="font-size:18px;">Android下使用SoundTouch实现变声并转为wav格式播放（山寨汤姆猫）</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/duguang77/article/details/18230867"><span style="font-size:18px;">BaseAnimation是基于开源的APP,致力于收集各种动画效果(最新版本1.3)</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/sunzn/archive/2013/05/10/3064129.html"><span style="font-size:18px;">Android 编程下 Touch 事件的分发和消费机制</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/sunzn/archive/2013/02/12/2910350.html"><span style="font-size:18px;">Android 编程下快捷图标的创建</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/sunzn/archive/2013/01/27/2878377.html"><span style="font-size:18px;">Android 编程下 SQLite 大数据量操作优化</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/wwj_748/article/details/10079311"><span style="font-size:18px;">Android开源框架ImageLoader的完美例子</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/minimicall/article/details/39484493"><span style="font-size:18px;">Android开发之多级下拉列表菜单实现（仿美团，淘宝等）</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/yellowcath/article/details/27527275"><span style="font-size:18px;">Android 自绘TextView解决提前换行问题，支持图文混排</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/youxilua/archive/2011/11/12/2246576.html"><span style="font-size:18px;">android实用测试方法之Monkey与MonkeyRunner</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/youxilua/archive/2011/09/16/2178554.html"><span style="font-size:18px;">JNI 学习笔记</span></a><br />
	
</p>
<p>
	<a href="http://blog.danlew.net/2014/03/30/android-tips-round-up-part-1/"><span style="font-size:18px;">Android Tips Round-Up, Part 1</span></a><br />
	
</p>
<p>
	<a href="http://blog.danlew.net/2014/04/14/android-tips-round-up-part-2/"><span style="font-size:18px;">Android Tips Round-Up, Part 2</span></a><br />
	
</p>
<p>
	<a href="http://blog.danlew.net/2014/04/28/android-tips-round-up-part-3/"><span style="font-size:18px;">Android Tips Round-Up, Part 3</span></a><br />
	
</p>
<p>
	<a href="http://blog.danlew.net/2014/05/12/android-tips-round-up-part-4/"><span style="font-size:18px;">Android Tips Round-Up, Part 4</span></a><br />
	
</p>
<p>
	<a href="http://blog.danlew.net/2014/05/28/android-tips-round-up-part-5/"><span style="font-size:18px;">Android Tips Round-Up, Part 5</span></a><br />
	
</p>
<p>
	<a href="http://www.williamlong.info/archives/2918.html"><span style="font-size:18px;">为什么Android没有iOS那么顺滑</span></a><br />
	
</p>
<p>
	<a href="https://medium.com/google-developer-experts/event-driven-programming-for-android-part-i-f5ea4a3c4eab"><span style="font-size:18px;">Event-driven programming for Android (part I)</span></a><br />
	
</p>
<p>
	<a href="https://medium.com/google-developer-experts/event-driven-programming-for-android-part-ii-b1e05698e440"><span style="font-size:18px;">Event-driven programming for Android (part II)</span></a><br />
	
</p>
<p>
	<a href="https://medium.com/google-developer-experts/event-driven-programming-for-android-part-iii-3a2e68c3faa4"><span style="font-size:18px;">Event-driven programming for Android (part III)</span></a><br />
	
</p>
<p>
	<a href="http://www.developerphil.com/dont-store-data-in-the-application-object/"><span style="font-size:18px;">Don't Store Data in the Application Object</span></a><br />
	
</p>
<p>
	<a href="http://cyrilmottier.com/2014/08/26/putting-your-apks-on-diet/"><span style="font-size:18px;">Putting Your APKs on Diet</span></a><br />
	
</p>
<p>
	<a href="http://droidyue.com/blog/2014/12/07/differences-between-stack-and-heap-in-java/"><span style="font-size:18px;">Java中的堆和栈的区别</span></a><br />
	
</p>
<p>
	<a href="https://code.facebook.com/posts/485459238254631/improving-facebook-on-android/"><span style="font-size:18px;">Improving Facebook on Android</span></a><br />
	
</p>
<p>
	<a href="http://droidyue.com/blog/2015/01/18/deal-with-touch-icon-in-android/"><span style="font-size:18px;">Android中处理Touch Icon的方案</span></a><br />
	
</p>
<p>
	<a href="http://www.bongizmo.com/blog/android-resources-each-developer-should-know/"><span style="font-size:18px;">Resources every Android developer must know</span></a><br />
	
</p>
<p>
	<a href="http://stackoverflow.com/questions/218384/what-is-a-null-pointer-exception-and-how-do-i-fix-it"><span style="font-size:18px;">What is a Null Pointer Exception, and how do I fix it?</span></a><br />
	
</p>
<p>
	<a href="https://code.facebook.com/posts/879498888759525/fast-rendering-news-feed-on-android/"><span style="font-size:18px;">Fast Rendering News Feed on Android</span></a><br />
	
</p>
<p>
	<a href="http://lucasr.org/2012/04/05/performance-tips-for-androids-listview/"><span style="font-size:18px;">Performance Tips for Android’s ListView</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/dengshengjin2234/article/details/8502097"><span style="font-size:18px;">Android涉及到的设计模式</span></a><br />
	
</p>
<p>
	<a href="https://www.bignerdranch.com/blog/customizing-android-listview-rows-subclassing/"><span style="font-size:18px;">Customizing Android ListView Rows by Subclassing</span></a><br />
	
</p>
<p>
	<a href="http://blog.aaapei.com/article/2015/02/facebookxin-wen-ye-listviewyou-hua"><span style="font-size:18px;">facebook新闻页ListView优化</span></a><br />
	
</p>
<p>
	<a href="https://github.com/android-cn/android-discuss/issues/54"><span style="font-size:18px;">请问平时开发过程中，你是如何做到多分辨率适配的？平板和手机可以共用一套代码吗？</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/love_xsq/article/details/44616925"><span style="font-size:18px;">Android ocr识别文字介绍</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/tianjian4592/article/details/44222565"><span style="font-size:18px;">自定义view实现水波纹效果</span></a><br />
	
</p>
<p>
	<a href="https://github.com/android-cn/android-discuss/issues"><span style="font-size:18px;">Android常见面试问题讨论</span></a><br />
	
</p>
<p>
	<a href="http://isux.tencent.com/introduction-of-webp.html"><span style="font-size:18px;">WebP 探寻之路</span></a><br />
	
</p>
<p>
	<a href="http://hukai.me/android-tips-for-reduce-apk-size/"><span style="font-size:18px;">Android APK安装包瘦身</span></a><br />
	
</p>
<p>
	<a href="http://my.oschina.net/xesam/blog/143985"><span style="font-size:18px;">自定义ADT模板</span></a><br />
	
</p>
<p>
	<a href="http://zmywly8866.github.io/2015/04/06/decrease-apk-size.html"><span style="font-size:18px;">关于APK瘦身值得分享的一些经验</span></a><br />
	
</p>
<p>
	<a href="http://stormzhang.com/android/2015/03/29/android-support-library/"><span style="font-size:18px;">Android Support兼容包详解</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/ekeuy/article/details/44974549"><span style="font-size:18px;">Android xml资源文件中@、@android:type、@*、？、@+含义和区别</span></a><br />
	
</p>
<p>
	<a href="http://www.zcool.com.cn/article/ZNjI3NDQ%3D.html"><span style="font-size:18px;">Android屏幕适配知识大全（美工与程序员须知）</span></a><br />
	
</p>

<p>
	<a href="http://yanbober.github.io/timeline.html"><span style="font-size:18px;">Android开发系列干货博客</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/u013045971/article/details/41984879"><span style="font-size:18px;">Android 闪电效果 (Electric Screen,电动屏幕)</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/u013045971/article/details/42016501"><span style="font-size:18px;">Android 碎屏效果 (Crack Screen,击碎屏幕)</span></a><br />
	
</p>
<p>
	<a href="https://github.com/simple-android-framework/android_design_patterns_analysis"><span style="font-size:18px;">Android源码设计模式分析项目</span></a><br />
	
</p>
<p>
	<a href="http://www.cnblogs.com/qianxudetianxia/category/312863.html"><span style="font-size:18px;">Android设计模式系列</span></a><br />
	
</p>
<p>
	<a href="http://www.tianmaying.com/tutorial/AndroidMVC"><span style="font-size:18px;">Android App的设计架构</span></a><br />
	
</p>
<p>
	<a href="http://blog.csdn.net/gemmem/article/details/13017999"><span style="font-size:18px;">Android内存泄漏分析</span></a><br />
	
</p>

&emsp;&emsp;更多原创文章和优质资源请关注公众号：

![微信公众号](http://ww1.sinaimg.cn/large/6d17e381gw1f6jts23i3aj207607674t.jpg)

&emsp;&emsp;更多Android技术资源交流请加群，本群的建群宗旨是分享优质的Android技术资源。群成员可以自由分享任何Android方面的技术资源和文章，并会不定期总结成文章方便大家阅读。若二维码过期或者群满，请加群助理微信：Jf-1994，并注明原因是加Android技术资源交流群：

![Android技术资源交流群](http://ww4.sinaimg.cn/large/6d17e381gw1f6jtrrudr1j20gy0ih76f.jpg)


&emsp;&emsp;Android开发在国内越来越成熟，网上的资源也越来越多，质量却参差不齐。对于忙于学习或工作、很少有自由时间的我们，需要的是省去不必要筛选信息的环节，直接获取更优质的资源。结合我们团队平时获取技术信息的途径，把优质的Android技术资源整理出来，让更多地人看到，同时也建了一个“Android技术资源交流群”（详见文末），方便各位同学实时分享更优质的资源，共同学习进步。


## 精选技术日/周报

- [Android开发技术周报](http://www.androidweekly.cn/)

- [Android博客周刊](http://www.androidblog.cn/)

- [移动开发前线](http://mobilefrontier.github.io/archives/)

- [Android Weekly](http://androidweekly.net/)

- [平安金融科技移动开发周报](https://github.com/PaicHyperionDev/MobileDevWeekly)

- [干货集中营](http://gank.io/)

- [Android 周报](http://www.race604.com/tag/android-weekly/)

- [App开发日报](http://app.memect.com/)

- [ANDROIDDEV DIGEST](https://www.androiddevdigest.com/)

## 精选技术资讯

- [极客头条](http://geek.csdn.net/forum/65)

- [掘金](https://gold.xitu.io)

- [开发者头条](http://toutiao.io/)

- [推酷](http://www.tuicool.com/)

- [干货集中营](http://gank.io/)

## 精选技术视频

- [InfoQ演讲](http://www.infoq.com/cn/presentations)

- [阿里技术沙龙](http://club.alibabatech.org/)

- [Android Performance Patterns](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc9CBxr3BVjPTPoDPLdPIFCE)

- [腾讯大讲堂讲座视频](http://djt.qq.com/videos/)

## 精选面试题

- [skillgun](http://skillgun.com/android/interview-questions-and-answers)

- [Android Discuss](https://github.com/android-cn/android-discuss/issues)

- [Learning Notes](https://github.com/GeniusVJR/LearningNotes)

- [牛客网](https://www.nowcoder.com/)

## 精选资料集

- [androidcat](http://androidcat.com/)

- [awesome-android-complete-reference](https://github.com/amitshekhariitbhu/awesome-android-complete-reference)

- [codekk](http://p.codekk.com/)

- [android-open-project-analysis](https://github.com/android-cn/android-open-project-analysis)

- [free-programming-books-zh_CN](https://github.com/justjavac/free-programming-books-zh_CN)

- [java-design-patterns](https://github.com/iluwatar/java-design-patterns)

- [开发技术前线](http://www.devtf.cn/?cat=2)

- [Android-Dev-Favorites](https://github.com/ruijun/Android-Dev-Favorites)

- [Android_Data](https://github.com/Freelander/Android_Data)

- [Andriod-collect-blogs](https://github.com/ZQiang94/Andriod-collect-blogs)

- [awesome-android-performance](https://github.com/Juude/awesome-android-performance)

- [awesome-android-tips](https://github.com/jiang111/awesome-android-tips)

- [BookResource](https://github.com/mobileperfman/BookResource)

- [AndroidSdkSourceAnalysis](https://github.com/LittleFriendsGroup/AndroidSdkSourceAnalysis)

- [AndroidDifficultAnalysis](https://github.com/ZhaoKaiQiang/AndroidDifficultAnalysis)

- [android-tech-frontier](https://github.com/hehonghui/android-tech-frontier)

- [android_design_patterns_analysis](https://github.com/simple-android-framework/android_design_patterns_analysis)

- [Worth-Reading-the-Android-technical-articles](https://github.com/zmywly8866/Worth-Reading-the-Android-technical-articles)

- [Android开发经验谈](http://www.jianshu.com/collection/5139d555c94d)

- [Android知识](http://www.jianshu.com/collection/3fde3b545a35)

- [AndroidGuide](https://github.com/ColorfulCat/AndroidGuide)

## 精选知乎专栏

- [Android 科学院](https://zhuanlan.zhihu.com/andlib)

- [安卓同学](https://zhuanlan.zhihu.com/androidmate)

- [中二病也要开发ANDROID](https://zhuanlan.zhihu.com/kaede)

- [Android 还可以这样开发](https://zhuanlan.zhihu.com/kotandroid)

- [Android 科学院](https://zhuanlan.zhihu.com/andlib)

- [AndroidDeveloper](https://zhuanlan.zhihu.com/stormzhang)

- [张明云的知识共享](https://zhuanlan.zhihu.com/zmywly8866)

## 开源项目库

- [android-open-project](https://github.com/Trinea/android-open-project)

- [23code](http://www.23code.com/)

- [Android Arsenal](http://android-arsenal.com/)

- [Android Libraries and Resources](http://alamkanak.github.io/android-libraries-and-resources/)

- [泡在网上的日子](http://www.jcodecraeer.com/plus/list.php?tid=31)

- [appance](http://www.appance.com/category/android/)

## 精选工具

- [AndroidDevTools](http://www.androiddevtools.cn/)

- [在线工具](http://tool.oschina.net/)

- [APIStore](http://apistore.baidu.com/)

## 精选浏览器插件

- [后台接口调试工具：Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)

- [Github项目结构化：Octotree](https://chrome.google.com/webstore/detail/octotree/bkhaagjahfmjljalopjnoealnfndnagc?hl=ja)

- [网页标尺：Page Ruler](https://chrome.google.com/webstore/detail/page-ruler/jlpkojjdgbllmedoapgfodplfhcbnbpn?hl=zh-CN)

- [文章图片上传工具：微博是个好图床](https://chrome.google.com/webstore/detail/%E5%9B%B4%E8%84%96%E6%98%AF%E4%B8%AA%E5%A5%BD%E5%9B%BE%E5%BA%8A/pngmcllbdfgmhdgnnpfaciaolgbjplhe?hl=zh-CN)

- [Axure文档查看工具](https://chrome.google.com/webstore/detail/axure-rp-extension-for-ch/dogkpdfcklifaemcdfbildhcofnopogp?hl=zh-CN)

&emsp;&emsp;文档会持续更新，请关注：[Worth-Reading-the-Android-technical-articles](https://github.com/zmywly8866/Worth-Reading-the-Android-technical-articles)

&emsp;&emsp;更多原创文章和优质资源请关注公众号：

![微信公众号](http://ww1.sinaimg.cn/large/6d17e381gw1f6jts23i3aj207607674t.jpg)

&emsp;&emsp;更多Android技术资源交流请加群，本群的建群宗旨是分享优质的Android技术资源。群成员可以自由分享任何Android方面的技术资源和文章，并会不定期总结成文章方便大家阅读。若二维码过期或者群满，请加群助理微信：Jf-1994，并注明原因是加Android技术资源交流群：

![Android技术资源交流群](http://ww4.sinaimg.cn/large/6d17e381gw1f6jtrrudr1j20gy0ih76f.jpg)
