# Express.js

Express.js is a module for nodejs that allows easy routing and response.

## Requiring Express.js

Typically requiring express.js is done through:
```js
var express = require('express');

var app = express();

...

app.listen(3000);
```

## HTTP methods

We can tell express.js to handle HTTP methods (GET, POST, etc...) for us. For example:

```js
app.get('/', (res, req)=>{
    res.send("homepage!");
});
```
Notice that expressjs has handle the response header for us.

## Route parameters

Usually there is something useful in the url. We can obtain and route those with route parameters:

```js
app.get('/profile/:name/:id', (res, req)=>{
    res.send("you requested " + res.param.name + " " + res.param.id);
})
```

## Nodemon

Can use nodemon for easy refreshing:

```bash
nodemon app.js
```

## Template engines (ejs)

Most of the time we don't want to return a simple plain html, but instead want to substitute data. We can use ejs for that:

```html
<!--views/profile.ejs-->
<!DOCTYPE html>
<html>
    <body>
        <h1>Welcome <%= person %></h1>
    </body>
</html>
```

```js
// app.js
app.set('view engine', 'ejs');

...

app.get('/profile/:id', (req, res)=>{
    res.render("profile.ejs", {person: req.params.id});
})
```

Note that express.js's default template folder is in `views`.

## JS in ejs

We can use javascript in ejs, simply use `<% your command>`.

## Partial views

Most of the time, elements of each page are reused (like nav bar). In that case, partial views are useful:

```html
<!--/views/partial/nav.ejs-->
<nav>
...
</nav>
```

```html
<!--views/profile.ejs-->
<!DOCTYPE html>
<html>
    <body>
        <% include partials/nav.ejs %>
        <h1>Welcome <%= person %></h1>
    </body>
</html>
```

## Middleware

Middleware is basically codes that run between receiving the req and sending back the res.

To apply middlewares, use:

```js
app.use('/profile', (req, res, next)=>{
    // any middleware function u want
    next(); // Call next so that the next matching middleware can be run
})
```

## Serving static file

We can serve static files with built-in middleware provided by expressjs
```js
app.use('/assets', express.static('assets'))
// The first /assets indicates the url path, while the second 'assets' indicates the folder.
```

## Query strings

Query string in expressjs is easy:

```js
app.get('/path', (req, res)=>{
    console.log(req.query);
})
```

To parse queries, can use the `body-parser` module. Note that there is a complex type called `multipart bodies` that is not supported by `body-parser`.