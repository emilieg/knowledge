#How to alter your database schema with Sequelize

>If you want to make a change to your data model you have
>to set up a migration file before you make the change to your model itself.
>
>Sequelize is an ORM (Object Relational Mapper) - a layer of software in your application that lets you map objects to 
>your database. This is the "opposite" of a raw query. 
>Basically, you write code on your backend to perform CRUD 
>operations. 
>In this example I am using Sequelize in a Node.JS environment. Assuming you have sequelize installed already, follow these steps:

###In your terminal:
- sequelize migration:create --name migrationfilename


>This will create a migration file that will be 
>automagically placed in your migrations folder. 

###Open the file: 
>You will see some auto generated code that shows the 
>basic skeletor of the migration. 

    module.exports = {
      up: function (queryInterface, Sequelize) {
          /* Add altering commands here.
          Return a promise to correctly handle asynchronicity.
          */
      
      },

      down: function (queryInterface, Sequelize) {
        /*
          Add reverting commands here.
          Return a promise to correctly handle asynchronicity.
        */
       
      }
    };
>You need to make sure that you create the *UP* and *DOWN* 
>function. In my example I am adding column "address" to a table called "users". Take a look at other examples in 
>[the sequelize documentation](http://docs.sequelizejs.com/en/latest/docs/migrations/).

    module.exports = {
      up: function (queryInterface, Sequelize) {
        return queryInterface.addColumn(
                'users',
                'address',
                {
                  type: Sequelize.STRING
                }
              )
      },

      down: function (queryInterface, Sequelize) {
        return queryInterface.removeColumn(
            'users',
            'address'
          );
      }
    };

###In your terminal run:
- sequelize db:migrate

###In your app:
- go to your "models" folder in the application and make the appropriate change on the model. 
- save 
- npm start (or whatever command you use to start your app)

>Connect to your database to make sure the table has 
>been altered! If you are using PG commander for 
>PostgreSQL  you may need to re-connect in order to see 
>the change. 


