
Ruby On Rails: Angular

In "app/views/layouts/application.html.erb":

<html ng-app="NameOFApp">
	<head>
	</head>
		<body ng-controller="NameOfAngularController">
        	<script>
	            var app = angular.module('NameOfApp', []);
                app.controller('NameOfAngularController', ['$scope', '$http', function($scope, $http) {
                    $http.get('API_URL').success(function(ReturnObjectName) {
                        $scope.Variable1 = ReturnObjectName.AttributeName;
                        $scope.Variable2 = ReturnObjectName.AttributeName;
                        $scope.Variable4 = ReturnObjectName.AttributeName || "defualut_value";
                        $scope.Variable4 = ReturnObjectName.AttributeName || "defualt_value";
                    });
                }]);
        </script>
	</body>
</html>


In "app/assets/javascripts":

//= require jquery
//= require angular


Create new file file: "app/vendor/javascripts/angular.js"

and add the content from the following link:

https://ajax.googleapis.com/ajax/libs/angularjs/1.2.0/angular.min.js

Alternatively, add the following in the <head> section of "app/views/layouts/application.html.erb":
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.0/angular.min.js"></script>


In the relevant "filename.html.erb":

		<li class="profile-content-form"> {{ Variable1 }} </li>
		<li class="profile-content-form"> {{ Variable2 }} </li>
		<li class="profile-content-form"> {{ Variable3 }} </li>
		<li class="profile-content-form"> {{ Variable4 }} </li>
	</ul>
</div>

