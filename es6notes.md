#React

state is isolated to the component that it's in 

props are something that can be passed in 

#ES6

babeljs.io compiler to use es6 in browser

# Destructuring 

for 

```js
var foo = {
	bar: 1,
	boz: 2
};
```
you would normally do 

```
var bar = foo.bar; 
var boz = foo.boz; 
```

but with destructuring you can simply do 

```
var { bar, boz } = foo; 
```

this also works with arrays. Pull out the first argument like so: 

```
var tenses = ["me", "you", "he"];
var [ firstPerson ] = tenses; 
```
Destructuring multiple promises: 

```
Promise.all([promise1, promise2]).then(function([result1, result2]) {

});
```

## Building objects

Simpler to build objects with existing variables

```
var foo = 2; 

var obj = {
	bar: 1, 
	foo,
	boz: 3
}

var name = 'will';
var age = 34; 

some.method({ name, age }); 
```

## Avoid hard-coupling with destructuring

prior to es6

```js
function calcBmi(weight, height, max, callback) {
	var bmi = weight / Math.pow(height, 2);
	if (bmi > max) {
		console.log('you're overweight');
	}
	if (callback) {
		callback(bmi);
	}
	
}

calcBmi(weight, height, 25);
calcBmi(weight, height, null, function() {}); 
// stuck with needing proper order and every attribute
// you could do opts and then do opts.weight opts.height etc… but there is a better way with es6
```
Destructuring the arguments: 

```js
function calcBmi({weight: w, height: h, max=25, callback}) {
	var bmi = w / Math.pow(h, 2);
	if (bmi > max) {
		console.log('you're overweight');
	}
	if (callback) {
		callback(bmi);
	}
	
}

calcBmi({ weight, height, max: 25 });
calcBmi({ weight, height, callback: function() {} }); 
```

# Template Strings

ES5 way of doing things: 

```js
var name = "will";
var greet = "Hi, my name is \n" + 
			name + "and I'm a werewolf";
```

ES6 way of doing things using backticks: 

```
var greet = `hi, my name is ${name} and I'm a werewolf`;
```
you can use multiple lines without having to do `\n` 

# block scoping

> __`let` is the new `var`__

`let` defines something in a block 

the es5 way of doing things via hoisting: 

```js
var a = 1;

if (true) {
	var b = 2; 
}

console.log(b); //2 because var b; is hoisted 
```

es6 way of doing things: 

```js
var a = 1;

if (true) {
	let b = 2; 
}

console.log(b); //2 because var b; is hoisted 
```
let creates a new variable for the block and destroys it afterwards. A loop for example: 

```js
for (20) {
	let b = 2; 
}

console.log(b); 
```

The only time you really might use var anymore is if you need to create a var that's accessible outside of the current block which is generally a bad idea anyways. In that case it should have been defined outside to begin with and redefined inside the block. 

### const

`const` is block scoped just like `let`
The differene is that `const` can't be overwritten

```js
const foo = 1; 

if (true) {
	const bar = 2;
	bar = 3; //ERROR 
}

console.log(bar); // not defined 
```

There is a way to change const if it is an object. 

```js
.. 

if (true) {
	const bar = { a: 1};
	bar.a = 2; 
}

console.log(bar); //2 
```

Though mutation is possible this way, const is generally preferred to be used as default for most things over let because it's safer to not mutate variables in javascript. 

# Classes 

ES5 way of constructors

```js
function Parent() {
	//const
}

Parent.prototype.foo = function() {}
Parent.prototype.bar = function() {}
```
ES6 way of doing things

```js
class Parent {
	constructor() {
	
	}
	
	foo() {
	
	}
	
	bar () {
	
	}
}

var parent = new Parent();
parent.foo()  
```

With ES7 (woah) you can even define some static class properties!

```js
class Parent {
	age = 34;
	constructor() {
	
	}
	
	static foo() {
	
	}
..
}
var parent = new Parent(); 
parent.age //34 
Parent.foo(); //static function! 
```
And you can extend easily too: 

```js
class Child extends Parent {
	constructor() {
		super() // call parent constructor
	}
	
	boz() {
		
	}
}

var child = new Child();
child.bar(); // can use bar from Parent 
child.foo() // ERROR can't use because static 

```
# arrow function

New ways of writing functions

ES5 way of doing things 

```js

var foo = function(a, b) {
	return a + b; 
};
```
ES6 way of doing things 

```js
var foo = (a, b) => {
	return a + b; 
}
```
very helpful when passing arguments as functions 

```js
do.something((a, b) => { return a + b; })
```

implicit returns (shortcut only for one-liners)

```js
do.something((a,b) => a + b )
```
really useful for things like mapping 

```js
[0,1,2].map(val => val++); // [1,2,3] 
```

## lexial binding 

# es6 modules

ES5 way of doing things

```js 
module.exports.foo = function() {

};

module.exports.bar = function() {

};

//another file 

var myModule = require('myModule');
var foo = myMdoule.foo; 
```

ES6 way of doing things: 

```js
..
// another file 

import myModule from "myModule"; 
```
specific attributes/methods through destructuring 

```js
import { foo, bar } from "myModule"; 
```
cleaning up the exported functions with es6 exports

```js
export var foo = 3; 
export function bar() {

}

// another file 

import { foo, bar } from "myModule"; 

//also supports aliasing 

import { foo as fool, bar as barista } from "myModule";

console.log(fool);
```

you can also use `export default { }; for entire module`

#promises

for everything that starts an operation and will return a value at an indeterminate later time 

```js
var getProfile = $.ajax({type: 'GET', url: 'profile.json'}); 

/* jquery will either resolve on success or reject with xhr information. All promises have a 'then' function. */

// .then(success, error)

// if successful: 

getProfile.then(
	function(data) {
		console.log(data);
	}, 
// if error 
	function(xhr, state, error) {
		console.log(arguments);
	}
); 
```
chaining is where promises really shine 

```js

$.get('profile.json').then(function(profile) {
	return $.get('friend.json?id='.profile.id);
}).then(function(friend) {

}, function(xhr, status, error) {

});  
```
Notice: you only need to do one error function as when one promise fails it will skip down