$index

---

-------------

$index is a way to show which iteration or index of a loop in you’re in. For example

[Fiddle](http://jsfiddle.net/HB7LU/25934/)

HTML

```html
<div ng-controller="MyCtrl">
  <p ng-repeat="value in thinkster.split('')"> {{$index}} -> {{value}}</p>
</div>
```

JS

```js
var myApp = angular.module('myApp',[]);

function MyCtrl($scope) {
	$scope.thinkster = 'thinkster';
}
```

In the fiddle you can see that `$index` returns the index value of each letter. If you wanted to create a human friendly list, you can add one to the index like so:

[Fiddle Example](http://jsfiddle.net/HB7LU/25934/)

```html
<div ng-controller="MyCtrl">
  <p ng-repeat="value in thinkster.split('')"> {{$index + 1}} -> {{value}}</p>
</div>
```

---

$event
------

---

$event is a built in event listener that you can use [jQuery event properties](http://api.jquery.com/category/events/event-object/) on. For example:

[The Fiddle](http://jsfiddle.net/HB7LU/25939/)

HTML

```html
<div ng-controller="MyCtrl">
  <button 
    ng-repeat="value in thinkster.split('')"
    ng-click="ev = $event"
    > 
    {{$index + 1}} -> {{value}} 
    <br>
    Click X Value:
    {{ev.pageX}}
  </button>
</div>
```

JS

```js
var myApp = angular.module('myApp',[]);

function MyCtrl($scope) {
	$scope.thinkster = 'thinkster';
}
```

We have bound the jQuery property `pageX`​ to `ng-click`, so when you click a button you’ll see the X value of where you clicked on the button.

---

$log
----

---

Instead of showing the event, you can also log the event out, which can be useful to understand what your app is doing during development. The example below will log the click event, the X coordinate of our mouse click, to the console. In order to use `$log` without setting up a controller, you can put it on the `$rootScope` of our app.

JS

```js
var app = angular.module("app", []);

app.run(function($rootScope, $log){
  $rootScope.$log = $log;
});
```

HTML

```html
<div ng-app="app">
  <div
    class="button"
    ng-repeat="item in 'somewords'.split('')"
    ng-click="$log.debug($event)"
    >
    {{$index + 1}}. {{item}}
    {{ev.pageX}}
  </div>
</div>
```

Angular provides different types of logging such as info and warnings. [Check out the docs](https://docs.angularjs.org/api/ng/service/$log) for more detail and a wonderful interactive example!









