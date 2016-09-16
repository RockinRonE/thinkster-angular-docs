Angular element allows you to wrap a raw DOM element or HTML string as a jQuery element. 

Here’s our setup: 

HTML

```html
<div ng-app="app">
  <input-box></input-box>
</div>
```

JS

```js
var app = angular.module("app", [])

app.directive("inputBox", function () {
  return {
    restrict: "E", // restricts to element
    replace: true, // replaces <input-box> with template on line 7
    template: `<div>
  <input type="text" ng-model="model.input">
  <div>{{ model.input }}</div>
</div>`,
    link: function (scope, element) {
      scope.$watch("model.input", function (value) {
        if(value === "thinkster") {
          element.children(1).toggleClass("alert-box");
        }
      });
    }
  };
});


```

[The fiddle](https://jsfiddle.net/chwf4bt5/3/)

This directive injects our template code inbetween the divs, replacing `<input-box>`​ since we have `replace: true`​. Our template code is an input field that’s bound to `model.input`​.

In the linking function you’ll notice that we’re using `scope.$watch`, which sets a listener on our `model.input` and “listens” for it to change. When it does change (i.e. we input text), our input value will be checked to see if it is “thinkster” and if so, traverse the DOM and invoke `toggleClass`​. In the fiddle go ahead and type in “thinkster” and you’ll see that we toggle the `.alert-box` class in the CSS, which changes the text to Thinkster blue and capitalizes it for fun.

---

There is a cleaner way to write this using Angular’s `angular.element` and `$compile`​. Lets start by removing our template code and saving it as a variable.

```js
var inputElement = angular.element('<div>{{ model.input }}</div>');
```

`angular.element` acts as an interface between jQuery/jqLite and Angular. All angular.element references are always wrapped with jQuery or jqLite; they are never raw DOM references. 

$compile takes a HTML string and compiles it into a template function, returning a link function that is used to bind the template to a scope. For example:

[Fiddle](https://jsfiddle.net/chwf4bt5/6/)

```js
var app = angular.module("app", [])

app.directive("inputBox", function () {
	var inputElement = angular.element('<div>{{ model.input }}</div>');

  return {
    restrict: "E",
    replace: true,
    template: `<div>
  		<input type="text" ng-model="model.input">
				</div>`,
    compile: function(tElem) {
    	tElem.append(inputElement);
      
      return function(scope) {
      	scope.$watch('model.input', function(value) {
        	if(value === 'thinkster') {
          	inputElement.toggleClass('alert-box');
          }
        });
      }
    }
  };
});
```

The `tElem` parameter is the jqLite wrapper of our template. Needed? -\> Since we created the angular.element reference before as a variable in the directive, the tElem jqLite API is used to append the element validElement.

The `$compile` service returns a link function, which is similar the our previous example except that now we can reference `inputElement` instead of the messy `element.children(1)` reference.

One last refactor is to move the link function into a directive variable (like inputElement) for better readability.

[Final Code](https://jsfiddle.net/chwf4bt5/8/)

```js
var app = angular.module("app", [])

app.directive("inputBox", function () {
	var inputElement = angular.element('<div>{{ model.input }}</div>');
  
	var link = function(scope) {
    scope.$watch('model.input', function(value) {
      if(value === 'thinkster') {
        inputElement.toggleClass('alert-box');
      }
    });
   };
   
  return {
    restrict: "E",
    replace: true,
    template: `<div>
  		<input type="text" ng-model="model.input">
				</div>`,
    compile: function(tElem) {
    	tElem.append(inputElement);
      
      return link; 
    }
  };
});
```











