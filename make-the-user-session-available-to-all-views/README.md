<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Make the User Available to All Views</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to use `res.locals` to make session user data available in every EJS view.

> 💡 This is useful when several pages share a navbar that changes after sign-in.

## Create the middleware

```bash
mkdir -p middleware
touch middleware/pass-user-to-view.js
```

Add the following:

```js
const passUserToView = (req, res, next) => {
    res.locals.user = req.session.user ? req.session.user : null
    next()
}

module.exports = passUserToView
```

## Import the middleware

In `server.js`:

```js
const passUserToView = require('./middleware/pass-user-to-view')
```

Add it immediately after the session middleware:

```javascript
app.use(
  session({
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: true,
  })
);
// Add our custom middleware right after the session middleware
app.use(passUserToView);
```

Now every EJS view can access `user`.

## Simplify render calls

The home controller no longer needs to pass `user`:

```js
const home = (req, res) => {
    res.render('home.ejs')
}
```

The dashboard controller can also be simplified:

```js
const showDashboard = (req, res) => {
    res.render('dashboard.ejs')
}
```
