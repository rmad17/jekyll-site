---
layout: post
title: API calls with AngularJS
---

Few days ago I wanted to learn [AngularJS](https://angularjs.org/). So I decided to start on building a small app with [Hacker News](https://news.ycombinator.com/) [APIs](https://github.com/HackerNews/API).
So I started with the [Andular Seed Project](https://github.com/angular/angular-seed). I read a little bit in the angularjs documentation to get a little idea.

However very soon I was stuck with making an API call. Most of the examples I felt were not complete. Countless googling, reading blogs and a few days later I figured why where I went wrong. The problem was that none of the examples I found made it extremely simple for a newbie user. I for one ain't an expert in Javascript. I know nothing about a front-end framework.

Assuming you have read about [Controllers](https://docs.angularjs.org/guide/controller), [Factories](https://docs.angularjs.org/guide/providers) and [Services](https://docs.angularjs.org/guide/services) I will skip them. if you have not read about them I suggest you do read them first.

Moving on, I used the [angular-seed](https://github.com/angular/angular-seed) project to get started. It uses bower and npm together. In case you are not aware of bower, its just another package manager in the incredibly fragmented Javascript world.
To declare any element as `ng-app` means that element is the portion of the code where the angular application can perform any action.
<br>I declare my ng-app with the <html> tag like this:
<br>`<html lang="en" ng-app="myApp" class="no-js">`
I broke down the two views into view1 and view2 and used `ng-include` to inject them.
<br>

```html
<div class="container-fluid">
  <div class="col-md-10" ng-include="'./view1/view1.html'"></div>
  <div class="col-md-10" ng-include="'./view2/view2.html'"></div>
</div>
```

<br> For reference to the full code of index.html look at  <https://github.com/rmad17/angular-yc-news/blob/master/app/index.html>.
