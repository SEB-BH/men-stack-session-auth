<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Protect Routes with Middleware</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to move repeated route-protection logic into middleware.

> 💡 This refactor becomes useful when many routes require sign-in.

## Create the middleware

```bash
mkdir middleware
touch middleware/is-signed-in.js
```

Add the following to `middleware/is-signed-in.js`:

```js
const isSignedIn = (req, res, next) => {
    if (req.session.user) {
        return next()
    }

    res.redirect('/auth/sign-in')
}

module.exports = isSignedIn
```

## Import the middleware

In `server.js`:

```js
const isSignedIn = require('./middleware/is-signed-in')
```

## Add it to the protected route

Update the dashboard route:

```js
app.get('/dashboard', isSignedIn, authCtrl.showDashboard)
```

## Simplify the controller

The middleware now performs the session check, so `showDashboard` only needs to render the page:

```js
const showDashboard = (req, res) => {
    res.render('dashboard.ejs', {
        user: req.session.user
    })
}
```

You can reuse `isSignedIn` on future routes that require authentication.
