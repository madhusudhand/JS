## Let and Const

Block-scoped binding constructs.

`let` is the new `var`.

`const` is single-assignment. 

Static restrictions prevent use before assignment.

### Scopes

var | let | const
--- | --- | ---
inside the `function` | inside the `block` | inside the `block`


**let**

```js
var a = 1;
var b = 2;

if (a === 1) {
  var a = 11; // the scope is global
  let b = 22; // the scope is inside the if-block

  console.log(a);  // 11
  console.log(b);  // 22
} 

console.log(a); // 11
console.log(b); // 2
```


**const**

```js
const foo = 123;
foo = 100; // assignment is not allowed

var foo = 10; // not allowed
let foo = 10; // not allowed

const bar;  // error, missing initializer in const declaration
```

However with `const`, object keys and array values are not protected.

```js
const obj = {'key': 'value'};

// Attempting to overwrite the object throws an error
obj = {'OTHER_KEY': 'value'};

// object keys are not protected,
// so the following statement is executed without problem
obj.key = 'otherValue'; // Use Object.freeze() to make object immutable

// The same applies to arrays
const arr = [];

// It's possible to push items into the array
arr.push('A'); // ['A']

// However, assigning a new array to the variable throws an error
arr = ['B']
```


### Temporal dead zone

> For both let and const

Redeclaring the same variable within the same function or block scope.

```js
if (x) {
  let foo;
  let foo; // SyntaxError thrown.
}
```

let will hoist the variable to the top of the block.

However, referencing the variable in the block before the variable declaration not allowed.

The variable is in a `temporal dead zone` from the start of the block until the declaration is processed.

```js
function do_something() {
  console.log(foo); // ReferenceError
  let foo = 2;
}
```


