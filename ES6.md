# ES6 - Good Parts - Kyle Simpson

## Arrow function

`foo = x => 3;`

You can omit the curly braces in the return part if it is an expression. For statements, you have to put curly braces.

With curly braces, explicit return statement is needed.

For objects, `x => ({ y: x})`

Arrow function does not have a _this_ context. It goes up one level in lexical scope to determine _this_. For example, using setTimeout inside an object will set _this_ to global scope when used with regular function.

```js
var obj = {
    id: 2,
    foo: setTimeout(function() {
        console.log(this.id); // undefined
        }, 1000)
};
```

## Scoping - Let and Const

_let_ is useful with closures and 'for statement'

```js
var funcs = [];
for (var i = 0; i < 3; i++) {      // let's create 3 functions
  funcs[i] = function() {          // and store them in funcs
    console.log("My value: " + i); // each should log its value.
  };
}
for (var j = 0; j < 3; j++) {
  funcs[j]();                      // and now let's run each one to see
}
```

Prints - My value: 3 - three times

if _let_ is used in for statement instead of _var_ then it makes separate copy of the variable.

`const x = [1, 2, 3]`

x cannot be reassigned but value can be changed.
x[1] = 42 is allowed.

Can use Object.freeze instead. `Object.freeze([1,2,3])`

## Default Params

```js
function required(paramName) {
    throw "Parameter " + paramName + " required!";
}
function foo(id = required("id")) { 
    // or id = uniqId() to generate a unique id if none is given
}
```

## Spread Operator

```js
function foo(x,y,z) {
    console.log( x, y, z );
}

foo( ...[1,2,3] );              // 1 2 3
```

```js
var a = [2,3,4];
var b = [ 1, ...a, 5 ];

console.log( b );                   // [1,2,3,4,5]
```

**Gather**

```js
function foo(x, y, ...z) {
    console.log( x, y, z );
}

foo( 1, 2, 3, 4, 5 );           // 1 2 [3,4,5]
```

## Array Destructuring

```js
function foo() {
    return [1, 2, 3, [4,5,6]];
}
var [
    a, 
    b=42, 
    c,
    [
        d, // can use this kind of structure if getting back json from an api
        ,
        e
    ]
    ] = foo(); // Can be without var too if a,b,c are already declared
```

`[x, y] = [y, x] // Swapping`

**Dumping Variables**

```js
var a = [1,2,3];
[ , , ...a] = [ 0, ...a, 4]; // a = [2,3,4]
```

## Object Destructuring

```js
var o = {p: 42, q: true, r: {s: 11}};
var { // if you don't want to use var (if variables are already declared), have to start with "({". Can't start with {.
    p, 
    q,
    r : {
        s
    } = {} // default value
} = o; // same variable name
console.log(p, q, s); // 42 true 11
var {p: foo, q: bar} = o; // different variable name
console.log(foo, bar); // 42 true 
```

## Concise Properties and Methods

```js
var a = 1;
var c =  "hello";
var obj = {
    a, // a: a
    b() {}, // b: function() {}
    [c]: 42 // Computed Property. Earlier, we had to assign it outside.
            // obj[c] = 42
}
```

## Template Strings/Literals

```js
var name = "Kyle";
var orderNo = 123;
var total = 319;

var msg = "Hello, " + name + ", your \
order (#" + orderNo + ") was $" + total + ".";

var msgES6 = `Hello, ${name}, your order (#${orderNo}) was $${total}.`;
```

**Tag Functions/Tagged Template Literals**
Can be used for Internationalization, localization, currency etc.
JSX is one.

Example (Uppercase only the values and not the strings): 
```js
function upper(strings,...values) {
    var str = "";
    for (let i = 0; i < strings.length; i++) {
        if (i > 0) {
            str += values[i-1].toUpperCase(); 
        }
        str += strings[i];
        
    }
    return str;
}

var name = "kyle",
    twitter = "getify",
    classname = "es6 workshop";

console.log(
    upper`Hello ${name} (@${twitter}), welcome to the ${classname}!` ===
    "Hello KYLE (@GETIFY), welcome to the ES6 WORKSHOP!"
);


console.log(
    upper`Hello ${name} (@${twitter}), welcome to the ${classname}!`);
```

## Iterators

Defining iterator for an object

```js
var obj = {
    [Symbol.iterator]() { // same as [Symbol.iterator] = function() {}
        var idx = this.start, en = this.end;
        var it = {
            next: () => { // could have done next() {} but using arrow function because we need to use 'this' below
                if (idx <= en) {
                    var v = this.values[idx];
                    idx++;
                    return { value: v, done: false };        
                } else {
                    return { done: true } // value = undefined automatically
                }

            }
            
        };
        return it;
    },
    values: [2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32],
    start: 4,
    end: 13
};

console.log([...obj]) // [10, 12, 14, 16, 18, 20, 22, 24, 26, 28]
```

## Generators

```js
function *main() {
    console.log("hello");
    yield 9;
    console.log("world");
    yield 10;
    console.log("random");
    return 11;
}

var it = main();

it.next(); // { value: 9, done: false } // console: hello
it.next(); // { value: 10, done: false } // console: world
it.next(); // { value: 11, done: true } // 'for of' operator will discard last value though
```

Coming back to earlier example of defining a iterator

```js
var obj = {
    *[Symbol.iterator]() { // same as [Symbol.iterator] = function() {}
        for ( var i = this.start; i <= this.end; i++) {
            yield this.values[i];
        }
    },
    values: [2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32],
    start: 4,
    end: 13
};

console.log([...obj]) // [10, 12, 14, 16, 18, 20, 22, 24, 26, 28]
```

Exercise 5

```js
var numbers = {
    *[Symbol.iterator]( { 
        start = 0, 
        end = 100,
        step = 1
    } ) { // Object destructuring with default values in the function params
        for (var i = start; i <= end; i += step) {
            yield i;
        }
    }
};

// should print 0..100 by 1s
for (let num of numbers) {
    console.log(num);
}

// should print 6..30 by 4s
for (let num of numbers[Symbol.iterator]({
    start: 6,
    step: 4,
    end: 30
})) {
    console.log(num);
}
```

## Ranges

```js
Number.prototype[Symbol.iterator] = function* () {
    for ( var i = 0; i < this; i++) {
        yield i;
    }
}
[...7] // [0, 1, 2, 3, 4, 5, 6]
```
