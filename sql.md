#How to alter your database schema with Sequelize.

>If you want to add a column to your model or remove a column, you have 
>to set up a migration file before you make the change to your model.
>
>Sequelize is an ORM (Object Relational Mapper) - a layer of >software in your application that lets you map objects to 
>your database. This is the "opposite" of a raw query. 
>Basically, you write code on your backend to perform CRUD 
>operations. 
>In this example I am using Sequelize in a Node.JS environment. Assuming you have sequelize installed already, follow these steps:

###In your terminal:
1. sequelize migration:create --name migrationfilename
2. sequelize db:migrate

>This will create a migration file that will be 
>automagically placed in your migrations folder. 

###Open the file: 
>You will see some auto generated code that shows the 
>structure of the migration. 
    'use strict';

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

    module.exports = {
      up: function (queryInterface, Sequelize) {

        return queryInterface.addColumn(
                'users',
                'addresslink',
                {
                  type: Sequelize.STRING
                }
              )
      },

      down: function (queryInterface, Sequelize) {
        return queryInterface.removeColumn(
            'users',
            'addresslink'
          );
      }
    };



