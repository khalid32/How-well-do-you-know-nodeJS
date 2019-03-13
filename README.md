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

