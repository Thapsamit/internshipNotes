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
