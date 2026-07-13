<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Protecting Routes</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to protect a page by checking for a signed-in user.

## Create a protected page

Create a dashboard view:

```bash
touch views/dashboard.ejs
```

Add the following:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Dashboard</title>
  </head>
  <body>
    <h1><%= user.username %>'s Dashboard</h1>
    <p>Only signed-in users can see this page.</p>
    <p><a href="/">Return home</a></p>
  </body>
</html>
```

## Create the controller function

Add this function to `controllers/auth.js`:

```js
const showDashboard = (req, res) => {
    if (!req.session.user) {
        return res.redirect('/auth/sign-in')
    }

    res.render('dashboard.ejs', {
        user: req.session.user
    })
}
```

The controller checks for `req.session.user`.

- If there is no user, the visitor is redirected to sign in.
- If there is a user, the dashboard is rendered.

## Export the function

```js
module.exports = {
    home,
    showSignUpForm,
    signUp,
    showSignInForm,
    signIn,
    signOut,
    showDashboard
}
```

## Add the route

In `server.js`:

```js
app.get('/dashboard', authCtrl.showDashboard)
```

## Add the dashboard link

Inside the signed-in section of `views/index.ejs`:

```html
<p><a href="/dashboard">View dashboard</a></p>
```

## Test the protected route

Test both conditions:

1. Sign out and visit `http://localhost:3000/dashboard`. You should be redirected to sign in.
2. Sign in and visit the same URL. You should see the dashboard.

> 💡 This checks authentication: is the visitor signed in? Checking whether a user owns a specific database resource is authorization, which will be covered later.
