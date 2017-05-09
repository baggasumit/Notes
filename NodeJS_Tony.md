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