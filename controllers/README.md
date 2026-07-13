<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Controllers</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to export controller functions and connect them to routes in `server.js`.

## Why use a controller?

As an application grows, `server.js` can become difficult to read.

We will keep the route paths in `server.js` and move the route handler functions into a controller.

Create the controller:

```bash
mkdir controllers
touch controllers/auth.js
```

## Create the first controller function

Add the following to `controllers/auth.js`:

```js
const home = (req, res) => {
    res.send('Welcome to the app!')
}

module.exports = {
    home
}
```

The `home` function receives `req` and `res`, just like a route handler inside `server.js`.

## Connect the controller to the route

Import the controller near the top of `server.js`:

```js
const authCtrl = require('./controllers/auth')
```

Add the route above `app.listen()`:

```js
app.get('/auth/home', authCtrl.home)
```

The route path stays in `server.js`. The function that handles the request lives in `controllers/auth.js`.

## Test the route

Visit:

```plaintext
http://localhost:3000
```

You should see:

```plaintext
Welcome to the app!
```

Once we've tested, we don't need to keep this controller, since we already have a landing page.