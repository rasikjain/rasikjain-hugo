+++
date = "2021-05-19"
title = "Simple Notes app using React, TypeScript, Node.js, GraphQL and MongoDB"
slug = "simple-notes-app-using-react-typescript-nodejs-graphql-mongodb"
tags = ["React", "TypeScript", "Node.js", "GraphQL", "MongoDB", "JavaScript"]
draft = true
+++

In the series of blog post, I am going to walkthrough step by step process of build a [Simple Notes](https://github.com/rasikjain/SimpleNotes) App using [React](https://reactjs.org/), [TypeScript](https://www.typescriptlang.org/), [Node.js](https://nodejs.org/en/), [GraphQL ](https://graphql.org/)and [MongoDB](https://www.mongodb.com/). During the process of our development we are going to use some useful npm packages like [Express](https://expressjs.com/), [Apollo-Server](https://www.apollographql.com/docs/apollo-server), [Typegoose](https://github.com/typegoose/typegoose), [Mongoose](https://mongoosejs.com/), [TypeGraphQL ](https://typegraphql.com/)and [Bootstrap](https://getbootstrap.com/).

## Dev Environment Setup

We are going to setup our initial code base and folder strucuture to get started. In this tutorial, I am going to use the Windows environment. The command structure is mostly same and can be easily replicated to macOS.

Create a root folder called `SimpleNotes`. Underneath this folder create two subfolders **`client`** and **`server`** with lowercase naming convention. We will use [Visual Studio Code](https://code.visualstudio.com/download) as editor of our choice for this excercise.

```
mkdir SimpleNotes
cd SimpleNotes
mkdir client
mkdir server
```

## GraphQL Server Backend

In this section we will set up the backend GraphQL server connecting to MongoDB. Following is our folder for our server. All of the source code will be in `src` folder. We created seperate folders to store our `models` and `resolvers`.

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

Lets get started on creating the project. Open the `SimpleNotes` folder in the VSCode. Once you are in VSCode, go to `Terminal Window`.

Change the working directory to `server`. We will executing our commands from `server` folder.

```
CD server
```

In order to create a new node project, we will execute following command. `-y` flag accepts the default values while creating `package.json` file.

```
npm init -y
```

Configuring TypeScript

TypeScript is superset of JavaScript. It offers all the features of Javascript in addition to compile time checks. We We will use TypeScript to develop our application. We need to install TypeScript dependency and configure our node project with TypeScript.

```
npm install -D typescript
```

The `-D` or `--save-dev` flag will install the package as dev dependency. Packages installed with this flag will not be required to run the application in production.

Create `tsconfig.json` file using `tsc`command. This `tsconfig.json` allows you to specify root files and different compiler options required to compile the typescript project.

```
tsc init
```

Here is the `tsconfig.json` with the configuration required to compile and run our application.

```json
{
  "compilerOptions": {
    "allowJs": true,
    "module": "commonjs",
    "moduleResolution": "node",
    "pretty": true,
    "sourceMap": true,
    "target": "es2017",
    "outDir": "./dist",
    "lib": ["es6"],
    "resolveJsonModule": true,
    "types": ["node"],
    "typeRoots" : ["./node_modules/@types", "./src/@types"],
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "esModuleInterop": true
  },
  "include": [
    "src/**/*.ts",
  ],
  "exclude": [
    "node_modules",
  ]
}
```

Now it is time to install dependency packages which are need for our application. Run the following command to install npm packages.

```command
npm i apollo-server-express dotenv express graphql 
npm i mongoose reflect-metadata type-graphql @typegoose/typegoose
```

* `apollo-server-express` - This is the Apollo GraphQL Server with Express.

* `dotenv` - Dotenv module loads environment variables from a .env file into process.env.

* `express` - Express is a minimal and flexible Node.js web application framework.

* `graphql` - Implementation of GraphQL for creating APIs.

* `mongoose` - Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js.

* `reflect-metadata` - Allows you to perform runtime reflection on types.

* `type-graphql` - Create GraphQL schema and resolvers with TypeScript, using classes and decorators.

* `@typegoose/typegoose` - Allows you to define Mongoose models using TypeScript classes.
