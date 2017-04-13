# Generators

> ES2015

An Iterable and Pausable function. They are Producers.

```js
function* genFunc() {
  yield 'hello world!!';
}
```
Returns
```js
{ value: 'hello world!!', done: false }
```


1. They can be called using **.next()**
2. Since it implements Iterable 

```js
for (let x of genFunc()) {
    console.log(x);
}
```

3. Can be used with spread operator (...), which turns iterated sequences into elements of an array.
```js
let arr = [...genFunc()]; // ['a', 'b', 'c']
let [x, y] = genFunc();
```

## Producer

```js
function* foo() {
    yield 'a';
    yield 'b';
    return 'c';
}
```

### yield*

The operator yield* for making recursive generator calls.

```js
function* bar() {
    yield 'x';
    yield* foo();
    yield 'y';
}
```
This actually means

```js
function* bar() {
    yield 'x';
    for (let value of foo()) {
        yield value;
    }
    yield 'y';
}
```

However yield* does not have to be a generator object, it can be any iterable.

```js
function* bar() {
    yield 'sequence';
    yield* ['of', 'yielded'];
    yield 'values';
}
```


## Consumers/Observers

```js
genObj.next('a');
```

#### yield binds loosely

**yield a + b + c;** evaluated as **yield (a + b + c);** but not as **(yield a) + b + c;**


#### returnig from a generator at any time (completion)

```js
> function* genFunc() {}
> genFunc().return('yes')
{ value: 'yes', done: true }
```






## Examples

1.
```js
var genFunction = function *() {
  var fn1 = 1;
  var fn2 = 1;
  while (true) {
    var current = fn2;
    fn2 = fn1;
    fn1 = fn1 + current;
    yield current;
  }
};

var generator = genFunction();
var array = [];
for (var i = 0; i < 100; i++) {
  array.push(generator.next().value);
}
```

2.

```js
function* foo(x) {
    var y = 2 * (yield (x + 1));
    var z = yield (y / 3);
    return (x + y + z);
}

var it = foo( 5 );

// note: not sending anything into `next()` here
console.log( it.next() );       // { value:6, done:false }
console.log( it.next( 12 ) );   // { value:8, done:false }
console.log( it.next( 13 ) );   // { value:42, done:true }
```



## Going Async

A simple wrapper

```js

function wrapper(fn){
  return function() {
    var gen = fn.apply(this, arguments);
    var v = gen.next();
    gen.next(value);
  }
}

var hello = wrapper(function* () {
    console.log('start');
    var x = yield 1;
    console.log(x);
    console.log('end');
});

hello();

```


Going async with Promises

```js

function wrapper(fn){
  return function() {
    var gen = fn.apply(this, arguments);
    function step(value) {
      var v = gen.next(value);
      if (v.done) return Promise.resolve(v.value);

      return Promise.resolve(v.value).then((value) => {
        // gen.next(value);
        return step(value);
      });
    }

    step();
  }
}


var hello = wrapper(function* () {
    console.log('start');
    
    var x = yield Promise.resolve(10);
    console.log('x resolved');
    
    var y = yield Promise.resolve(20);
    console.log('y resolved');
    
    console.log(x+y);
    
    console.log('end');
});

hello();

```



Forget everything, lets make it simple.

```js

async hello() {
  console.log('start');
    
  var x = await Promise.resolve(10);
  console.log('x resolved');

  var y = await Promise.resolve(20);
  console.log('y resolved');

  console.log(x+y);

  console.log('end');
}


hello();

```


Which is exactly same as above generator implementation.




## References

1. http://www.2ality.com/2015/03/es6-generators.html
2. https://medium.com/javascript-scene/the-hidden-power-of-es6-generators-observable-async-flow-control-cfa4c7f31435#.rbvkpu6bi
3. https://gist.github.com/learncodeacademy/bf04432597334190bef4
4. https://github.com/tj/co
