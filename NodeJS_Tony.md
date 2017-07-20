# Node.js - Tony Alicea

## What we need to add to JS 

    + Better ways to Organize our code into reusable pieces
    + Ways to deal with files
    + The ability to communicate over the internet
    + The ability to accept Requests and Send Responses
    + A way to deal with work that takes a long time

## Events

System Events (C++ core - libuv)
Custom Events (JavaScript Core - Event Emitter)

## JavaScript Aside - Object.create and Prototype

```js
var person = {
    firstname: '',
    lastname: '',
    greet: function() {
        return this.firstname + ' ' + this.lastname;
    }
}

var john = Object.create(person);
john.firstname = 'John';
john.lastname = 'Doe';

var jane = Object.create(person);
jane.firstname = 'Jane';
jane.lastname = 'Doe';

console.log(john.greet());
console.log(jane.greet());
```

![Prototype Chain Diagram](/images/Prototype_Chain.png)

## util.inherits

```js
var EventEmitter = require('events');
var util = require('util');

function Greetr() {
    EventEmitter.call(this); // If we don't don't call the parent constructor, parent's properties would not be assigned to object created using child constructor. (In this example, greeter1 created using new Greetr(). [Lecture 39 Policeman and Person example]
    // Calling the parent constructor makes it run and attaches the properties to the object created
    this.greeting = 'Hello world!';
}

util.inherits(Greetr, EventEmitter);

Greetr.prototype.greet = function(data) {
    console.log(this.greeting + ': ' + data);
    this.emit('greet', data);
}

var greeter1 = new Greetr();

greeter1.on('greet', function(data) {
    console.log('Someone greeted!: ' + data);
});

greeter1.greet('Tony');
```

![Inherits Diagram](/images/NodeJS_Inherits.png)

## ES6 Classes

```js
function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}

Person.prototype.greet = function() {
    console.log('Hello, ' + this.firstname + ' ' + this.lastname);
}

// above code is equivalent to --->

class Person {
    constructor (firstname, lastname) {
        this.firstname = firstname;
        this.lastname = lastname;
    }

    // Apart from constructor other methods will be put on the prototype
    greet() {
        console.log('Hello, ' + this.firstname + ' ' + this.lastname);
    }
}

var john = new Person('John', 'Doe');
john.greet()

```

## JS Aside - Synchronous and Asynchronous

JavaScript is Synchronous. One line of code at a time.
NodeJS is asynchronous.

## File I/O

```js

var fs = require('fs');

var greet = fs.readFileSync(__dirname + '/greet.txt', 'utf8');
console.log(greet);

var greet2 = fs.readFile(__dirname + '/greet.txt', 'utf8', function(err, data) {
    console.log(data);
});

console.log('Done!');

// Hello World // Code will wait for the file to be read
// Done!
// Hello World // Code will NOT wait because asynchronous
```

## Streams

```js
var fs = require('fs');

var readable = fs.createReadStream(__dirname + '/greet.txt', { encoding: 'utf8', highWaterMark: 16 * 1024 }); // highWaterMark = The maximum number of bytes to store in the internal buffer

var writable = fs.createWriteStream(__dirname + '/greetcopy.txt');

readable.on('data', function(chunk) {
    console.log(chunk);
    writable.write(chunk);
});
```


## Pipes 

```js
var fs = require('fs');
var zlib = require('zlib');

var readable = fs.createReadStream(__dirname + '/greet.txt');

var writable = fs.createWriteStream(__dirname + '/greetcopy.txt');

var compressed = fs.createWriteStream(__dirname + '/greet.txt.gz');

var gzip = zlib.createGzip();

readable.pipe(writable);

readable.pipe(gzip).pipe(compressed);
```
