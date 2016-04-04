---
layout: post
title: API calls with AngularJS
---

####How it all started:
Few days ago I wanted to learn [AngularJS](https://angularjs.org/). So I decided to start on building a small app with [Hacker News](https://news.ycombinator.com/) [APIs](https://github.com/HackerNews/API).
So I started with the [Andular Seed Project](https://github.com/angular/angular-seed). I read a little bit in the angularjs documentation to get a little idea.

####Objective:
Render the top 20 `new stories` and `top stories` from HackerNews.
***

####How I realized the problem:
However very soon I was stuck with making an API call. Most of the examples I felt were not complete. Countless googling, reading blogs and a few days later I figured why where I went wrong. The problem was that none of the examples I found made it extremely simple for a newbie user. I for one ain't an expert in Javascript. I know nothing about a front-end framework.

Assuming you have read about [Controllers](https://docs.angularjs.org/guide/controller), [Factories](https://docs.angularjs.org/guide/providers) and [Services](https://docs.angularjs.org/guide/services) I will skip them. if you have not read about them I suggest you do read them first.


####How I solved it:
I used the [angular-seed](https://github.com/angular/angular-seed) project to get started. It uses bower and npm together. In case you are not aware of bower, its just another package manager in the incredibly fragmented Javascript world.
To declare any element as `ng-app` means that element is the portion of the code where the angular application can perform any action.
<br>I declare my ng-app with the <html> tag like this:
<br>
```html
<html lang="en" ng-app="myApp" class="no-js">
```
I broke down the two views into view1 and view2 and used `ng-include` to inject them.
<br>

```html
<div class="container-fluid">
  <div class="col-md-10" ng-include="'./view1/view1.html'"></div>
  <div class="col-md-10" ng-include="'./view2/view2.html'"></div>
</div>
```
Going inside view1 which is a directory located in `app/view1` we have a `view1_test.js`, `view1.html` and `view1.js`. In this article we will focus on only `view1.html`. Lets dive into the code of `view1.html`.

```html
<div  ng-controller = "nsCtrl">
</div>
```

I also created two files called `controllers.js` and `factories.js` inside `app/js`. Its declared in `index.html` as

```html
<script src="app.js"></script>
<script src="js/controllers.js"></script>
<script src="js/factories.js"></script>
<script src="view1/view1.js"></script>
```
Initially I had written all the js code in `app.js`. However after managing to figure out things I decided to make the code a bit cleaner and separation of concerns.
<br>When the html code invokes the controller `nsCtrl` it runs the controller from `controllers.js`

```javascript
(function(){
  'use strict';

    var app = angular.module('myApp');

    app.
    //define controller
    // New Stories
    controller('nsCtrl',['nsFactory','$scope',function(nsFactory, $scope){
        nsFactory.getNewStories().then(function(response){
            $scope.nsitems = response; //Assign data received to $scope.data
        });
    }]);
})();
```

The `nsCtrl` controller calls `nsFactory` method `getNewStories`.


```javascript
(function(){
  'use strict';

    var app = angular.module('myApp');

    app
    // New Stories
    .factory('nsFactory',['$http',function($http){
        var ids = [];
        var nsList = [];
        return {
            getNewStories : function(){
                return  $http.get('https://hacker-news.firebaseio.com/v0/newstories.json?print=pretty').then(function(response){ //wrap it inside another promise using then
                            ids = response.data;
                            var getStoryDetails = function(id){
                                var u1 = 'https://hacker-news.firebaseio.com/v0/item/';
                                var u2 = id.toString();
                                var u3 = '.json?print=pretty';
                                var url = u1 + u2 + u3;
                                return  $http.get(url).then(function(response) { //wrap it inside another promise using then
                                        nsList.push(response.data);
                                        return response.data;  //only return friends
                                });
                            };
                            for(var i = 0; i < 20; i++){
                                getStoryDetails(ids[i]);
                            }
                            return nsList;
                });
        }}}])
})();
```

To get the list of new stories from hacker-news we call need to utilize two APIs. The first one returns an array  of new stories with id(with a length of 500). The second one with an id returns a story.
The `getNewStories` method calls the first API and then iterates through the array to get each story object and store it in an array called `nsList`. The factory functions then return the list to the controller using `$scope`.
In view1.html now we utilise the data received by the controller by inserting the following as shown.

```html
<div  ng-controller = "nsCtrl">
 <h3> New Stories </h3>
 <table class="table">
    <tr class="table-striped" ng-repeat = "nsitem in nsitems">
       <td> <a href="{{ nsitem.url }}"> {{ nsitem.title }}</a> </td>
    </tr>
 </table>
</div>
```

Finally, we use `ng-repeat` to iterate through the list and use each item to print the url and title.
I used the same concept in case of `view2` where I populate with `Top Stories`.

<br> For reference to the full code look at  <https://github.com/rmad17/angular-yc-news>.
