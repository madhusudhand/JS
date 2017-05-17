# Strict mode

Converting mistakes into errors

**Usage**

```
'use strict'; or "use strict";
```

### Scope

1. not hoisted: strict mode gets applied to statements that appears after 'use strict'.
2. It is block scoped;


The following would fail with TypeError or ReferenceError in strict mode, where as in normal mode they fail silently.

1. No global contexts

```js
y = 123;          // throws a ReferenceError
```

2. Assignment to a non-writable globals

```js
NaN = 5;          // throws a TypeError
undefined = 10;   // throws a TypeError
Infinity = 15;    // throws a TypeError
```

3. Assignment to a non-writables

```js
var obj1 = {};
Object.defineProperty(obj1, 'x', { value: 42, writable: false });
obj1.x = 9;       // throws a TypeError

// Assignment to a getter-only property
var obj2 = { get x() { return 17; } };
obj2.x = 5; // throws a TypeError

// Assignment to a new property on a non-extensible object
var fixed = {};
Object.preventExtensions(fixed);
fixed.newProp = 'ohai'; // throws a TypeError
```

4. Attempts to delete undeletable properties

```js
delete Object.prototype; // throws a TypeError
```

5. Function params must be unique

```js
function f1(a, a, c) { // !!! syntax error

}
```

6. Blocks accedentaly setting properties on primitive values. 

```js
false.true = '';          // TypeError
(14).sailing = 'home';    // TypeError
'with'.you = 'far away';  // TypeError
```

7. 

## References

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
