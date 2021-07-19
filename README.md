Express middleware

This is a basic todo app that make use of midleware.

**Middleare** is a very cool _Express_ features that allows us to break our app into small functions.

Speaking in another way. **Middleware** functions are functions that have access to the `request` object (req), the `response` object (res), and the next middleware function in the application’s request-response cycle.

So middleware is the between request and response. So when a client request something to the server, this guy can handle this request and decide if it is a good or bad request.

This is a good example:

The client ask to the server: Hey bud, does this user exists?
So the guy in the middle answer: Just a second. Let me check.

And if the answer is _ok_ it move to the next check or request.

```js
function checksExistsUserAccount(request, response, next) {
  const { username } = request.headers;

  const user = users.find((user) => user.username === username);

  if (!user) {
    return response.status(404).json({ error: "User not found" });
  }

  request.user = user;

  return next();
}
```

To use this middleware is simple.

```js
app.get("/todos", checksExistsUserAccount, (request, response) => {
  const { user } = request;

  return response.json(user.todos);
});
```

So everytime we call this route the `checkExistsUserAccount` is called.
