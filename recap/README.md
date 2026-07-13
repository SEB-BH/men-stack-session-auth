<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Recap</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to articulate the process of session authentication in an express application.

## Recap

Congratulations! We've completed an express application in which users can sign up for a new account, sign in to the app, view resources restricted to signed-in users, and sign themselves out. This was a big project, so it's worth reviewing the key aspects from each of the major steps we took.

## Modularization of the controllers

We have some great news for you: once you've written authentication once, you can re-use it for all your future express applications. This is why we separated our routes into a new `controllers` directory that you can easily copy into future projects. While we didn't create a second model in this application, you can imagine that future applications will start with a User as the very first model, then add more models for the resources users are allowed to create and manage in the app. Nearly all of the app ideas swimming around in your head are apps that incorporate users, authentication, and all the concepts that go with them, so it's a great template to kickstart your next express application.

## Session-based authentication

For this application, we chose session-based authentication as our strategy. This relies on cookies, stored in both the browser and the server's internal memory, and encrypted using a `SESSION_SECRET` environment variable. This cookie gets attached to all future requests coming from users of our site, and our server will check to confirm that the user hasn't added anything to the cookie that doesn't match the version stored in the server.

## Encrypted passwords

One very, very important thing we did was to encrypt the user's passwords before storing them in the database. If you remember only one thing from this experience, make it this: **never store passwords in plain-text**.

In line with best practices for security-related programming, we've chosen to use the industry-standard `bcrypt` library for our cryptographic needs. This approach teaches an important lesson: whenever possible, it's wise to rely on tools that are well-established and trusted in the industry. These tools have been rigorously tested and are widely used by major players in the field. Remember, when it comes to security, especially cryptography, it's not advisable to create your own solutions from scratch. Trust in the proven and tested methods!

## User model constraints

We used a simple `if` statement to determine whether a `username` was already taken, and this should work for our demo applications. For production-ready applications, however, you'd want a bit more validation than that. For example, in our application, `spongebob` and `sPoNgEbOb` would be two totally separate usernames thanks to the difference in capitalization. Not great!

In most applications, a common practice is to store the `username` in the database in all lowercase. This approach ensures consistency and avoids issues with case-sensitive login processes. Alongside this, applications often include a separate `display name` field. This field allows users the flexibility to customize their name's capitalization and style as they wish. It's a user-friendly feature that lets individuals express themselves while keeping the underlying login mechanism straightforward and reliable. Many social media tags work this way: you can change your display name daily if you like, but the actual `username` handle remains static.

There's also the concept of using emails to validate accounts before permitting them to do certain things in the app. This `email` field would also have to be unique to each user model, and it could be used to send a special link through email to new users requiring them to validate before continuing in the app. We would also rely on this email when implementing functionality for resetting a forgotten password, which brings with it even further programming challenges.

## We have to end somewhere!

For now, username validations, email verification, and resetting passwords are all well beyond the scope of our introduction to authentication, but we want to give you an idea of the many authentication-related tasks that a mature, production-ready application needs to consider. You have enough here in our authentication template to create a great portfolio-worthy demo app, but not quite enough to start signing up users who expect your app to work like more mature applications out in the real world.

Fortunately, since these needs are mostly the same for all applications, there are plenty of ready-made libraries and solutions out there competing for developer adoption. For express in particular, `passport.js` is a common authentication solution that's relatively easy to plug in, and allows you to add social authentication such as signing in with Google accounts.

There are also plenty of minor optimizations in the Level Up section of this module, including saving sessions to your MongoDB database, creating a smooth user experience of signing in automatically after signing up, and more!
