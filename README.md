# ECMA ESSENTIALS

This is a walkthrough of features found in modern JavaScript. It won't cover everything, just the basic new features that will be found in virtually any modern JS project.

## Constants and letdowns

No more use of `var`, use `const` and `let` instead. `const` can only be assigned to once, otherwise it will throw an error:
```js
const name = 'Magnus';
name = 'Mangolf'; // Uncaught TypeError: Assignment to constant variable.
```

If you need to be able to re-assign the variable, use ´let´:
```js
let name = 'Magnus';
name = 'Mangolf'; // Acceptable!
```

The difference between `let` and `var` in such a case is that `var` is declared globally and `let` is block scoped
```js
// var
console.log(name); // Undefined
var name = 'Magnus';
console.log(name); // Magnus

// let
console.log(name); // Uncaught ReferenceError: name is not defined
let name = 'Magnus';
console.log(name);
```

## Fat arrows

Instead of functions we can now use arrow functions:

```js
// Life with arrow functions
const myFunction = (callback) => {
	callback(); // Foo
}

myFunction(() => console.log('Foo'));

// Life without arrow functions
function myOldFunction(callback) {
	callback(); // Foo
}

myOldFunction(function() {
	console.log('Foo')
})
```

Apart from a briefer syntax arrow functions has the added benefit of not creating a separate scope for `this`:

```js
const person = {
	age: 34,
  foo: function foo() {
  	setTimeout(function() {
    	console.log(this.age); // Undefined, this is bound to the global scope
    },0);
    setTimeout(() => {
    	console.log(this.age); // 34, this is bound to the local scope
    },0)
  },
}

person.foo();
```

## Default parameters

Parameters in functions can now have default values, very nifty:

```js
function foo(bar = 15) {
	console.log(bar);
}

foo(12); // 12
foo(); // 15
```

You also have a "rest" parameters that you can use that sums up all the remaining parameters:

```js
function foo(a, b, ...c) {
	console.log(a); // 1
    console.log(b); // 2
    console.log(c); // [3, 4]
}

foo(1, 2, 3, 4);
```

## Spread operator

You can now spread the elements of an array:

```js
const turtles = ['Mike', 'Leo', 'Raf', 'Donny'];
const heroes = ['Superman', 'Batman', ...turtles]; 
console.log(heroes); // ['Superman', 'Batman', 'Mike', 'Leo', 'Raf', 'Donny']
```

## Improved object management

You can now create an object with variables directly as properties:

```js
const name = 'Magnus';
const age = 34;

const person = { name, age };

console.log(person.name); // Magnus
console.log(person.age); // 34
```

You can also destruct an object back into variables instead of assigning them one by one:

```js
const person = {
	name: 'Magnus',
    isDumb: true
};

const { name, isDumb } = person;

console.log(name); // 'Magnus'
console.log(isDumb); // true
```

The same goes for an array, you can destructor it into variables according to their indexes:

```js
const turtles = ['Mike', 'Leo', 'Raf', 'Donny'];

const [t1, t2, t3, t4] = turtles;

console.log(t1); // 'Mike';
console.log(t2); // 'Leo';
console.log(t3); // 'Raf';
console.log(t4); // 'Donny'
```

## Template literals

You can now concatenate text with ease using template literals:

```js
const name = 'Magnus';

console.log('My name is ' + name); // Old, peasant way
console.log(`My name is ${name}`); // Spectacular!

const multiLineString = `I can
write on many
lines, lol
`; // Write multiline strings
```

## UTF-8 variables

...

```js
// For whatever reason, this is valid code...
const ಠ_ಠ = 'Why...';

console.log(ಠ_ಠ);
```

