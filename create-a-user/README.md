<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Create a <code>User</code></span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create a new user account through the sign up route.

## Get to the action

For our form to make submissions to the server, we need to modify the `action` and `method` properties to send a `POST` request to the `/auth/sign-up` route.

Modify the following in `views/auth/sign-up.ejs`:

```html
<form action="/auth/sign-up" method="POST">
```

## Define the route

Now, we need to create the controller function handling this request. Let's start with a simple `res.send` response so we can test as we go. Note the use of `async`, as this function will eventually require a database call.

Add the following in `controllers/auth.ejs`:

```javascript
const signUp = async (req, res) => {
  res.send("Form submission accepted!");
};
```

## Export the function

Update `module.exports`:

```js
module.exports = {
    home,
    showSignUpForm,
    signUp
}
```

## Add the route

In `server.js`:

```js
app.post('/auth/sign-up', authCtrl.signUp)
```

## Bring in the User model

Since we want this route to create a new `User` in the database, we'll first need to import the `User` model into this file. 

Add the following at the top of `controllers/auth.ejs`:

```javascript
const User = require("../models/user.js");
```

It would be **great** if we could use a simple `User.create` statement in the route and proceed. Unfortunately, authentication is a bit more complicated than that! 

For our app to be secure and functional, we need to make the following considerations:

1. Usernames need to be unique: two people can't share the same username!
1. The password and confirmPassword fields must match to verify there were no typos.
1. Passwords cannot be stored directly as plain-text in the database, this is not secure.

## Enforcing unique usernames

To make sure somebody hasn't already taken the username being submitted, we need to check the database for any existing user with that username.

Add the following inside the controller function:

```javascript
const userInDatabase = await User.findOne({ username: req.body.username });
if (userInDatabase) {
  return res.send("Username already taken.");
}
```

## Checking `password` and `confirmPassword`

This check will be a little simpler, as we only need a simple comparison of values already submitted through the form:

Add the following inside the controller function, below the user validation:

```javascript
if (req.body.password !== req.body.confirmPassword) {
  return res.send("Password and Confirm Password must match");
}
```

## Securely storing passwords

If all the previous validations are successful, we're ready to create the user in our database. However, storing passwords as plain text is a security risk. In the event of a database breach, plain-text passwords could be easily accessed by hackers. This is particularly concerning since many users often reuse the same passwords across different applications: a security nightmare!

For this and most security-related problems, we should not attempt to solve this ourselves. Instead, its better to rely on established, third-party tools designed for security. These tools ensure that passwords are securely encrypted before being stored in the database. For this we'll use [`Bcrypt`](https://www.npmjs.com/package/bcrypt), a widely-recognized hashing library.

Using the `bcrypt` library, we will perform what's called a `hashing` operation which will scramble the user's password into a difficult-to-decrypt string. The hashing function also requires the use of a `salt` string, which ensures that even if two users have the exact same passwords, we end up with different encrypted strings in the database.

First, install the bcrypt package and import it at the top of the `controllers/auth.ejs` file:

```bash
npm i bcrypt
```

```javascript
const bcrypt = require("bcrypt");
```

Next, add the following lines to the controller function, beneath our previously written validations:

```javascript
const hashedPassword = bcrypt.hashSync(req.body.password, 10);
req.body.password = hashedPassword;
```

The number `10` in the `hashSync` method represents the amount of salting we want the hashing function to execute: the higher the number, the harder it will be to decrypt the password. However, higher numbers will take longer for our application when we're checking a user's password, so we need to keep it reasonable for performance reasons.

With all of our validations in place, we can finally create the new `User` in the database:

```javascript
// validation logic

const user = await User.create(req.body);
res.send(`Thanks for signing up ${user.username}`);
```

## Test the route

You should now be able to navigate to `localhost:3000/auth/sign-up` and create a new user account through the form. You can validate that the password was properly encrypted by viewing the user in MongoDB Atlas.