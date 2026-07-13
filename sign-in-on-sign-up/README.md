<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Sign In on Sign Up</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to implement an automatic sign-in feature during the account creation process in an Express application. 

## Sign 'em up, sign 'em in!

A typical user flow for most applications will automatically sign a new user in right after they've created a new account. This makes sense, considering you've just gotten all the same details from the user that they'd need to sign in, and it's kind of redundant to make them go back and fill out a nearly identical form to sign in right after they've signed up.

Let's make our user experience better and do this for them in our app.

Briefly, we'll copy the same code from our `sign-in` controller into our `sign-up` controller, and change the redirect statement to go from `/sign-in` straight to the landing page `/` route.

Add the following to `controllers/auth.js` right beneath the `User.create` statement in our `sign-up` route:

```javascript
req.session.user = {
  username: user.username,
};

req.session.save(() => {
  res.redirect("/");
});
```

If you haven't yet implemented saving sessions to MongoDB, the callback pattern in our `req.session.save()` statement will look unfamiliar. In that case, you can still copy over the same code you're already using in the `sign-in` controller, without the `req.session.save()` callback pattern.

And that's it! You should now see that a newly created user will automatically be redirected to the landing page with a greeting for them, and they're properly signed in to the app.