


# How to connect nodejs with mysql using sequelize,mysql2 and sequlize-cli(to do migrations)


- Install sequelize, mysql2 and sequelize-cli

- on installing sequelize-cli it will create config - > config.json , migrations and models folder

- After this create .sequlizerc file using documentation of sequelize in root project 

-  Create a new model like below query:-
```
 npx sequelize-cli model:generate --name User --attributes firstName:string,lastName:string,email:string
```

- The above query will create a model file and along with a migration file

- then exceute below query to migrate

```
npx sequelize-cli db:migrate

```


































