# ECMA ESSENTIALS

_To Micke and Anders, vincit qui se vincit_

This is a walkthrough of features found in modern JavaScript. It won't cover everything, just basic features that will be found in virtually any modern JS project. Some of these features have existed for a long time, others
were added in ES6, ES7 or any other new release of the ECMA Script specification.

## Constants and letdowns

Don't use `var`, use `const` and `let` instead. 

`const` is, as the name implies, a constant and can only be assigned to once, otherwise it will throw an error:
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
console.log(name); // Undefined, but no crash
var name = 'Magnus';
console.log(name); // Magnus

// let
console.log(name); // Uncaught ReferenceError: name is not defined
let name = 'Magnus';
console.log(name);
```

## Fat arrows

Instead of boring old standard functions we can use arrow functions:

```js
// Life with arrow functions
const myFunction = (callback) => {
  callback(); // Foo
};

myFunction(() => console.log('Foo'));

// Life without arrow functions
function myOldFunction(callback) {
  callback(); // Foo
}

myOldFunction(function() {
  console.log('Foo')
});
```

Apart from a briefer syntax arrow functions has the added benefit of not creating a separate scope for `this`:

```js
const person = {
  age: 34,
  foo: function foo() {
    setTimeout(function() {
      console.log(this.age); // Undefined, this is bound to the global scope
    }, 0);
    setTimeout(() => {
      console.log(this.age); // 34, this is bound to the local scope
    }, 0)
  },
}

person.foo();
```

## Default parameters

Parameters in functions can have default values, very nifty:

```js
function foo(bar = 15) {
  console.log(bar);
}

foo(12); // 12
foo(); // 15
```

You also have a "rest" parameters that you can use that sums up all the remaining parameters into an array:

```js
function foo(a, b, ...c) {
  console.log(a); // 1
  console.log(b); // 2
  console.log(c); // [3, 4]
}

foo(1, 2, 3, 4);
```

## Spread operator

You can spread the elements of an array:

```js
const turtles = ['Mike', 'Leo', 'Raf', 'Donny'];
const heroes = ['Superman', 'Batman', ...turtles]; 
console.log(heroes); // ['Superman', 'Batman', 'Mike', 'Leo', 'Raf', 'Donny']
```

## Improved object management

You can create an object with variables directly as properties:

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

const {
  name,
  isDumb
} = person;

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

You can also destroy incoming properties in a function:

```js
// Without destructuring
const func1 = (person) => {
  console.log(person.name);
  console.log(person.age);
}

// With destructuring
const func2 = ({
  name,
  age
}) => {
  console.log(name);
  console.log(age);
}

const p = {
  name: 'Magnus',
  age: 34
};

func1(p);
func2(p);
```

## Template literals

You can concatenate text with ease using template literals:

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

console.log(ಠ_ಠ); // 'Why...'
```

## Import/Export

You can export values from a file and import them in another. This is benefitial for many reasons, the main one being code structure.

```js
// NAMED EXPORT 

// myFunc.js
// Export a function called 'myFunc'
export const myFunc = () => {
  console.log('Doing something funky...');
}

// someOtherFile.js
// Import from ./myFunc a value named 'myFunc'
import {
  myFunc
} from './myFunc';

myFunc(); // 'Doing something funky...';
```

```js
// DEFAULT EXPORT

// myFunc.js
// Export a default function
export default () => {
  console.log('Doing something funky...');
}

// someOtherFile
// Import from ./myFunc the default export
import myFunc from './myFunc';

myFunc(); // 'Doing something funky...';
```

If you export a default value you can import it without brackets. Brackets in import statements are used to import named exports, such as in the first example.

There are many ways to export values:

```js
// Default export a function
export default () => console.log('hello werd');

// Export a named function
export const myFunction = () => console.log('goodbai werd');

// Export a named function (old school)
export function myFunction() {
  console.log('Ugh, old world problems...');
}

// Export a named object
export const person = {
  name: 'Magnus',
  age: 34
};

// Default export a declared function
const myFunc = () => console.log('Wee Woorld');
export default myFunc;
```

## Classes

You can use proper class syntax in ES6, if that's your cup of tea:

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  makeOlder() {
    this.age++;
  }
}

