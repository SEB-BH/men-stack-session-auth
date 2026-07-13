<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Saving Sessions in MongoDB</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to integrate MongoDB for session storage in an Express application.

## Saving Sessions in MongoDB

You're probably here because it's quickly become very annoying to lose your session every time the server restarts, and testing your express app has become an endless struggle of signing in before being able to do anything. Storing anything in the internal memory of the server is not ideal, so let's fix that by using an npm package that stores our session data in MongoDB.

First, install the package [connect-mongo](https://www.npmjs.com/package/connect-mongo) from the command line:

```bash
npm i connect-mongo
```

Next, we'll set things up in `server.js` to modify our session settings and tell `express-session` to use MongoDB as its chosen storage location.

First, add the import of `MongoStore` right below our list of other require statements, just under the session require statement.

```javascript
const { MongoStore } = require("connect-mongo");
```

Then, we modify the session `app.use` statement to include a `store` property inside the options object provided to our session middleware. If you're wondering where we got this from, it's straight from the connect-mongo documentation linked above!

```javascript
app.use(
  session({
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: true,
    store: MongoStore.create({
      mongoUrl: process.env.MONGODB_URI,
    }),
  })
);
```

## WARNING: Session modifications are now asynchronous

Previously, our sessions lived in the internal memory of the server, so we didn't need to worry about the time it took to update these objects before proceeding to the next line of code.

Now that sessions live in MongoDB, however, any updates to the session object are asynchronous operations that take just as much time as any of the other database calls throughout our app. This means we need to wait for the operation to complete before proceeding to the next part of a user's journey.

For example, if you tested the app in its current state, you'd find that signing out produces a bug called a **race condition** : while the controller function moves happily along from `req.session.destroy()` to `res.redirect(/)`, the redirect might finish before the session has had time to actually update, so the landing page will still display the username and a greeting despite the fact that the user just tried to sign out. Not great!

This means we'll have to go in to all the controller functions that modify `req.session` and revise them to use the asynchronous **callback** pattern provided by `express-session`.

## Modifying the sign in controller

Fortunately, this is a pretty straightforward fix. All we need to do is update the very end of our controller function from the synchronous pattern:

```javascript
req.session.user = {
  username: userInDatabase.username,
};

res.redirect("/");
```

...to an asynchronous pattern by providing a callback function to the `req.session.save` method:

```javascript
req.session.user = {
  username: userInDatabase.username,
};

req.session.save(() => {
  res.redirect("/");
});
```

## Modifying the sign out controller

Similarly, we want to avoid the race condition described above in our sign out process, so let's update that using the same pattern.

```javascript
req.session.destroy(() => {
  res.redirect("/");
});
```
