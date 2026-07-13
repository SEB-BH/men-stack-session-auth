<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Sign Users Out</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to implement user sign out functionality by deleting a session.

## Signing the user out

Now that we're creating sessions when users sign in, we can simply delete those sessions when a user wants to sign out.

Let's start by adding a link on the landing page for users to sign out. We only want to show this link if there is a currently signed in user, so we should put it inside the same conditional that displays that user's username.

In `views/home.ejs`:

```html
<% if (user) { %>
<h1>Welcome to the app, <%= user.username %>!</h1>
  <!-- new sign out link -->
  <form action="/auth/sign-out?_method=DELETE" method="POST">
    <button type="submit">Sign out</button>
  </form>
<% } else { %>
<h1>Welcome to the app, guest.</h1>
<p>
  <a href="/auth/sign-up">Sign up</a> or <a href="/auth/sign-in">Sign in</a>.
</p>
<% } %>
```

## Define the route

The sign out link will send a `GET` request to `/auth/sign-out`, so we should prepare to accept that request in the auth controller. Let's set up a minimal route with a `res.send` for testing purposes.

Add the following to `/controllers/auth.js`:

```javascript
const signOut = (req, res) => {
  res.send("The user wants out!");
};
```

## Export the function

```js
module.exports = {
    home,
    showSignUpForm,
    signUp,
    showSignInForm,
    signIn,
    signOut
}
```

## Add the route

In `server.js`:

```js
app.delete('/auth/sign-out', authCtrl.signOut)
```

## Finish the controller function

To sign a user out, we need to get rid of the session attached to the `req` object. Lucky for us, the express session object has a built-in method conveniently named `destroy`, allowing us to easily delete the session using `req.session.destroy()`. Nice!

After signing out, most applications will redirect the user back to the landing page, so let's do just that.

The final controller method should look like this:

```javascript
const signOut = (req, res) => {
  req.session.destroy();
  res.redirect("/");
};
```

## Testing the sign out functionality

That's all there is to it! We should be able to manually test the application by signing in, seeing our username on the landing page, and clicking the sign out link to be returned to the landing page as an anonymous guest. You can also validate this by seeing whether the sign in and sign up links are present.
