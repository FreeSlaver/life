---
layout: page
title:  关于静态网页的几个疑问？？
category: webmaster
tags:
keywords:
description:
published:  true
---

## js，css用CDN的好还是本地的？
用本地的，网络传输耗流量，对啊，但是这个请求的过程是怎样的？  
比如html里面引用了本地a.js,b.js这样，是会发2个请求吗？草，妈的，基础不牢，地动山摇啊，  
还是浏览器看域名一样合并成为一个请求？？之后拿到这两个文件在合并渲染？   
因为这里显然请求过来之后已经建立了连接，不会再去重新发起一个连接，应该就是浏览器和服务器说给我a.js，传输完成，
再给我b.js，对应该是这样，就是在一个请求里面，
如果是引用的CDN了？a.js，b.js是在2个不同的
哎，我现在的技术完全不支撑能够单干赚钱啊。  
股票他妈的也是差点火候，痛苦啊。也没人支持，哎。

## 比如现在有几十个资源请求，怎么做了？像现在做的导航，太正常不过了
如果全部存CDN，浏览器几十个跨域的http请求，浏览器多耗电脑的性能啊，而且一个地方对这多个地方的网速不一样，  
搞不好半天弄不到css，你这不就全乱了。。
全部放本机？那就是下载20M左右的东西把，第一次访问慢成翔，而且搞不好用户恶意刷新，ctrl+shift+r，你这流量不全被刷了？

## 还有这js，css是不是能压缩合并？
草，现在一个单页怎么弄？之前玩wp的时候我记得是有插件的，但是是怎么实现的？？？对啊，这些所有的压缩都是咋弄的？？
js，css我知道最有效最简单的方式是把空格全部去掉，但是比如把单个js搞成rar会更小吗？？草又是不知道。

做东西做东西，一定要去做东西，你不去做东西，不遇到问题，永远是撒都不会不问。不要老鸡儿百度，Google。
哎，为毛重来没思考过这些问题？？？因为之前没碰到过，就是一种封闭的状态，生活上一直在挨日子，哎。  
不过也是每天累的他妈的像个傻逼似得，996的福报只会把你变成傻逼。
## 怎么将这个html页面整个的缓存到内存中，或者说全量保存到CDN上面？
你想一想，是咋玩的，比如现在弄个Nginx（不要nginx行不行？又煞笔了，比如我现在开放了80端口，直接ip过去，   
那么应该要做的是开启这个服务器上的文件访问权限，这样就行了？不行，得处理http协议卧槽，那再问一个问题，  
你现在就在这个linux服务器上写个http协议处理的东西，怎么弄？哎，完全蒙蔽，抓瞎啊，理论上了说是：
就这么说把，比如你在linux上curl ip.me，得到你本机的ip，别人对方linux服务器是咋处理的？？不可能也假个nginx把？？

原来我们所有的自学能力，好奇心都被抹杀殆尽了。  
），监听80端口，然后指向到/var/www/html目录，  
直接把这个静态页面丢到这个目录下，用户访问，



## 为什么要用jekyll？？给我一个理由好吧！
我直接生成静态的html文件，不就行了，为什么要用jekyll？  
jekyll主要就是哪些插件，代码高亮啊，seo啊，sitemap啊之类的，翻页，目录啊之类的。。  
对，html文件之间还要链接起来，仅此而已。