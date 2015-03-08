---
author: hesicong
comments: true
date: 2008-12-05 15:53:26+00:00
layout: post
slug: increase-godaddy-host-site-for-the-gzip-compression
title: 为godaddy主机站点增加gzip压缩
wordpress_id: 780
categories:
- 软件
---

一直觉得Wordpress访问速度很不理想，再加上godaddy主机在国外，更是雪上加霜了。最近用Firebug查看网页的流量发现载入的html页面，js脚本和css加起来很庞大，造成了很多不必要的开支。使用了Wordpres的插件WP Super Cache后，感觉还是不理想，主要是之压缩了php生成的页面，对js和css一直没有压缩。

从godaddy主机的php信息中得知，主机用的是apache 1.3.3，在网上找的很多教程都是关于apache 2.0的，试了试不管用。幸好主机支持.htaccess对访问进行控制，但试了几种方法依然不凑效。

经过反复实验，终于搞定，方法如下：

在博客的站点下建立一个.htaccess，添加如下规则：

```
RewriteCond %{REQUEST_FILENAME} -f
RewriteCond %{REQUEST_FILENAME} ^.*.(css|js)$
RewriteRule ^(.*)$ gzip.php?url=$1 [QSA,L]
```

编辑一个gzip.php

``` php
<?php

$allowed = array(
'css' => 'text/css',
'js' => 'text/javascript'
);

$file = isset($_GET['url']) ? $_GET['url'] : null;
$extension = explode('.', $file);
$extension = array_pop($extension);

if(isset($allowed[$extension]))
{
$pos = strpos($file, '..');
if ($pos === false && is_file($file))
{
@ob_start ('ob_gzhandler');
header("Content-type: {$allowed[$extension]}; charset: UTF-8");
readfile($file);
} else {
header('HTTP/1.1 404 Not Found');
}
}

?>
```
