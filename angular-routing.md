#How to use control Angular views using resolve property.

>I needed to set up a 'wild card route' that would be verfified on the back-end.
>If valid, the view would show a certain templateUrl, if validation failed the view 
>should be changed to home - '/'.
>The project was set up using the angular routeProvider. 
>  

      myModule.config(['$routeProvider', '$locationProvider', '$httpProvider', 
          function ($routeProvider, $locationProvider, $httpProvider) {   
            .when('/:name', {
              templateUrl: '/form.html',
              controller: 'formCtrl'
              })
>My first instinct was to  inject $http into the config and use it like:
>

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
      
>But this approach will not work because a service such as $http will not have been initialized yet in the "config" phase. If you move the $http request to the controller and change the $location.path from there, the viewer will see a flicker of the template defined in the config function then a jump to the view defined by $location.path. Not a very good user experience.


>This is where the resolve property becomes useful per [documentation][1]: "An optional map of dependencies which should be injected into the controller. If any of these dependencies are *_promises_*, the router will wait for them all to be *resolved or one to be rejected before the controller is instantiated*. If all the promises are resolved successfully, the values of the resolved promises are injected and $routeChangeSuccess event is fired. If any of the promises are rejected the $routeChangeError event is fired." 




Link References
[1]: https://docs.angularjs.org/api/ngRoute/provider/$routeProvider