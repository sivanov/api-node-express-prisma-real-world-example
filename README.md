# ![Rails Example App](media/realworld.png)

> Official NodeJS codebase that adheres to the [RealWorld](https://gothinkster.github.io/realworld/docs/specs/backend-specs/introduction) API spec.

**Notice: this repo is clone and will be used for test purpose.**
This collection of project is awesome.
Some ot commands and API documentation was inocorect and I will try to improve it.

# Deploy to Heroku

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

# Getting started

This project is clone of www.medium.com

### Clone the repository

run `git clone https://github.com/gothinkster/node-express-prisma-v1-official-app.git`

### Install the dependancies

> [NodeJS](https://nodejs.dev/) is required

```
cd node-express-prisma-v1-official-app
npm install
```

### Download pgAdmin for PostgreSQL

[PostgreSQL](https://www.postgresql.org/download/) downloads page
TODO: add instruction for linux users

### Create a server

run **pgAdmin**  
create a server (Object/Create/Server)  
required fields:

- name
- HOST name/address

#### From terminal
create new DB and user and password

```sql
sudo -u postgres psql

-- create new DB
CREATE DATABASE node_express_prisma;
-- create password for new created user
CREATE USER node_express_prisma WITH ENCRYPTED PASSWORD 'password';
-- add permissions to create Database and tables
ALTER ROLE node_express_prisma CREATEDB;
GRANT ALL PRIVILEGES ON DATABASE node_express_prisma TO node_express_prisma;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO node_express_prisma;
```
Use "\q" to quit

### Connect the created server

create a _.env_ file at the root of the project  
populate it with the url of your database

```ini
DATABASE_URL="postgresql://node_express_prisma:password@localhost:5432/node_express_prisma?schema=public"
# example
DATABASE_URL="postgresql://<username>:<password>@<host_name>:<port>/<database_name>?schema=public"
```

### Run the project locally
run development server with command:
```
npm run dev
```

## Advanced usage

### Prisma
to have code higlite for Prisma schema download extension for that.
For example for VSC editor you can use this : [Prisma extension](https://marketplace.visualstudio.com/items?itemName=Prisma.prisma)

### Format the Prisma schema

```bash
npm run prisma:format
```

### Migrate the SQL schema

```bash
npm run prisma:migrate dev --name added_job_title
```

### Update the Prisma Client

```bash
npm run prisma:generate
```

_with watch option_

```bash
npm run prisma:generate:watch
```

### (Optional step) Seed the database
Seed will populate database with some fake(demo) information.
From official documentation: [prisma.io/docs](https://www.prisma.io/docs/guides/database/seed-database)
open new terminal tab or window and run
```bash
npm run prisma:seed
```

### (Optional step) Launch Prisma Studio
With a simple tabular interface you can quickly have a look at the data of your local database and check if your app is working correctly. More information: [https://www.prisma.io/studio](https://www.prisma.io/studio)

open new terminal tab or window and run
```bash
npm run prisma:studio
```

## creating user
for now we do not use HTTPS and ursl will startd with "http://" only
```sh
curl -X 'POST' \
  'http://localhost:3000/api/users' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "user": {
    "username": "demo",
    "email": "demo@mail.com",
    "password": "password"
  }
}'
```
returnet result
```json
{"user":{"email":"demo@mail.com","username":"demo","bio":null,"image":"https://api.realworld.io/images/smiley-cyrus.jpeg","token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImRlbW9AbWFpbC5jb20iLCJ1c2VybmFtZSI6ImRlbW8iLCJiaW8iOm51bGwsImltYWdlIjoiaHR0cHM6Ly9hcGkucmVhbHdvcmxkLmlvL2ltYWdlcy9zbWlsZXktY3lydXMuanBlZyIsImlhdCI6MTY1NjE2ODkwMCwiZXhwIjoxNjYxMzUyOTAwfQ.XXqgIr8w-saXXOyFYBeYtZs0XmyyumYy8AnDO7qPAzA"}}
```



add new article:
```sh
curl -X 'POST' \
  'http://localhost:3000/api/articles' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Token eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImRlbW9AbWFpbC5jb20iLCJ1c2VybmFtZSI6ImRlbW8iLCJiaW8iOm51bGwsImltYWdlIjoiaHR0cHM6Ly9hcGkucmVhbHdvcmxkLmlvL2ltYWdlcy9zbWlsZXktY3lydXMuanBlZyIsImlhdCI6MTY1NjE2ODkwMCwiZXhwIjoxNjYxMzUyOTAwfQ.XXqgIr8w-saXXOyFYBeYtZs0XmyyumYy8AnDO7qPAzA' \
  -d '{
  "article": {
    "title": "My first artcle from cURL",
    "description": "Some description for this article",
    "body": "Some lon long article text",
    "tagList": [
      "test", "foo", "article", "curl", "token", "CRUD" 
    ]
  }
}'
```
responce from API:
```json
{"article":{"slug":"My-first-artcle-from-cURL-1","title":"My first artcle from cURL","description":"Some description for this article","body":"Some lon long article text","createdAt":"2022-06-25T15:17:38.060Z","updatedAt":"2022-06-25T15:17:38.060Z","tagList":["test","foo","article","curl","token","CRUD"],"author":{"username":"demo","bio":null,"image":"https://api.realworld.io/images/smiley-cyrus.jpeg"},"favoritedBy":[],"_count":{"favoritedBy":0},"favoritesCount":0,"favorited":false}}
```

Get last newest articles
```sh
curl -X 'GET' \
  'http://localhost:3000/api/articles?limit=20&offset=0' \
  -H 'accept: application/json'
```

```sh
curl -X 'GET' 'http://localhost:3000/api/articles' -H 'accept: application/json'
```


## Progress status
query get all articles not work, return 0 records and no http or code error

to debug Prisma you need to add this to .env file:
```
# enable all prisma debugging options
export DEBUG="prisma*"
```
and  update row in file : /home/ivanov/s/git/sivanov/api-node-express-prisma-real-world-example/prisma/prisma-client.ts
like this:
```js

const prisma = global.prisma || new PrismaClient({
  log: [
    {
      emit: 'stdout',
      level: 'query',
    },
    {
      emit: 'stdout',
      level: 'error',
    },
    {
      emit: 'stdout',
      level: 'info',
    },
    {
      emit: 'stdout',
      level: 'warn',
    },
  ],
});
```
update rows in function getArticles:
```js
export const getArticles = async (query: any, username?: string) => {
  // mandatory to be on the TOP
  // source: https://www.prisma.io/docs/concepts/components/prisma-client/working-with-prismaclient/logging
  prisma.$on('query', (e) => {
    console.log('Query: ' + e.query)
    console.log('Params: ' + e.params)
    console.log('Duration: ' + e.duration + 'ms')
  })
```



advanced query not work ok. replace limit $1 offset $2 like this: limit 0 offset 10 :
```sql
select
	"public"."Article"."id",
	"public"."Article"."slug",
	"public"."Article"."title",
	"public"."Article"."description",
	"public"."Article"."body",
	"public"."Article"."createdAt",
	"public"."Article"."updatedAt",
	"public"."Article"."authorId",
	"aggr_selection_0_User"."_aggr_count_favoritedBy"
from
	"public"."Article"
left join (
	select
		"public"."Article"."id",
		COUNT(*) as "_aggr_count_favoritedBy"
	from
		"public"."_UserFavorites"
	inner join "public"."Article" on
		("public"."Article"."id" = ("public"."_UserFavorites"."A"))
	inner join "public"."User" on
		("public"."User"."id" = ("public"."_UserFavorites"."B"))
	group by
		"public"."Article"."id") as "aggr_selection_0_User" on
	("public"."Article"."id" = "aggr_selection_0_User"."id")
where
	("public"."Article"."id") in (
	select
		"t0"."id"
	from
		"public"."Article" as "t0"
	inner join "public"."User" as "j0" on
		("j0"."id") = ("t0"."authorId")
	where
		(1 = 0
			and 1 = 1
			and "t0"."id" is not null))
order by
	"public"."Article"."createdAt" desc
limit $1 offset $2
```


### Reset the database
**Notice: this will delete all information in your database!**

- Drop the database
- Create a new database
- Apply migrations
- Seed the database with data

```bash
npm run prisma:reset
```

## API documentation
links:
* [endpoints](https://realworld-docs.netlify.app/docs/specs/backend-specs/endpoints/)
* [JS*ON Objects returned by API](https://realworld-docs.netlify.app/docs/specs/backend-specs/api-response-format/)
* [Templates fro pages](https://realworld-docs.netlify.app/docs/specs/frontend-specs/templates/)
* [Old benchmarks](https://realworld-docs.netlify.app/docs/community/resources/)
* [Postman collections](https://github.com/gothinkster/realworld/blob/main/api/Conduit.postman_collection.json)


## Node_modules directory statistics

```
18 792 items, totalling 416,7 MB
```