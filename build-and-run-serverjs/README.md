<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Build and Run <code class="filepath">server.js</code></span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to set up and run an Express server integrated with MongoDB, use environment variables for configuration, and include the proper middleware stack to process form requests.

## Boilerplate express set-up

Let's begin with the same boilerplate Express code and libraries we've come to know and love. As a quick reminder, this setup includes the essentials that you've already worked with in previous applications - Express for our web server framework, Mongoose for MongoDB interactions, Dotenv for managing our environment variables, EJS for templating, and Morgan for logging requests.

Install packages:

```bash
npm i express mongoose dotenv ejs morgan method-override
```

Create your `.env` file at the root level of the repository and include your `MONGODB_URI` connection string variable.

```bash
touch .env
```

Your `.env` should look like this:

```
MONGODB_URI=<connection-string-from-mongodb-atlas>
```

Add the following code to `server.js`:

```javascript
const dotenv = require("dotenv");
dotenv.config();
const express = require("express");
const app = express();

const mongoose = require("mongoose");
const methodOverride = require("method-override");
const morgan = require("morgan");

// Set the port from environment variable or default to 3000
const port = process.env.PORT ? process.env.PORT : "3000";

mongoose.connect(process.env.MONGODB_URI);

mongoose.connection.on("connected", () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name}.`);
});

// Middleware to parse URL-encoded data from forms
app.use(express.urlencoded({ extended: false }));
// Middleware for using HTTP verbs such as PUT or DELETE
app.use(methodOverride("_method"));
// Morgan for logging HTTP requests
app.use(morgan('dev'));

app.listen(port, () => {
  console.log(`The express app is ready on port ${port}!`);
});
```

## process.env.what ?

You probably noticed a new `port` variable we're defining and using in the `app.listen` statement. This will come in handy when we're deploying our applications and cannot know in advance which port the hosting service will use.

The syntax commonly used to set this variable is a **ternary** statement, which is essentially a condensed `if...else` statement. You could replace it with the following:

```javascript
let port;
if (process.env.PORT) {
  port = process.env.PORT;
} else {
  port = 3000;
}
```

The ternary statement decides which value to assign to the variable. If the `process.env.PORT` variable is defined and has a 'truthy' value, then that value is set as the port number. Otherwise, if it's 'falsy' (undefined or holding no value), the default port `3000` is used. This way, we get to use the environment variable in a hosted environment, and still use `3000` when running the application locally.

## Run the server.js

Let's confirm the application is working before moving on. Run `server.js` and confirm you've received the mongoose connection message.

```bash
nodemon
```

You should see something in your terminal like:

```
Connected to MongoDB <mongoose-uri>.
The express app is ready on port 3000!
```

<!-- 
[Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-session-auth-template/tree/build-and-run-serverjs-start)

[Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-session-auth-template/tree/build-and-run-serverjs-complete) -->
