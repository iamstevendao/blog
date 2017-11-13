--- 
layout: default 
---

<div class="home">
    {% for post in site.posts %}
    <article class="post">
      <header>
       <a href="{{ post.url | prepend: site.baseurl }}">
	       <h1>{{ post.title }}</h2>
       </a>
       <time datetime="{{ post.date | date: " %Y-%m-%d " }}">
         {{ post.date | date: "%-d %B %Y" }}
       </time>
      </header>
      <div class="body">
        {{post.content}}
      </div>
    </article>
    {% endfor %}
</div>