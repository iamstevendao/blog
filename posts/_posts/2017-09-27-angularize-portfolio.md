---
layout: post
title:  "Angularize my portfolio"
categories: general
---

This post will be about how I found that [AngularJS](https://angularjs.org/) is really a game-changing library for my [portfolio website](https://iamstevendao.github.io/portfolio/).

Straight to the point, this is how my timeline looks like in the website:

<p align="center">
<img alt="timeline" src="https://rzxtwq.bn1302.livefilestore.com/y4mDPcJDBKB1vx2OkD5N11BH7gJXOmkbdGV5TpzYGF6beWfpUk8HkHykNDsX9vT5oKpbxttcq7gInIYj9RnR3J_u1mv9wRBSIxd1M4KdYjlW5qzmR-BPzlTseaNWCbpwlSLIhznDc7TC-aBQyzM33tpt6cBD-6E9czINuY0RwF4KCr_nz0vMzZL8OcRqdEJA4tlfQFSaek4DmRFL4wTlUljMw?width=2058&height=1035&cropmode=none">
</p>

It looks really short and simple, but the code behind is not that short, it is far different from what the page displays.
For example, to have:

```cs
{ "June 2017" , "Software Developer & IT Support at Becas Technology" }, 
// Develop, implement applications, APIs for paging system solutions, 
// restaurant and gas station management. 
// Customer support with errors, bugs and required implementation. 
```

what I need to put in HTML:
```html
<!-- June 2017 -->
<p>

  <!-- { "June 2017" , "Software Developer & Customer Support at Becas Technology " },  -->
  <span class="cl-txt tab2 w-wk">{</span>
  <span class="cl-str w-wk">"</span>
  <span class="cl-str">June 2017</span>
  <span class="cl-str w-wk">"</span>
  <span class="cl-txt w-wk">,</span>
  <span class="cl-str w-wk">"</span>
  <span class="cl-str">Software Developer & Customer Support at
    <a target="_blank" href="http://www.becas.com.au/">
      <span class="link link-string">Becas Technology</span>
    </a>
    <span class="w-wk">"</span>
  </span>
  <span class="cl-txt w-wk">},</span>
  <br>

  <!-- /* Develop, implement applications, APIs for paging system solutions,	-->
  <span class="cl-cmt tab2 w-wk">/*</span>
  <span class="cl-cmt">Develop, implement applications, APIs for paging system solutions,</span>
  <br>

  <!-- restaurant and gas station management. */ -->
  <span class="cl-cmt tab2">restaurant and gas station management.</span>
  <span class="cl-cmt w-wk">*/</span>
  <br>

  <!-- // Customer support with errors, bugs and required implementation. -->
  <span class="cl-cmt tab2 w-wk">//</span>
  <span class="cl-cmt">Customer support with errors, bugs and required implementation.</span>
</p>
```

such a pain, isn't it. Moreover, I want to highlight what needs to be highlighted and make it outstanding from others, so I put class `w-wk` in those to change their opacity.

It also means that for **3 lines** in the website, I need around **30 lines** of code in my HTML file, it leads to the length of ~ **800 lines** of code in my HTML file (wow). AND, whenever I want to change a tiny part in my display, for example when I want to have a space between `{` and `"`, I need to change every part where it occurs at in the code.

All of a sudden, I need to prepare up AngularJS for my work.

#### That was how the game changed.

```html
<!-- Timeline Content -->
<p ng-repeat="event in timeline">
  <span class="cl-txt tab2 w-wk">{</span>
  <span class="cl-str"><span class="w-wk">"</span>{{event.date}}<span class="w-wk">"</span></span>
  <span class="cl-txt w-wk">,</span>
  <span class="cl-str"><span class="w-wk">"</span>{{event.job.title}}</span><a ng-if="event.job.hasOwnProperty('place')"
            ng-href="{{event.job.link}}">
  <span class="cl-str lk-str"> {{event.job.place}}</span></a><span class="w-wk cl-str">"</span>
  <span class="cl-txt w-wk">},</span>
  <br>
  <span class="cl-cmt" ng-repeat="desc in event.description">
    <span class="tab2 w-wk">//</span>
    <span>{{desc}}</span>
    <br>
  </span>
</p>
```

these lines are not short, kinda same with the previous one, BUT, it is for the **whole timeline**, by using `ng-repeat` at the front and [`$http.get`](https://docs.angularjs.org/api/ng/service/$http) in the back: 

```js
//timeline
$http.get('json/timeline.json')
  .then(function (res) {
    $scope.timeline = res.data;
  });
```

all I have to do now is to insert everything I want to insert in a **json file**, Angular will take care the rest

```json
[
  {
    "date": "September 2017",
    "job": {
      "title": "Embarrass myself by writing",
      "place": "blog",
      "link": "https://iamstevendao.github.io/blog"
    },
    "description": [
      "Feel like it is the biggest step in my life so far,",
      "I will work on any dumb things I come up with, and learn writing like other awesome people"
    ]
  },
  {
    "date": "June 2017",
    "job": {
      "title": "Software Developer & IT Support at",
      "place": "Becas Technology",
      "link": "http://www.becas.com.au/"
    },
    "description": [
      "Develop, implement applications, APIs for paging system solutions,",
      "restaurant and gas station management.",
      "Customer support with errors, bugs and required implementation."
    ]
  },
  ...
]
```

It is all the basic Angular stuff, but it is how my **index.html** file get reduced from **800 lines** into **300 lines** of code. And now, I even can think about changing theme for my portfolio without touching every part of the page.

That's all, how I angularized my portfolio webpage.