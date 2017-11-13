--- 
layout: default 
---
<article class="post" itemscope itemtype="http://schema.org/BlogPosting">
  <header>
    <h1>{{ page.title | escape}}</h2>
    <time datetime="{{ page.date | date: " %Y-%m-%d " }}">
      {{ page.date | date: "%-d %B %Y" }}
    </time>
  </header>
  <div class="body">
    {{content}}
  </div>
</article>