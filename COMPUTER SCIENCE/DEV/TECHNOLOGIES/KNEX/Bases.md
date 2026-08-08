---
title: "KNEX .JS"
date: "2022-11-02"
---

No, this is not a clinex! [https://knexjs.org/guide/](https://knexjs.org/guide/)

Knex is a query builder for node!

## GETTING STARTED

```
npm install knex --save //Main Install

npm install <dialect> // Chose your dialect
```

Then you need to `init` knex

```
npx knex init
```

This will create a `knexfile.js` that will have different types of databases per environment. So development, staging, production etc.  
By default, the development database is SQLite, and I think you may want to change that:

```
 development: {

    client: "mysql",

    connection: {

      database: "testing",

      user: "root",

      password: "root",

    },

  },
```

And at the end, you will find the `migrations` table name config! Migrations are one of the most important concepts, and lets you always have the database's schema updated (dev staging prod)

## CLI UTILITY

Knex comes with a CLI utility that let's you do some stuff (like migrations):

[https://knexjs.org/guide/migrations.html#migration-cli](https://knexjs.org/guide/migrations.html#migration-cli)
