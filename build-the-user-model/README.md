<h1>
  <span class="headline">MEN Stack Session Auth</span>
  <span class="subhead">Build the <code>User</code> Model</span>
</h1>

**Learning objective:** By the end of this lesson, students will be able to create a User model with mongoose.

## Setting up the models directory

Create a new `models` directory and a `user.js` file for the User model:

```bash
mkdir models
touch models/user.js
```

## The `User` model

Applications vary in their implementation of the user model. Some rely on an `email` field, others simply have a `username` as a way to identify and differentiate users. To keep things simple, we'll use `username` and `password` for our two fields. For authentication to work properly, we need both fields to be required in the user schema.

Add the following in `user.js`:

```javascript
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
  },
  password: {
    type: String,
    required: true,
  },
});

const User = mongoose.model("User", userSchema);

module.exports = User;
```