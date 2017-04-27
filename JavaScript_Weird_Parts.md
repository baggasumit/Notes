# JavaScript - The Weird Parts

## Asynchronous Callbacks


JS engine waits for the Execution context to clear before running any callbacks.

![Execution Context Image](/images/Execution_Context_Image.png)

Functions are objects
```js
function greet() {
    console.log('hi');   
}

greet.language = 'english';
console.log(greet.language);
```

## Closures:

```js
function greet(whattosay) {

   return function(name) {
       console.log(whattosay + ' ' + name);
   }

}


var sayHi = greet('Hi');
sayHi('Tony');
```

------------

```js
function buildFunctions() {
 
    var arr = [];
    
    for (var i = 0; i < 3; i++) {
        
        arr.push(
            function() {
                console.log(i);   
            }
        )
        
    }
    
    return arr;
}

var fs = buildFunctions();

fs[0](); // 3 (Val of i when func is returned) 
fs[1](); // 3 
fs[2](); // 3 
```

**How to solve the problem:**
```js
function buildFunctions2() {
 
    var arr = [];
    
    for (var i = 0; i < 3; i++) {
        arr.push(
            (function(j) { //IIFE
                return function() {
                    console.log(j);   
                }
            }(i))
        )
        
    }
    
    return arr;
}

var fs2 = buildFunctions2();

fs2[0](); // 0
fs2[1](); // 1
fs2[2](); // 2
```

## Function Factories:

```js
function makeGreeting(language) {
 
    return function(firstname, lastname) {
     
        if (language === 'en') {
            console.log('Hello ' + firstname + ' ' + lastname);   
        }

        if (language === 'es') {
            console.log('Hola ' + firstname + ' ' + lastname);   
        }
        
    }
    
}

var greetEnglish = makeGreeting('en');
var greetSpanish = makeGreeting('es');

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doe');
```

-----------------------------------------------------------

## Call, Apply, Bind 

```js
var person = {
    firstname: 'John',
    lastname: 'Doe',
    getFullName: function() {
        
        var fullname = this.firstname + ' ' + this.lastname;
        return fullname;
        
    }
}

// function borrowing
var person2 = {
    firstname: 'Jane',
    lastname: 'Doe'
}

console.log(person.getFullName.apply(person2));
```

## Function currying 
Creating a copy of a func with some present params
```js
function multiply(a, b) {
    return a*b;   
}

var multipleByTwo = multiply.bind(this, 2);
console.log(multipleByTwo(4));
```

## Constructors and ‘new’

```js
function Person(firstname, lastname) {
 
    console.log(this);
    this.firstname = firstname;
    this.lastname = lastname;
    console.log('This function is invoked.');
    
}



Person.prototype.getFullName = function() {
    return this.firstname + ‘ ‘ + this.lastname;
}

var john = new Person('John', 'Doe'); // ‘new’ creates a new empty object and then the following function is invoked. ‘this’ is set to the new empty object. Properties or methods inside the constructor function gets assigned to the new object as well. The new object’s prototype = Person.prototype

// Why not just add getFullName to the constructor function? To save memory space because each new object will get it’s own copy of the function.
console.log(john);
```