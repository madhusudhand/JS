# JS Shots

Tiny and Powerful.

## void

nullify any return value.


```js
function abc() {
  if (true) {
    doSomething();  // this could be returning some value, but we are not interested in it.
    return;
  }

  // some other code
}


function abcNew() {
  if (true) {
    return void doSomething();  // this could be returning some value, but we are not interested in it.
  }

  // some other code
}

```



# ...

The spread/collect values for Array and Object.



# **

Exponentiation Operator

```js
var p1 = 2**3;
var p2 = Math.pow(2,3);
```

# ~

```js
var vals = [ "foo", "bar", 42, "baz" ];

if (vals.indexOf( 42 ) > -1) {
  // found it
}

if (~vals.indexOf( 42 )) {
  // found it!
}
```
