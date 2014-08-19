##Ruby On Rails 
#Angular Setup

__Written by:__ [Nadav](https://github.com/nadavmatalon)
(May 2014@[Makers Academy](http://www.makersacademy.com/))

###General Notes

* For a basic Ruby on Rails project setup please refer to the [New Project Setup](./ror_new_project_setup.md) 
in this folder.
* The following instructions have been written for projects using 
[Rails 4.0](http://rubyonrails.org/) or later.
* Text in ALL_CAPITALS_AND_UNDERSCORES indicated a __placeholder__ for your own text 
* If a file's location within the file system isn't specified explicitly, that file is 
in the root directory of the project.


###What is Angular?

From the Angular documentation ([source](https://docs.angularjs.org/guide/introduction)):

```
AngularJS is a structural framework for dynamic web apps. It lets you use HTML as your 
template language and lets you extend HTML's syntax to express your application's components 
clearly and succinctly. 

Angular is what HTML would have been had it been designed for 
applications. HTML is a great declarative language for static documents. It does not contain 
much in the way of creating applications, and as a result building web applications is an 
exercise in what do I have to do to trick the browser into doing what I want?

Angular takes another approach. It attempts to minimize the impedance mismatch 
between document centric HTML and what an application needs by creating new HTML 
constructs.
```

###Setting up Angular

In `app/views/layouts/application.html.erb`, add the following content:

```html
<html ng-app="NAME_OF_APP">
	<head>
	</head>
	<body ng-controller="NAME_OF_ANGULAR_CONTROLLER">
    	<script>
            var app = angular.module('NAME_OF_APP', []);
            app.controller('NAME_OF_ANGULAR_CONTROLLER', ['$scope', '$http', function($scope, $http) {
                $http.get('API_URL').success(function(ReturnObjectName) {
                    $scope.Variable1 = ReturnObjectName.AttributeName;
                    $scope.Variable2 = ReturnObjectName.AttributeName;
                    $scope.Variable4 = ReturnObjectName.AttributeName || "defualut_value";
                    $scope.Variable4 = ReturnObjectName.AttributeName || "defualt_value";
            	}).error(function(profileData) {
                	// here define actions that happens if that call fails //
            	}).finally(function() {
            		// here define actions that will always be executed //
            		// (regardless of success or failure of the call) //
            	});
        	}]);
    	</script>
	</body>
</html>
```

Then, in `app/assets/javascripts/application.js`, add these lines:

```js
//= require jquery
//= require angular
```

Make sure that these lines are placed __above__ this line:

```js
//= require_tree .
```


If you want the Angular code to be included locally in your app's code (rather than depend 
on it being downloaded from the web every time ), create new file file: 
`app/vendor/javascripts/angular.js`, and copy the content for it from the following link:

```
https://ajax.googleapis.com/ajax/libs/angularjs/1.2.0/angular.min.js
```

Alternatively, add the following in the `<head>` section of `app/views/layouts/application.html.erb`:

```html
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.0/angular.min.js"></script>
```

Then, in the relevant `*.html.erb` file where you want to use Angular, you can add the 
following lines:

```html
<div>
	<ul>
		<li class="profile-content-form"> {{ Variable1 }} </li>
		<li class="profile-content-form"> {{ Variable2 }} </li>
		<li class="profile-content-form"> {{ Variable3 }} </li>
		<li class="profile-content-form"> {{ Variable4 }} </li>
	</ul>
</div>
```

If everything was done right, when you run the app in the browser, the {{ variables }}
should be replaced by their values as defined by the JavaScript function in
`app/views/layouts/application.html.erb` as set above.

