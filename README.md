Curriculum <br>
**Short Specialization** <br>

# 0x04. Files manager

`Back-end` `JavaScript` `ES6` `NoSQL` `MongoDB` `Redis` `NodeJS` `ExpressJS` `Kue`

```
This project is a summary of this back-end trimester: authentication, NodeJS, MongoDB, Redis, pagination, and background processing.

The objective is to build a simple platform to upload and view files:

* User authentication via a token
* List all files
* Upload a new file
* Change permission of a file
* View a file
* Generate thumbnails for images
```

## Resources

**Read or watch:**

- [Node JS getting started](https://www.nodejs.org/en/docs/guides/getting-started-guide/)
- [Process API doc](https://www.node.readthedocs.io/en/latest/api/process/)
- [Express getting started](https://www.expressjs.com/en/starter/installing.html)
- [Mocha documentation](https://www.mochajs.org)
- [Nodemon documentation](https://www.github.com/remy/nodemon#nodemon)
- [MongoDB](https://www.github.com/mongodb/node-mongodb-native)
- [Bull](https://www.github.com/OptimalBits/bull)
- [Image thumbnail](https://www.npmjs.com/package/image-thumbnail)
- [Mime-Types](https://www.npmjs.com/package/mime-types)
- [Redis](https://www.github.com/redis/node-redis)

## Requirements

- Allowed editors: `vi`, `vim`, `emacs` `Visual Studio Code`
- Files interpreted/compiled on Ubuntu 18.04 LTS using `node` (version 12.x.x)
- All files should end with a new line
- Mandatory `README.md` file
- Code use the `js` extension
- Code verified against lint using ESLint

## Provided files

`package.json`

<details>
  <summary>Click to show/hide file contents</summary>

```json
{
  "name": "files_manager",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "lint": "./node_modules/.bin/eslint",
    "check-lint": "lint [0-9]*.js",
    "start-server": "nodemon --exec babel-node --presets @babel/preset-env ./server.js",
    "start-worker": "nodemon --exec babel-node --presets @babel/preset-env ./worker.js",
    "dev": "nodemon --exec babel-node --presets @babel/preset-env",
    "test": "./node_modules/.bin/mocha --require @babel/register --exit"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "bull": "^3.16.0",
    "chai-http": "^4.3.0",
    "express": "^4.17.1",
    "image-thumbnail": "^1.0.10",
    "mime-types": "^2.1.27",
    "mongodb": "^3.5.9",
    "redis": "^2.8.0",
    "sha1": "^1.1.1",
    "uuid": "^8.0.0"
  },
  "devDependencies": {
    "@babel/cli": "^7.8.0",
    "@babel/core": "^7.8.0",
    "@babel/node": "^7.8.0",
    "@babel/preset-env": "^7.8.2",
    "@babel/register": "^7.8.0",
    "chai": "^4.2.0",
    "chai-http": "^4.3.0",
    "mocha": "^6.2.2",
    "nodemon": "^2.0.2",
    "eslint": "^6.4.0",
    "eslint-config-airbnb-base": "^14.0.0",
    "eslint-plugin-import": "^2.18.2",
    "eslint-plugin-jest": "^22.17.0",
    "request": "^2.88.0",
    "sinon": "^7.5.0"
  }
}
```

</details>

`.eslintrc.js`

<details>
  <summary>Click to show/hide file contents</summary>

```javascript

module.exports = {
  env: {
    browser: false,
    es6: true,
    jest: true,
  },
  extends: [
    'airbnb-base',
    'plugin:jest/all',
  ],,
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
  },
  parserOptions: {
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  plugins: ['jest'],
  rules: {
    'max-classes-per-file': 'off',
    'no-underscore-dangle': 'off',
    'no-console': 'off',
    'no-shadow': 'off',
    'no-restricted-syntax': [
      'error',
	'LabeledStatement',
	'withStatement',
    ],
  },
  overrides:[
    {
      files: ['*.js'],
	excludedFiles: 'babel.config.js',
    }
  ]
};
```

</details>

`babel.config.js`

<details>
  <summary>Click to show/hide file contents</summary>

```javascript
module.exports = {
  presets: [
    [
      "@babel/preset-env",
      {
        targets: {
          node: "current",
        },
      },
    ],
  ],
};
```

</details>

### and...

Don't forget to run `$ npm install` when you have the `package.json`

Task

0. Redis utils
   mandatory

Inside the folder utils, create a file redis.js that contains the class RedisClient.

RedisClient should have:

    the constructor that creates a client to Redis:
        any error of the redis client must be displayed in the console (you should use on('error') of the redis client)
    a function isAlive that returns true when the connection to Redis is a success otherwise, false
    an asynchronous function get that takes a string key as argument and returns the Redis value stored for this key
    an asynchronous function set that takes a string key, a value and a duration in second as arguments to store it in Redis (with an expiration set by the duration argument)
    an asynchronous function del that takes a string key as argument and remove the value in Redis for this key

After the class definition, create and export an instance of RedisClient called redisClient.

11. New user - welcome email
    #advanced

Update the endpoint POST /users endpoint to start a background processing for sending a “Welcome email” to the user:

    Create a Bull queue userQueue
    When a new user is stored (in DB), add a job to this queue with the userId

Update the file worker.js:

    By using the module Bull, create a queue userQueue
    Process this queue:
        If userId is not present in the job, raise an error Missing userId
        If no document is found in DB based on the userId, raise an error User not found
        Print in the console Welcome <email>!

In real life, you can use a third party service like Mailgun to send real email. These API are slow, (sending via SMTP is worst!) and sending emails via a background job is important to optimize API endpoint.
