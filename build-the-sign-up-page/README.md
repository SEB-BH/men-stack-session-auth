<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Build the Sign Up Page</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create a sign up page with a form allowing the creation of new users.

## Create the sign up page

To begin our authentication journey, we will need to be able to create new user accounts. Let's start by creating the template with the form for creating an new user.

Create a new `auth` directory inside of `views`. This will hold views specific to our authentication routes. Within `views/auth/` create the `sign-up` template:

```bash
mkdir views/auth
touch views/auth/sign-up.ejs
```

This template will have a simple form, like so:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Sign Up</title>
  </head>
  <body>
    <h1>Create a new account!</h1>
    <form action="" method="">
      <label for="username">Username:</label>
      <input type="text" name="username" id="username" />
      <label for="password">Password:</label>
      <input type="password" name="password" id="password" />
      <label for="confirmPassword">Confirm Password:</label>
      <input type="password" name="confirmPassword" id="confirmPassword" />
      <button type="submit">Sign up</button>
    </form>
  </body>
</html>
```

Note the use of `type="password"` on our password input. This results in the classic hidden text, replaced by dots, that all password inputs should have across all applications.

We've also set up a `confirmPassword` field to ensure users aren't the victims of typos when creating their new account. These fields must match before we create their account. We'll check for that, among other things, in the controller function!

## Adding a navigation link to the landing page

This is a good time to add a simple link to our landing page so a new user can directly navigate to the sign up page:

In `views/home.ejs`:

```html
<a href="/auth/sign-up">Sign up</a>
```

## Defining the route

Now that our authentication routes are defined in a separate controller file, our route handler function will look a little different.

Add this function to `controllers/auth.js`:

```js
const showSignUpForm = (req, res) => {
    res.render('auth/sign-up.ejs')
}
```
Export it:

```js
module.exports = {
    home,
    showSignUpForm
}
```

## Add the route

In `server.js`:

```js
app.get('/auth/sign-up', authCtrl.showSignUpForm)
```

## Test the route

To test the route, visit `localhost:3000/auth/sign-up` in your browser. The `server.js` file handles the `/auth` section of the URL, and the `sign-up` is handled by the controller function we just wrote!

You should see the form rendered in the browser.
