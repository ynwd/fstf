# Fastify & Sequelize 

This webapp using `node.js` and `fastify`. 

## Table of content
- [Why fastify](#why-fastify)
- [Folder Structure](#folder-structure)
  - [Webapp Folders and Files](#webapp-folders-and-files)
  - [Database Folders](#database-folders)
  - [Tools and Configuration Files](#tools-and-configuration-files)
- [Setup](#setup)
- [How to use](#how-to-use)
- [Testing](#testing)
- [Docker](#docker)


## Why Fastify
- Simple setup & syntax
- Route validation and serialization
- High perfomance. [(See benchmarks)](https://www.fastify.io/benchmarks/)
- Ecosystem support & communities : 38 core plugins dan 102 community plugins. [(See plugins list)](https://www.fastify.io/ecosystem/)

## Folder Structure
```
.
├── .env
├── .gitignore
├── .dockerignore
├── Dockerfile
├── docker-compose.yml
├── package.json
├── readme.md
├── app.js
├── seeders
│   ├── 20200815085544-customer.js
│   └── 20200815093736-account.js
├── config
│   └── config.json
├── migrations
│   ├── 20200815065515-create-customer.js
│   └── 20200815070338-create-account.js
├── models
│   ├── account.js
│   ├── customer.js
│   └── index.js
├── modules
│   └── init.js
└── services
    ├── account
    │   ├── __test__
    │   │   ├── account_dao.spec.js
    │   │   └── index.spec.js
    │   ├── account_dao.js
    │   └── index.js
    └── transfer
        ├── __test__
        │   ├── index.spec.js
        │   └── transfer_dao.spec.js
        ├── index.js
        └── transfer_dao.js

10 directories, 25 files
```

## Webapp Folders and Files
|Item|Type|Description|
|--|--|--|
|.env|file|environtment configuration|
|package.json|file|node modules package|
|app.js|file|application entry point|
|services|folder | place for controller & data access object|
|services/account|folder| place for account service module|
|services/account/index.js|file| account controller|
|services/account/account_dao.js|file| data access object |
|services/account/\_test\_/account_dao.spec.js|file| unit test for account_dao.js |
|services/account/\_test\_/index.spec.js|file| unit test for index.js |
|services/transfer|folder| place for account service module|
|services/transfer/index.js|file| transfer controller|
|services/transfer/transfer_dao.js|file| data access object |
|services/transfer/\_test\_/transfer_dao.spec.js|file| unit test for transfer_dao.js |
|services/transfer/\_test\_/index.spec.js|file| unit test for index.js |
|modules| folder | place for library and helper files|
|modules/init.js| file | helper file for initiating tables and data for testing| 

## Database Folders
These folders are generated by `npx sequelize init`
|Item|Type|Description|
|--|--|--|
|seeders|folder | place for insert some data into a few tables by default |
|config|folder | place for db config |
|migrations|folder| place for table creation definition |
|models|folder| place for table definition |

## Tools and Configuration Files
|Item|Type|Description|
|--|--|--|
|.gitignore|file|git ignore|
|.dockerignore|file|docker ignore|
|Dockerfile|file|Dockerfile|
|docker-compose.yml|file|Docker compose|
|readme.md|file|manual|


## Setup
- Install nodejs

  mac:
  ```
  brew install node
  ```
  another OS:

  https://nodejs.org/en/download/package-manager/
  
- Install semua package:
  ```
  npm install
  ```
- Install mysql-server

  mac:
  ```
  brew install mysql@5.7
  ```
  Start mysql:
  ```
  mysql.server start 
  ```

## How to use
- Configuration

  file `.env`
  ```conf
  APP_HOST=0.0.0.0
  APP_PORT=3000
  DB_PORT=3306
  DB_HOST=localhost
  DB_USER=root
  DB_PASSWORD=root
  DB_DATABASE=test
  ```

- development
  ```
  npm run dev
  ```
- production
  ```
  npm start
  ```
- init table
  ```
  npx sequelize-cli db:migrate
  ```
- init data
  ```
  npx sequelize-cli db:seed:all
  ```
- consume end point with postman

  https://documenter.getpostman.com/view/231995/T1LMiSmv?version=latest#429f4718-59f9-4c5e-bca3-7b60623b6795

## Testing
Testing webapp ini menggunakan [`jest`](https://jestjs.io/).

- service unit test
  - get account
    ```
    npx jest services/account/__test__/account_dao.spec.js
    ```
  - transfer
    ```
    npx jest services/transfer/__test__/transfer_dao.spec.js
    ```

- controller unit test
  - get account
    ```
    npx jest services/account/__test__/index.spec.js
    ```
  - transfer
    ```
    npx jest services/transfer/__test__/index.spec.js
    ```
- coverage
  ```
  npm test
  ```


## Docker

- Configuration

  Update file `.env`. 
  
  Ganti `DB_HOST` ke `mysql` -- nama database service yang didefinisikan di `docker-compose.yml`.
  ```conf
  APP_HOST=0.0.0.0
  APP_PORT=3000
  DB_PORT=3306
  DB_HOST=mysql
  DB_USER=root
  DB_PASSWORD=root
  DB_DATABASE=test
  ```

- Build image:
  ```
  docker-compose build
  ```
- Run mysql:
  ```
  docker-compose up -d mysql
  ```
- Run webapp:
  ```
  docker-compose up -d web
  ```
- Endpoint (IP tergantung local environment)

  init tables & data:
  ```
  http://192.168.99.122:3000/init
  ```
  get account:
  ```
  http://192.168.99.122:3000/account/555002
  ```
  transfer:
  ```
  http://192.168.99.122:3000/account/555002/transfer
  ```