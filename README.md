# Prisma 

### Installation 

- Install as dev dependencies
```bash
npm install prisma --save-dev
```
- Invoke prisma cli by below command
```bash
npx prisma
```
- Init prisma using below command
```bash
npx prisma init 
```
- The above command will create
  - creates a new directory called prisma that contains a file called schema.prisma, which contains the Prisma schema with your database connection variable and schema models
  - creates the .env file in the root directory of the project, which is used for defining environment variables (such as your database connection)
- Next Steps require
1. Set the DATABASE_URL in the .env file to point to your existing database. If your database has no tables yet, read https://pris.ly/d/getting-started
2. Set the provider of the datasource block in schema.prisma to match your database: postgresql, mysql, sqlite, sqlserver, mongodb or cockroachdb.
3. Run prisma db pull to turn your database schema into a Prisma schema.
4. Run prisma generate to generate the Prisma Client. You can then start querying your database.


- Define the models inside prisma.schema like below

```js

model Product {
  id          Int      @id @default(autoincrement())
  name        String
  description String?
  price       Float
}
```

- Then run following command

```bash
npm install @prisma/client

```
Notice that the @prisma/client node module references a folder named .prisma/client. The .prisma/client folder contains your generated Prisma Client, and is modified each time you change the schema and run the following command:
- Run generate command to generate prisma client for each model
```bash
npx prisma generate
```

- Migrate models to your database:-

```bash

npx prisma migrate

```

- Push existing prisma schema to db
```bash
npx prisma db push 
```
- npx prisma db push is primarily used for pushing changes from your Prisma schema file directly to the database without generating or applying migrations. This command is part of the Prisma Client, and it's useful for quickly applying changes to the database when you don't need migration files (for example, in a development or testing 


- Push migration files to deploymemt environment using migrate

```bash

npx prisma migrate deploy

```
The migrate deploy command applies all pending migrations, and creates the database if it does not exist. Primarily used in non-development environments. This command:

- Does not look for drift in the database or changes in the Prisma schema
- Does not reset the database or generate artifacts
- Does not rely on a shadow database
