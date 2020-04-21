# Angular.js

## How to include angular in your app?

1. Google (or other provider's) CDN
2. Include it in the html tag:
```html
<html lang="en" ng-app="">
...
</html>
```

## Basics

### Interactive div!

A simple interactive div can be built by:

```html
<body>
    <input type="text" ng-model="name">
    <div>Hello {{name}}</div>
</body>
```

### Directives

Keywords in angular.js is call directives. E.g. `ng-model`, `ng-bind`. Generally they are attributes of html tags starting with `ng-*`. Its purpose is to grant some specific functions to the DOM (or even its child) it attached

### Normalization

Angular.js would normalize the `ng` tags such that `ng-bind` could be written as `data-ng-bind`.

### `ng-model`

`ng-model` is like a "variable" that allow you to copy the content of such element in to other elements. Note that it only works for specific elements (e.g. `input`, `textarea`, etc.).

### `ng-bind`

`ng-bind` retrieves the content from the corresponding `ng-model` and put it in the specific elements. (Another way is `{{}}`).

### `ng-init`

Its like initializing a variable:

```html
<div ng-init="language='AngularJS'">
I am using {{language}}</div>
```

### `np-repeat`

Use it to loop through ng variables!

```html
<div ng-init="item_list=[{item:'aaa'},{item:'bbb'},{item:'ccc'}]"></div>
    <span ng-repeat="item in item_list">
        Item: {{item.item}}
    </span>
```

## AngularJS as a controller

In the `<script>` tags we can utilize angularJS as a controller:

```html
    <div ng-controller="languages">
        Select a language:
        <button ng-click='french()'>French</button>
        <button ng-click='german()'>German</button>
        <button ng-click='cantonese()'>Cantonese</button>

        <p>You have selected: {{language | uppercase }}</p>
    </div>
    <script>
        var application = angular.module('testapp', []);
        application.controller('languages', ['$scope', ($scope)=>{
            $scope.language = 'None';

            $scope.french = ()=>{
                $scope.language = 'French';
            }
            $scope.german = ()=>{
                $scope.language = 'German';
            }
            $scope.cantonese = ()=>{
                $scope.language = 'Cantonese';
            }
        }]);
    </script>
```

Notice we defined `np-controller` is the `<div>` tag.

Also notice the ugly `['$scope', ($scope)...]`, it is to prevent errors after the obfuscation of the javascript code.

## Filters (or more like pipes)

We can use filters to apply to varibale:

```html
<span>{{'test' | uppercase}}</span> <!-- Will show TEST-->
```

Useful filters include `uppercase`, `currency`, `number`, etc...

## `np-include`

`np-include` is mainly used to split a webpage into different parts.

Usage:

```html
<div ng-include="'nav.html'"></div>
```

## Routing with AngularJS

### `ng-view`

1. Download `angular-router.js` from the web.
2. Edit the script part
```js
//controller,js
var app = angular.module('mainApp', ['ngRoute']); //'mainApp' is the name of the np-app.

app.config(($routeProvider)=>{
    $routeProvider
    .when('/',{
        template: 'Welcome!'
    })
    .when('/anotherPage', {
        templateUrl: 'anotherpage.html' // the browser would do an AJAX request to fetch the file, and append it inside the div we specified in Step 3.
    })
    .otherwise({
        redirectTo: '/'
    });
});
```
3. Edit the html part:
```html
<div ng-view><div>
```
We can provide more tag to the `.when` function, like `resolve` (satisfy some condition before navigating to the page).

## Searching with AngularJS

Implementing a searching function is easy with the help of the filter pipe. Simply do:
```html
<input type="text" ng-model="searchBox">
...
<li ng-repeat="person in persons | filter:searchBox">
    ...
</li>
```

A more "advanced" way of searching is to search individual fields:

```html
<input type="text" ng-model="globalSearch.$">
<input type="text" ng-model="globalSearch.Name">
<input type="text" ng-model="globalSearch.Age">
<input type="text" ng-model="globalSearch.Hobbies">
...
<li ng-repeat="person in persons | filter:globalSearch">
    ...
</li>
```

## Serving data

AngularJS can serve data through the `$http` package:

```js
var URL = <some_mongol_url>
app.controller('people', ($scope, $http){
    $http.get(URL).success((response)=>{
        $scope.persons = response;
    })
})
```

## Breaking the scope of `ng-model`
Consider the following `html` code:
```html
<div name="scopes">
            <input type="text" ng-model="message">
            <p ng-repeat="i in [1]">
                <input type="text" ng-model="message">
                You wrote: {{message}}
            </p>
            [Outer]: You wrote: {{message}}
        </div>
```
Because the inner `<input>`has defined its own local `message` `ng-model`, changed in that inner `<input>` will not be reflected to the outer `{{message}}`. We can define a `message` "object" and only allow the inner to change its attribute:
```html
<div name="scopes">
            <input type="text" ng-model="message.property">
            <p ng-repeat="i in [1]">
                <input type="text" ng-model="message.property">
                You wrote: {{message.property}}
            </p>
            [Outer]: You wrote: {{message.property}}
        </div>
```

By that, changes will be propagated.

## Monitoring the changes of a `ng-model`

Use `$scope.$watch`:

```js
app.controller('app', ($scope)=>{
    $scope.$watch('myText', (newValue, OldValue)=>{
        console.log("New value: "+newValue);
        console.log("Old value: "+oldValue);
    })
})
```

## Digest cycle

The reason why angular know when to update which elements is when all commands or function run is within angular's scope. By then, the digest cycle would be called to update all related variable.

However, there will be times where the changes is made from outside the scope. By then, we could call `$scope.$digest()` to force the digest cycle to run.

## Services

Services are singletons that prevents multiple called to api/databases. Using random number as examples:

```js
//controller.js
app.service('random', function(){
    let num = Math.floor(Math.random()*10);
    this.generate = ()=>{
        return num;
    };
});

app.controller('app', ($scope, random)=>{
    $scope.randomNumber = random.generate();
    $scope.randomNumber2 = random.generate();
    //randomNumber === randomNumber2!
})
```

## Factories

Factories in angularjs is very similar to services, except it returns a singleton object for use. With the same example, the factory code would be:

```js
//controller.js
app.factory('random', function(){
    var randomObject = {}
    let num = Math.floor(Math.random()*10);
    randomObject.generate = ()=>{
        return num;
    };
    return randomObject;
});

app.controller('app', ($scope, random)=>{
    $scope.randomNumber = random.generate();
    $scope.randomNumber2 = random.generate();
    //randomNumber === randomNumber2!
})
```