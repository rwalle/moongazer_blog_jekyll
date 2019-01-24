---
layout: article
title: 使用nginx建立WebDAV服务器
tags:
  - Uncategorized
date: 2014-08-11 01:57:09
intro: 使用nginx的http_dav_module搭建WebDAV服务器。
---

当时这个事情研究了差不多一个下午。。。花那么长时间的确是因为自己姿势水平不够，对nginx不够熟悉，不过为什么中文的相关资料这么少？是因为nginx用得比较少，还是没有建WebDAV的需求，还是这件事情太简单了都没人愿意说说？真是郁闷。

我的目标是使用nginx建立一个WebDAV服务器，可以进行上传下载等基本的操作，需要有基于用户名和密码的身份验证。由于HTTP访问时的根目录已经有内容了，需要将WebDAV服务器通过某个“子目录”来访问或者通过80以外的端口访问。主要参考了[官方文档](http://nginx.org/en/docs/http/ngx_http_dav_module.html)和《[nginx webdav配置](http://blog.csdn.net/jollyjumper/article/details/8977596)》这篇文章。

前期准备：如果是通过apt-get安装的nginx，默认已经包含了webdav模块。如果是通过源码编译的，需要在编译时候指定参数：

```shell
./configure --with-http_dav_module
```

通过webdav/这个“子目录”来访问：

<span id="more-81"></span>

```

location /webdav/ {
    alias /usr/share/webdav/;
    autoindex on;
    dav_methods PUT DELETE MKCOL COPY MOVE;
    dav_ext_methods PROPFIND OPTIONS;
    create_full_put_path on;
    dav_access user:rw group:r all:r;
    auth_basic &quot;Authorized Users Only&quot;;
    auth_basic_user_file /usr/lib/squid/passwd;
}

```

alias后面是文件所在位置，设置autoindex on是为了通过网页访问时可以直接显示索引；create_full_put_path官方的说明为“默认情况下，Put 方法只能在已存在的目录里创建文件。当然了Nginx 必须得有这个目录的修改和写入权限”； auth_basic是提示信息；auth_basic_user_file用于存放加密后的用户名和密码的文件，通过htpasswd命令生成。如果这个工具不存在的话需要安装apache2-utils。

通过端口号访问：新建一个server，其它的基本和前面一样：

```

server {
    listen 9234;
    error_page 404 /404;
    error_page 503 /503;

    location / {
        root /usr/share/webdav;
        autoindex on;
        dav_methods PUT DELETE MKCOL COPY MOVE;
        dav_ext_methods PROPFIND OPTIONS;
        create_full_put_path on;
        dav_access user:rw group:r all:r;
        auth_basic &quot;Authorized Users Only&quot;;
        auth_basic_user_file /usr/lib/squid/passwd;
    }
}

```