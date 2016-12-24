#How to use control Angular views using resolve property.

>I needed to set up a 'wild card route' that would be verfified on the back-end.
>If valid, the view would show a certain templateUrl, if validation failed the view 
>should be changed to home - '/'.
>The project was set up using the angular routeProvider. 
>  
      myModule.config(['$routeProvider', '$locationProvider', '$httpProvider', '$provide',
          function ($routeProvider, $locationProvider, $httpProvider, $provide) {   
            .when('/:name', {
              templateUrl: '/form.html',
              controller: 'formCtrl'
              })
>My first instinct was to  inject $http into the config and use it like:
      templateUrl: function($route){
        $http({
          method: 'GET',
          url: $route.current.params.name
          }).then(function(res){
            //if response is valid
            //set the templateUrl to form.html
            //if response is not valid
            //set the templateUrl to home.html
            })
      }  
>But this approach will not work because you cannot inject the $http during the config period.
           
