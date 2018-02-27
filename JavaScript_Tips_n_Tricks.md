# JavaScript Tips and Tricks

#### Debugger
```js
if (thisThing) {
    debugger;
}
```

#### console.table(array_of_objects)

    Prints as a table
    
#### console.time()
```js
console.time('Timer1');
var items = [];
for(var i = 0; i < 100000; i++){
   items.push({index: i});
}
console.timeEnd('Timer1');
```

#### console.trace
For getting function trace. Who's calling who.
