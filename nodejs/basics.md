# Node.js basics

## Global object

In javascript (in browsers), the global object would be the window, and we can access attributes and methods of it (e.g. `window.alert()`).

In node.js, the global object is an object called `global` (e.g. `global.console`).

For a specific `.js` file, we can call the global attribute and methods, without specifying the `global.` (e.g. `console.log()`).

## Useful global attributes/function

`__dirname` : directory name of the specific `.js` file.

`__filename`: file name of the specific `.js` file.

`console`: pretty much same as the browser javascript's `console`.

## Function expressions

There are ways to create function. For example:

```js
// Called a function declaration
function hello(){
    console.log('hi');
}

hello();
```

OR

```js
// Called a function expression
var hello = function(){
    console.log('hi');
}

hello();
```

The main differences are:
1) Function declarations load before any code is executed, while function expressions load only when the interpreter reaches that line of code.

Another special type of function is called immediately-invoke function, and it works as its name implies:
```js
{function hello(){
    console.log('hi');
}}();
```

## Node.js modules

A module is basically another `.js` file that is used to separate functionality of the servers into different parts.

### Custom modules

For the subsidiary modules, you have to specific the functions that you wish to export, e.g.:

```js
// count.js
var counter = function(arr){
    return 'There are ' + arr.length + ' elements';
};
var adder = function(a,b){
    return `The sum of the two is ${a+b}.`
};

module.exports.counter = counter;
module.exports.adder = adder;
```

Then use the `require` keyword to load such module.

```js
// main.js
var count = require('./count');

console.log(count.counter(['a','b','c']));
```

Another way is to directly export it by defining the variable directly:

```js
//count.js
module.exports.counter = function(arr){
    return 'There are ' + arr.length + ' elements';
};
module.exports.adder = function(a,b){
    return `The sum of the two is ${a+b}.`
};
```

Another way is to use a concise dict:
```js
// count.js
var counter = function(arr){
    return 'There are ' + arr.length + ' elements';
};
var adder = function(a,b){
    return `The sum of the two is ${a+b}.`
};

module.exports = {
    counter: counter,
    adder: adder,
};
```

### Core Modules

Core modules are modules built-in for the node.js. They can be called in with the `require` keyword.

## Common useful core modules

1. `events` : A module that provides help create custom events for event listeners.

    a. `EventEmitter`: It extends to allow "toggling" of objects. For example:

    ```js
    const events = require('events');

    class Person extends events.EventEmitter{
        constructor(name){
            super();
            this.name = name;
        }
    }

    let james = new Person('james');

    james.on('speak', msg => {
        console.log(james.name + 'said: '+msg);
    })

    james.emit('speak', 'yo');
    ```
    b. `emit`: Used to invoke events. See the above example.

2. `fs`: A module for reading and writing files.
    
    a. `readFileSync()` and `readFile()`: The different is that `readFileSync` is a syncronize function, such that code after this function is blocked until the read is finished. On the other hand, `readFile` is an async function that involves a callback.

    b. `writeFileSync()`

    c. `unlink`: for deleting files.

    d. `mkdirSync` and `mkdir`

    e. `rmdirSync` and `rmdir`

    f. `createReadStream`

    g. `createWriteStream`

3. `http`: A module for hosting a server.

    a. `createServer`: A typical createServer would looks like this:

    ```js
    var server = http.createServer(function(req, res){
        res.writeHead(200, {Headers...});
        res.end('some text to end response...')
    })

    server.listen(3000, '127.0.0.1');
    console.log('Server listening at port 3000');
    ```

    Useful properties:

    - `req.url`: return the resource part of the request url.