#How to control Angular views with the resolve property and promises.

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
>My first instinct was to  inject $http into the config and use it like this:
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

>But this approach will not work because a service such as $http will not have been initialized yet in the "config" phase. If you move the $http request to the controller and change the $location.path from there, the viewer will see a flicker of the template defined in the config function first, then a jump to the view defined by $location.path. Not a very good user experience.


>This is where the resolve property becomes useful -  per [documentation][1]: "An optional map of dependencies which should be injected into the controller. If any of these dependencies are **promises**, the router will wait for them all to be **resolved or one to be rejected before the controller is instantiated**. If all the promises are resolved successfully, the values of the resolved promises are injected and $routeChangeSuccess event is fired. If any of the promises are rejected the $routeChangeError event is fired." 

>The cue here is *promises*. I want the router to wait until my $http promise is resolved or rejected before showing a view. This allows me to control which view is served based on the data I receive back from the $http request. Bingo! 
>
      myModule.config(['$routeProvider', '$locationProvider', '$httpProvider', 
          function ($routeProvider, $locationProvider, $httpProvider) {   
            .when('/:name', {
              resolve: {
                    item: ['$route', '$http', '$location', function($route, $http, $location) {                       
                      return $http({
                        method: 'GET',
                        url: $route.current.params.name
                      })    
                    }]
                    }, 
              templateUrl: '/form.html',
              controller: 'formCtrl',
              })      

>I return the $http call here which is a promise, but I don't have access to the data. My back-end route returns a response with a string in the data field or an empty string, so I need a way to check for the string and then decide whether that route it's valid or not. So I need to wrap my promise in another promise which I can resolve or reject. 


>###Angular $q Service
>"[A service that helps you run functions asynchronously, and use their return values (or exceptions) when they are done processing.][2]" The Angular native $q object exposes methods that can resolve(), reject() or notify() a promise. This service is similar to the [ES6 implementation][3] but the ES6 version is not as widely supported by browsers.

>###Implementation
> I decided to create a service for my $http call, which I set up to return true and false based on the data that was return to me from the database. I found that it made my resolve code cleaner and easier to follow. I'm also wrapping the service (which returns a promise) in the $q promise inside of my resolve property which will allow me to resolve or reject it. 

      resolve: {
          link: ['$route', 'ApiHttp', '$location', 'httpSrvc', '$q', function($route, ApiHttp, $location, httpSrvc, $q) {
               var d = $q.defer();
                  httpSrvc.getLink($route).then(function(data){
                      if(data !== false){
                          d.resolve()
                          return data;
                      } else {
                          d.reject('no valid link')
                      }
                  })
              return d.promise
              }]
          },
      templateUrl: '/form.html',
      controller: 'formCtrl',
      }) 

> The templateUrl is only shown if the promise was resolved. If the promise was rejected the $routeChangeError function is triggered which changes the view to a specified $location.path(). 
This function is implemented at the top of my scope inside module.run([]) function. 


    $rootScope.$on('$routeChangeError', function (evt, current, previous, rejection) {
        if (rejection === 'no valid link') {
            $location.path('/')
        }
    }); 



[1]: https://docs.angularjs.org/api/ngRoute/provider/$routeProvider
[2]: https://docs.angularjs.org/api/ng/service/$q
[3]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise