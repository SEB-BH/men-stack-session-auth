<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Build a Landing Page</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to build an Express route that renders a landing page for an application.

## The Landing page

For our authentication app, we'll use a landing page to direct users to links where they can sign up or sign in to the authenticated sections of the application.

## Define the route

First, let's create a basic route in our `server.js` file. This initial route will simply send back a text response for us to test. Once that's working, we'll return to it and link our EJS file. Remember, this code, like all express routes, should be placed above the `app.listen()` method.

```javascript
// server.js

// GET /
app.get("/", async (req, res) => {
  res.send("hello, friend!");
});
```

Test the route by browsing to `localhost:3000`. You should receive a message in the browser that says "hello, friend!"

Once We've confirmed the route is valid, we can do more than just send a message back.

## Create a `views` directory and landing page template

We're going to need a `views` directory to place all of our templates in, so let's create it now.

```bash
mkdir views
```

Our landing page will be an `home.ejs` file that is inside of this directory. Let's create that too:

```bash
touch views/home.ejs
```

Next, we'll add a basic HTML structure to our `views/home.ejs` file. Inside the `<body>` tag, include some content for display. Also, remember to update the `<title>` tag to reflect the page's purpose.

```html
<!-- views/index.ejs -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Home Page</title>
  </head>
  <body>
    <h1>Welcome to the app!</h1>
  </body>
</html>
```

Once we have this setup, the next step is to configure our app to serve this page in response to requests made to the root `/` route.

Change the existing `res.send()` method to `res.render()` the file we just created:

```javascript
// server.js

// GET /
app.get("/", async (req, res) => {
  res.render("home.ejs");
});
```