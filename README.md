# how to contribute a new database adapter or aervice





introduction

This project follows a functional programming style
Each adapter or service is built as a collection of functions instead of classes
If you want to add a new database adapter or service you need to follow a few steps so the code stays consistent and easy to test

--------

prerequisites

before starting make sure you

fork the project and work on a new branch

understand the folder structure

follow linting and coding standards

write proper tests for what you add

---------------




Folder Structure

The codebase is organized like this


i copied this shape to make it easy to be understanded

src  
 ├── adapters  
 │    ├── mongodb  
 │    ├── postgres  
 │    └── your new adapter  
 └── services  
      ├── email  
      ├── payment  
      └── your new service  


all database adapters go under adapters
all external services like email or payment go under services


-----------------------------------



adding a New Database Adapter

1 create a new folder under adapters with the name of your database
2 define the core functions connect query disconnect
3 these functions handle connecting running queries and closing the connection

example

 src/adapters/ db name/index.js

export const connect = (config) => {
 code to establish the connection
}

export const query = (sql params) => {
  code to execute the query
}

export const disconnect = () => {
  code to close the connection
}

----------------



4 write tests to ensure the adapter works

import * as db from ../src/adapters/db

test(connect should establish a connection async () => {
  const result = await db.connect({ url })
  expect(result).toBeTruthy()
})

----------------





adding a New Service

1 Create a new folder under services
2 Define functions such as init execute shutdown
3 init handles configuration execute performs the action shutdown cleans up

example

 src/services/ service name/index.js

export const init = (config) => {
  setup for the service for example api keys
}

export const execute = async (payload) => {
  perform the service action like sending an email or processing a payment
  return { success true data  }
}

export const shutdown = () => {
  cleanup resources
}


---------------------------


4 add tests for each function

testing

before submitting make sure you

run npm test to confirm all tests pass

run npm run lint to check style rules
--------
submitting a Pull Request

when your code is ready

push it to your branch

open a pull request with a clear description

include usage steps and examples
--------
best Practices

keep functions pure without hidden side effects

use dependency injection pass config or clients as arguments

avoid mutating global state

write modular and reusable code

cover important scenarios in your tests

------

example Usage

here is how another developer can use your adapter or service once merged

for a database adapter

import * as mydb from ./adapters/mydb

await mydb.connect({  db url })  
const result = await mydb.query(select * from users)  
await mydb.disconnect()


for a service

import * as mailservice from ./services/mail

mailservice.init({ apiKey service key })  
await mailservice.execute({ to user mail body Hello })  
mailservice.shutdown()


this way your contribution stays clean consistent and easy to use by anyone in the project

