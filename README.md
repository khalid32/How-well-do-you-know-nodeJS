# How-well-do-you-know-nodeJS

## Description
A list of specific questions by Samer Buna a Node.js developer is expected to answer

### 1. How come when you declare a global variable in any Node.js file it’s not really global to all modules?
A module's code is wrapped by a function wrapper that looks like the following
```
(function(exports, require, module, __filename, __dirname){
    // module code
})
```
This wrapping allows to keeps top-level variables (defined with var, const or let) scoped to the module, rather than to the global object.
In node, the top-level scope is not the golbal scope; `var something` inside a Node module will be **local to that module**.

### 2. When exporting the API of a Node module, why can we sometimes use `exports` and other times we have to use `module.exports`?
To understand the difference, we can look at this simplified view of a JavaScripe file in Node.js:
```
var module = { exports: {} };
var exports = module.exports;

// your code

return module.exports;
```

so, `exports` is initially an alias to `module.exports`. If you want to simply export an object with named fields, you can use the `exports` shortcut. For example, had we written `exports.a = 9`, we'd actually export this object: `{a: 9}`.

However, if you want to export a function or another object, you have to use the `module.exports` object. For example: `module.exports = function bar(){}`. Once you do that, `exports` and `module.exports` no longer reference the same object.

### 3. Can we require local files without using relative paths?
There are several options, but my favourites are these:

###### 1. The Alias
- Install the [module-alias](https://www.npmjs.com/package/module-alias) package
```
npm i --save module-alias
```
- Add paths to your `package.json` like this:
```
{
    "_moduleAliases": {
        "@lib": "app/lib",
        "@models": "app/models"
    }
}
```
- In your entry-point file, before any `require()` calls:
```
require('module-alias/register')
```
- You can now require files like this:
```
const Article = require('@models/article');
```

###### 2. The Global
- In your entry-point file, before any `require()` calls:
```
global.__base = __dirname + '/';
```
- In your very/far/away/module.js
```
const Article = require(`${__base}app/models/article`);
```

###### 3. The Module
- Install some module:
```
npm install app-module-path --save
```
- In your entry-point file, before any `require()` calls:
```
require('app-module-path').addPath(`${__dirname}/app`);
```
- In your very/far/away/module.js
```
const Article = require('models/article');
```

### 4. What is the Event Loop? Is it part of V8?
In event-driven programming, an application expresses interest in certain events and respond to them when they occur. This is the way Node.js can handle asynchronous execution while running the code in a single thread. When an asynchronous operation starts (for example, when we call `setTimeout`, `http.get` or `fs.readFile`), Node.js sends these operations to a different thread allowing V8 to keep executing our code. Node also calls the callback when the counter has run down or the IO/http operation has finished. In Node.js, the responsibility of gathering events from the operating system or monitoring other sources of events is handled by [libuv](https://github.com/libuv/libuv), and the user can register callbacks to be invoked when an event occurs. The event-loop usually keeps running forever.

> the event loop is something that embedders should have control over. However, it is also a fundamental abstract concept of the JavaScript programming model. V8's solution is to provide a default implementation that embedders can override. See [Relationship between event loop,libuv and v8 engine.](https://stackoverflow.com/questions/49811043/relationship-between-event-loop-libuv-and-v8-engine)

###### SUB QUES: Does the event loop and JavaScript code is running in the same thread?
Effectively **YES**. the "event loop" isn't really a thing that's running. It's mostly just a queue of callbacks waiting for their turn to run.

### 5. What is the Call Stack? Is it part of V8?

The call stack is the basic mechanisum for javascript code execution. When we call a function, we push the function parameters and the return address to the stack. This allows to runtime to know where to continue code execution once the function ends.
In NodeJS, the Call Stack is handled by V8.

[Understanding the JavaScript call stack](https://medium.freecodecamp.org/understanding-the-javascript-call-stack-861e41ae61d4)

The call stack is primarily used for `function invocation(call)`. Since the call stack is single, function(s) execution is done one at a time from top to bottom. means **the call stack is synchronous**.

> A call stack is a data structure that uses the **Last In, First Out(LIFO)** principle to temporarily store and manage `function invocation(call)`.

###### Temporarily Store
When a function is invoked(called), the function, its parameters and variables are pushed into the call stack to form a stack frame. This stack is a memory location in the stack. The memory is cleared when the function returns as it is pop out of the stack.
![Temporarily Store](https://user-images.githubusercontent.com/8571179/54331221-348e3980-4643-11e9-9597-6b0f99e977f7.png)

