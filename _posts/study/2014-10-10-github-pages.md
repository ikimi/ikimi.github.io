---
layout: post
title: 使用Github Pages搭建个人博客
category: study
description: 使用Github Pages来搭建博客并不是什么新鲜事，找到一个心仪的主题后，如何通过Github Pages来快速搭建一个博客呢？他的好兄弟Jekyll可以帮上不少忙:)
---

越来越多的人使用[Github Pages][GP]来搭建个人博客。使用Github Pages搭建博客到底有哪些优势呢？

<ul>
    <li>无需自己配置服务器，博客所有内容均托管于Github</li>
    <li>通过简单配置即可绑定自己的域名</li>
    <li>本地配置<a href="http://jekyllcn.com/docs/home">jekyll</a>后即可预览博客</li>
    <li>使用标记语言，比如<a href="http://wowubuntu.com/markdown">Markdown</a></li>
    <li>博客与普通的git项目一样, 使用git add, git commit, git push...</li>
</ul>

对于如何从零开始创建git项目到配置github个人信息，以及如何使用Jekyll, Disqus推荐阅读[Beiyuu][Beiyuu]的文章[使用Github Pages建独立博客][BYGP]。本文余下内容将在此基础上介绍如何根据自己的需求修改博客源代码。

###ikimi.github.io目录结构
[ikimi.github.io][ikimi]基于Jekyll，所以其目录结构遵循Jekyll的基本结构，如：含有_config.yml, _includes, _layouts, _posts, _sites等。

