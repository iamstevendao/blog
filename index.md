--- 
layout: page
title: Featured Posts
---
<div class="home">
  {% for post in site.categories.posts %}
  <article class="preview">
    <header>
     <a href="{{ post.url | prepend: site.baseurl }}">
	     <h1>{{ post.title }}</h1>
     </a>
     <time datetime="{{ post.date | date: " %Y-%m-%d " }}">
       {{ post.date | date: "%-d %B %Y" }}
     </time>
    </header>
  </article>
  {% endfor %}
  <div class="feed-container">
    <header>
      <h1>Diary Feed</h1>
    </header>
    {% for post in site.categories.diary %}
    <div class="feed">
    <a href="{{ post.url | prepend: site.baseurl }}">
	      <h3>{{ post.title }}</h3>
      </a>
      <time datetime="{{ post.date | date: " %Y-%m-%d " }}">
        {{ post.date | date: "%-d %B %Y" }}
      </time>
      
     </div>
    {% endfor %}
  </div>
</div>
