--- 
layout: page
title: Featured Posts
---
{% for post in site.categories.posts %}
<article class="post">
  <header>
   <a href="{{ post.url | prepend: site.baseurl }}">
     <h1 class="title">{{ post.title }}</h1>
   </a>
   <time datetime="{{ post.date | date: " %Y-%m-%d " }}">
     {{ post.date | date: "%-d %B %Y" }}
   </time>
  </header>
</article>
{% endfor %}
<div class="post diary-feed">
  <header>
    <h1 class="title">Diary Feed</h1>
  </header>
  {% for post in site.categories.diary %}
  <div class="feed">
    <header>
      <a href="{{ post.url | prepend: site.baseurl }}">
        <h3 class="title">{{ post.title }}</h3>
      </a>
      <time datetime="{{ post.date | date: " %Y-%m-%d " }}">
        {{ post.date | date: "%-d %B %Y" }}
      </time>
    </header>
   </div>
  {% endfor %}
</div>
