---
layout: page
title:  wordpress自动保存导致的问题
category: wordpress
tags:
keywords:
description:
published:  true
---

wordpress自动保存导致的问题:

wordpress 连接丢失。在连接恢复前，保存将无法进行。 以防万一，此文章已在您的浏览器中进行备份。

解决办法，关闭掉自动保存功能。

## 方法1：禁用自动保存功能（治本）

WordPress拥有自动保存功能，每2分钟就会自动存一份草稿。这无疑是个好功能，但自动保存会导致对数据库的频繁访问，并增加服务器压力，尤其是对小服务器来说更是如此。在文章编辑半途中，尤其是当文章内容量较大的时候，出现这个问题基本是这个原因。解决方法是调高文章自动保存的频率，或直接禁用（个人建议禁用，很少会直接在Wordpress里写文章吧……）


另外，还建议你关闭Wordpress中的修订功能，除非你用这个做Wiki之类的网站，否则历史版本保留的必要性也几乎没有。对于纯粹的博客来说，就更没有必要了，与其定期用数据库清理插件进行清扫，不如直接关掉： 


。 禁用方法：在functions.php中插入以下代码： 

//以下是我自己的加的 在程序出问题的时候可以删除掉
//禁用自动保存
```php
add_action('wp_print_scripts','disable_autosave');
function disable_autosave(){
wp_deregister_script('autosave');
}
//关闭Wordpress中的修订功能
add_filter( 'wp_revisions_to_keep', 'specs_wp_revisions_to_keep', 10, 2 );
function specs_wp_revisions_to_keep( $num, $post )
{
return 0;
}
```
完成后保存，尝试编辑文章即可。 当然，如果你购买的是中大型服务器，仍出现这类问题，请检查防火墙日志。如果出现拒绝数据库访问的记录，请联系服务器提供商设置白名单。

## 方法2：重启服务器（治标）

同样是因为修订文章、上传图片、读取标签等频繁访问和修改数据库而导致连接丢失，甚至500错误的时候，可采取重启服务器的方式。在此之前，您可以把文章正文编辑完毕（上传图片功能不可用，如果使用图床就没问题），切换到文本编辑模式，复制文章的代码备份到本地txt文件。 之后重启服务器即可，重启完成后贴入代码，重新修改标签、分类等属性发布。 从文本编辑模式复制出的代码，也可以通过制作XML文件通过导入功能输入Wordpress。具体请参见Wordpress的工具→导入和工具→导出选项里的介绍（导出一份替换内容再导入即可）。

## 方法3：插件检查（治本） 

部分插件，以及访问第三方服务器的同步类插件会导致这个问题。目前发现的插件列表：

WP-新浪登录插件

百度 Sitemap 插件

WPJAM

WP-QINIU

其他第三方同步/图床等服务也可能出现这个问题。解决方法如下： 登陆FTP页面，打开/wp-content目录，将plugins文件夹改名为plugin。 登陆wordpress检查，若没有问题，将plugin内可能访问外网或涉及到缓存的插件移出，并改名回plugins。 逐个移回上述插件，测试问题。 请优先保障插件为最新版本或稳定版本，如果有涉及到网站外观的插件（比如Aksmet类和CSS类插件）请优先还原，以防网站外观改变。 

## 方法4：直接检查500错误（治本

由于上述问题出现后，主机一般会进入10-30分钟的假死状态，在此期间访问会返回500错误（若为其他错误，如504错误，请检查网络问题和服务器运行状态）。  
利用这个500错误，可以深挖网站瘫痪原因。 登陆FTP页面，将根目录下的wp-config.php下载到本地并打开 替换 define('WP_DEBUG', false);  
为define('WP_DEBUG', true); 添加ini_set(‘display_errors’,’Off’); 保存，替换FTP内的wp-config.php。  
访问网站 观察返还值，如出现Error establishing a database connection，说明为数据库瘫痪，重启数据库即可解决。  
若为其他值可自行搜索错误原因。 解决问题后，还原wp-config.php文件

## 方法5：检查服务器状态/升级服务器（治本）

服务器状态会导致文章保存错误，或500错误。请进入主机管理界面，查看主机状态，主要检查内容包括： 网页空间、数据库空间的余量；   
主机的CPU和内存的占用情况； 主机的流量余量。 说实在的，换个好主机，可以解决很多问题（……）。  
站长曾使用过西部数码、美团云、阿里云等主机，后两者的稳定性比前者优秀很多。前者最糟糕的时候一天可以收到近10个异常通知（云监测服务的报警）。  