const p = new Person('Magnus', 34);
console.log(p.name); // 'Magnus'
console.log(p.age); // 34
p.makeOlder();
console.log(p.age); // 35
```

You can also have class inheritence:

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  makeOlder() {
    this.age++;
  }
}

class Employee extends Person {
  constructor(name, age, position) {
    super(name, age);

    this.position = position;
  }

  work() {
    if (this.age >= 35) {
      console.log('Boss, my back hurts. I think this is it...');
    } else {
      console.log('I am not listening...');
    }
  }
}

const e = new Employee('Magnus', 34, 'Developer');
e.work(); // 'I am not listening...'
e.makeOlder(); // Inherited
e.work(); // 'Boss, my back hurts. I think this is it...'
```

## Object assigning

You can source over properties from one object to another using the Object.assign method:

```js
// Old
const p1 = {name: 'Magnus'};
const p2 = p1;
p2.name = 'Mangolf';
console.log(p1.name); // 'Mangolf', argh, freaking copy by reference!

// New
const p1 = {name: 'Magnus'};
const p2 = Object.assign({}, p1); // Create an empty object and copy over the properties of p1 to it
p2.name = 'Mangolf';
console.log(p1.name); // 'Magnus'
```

If you source over from multiple objects and the same properties exist in multiple projects, the latest one will take precedence:

```js
const magnus = {name: 'Magnus', age: 34};
const electra = {name: 'Electra', power: 'Superkill'};
const z = Object.assign({}, magnus, electra);
console.log(z.name);	// 'Electra', name was found in both, but electra was added after magnus
console.log(z.age);	// 34
console.log(z.power); // 'Superkill'
```

## Promises

Promises are handy when you need to work asynchronously. A promise will accept a delegate to be run upon completion:

```js
const myFunc = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log('I am done now');
      resolve();
    }, 500);
  });
}

myFunc()
  .then(() => {
    console.log('Promise is now resolved');
  });

console.log('End of code');

// End of code
// I am done now
// Promise is now resolved
```

You can also use the `Promise.all` method to wait until multiple promises complete:

```js
const myFunc = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log('I am done now');
      resolve();
    }, 500);
  });
}

const myOtherFunc = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (new Date().getHours() > 12) {
        resolve();
      } else {
        reject();
      }
    }, 0);
  });
}

Promise.all([
  myFunc(),
  myOtherFunc()
]).then(() => console.log('All done'));
```

## Working with collections

There are many nifty features available when working with collections, here are some of the handiest and how to use them:

```js
const p1 = {
  name: 'Magnus',
  age: 34
};
const p2 = {
  name: 'Electra',
  age: 490
};
const p3 = {
  name: 'Dunderklumpen',
  age: 7
};

const people = [p1, p2, p3]; // A good list of people


// Use map to mutate an object into something different, such as getting all names from a list of people:
const names = people.map((person) => person.name);
console.log(names); // ['Magnus', 'Electra', 'Dunderklumpen'];


// Use filter to "filter" out objects that meet a certain criteria:
const seniors = people.filter((person) => person.age > 100);
console.log(seniors); // [{name: 'Electra', age: 490}];


// Use every to check if all elements in the array meets a certain criteria:
const areAllAdults = people.every((person) => person.age > 18);
console.log(areAllAdults); // false


// Use some to check if at least one element in the array meets a certain criteria:
const areSomeAdults = people.some((person) => person.age > 18);
console.log(areSomeAdults); // true


// Use find to find one specific element within the array:
const magnus = people.find((person) => person.name === 'Magnus');
console.log(magnus); // {name: 'Magnus', age: 34}


// Use findIndex to find the index of a specific element within the array:
const magnusIndex = people.findIndex((person) => person.name === 'Magnus');
console.log(magnusIndex); // 0


// Use reduce to reduce an array into a single value:
const combinedAge = people.reduce((total, person) => total += person.age, 0);
console.log(combinedAge); // 531


// Use reduce to find the highest age of a person in the array:
const highestAge = people.reduce((oldestAge, person) => {
  if (person.age > oldestAge) {
    oldestAge = person.age;
  }
  return oldestAge;
}, 0);
console.log(highestAge);


// Combine multiple array features:
const namesOfOldies = people.filter((person) => person.age > 18)
  .map((person) => person.name);
console.log(namesOfOldies); // ['Magnus', 'Electra']
```