[ikimi.github.io][ikimi]目录结构如下：

    |-- 404.html
    |-- about
    `--index.md
    |-- atom.xml
    |-- _config.yml
    |-- css
    |   |-- css3-ani.css
        `-- default.css
    |-- favicon.ico
    |-- images
    |-- index.md
    |-- js
    |   |-- css3-ani.js
        `-- jequry-1.7.1.min.js
        `-- post.js
        `-- prettify
    |-- _layouts
    |   |--defult.html
        `-- home.html
        `-- page.html
        `-- post.html
    |-- life
    `-- index.md
    |-- _posts
    |   |-- about
        `-- life
        `-- study
    |-- README.md
    |-- _sites
    |-- wiki.md

对于Jekyll包含的结构其意义已经在[使用Github Pages建独立博客][BYGP]中进行了介绍，下面着重介绍其它自定义的目录及文件，尤其是js, css目录。
####404.html
404页面，如果想自定义404页面可以修改此文件，当然也可以将其后缀名改为.md。
####atom.xml, wiki.md
博客atom和wiki内容
####favicon.ico
博客图标，你可以自定义博客logo然后取而代之，仅需要保证其符合favicon.ico像素要求（网上有很多生成.ico的应用）
####index.md
博客的内容分为3栏，从左到右依次为Study, Life, About。如果想要更改栏目的名称只需更改index.md中这段代码：

    <ul class="artical-cate">
        <li class="on"><a href="/"><span>Study</span></a></li>
        <li class="text-align:center"><a href="/life"><span>Life</span></a></li>
        <li class="text-align:right"><a href="/about"><span>About</span></a></li>
    </ul>

将上段代码中Study, Life, About改为你的名字，至于链接/, /life, /about的名字则需要在博客根目录下创建相应的目录。比如在我们的跟录下创建了life和about目录。因为博客默认显示Study栏，所以直接可以将Study链接到'/'下。
####life,about
life和about目录下仅有index.md,其作用是当链接进入life或about栏时，用来遍历所有分类归为life和about的文章。
####_posts
所有的博客文章,每篇文章开头都必须包含YAML头信息，例如：

    ---
    layout: post
    title: your title
    description: your description
    category: your blog's category
    ---

对于category用户可以自定义分类，例如我的blog共分为study, life和about3类。
####_layouts
这里包含所有的模板。

default.html是所有模板的基础，包括博客的作者，左上角的社交链接。如果你不更改的话都会链接到kimi袄~

post.html定义了每一篇博客的样式，代码如下:

    ---
    layout: default
    ---

    <link rel="stylesheet" href="/js/prettify/prettify.css" />
    <style type="text/css">
        body { background:#e8e8e8; }
        @media screen and (max-width: 750px){
         body { background:#fff; }
        }
        @media screen and (max-width: 1020px){
         body { background:#fff; }
     }
    </style>

    <div id="content">
        <div class="entry">
        <h1 class="entry-title"><a href="{ { page.url }}" title="{ { page.title }}">{{ page.title }}</a></h1>
        <p class="entry-date">{ { page.date|date:"%Y-%m-%d" }}</p>
        { { content }}

        <div id="disqus_container">
            <!--
            <div style="margin-bottom:20px" class="right">
                <script type="text/javascript" charset="utf-8">
                (function(){
                    var _w = 86 , _h = 16;
                    var param = {
                        url:location.href,
                        type:'6',
                        count:'', /**是否显示分享数，1显示(可选)*/
                        appkey:'', /**您申请的应用appkey,显示分享来源(可选)*/
                        title:'', /**分享的文字内容(可选，默认为所在页面的title)*/
                        pic:'', /**分享图片的路径(可选)*/
                        ralateUid:'2627982267', /**关联用户的UID，分享微博会@该用户(可选)*/
                        language:'zh_cn', /**设置语言，zh_cn|zh_tw(可选)*/
                        rnd:new Date().valueOf()
                    }
                    var temp = [];
                    for( var p in param ){
                        temp.push(p + '=' + encodeURIComponent( param[p] || '' ) )
                        }
                        document.write('<iframe allowTransparency="true" frameborder="0" scrolling="no" src="http://hits.sinajs.cn/A1/weiboshare.html?' + temp.join('&') + '" width="'+ _w+'" height="'+_h+'"></iframe>')
                        })()
                        </script>
            </div>
            -->
            <a href="#" class="comment" onclick="return false;">点击查看评论</a>
            <div id="disqus_thread"></div>
         </div>
        </div>

        <div class="sidenav">
            <iframe width="100%" height="75" class="share_self"  frameborder="0" scrolling="no" src="http://widget.weibo.com/weiboshow/index.php?language=&width=0&height=75&fansRow=2&ptype=1&speed=0&skin=5&isTitle=0&noborder=0&isWeibo=0&isFans=0&uid=2627982267&verifier=56a6017b&dpc=1"></iframe>
        </div>

        <div class="sidenav">
            <h2>Study</h2>
            <ul class="artical-list">
            { % for post in site.categories.study%}
            <li><a href="{ { post.url }}">{ { post.title }}</a></li>
            { % endfor %}
            </ul>

            <h2>Life</h2>
            <ul class="artical-list">
            { % for post in site.categories.life%}
            <li><a href="{ { post.url }}">{ { post.title }}</a></li>
            { % endfor %}
            </ul>

            <h2>About</h2>
            <ul class="artical-list">
            { % for post in site.categories.about%}
            <li><a href="{ { post.url }}">{ { post.title }}</a></li>
            { % endfor %}
            </ul>
        </div>
    </div>

在<code>div id="content"</code>输出文章的正文。紧接着在<code>div id="disqus_container"</code>调用Disqus评论引擎，对于Disqus如何更改成你自己的帐号，会在后面进行介绍。代码中有一段html代码被注释掉了，其作用是提供文章的新浪微博分享接口,如果想提供微博分享功能可以将这段代码注释取消，只需要将relateUid更改为你的微博id即可。

在第一个<code>div class="sidenav"</code>中，提供了微博秀的样式。可以访问[微博秀][wbShow]来生成微博秀代码，替换我的微博秀代码就可以了。

在第二个<code>div class="sidenav"</code>中，提供了分类文章列表，你只需要将分类的名字（这里是study, life, about）改为你自己的分类名就可以了。

page.html和home.html不需要额外更改。

####js
js目录下面包含所有的js脚本，我们关注的是post.js，其中包含了Disqus评论脚本。
在post.js中找到这行代码：

    window.disqus_shortname = 'ikimi';

将ikimi更改为你的disqus用户名即可，剩下的就尽情享受Disqus带来的方便体验吧:)

赶快动手，早弄早享受:)

[GP]: http://pages.github.com
[Beiyuu]: http://beiyuu.com
[BYGP]: http://beiyuu.com/github-pages/
[ikimi]: http://ikimi.github.io
[wbShow]: http://app.weibo.com/tool/weiboshow/
