---
layout: post
title: Apache + CodeIgniter去掉URL路径中的index.php
category: study
description: 使用Apache配合CodeIgniter搭建过web应用的盆友肯定都被URL中的index.php折磨过。这样的URL不仅每次访问都要加上index.php，最重要的是对我们的“审美”造成了极大的摧残。下面让我们一步一步把URL中的index.php去掉吧！
---

使用Apache + CodeIgniter搭建web应用时，在domain后面必须加上index.php，这样的URL看起来非常冗余而且不美观。下面我们就一步步把URL中的index.php去掉～

去掉URL中的index.php需要以下三步:

*   开启Apache Rewrite模块
*   修改Apache配置文件
*   在网站主目录配置.htaccess文件

####开启Apache Rewite模块####
Ubuntu安装Apache后，默认情况下在 <code>/etc/apache2/</code>目录下放置配置文件，其目录大致如下：

    |-- apache2.conf
    |-- envvars
    |-- magic
    |-- mods-enabled
    |-- mods-available
    `-- rewrite.load
    |-- sites-enabled
    `-- 000-default
    |-- sites-available
    |-- conf.d
    |-- httpd.conf
    |-- ports._conf

apache2.conf是apache的配置文件，linux下的apache将配置信息分解到几个文件中，通过在apache2.conf文件中<code>Include</code>指定配置文件实现，apache2.conf文件的最后一部分内容如下：

    # Include module configuration:
    Include mods-enabled/*.load
    Include mods-enabled/*.conf

    # Include all the user configurations:
    Include httpd.conf

    # Include ports listing
    Include ports.conf

    // 一部分其它内容

    # Include generic snippets of statements
    Include conf.d/

    # Include the virtual host configurations:
    Include sites-enabled/

从代码中我们可以看到apache2将配置文件分开了，其中<code>httpd.conf</code>是用户自定义配置（下面我们会用到），同时将加载的模块分到 <code>mods-enabled</code>中。

通过调用<code>phpinfo()</code>查看apache是否加载了Rewite模块。

![no rewrite](/images/CodeIgniter/no_write.png)

我们发现在Loaded Modules中没有加载Rewrite模块。
执行命令加载Rewrite模块：

    sudo ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enables/

重启apache后，重新调用<code>phpinfo()</code>发现Rewrite模块已经加载。

![rewrite](/images/CodeIgniter/write.png)

####修改Apache配置文件####
成功开启Apache的Rewrite模块后，我们要对Apache进行一些配置，因为mod_rewrite时针对于目录的，所以我们要告诉apache哪些目录需要rewrite，哪些目录不需要rewrite。打开httpd.conf配置文件，发现其为空！别急，其实配置文件已经被移动到<code>/etc/apache2/sites-enabled/000-default</code>中。打开配置文件，中间某段配置如下：

    <Directory /var/www>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride None
        Order allow,deny
        allow from all
    </Directory>

因为我的web应用放到了<code>/var/www</code>目录下，所以只需要配置<code>/var/www</code>部分就可以了，将<code>AllOverride None</code>改为<code>AllOverride All</code>保存退出即可~
####配置.htaccess文件####
回到web应用的根目录下，编辑.htaccess文件。

    RewriteEngine on
    RewriteBase /
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ /index.php/$1 [L]

如果你的web应用不是在<code>/var/www</code>根目录下，则需要将<code>ReWriteRule</code>配置成

    RewriteRule ^(.*)$ /your_folder/index.php/$1 [L]

That's all!
