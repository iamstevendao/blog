--- 
layout: default 
---

<div class="home">

  <h1 class="page-heading">Posts</h1>

  {{ content }}

  <div class="post-list">
    {% for post in site.posts %}
    <article class="post-preview">
      <a href="{{ post.url | prepend: site.baseurl }}">
	      <h2 class="post-title">{{ post.title }}</h2>
      </a>
      {% assign date_format = site.minima.date_format | default: "%b %-d, %Y" %}
      <p class="post-meta">{{ post.date | date: date_format }}</p>
    </article>
    {% endfor %}
  </div>

  <!-- <p class="rss-subscribe">subscribe <a href="{{ " /feed.xml " | relative_url }}">via RSS</a></p> -->
</div>