#How to use control Angular views using resolve property.

>I needed to set up a 'wild card route' that would be verfified on the back-end.
>If valid the view would show a certain templateUrl, if validation failed the view 
>should be changed to home - '/'.
>The project was set up using the angular routeProvider. 
>     .when('/:name', {
        templateUrl: '/form.html',
        controller: 'formCtrl'
        })
