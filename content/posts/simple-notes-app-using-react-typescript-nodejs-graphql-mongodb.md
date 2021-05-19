+++
date = "2021-05-25"
title = "Simple Notes app using React, TypeScript, Node.js, GraphQL and MongoDB"
slug = "simple-notes-app-using-react-typescript-nodejs-graphql-mongodb"
tags = ["React", "TypeScript", "Node.js", "GraphQL", "MongoDB", "JavaScript"]
draft = true
+++

In the series of blog post, I am going to walkthrough step by step process of build a [Simple Notes](https://github.com/rasikjain/SimpleNotes) App using [React](https://reactjs.org/), [TypeScript](https://www.typescriptlang.org/), [Node.js](https://nodejs.org/en/), [GraphQL ](https://graphql.org/)and [MongoDB](https://www.mongodb.com/). While doing the development we are going to use some useful npm packages like [Express](https://expressjs.com/), [Apollo-Server](https://www.apollographql.com/docs/apollo-server), [Typegoose](https://github.com/typegoose/typegoose), [Mongoose](https://mongoosejs.com/), [TypeGraphQL ](https://typegraphql.com/)and [Bootstrap](https://getbootstrap.com/).

## Dev Environment Setup

We are going to setup our initial code base and folder strucuture to get started. In this tutorial, I am going to use the Windows environment. The command structure is mostly same and can be easily replicated to macOS.

Create a root folder called `SimpleNotes`. Underneath this folder create two subfolders **`client`** and **`server`** with lowercase naming convention. We will use [Visual Studio Code](https://code.visualstudio.com/download) as editor of our choice for this excercise.


## GraphQL Server Backend

In this section we will work on setting up the backend GraphQL server connecting to MongoDB.  

```
server/
 ┣ src/
 ┃    ┣ models/
 ┃    ┃    ┗ notes.model.ts
 ┃    ┣ resolvers/
 ┃    ┃    ┣ types/
 ┃    ┃    ┃    ┗ notes-input.ts
 ┃    ┃    ┗ notes.resolvers.ts
 ┃    ┗ index.ts
 ┣ .env
 ┣ apollo.config.js
 ┣ nodemon.json
 ┣ package-lock.json
 ┣ package.json
 ┣ schema.gql
 ┗ tsconfig.json
```
