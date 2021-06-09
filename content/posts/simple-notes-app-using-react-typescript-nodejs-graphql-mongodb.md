+++
date = "2021-05-20"
title = "Simple Notes app using React, TypeScript, Node.js, GraphQL and MongoDB"
slug = "simple-notes-app-using-react-typescript-nodejs-graphql-mongodb"
tags = ["React", "TypeScript", "Node.js", "GraphQL", "MongoDB", "JavaScript"]
+++

In the series of the blog post, I am going to walk through step by step process of building a [Simple Notes](https://github.com/rasikjain/SimpleNotes) App using [React](https://reactjs.org/), [TypeScript](https://www.typescriptlang.org/), [Node.js](https://nodejs.org/en/), [GraphQL ](https://graphql.org/) and [MongoDB](https://www.mongodb.com/). During the process of our development, we are going to use some useful npm packages like [Express](https://expressjs.com/), [Apollo-Server](https://www.apollographql.com/docs/apollo-server), [Typegoose](https://github.com/typegoose/typegoose), [Mongoose](https://mongoosejs.com/), [TypeGraphQL ](https://typegraphql.com/) and [Bootstrap](https://getbootstrap.com/).

## Dev Environment Setup

We are going to set up our initial codebase and folder structure to get started. In this tutorial, I am going to use the Windows environment. The command structure is mostly the same and can be easily replicated to macOS.

Create a root folder called `SimpleNotes`. Underneath this folder create two subfolders **`client`** and **`server`** with a lowercase naming convention. We will use [Visual Studio Code](https://code.visualstudio.com/download) as the editor of our choice for this exercise.

```shell
mkdir SimpleNotes
cd SimpleNotes
mkdir client
mkdir server
```

## GraphQL Server Backend

In this section, we will set up the backend GraphQL server connecting to MongoDB. Following is our folder for our server. All of the source code will be in `src` folder. We created separate folders to store our `models` and `resolvers`.

### Folder Structure

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

### Creating Node.js project

Let's get started on creating the project. Open the `SimpleNotes` folder in the VSCode. Once you are in VSCode, go to `Terminal Window`.

Change the working directory to the `server`. We will execute our commands from the `server` folder.

```shell
CD server
```

To create a new node project, we will execute the following command. `-y` flag accepts the default values while creating the `package.json` file.

```
npm init -y
```

### Configuring TypeScript

`TypeScript` is a superset of JavaScript. It offers all the features of Javascript in addition to compile-time checks. We will use TypeScript to develop our application. We need to install TypeScript dependency and configure our node project with TypeScript.

```
npm install -D typescript
```

The `-D` or `--save-dev` flag will install the package as a dev dependency. Packages installed with this flag will not be required to run the application in production.

Create `tsconfig.json` file using `tsc` command. This `tsconfig.json` allows you to specify root files and different compiler options required to compile the typescript project.

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

### Installing Dependency Packages

Now it is time to install dependency packages that are needed for our application. Run the following command to install npm packages.

```command
npm i apollo-server-express dotenv express graphql 
npm i mongoose reflect-metadata type-graphql @typegoose/typegoose
```

* `apollo-server-express` - This is the Apollo GraphQL Server with Express. 
  
* `dotenv` - Dotenv module loads environment variables from a .env file into process.env.
* `express` - Express is a minimal and flexible Node.js web application framework.
* `graphql` - Implementation of GraphQL for creating APIs.
* `mongoose` - Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js.
* `reflect-metadata` - This allows you to perform runtime reflection on types.
* `type-graphql` - Create GraphQL schema and resolvers with TypeScript, using classes and decorators.
* `@typegoose/typegoose` - This allows you to define Mongoose models using TypeScript classes.

In the next step, we will install npm packages as dev dependencies. These packages are only used for development purposes. We will install few utility packages and typescript definition packages. These typescript definition files help the typescript compiler in type checking and also to understand the structure of their respective javascript npm packages (e.g express, graphql).

```console
npm i -D @types/express @types/graphql @types/mongodb @types/mongoose @types/node
npm i -D nodemon ts-node
```

* `nodemon` - Automatically restarting the node application when file changes in the directory are detected.
  
* `ts-node` -  Run typescript files directly, without the need for precompilation using tsc

### Creating Mongoose Schema and GraphQL Schema

In this section, we will create `Mongoose Schema` and `GraphQL Schema`. Mongoose Schema will interact with the `MongoDB` database. We will be using `GraphQL Schema` in our `Resolvers` to create endpoints using `Apollo-Server`.

Create a new file `notes.model.ts` under `src\models` folder. We use the following code to create our `Model` for `Notes`. This `Notes` class has properties like `id, title, description, backgroundcolor, etc`.

We use the `Typegoose` library which acts as a wrapper for easily writing `Mongoose` models with TypeScript. The `@Prop (@Property)` decorator is used for defining the properties for our mongoose schema.

We use the `Type-GraphQL` library to create our GraphQL schema. `@ObjectType` and `@Field` decorators are used for defining properties for `Notes` GraphQL Schema.

By using `Typegoose` and `Type-GraphQL` packages, we eliminate the duplication of code for writing models and schemas separately for MongoDB and GraphQL. We use one single model `notes.model.ts` which acts as a bridge between `MongoDB` and `GraphQL`. 

```typescript
import { prop as Property, getModelForClass, modelOptions } from '@typegoose/typegoose';
import { Field, ObjectType, ID } from 'type-graphql';

@ObjectType({ description: 'The Notes Model' })
@modelOptions({ schemaOptions: { collection: 'notes', timestamps: true } })
export class Notes {
  @Field(() => ID)
  id: string;

  @Field()
  @Property({ type: () => String, required: true })
  title: string;

  @Field()
  @Property({ type: () => String, required: true })
  description: string;

  @Field({ nullable: true })
  @Property({ type: String, required: false })
  backgroundColor: string;

  @Field({ nullable: true })
  @Property({ type: Boolean, required: false })
  isArchived: boolean;

  @Field()
  @Property({ required: true, default: Date.now })
  createdAt: Date;

  @Field()
  @Property({ required: true, default: Date.now })
  updatedAt: Date;
}

export const NotesModel = getModelForClass(Notes);
```

### Create GraphQL Resolvers

Now it is time to create our `notes` resolver. Resolver is collection of functions which helps to retrieve field data (`Query`) and update data in the database (`mutation`). In our `notes` resolver we will implement following Queries and Mutations.

* **Queries**
  * getAllNotes()
  * getNotesById()

* **Mutations**
  * createNotes()
  * updateNotes()
  * deleteNotes()

Lets start the resolver implementation by creating a file `notes.resolvers.ts` in `src\resolvers` folder.

```TypeScript
import { Resolver, Mutation, Arg, Query, ID } from 'type-graphql';
import { NotesModel, Notes } from '../models/notes.model';
import { NotesInput } from './types/notes-input';

@Resolver((_of) => Notes)
export class NotesResolver {
  @Query((_returns) => Notes, { nullable: false, name: 'notes' })
  async getNotesById(@Arg('id') id: string) {
    return await NotesModel.findById({ _id: id });
  }

  @Query(() => [Notes], { name: 'notesList', description: 'Get List of Notes' })
  async getAllNotes() {
    return await NotesModel.find();
  }

  @Mutation(() => Notes, { name: 'createNotes' })
  async createNotes(@Arg('newNotesInput') { title, description, backgroundColor }: NotesInput): Promise<Notes> {
    const notes = (
      await NotesModel.create({
        title,
        description,
        backgroundColor,
        isArchived: false,
      })
    ).save();

    return notes;
  }

  @Mutation(() => Notes, { name: 'updateNotes' })
  async updateNotes(
    @Arg('editNotesInput') { id, title, description, backgroundColor, isArchived }: NotesInput
  ): Promise<Notes> {
    const notes = await NotesModel.findByIdAndUpdate(
      { _id: id },
      {
        title,
        description,
        backgroundColor,
        isArchived,
      },
      { new: true }
    );

    return notes;
  }

  @Mutation(() => String, { name: 'deleteNotes' })
  async deleteNotes(@Arg('id') id: string): Promise<String> {
    const result = await NotesModel.deleteOne({ _id: id });

    if (result.ok == 1) return id;
    else return '';
  }
}
```

We use `@Resolver`, `@Query` and `@Mutation` directives from `TypeGraphQL` package to create our GraphQL schema (`schema.gql`).

For `createNotes` and `updateNotes` mutations, we use `NotesInput` type as the input paramter. This `NotesInput` contains the data for creating and editing the notes.

Create a new file `notes-input.ts` in the `src\resolvers\types` folder.

```TypeScript
import { Field, InputType, ID } from 'type-graphql';
import { Notes } from '../../models/notes.model';

@InputType()
export class NotesInput implements Partial<Notes> {
  @Field(() => ID, { nullable: true })
  id: string;

  @Field()
  title: string;

  @Field()
  description: string;

  @Field({ nullable: true })
  backgroundColor: string;

  @Field({ nullable: true })
  isArchived: boolean;
}
```

##### (Work in progress)
