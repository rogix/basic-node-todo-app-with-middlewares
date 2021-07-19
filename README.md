# Express middleware

This is a basic todo app that make use of midleware.

**Middleare** is a very cool _Express_ features that allows us to break our app into small functions.

Speaking in another way. **Middleware** functions are functions that have access to the `request` object (req), the `response` object (res), and the next middleware function in the application’s request-response cycle.

So middleware is the guy between request and response. When a client ask for something to the server, this guy can handle this request and decide if it is a good or bad request.

This is a good example:

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

The client ask to the server: Hey bud, I need to see all the `todos` of this user.

The route then ask to the middle guy: Hey, middle guy, does this user exists?.

The middle guy check is the user exists and reply: Sure, let me call the next guy. Oh, there is no next guy (if there are no others middlewares funtions), so go on, show the todos.

```js
app.get("/todos", checksExistsUserAccount, (request, response) => {
  const { user } = request;

  return response.json(user.todos);
});
```

So everytime we call this route the `checkExistsUserAccount` is called.

> If there are other middleares, for example: to check the todos, the credentials etc. they will be called until the last one. If some middleware fail it returns an error.
