---
layout: home
---

<div class="index-content project">
    <div class="section">
        <ul class="artical-cate">
            <li><a href="/"><span>Study</span></a></li>
            <li style="text-align:center"><a href="/life"><span>Life</span></a></li>
            <li class="on" style="text-align:right"><a href="/about"><span>About</span></a></li>
        </ul>

        <div class="cate-bar"><span id="cateBar"></span></div>

        <div class="cate-bar">
            <p>C/C++/PHP/Ruby 红魔死忠，喜欢地理，想成为Geek，却总不能坚持。<br/>
            欢迎去我的<a href="http://github.com/ikimi" target="_blank">Github</a>，并且<a href="mailto:jiajun.li13@gmail.com">"骚扰"kimi</a>。</p>
        </div>

        <ul class="artical-list">
        {% for post in site.categories.about %}
            <li>
                <h2>
                    <a href="{{ post.url }}">{{ post.title }}</a>
                </h2>
                <div class="title-desc">{{ post.description }}</div>
            </li>
        {% endfor %}
        </ul>
    </div>
    <div class="aside">
    </div>
</div>
