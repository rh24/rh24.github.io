---
layout: post
title:      "JS - The Hard Parts"
date:       2018-05-30 14:07:12 -0400
permalink:  js_-_the_hard_parts
---


I love coding. I love building things. I love trying out new methods, and solving frustrating problems for hours, but one of the things I really wanted to develop over the course of this career-changing experience is the ability to explain difficult concepts in a clear and concise way. Two of those concepts I want to brush up on are Hoisting and Scope.

## What is hoisting?

The most important part of understanding hoisting comes in knowing that when JavaScript code is run, it's processed in two phases: first, the compilation phase and, second, the execution phase.

```
var cat = 'alfred';
```

`var` is the **keyword** that lets the engine know what comes after it is a variable.
`cat` is our **identifier** with which we use to access its assigned **value** `'alfred'`.

In the compliation phase, the engine stores the keyword and identifiers in memory, effectively *hoisting* them to the top of the stack. It is only in the execution phase that its assignment is saved to memory.

```
function breedOf() {
  return `${cat} is a ${breed}.`;
}

breedOf(); //=> 'undefined is a undefined.'

var cat = 'alfred';
var breed = 'scottish fold';
```
Wait, so I thought that since cat and breed variables are being declared in the global execution context, that when `breedOf()` is invoked, we'd have a return value of ```alfred is a scottish fold.``` What's happening?

Well, according to the rules of hoisting, ```var cat;``` and ```var breed;``` are stored in memory in the first phase at runtime, along with the function declaration `function breedOf() {}` and its definition ```return ${cat} is a ${breed}.``` So, upon the first compilation phase, our code would look like this:

```
// keyword and identifiers are 'hoisted' to the top of their lexical scope
var cat;
var breed;

function breedOf() {
  return `${cat} is a ${breed}.`;
}

breedOf(); //=> 'undefined is a undefined.'
```
So, the first time our `breedOf` function is invoked at execution, our two variables are still undefined.
However, right after breedOf is invoked, the variable assignments occur. That's right: variable declarations are hoisted, while assignments *are not*. They are left in place! Therefore, if I were to invoke `breedOf` once more, both the variables and assignments will have been saved in the execution context's memory and we'll see ```alfred is a scottish fold``` printed in the console.

```
function breedOf() {
  return `${cat} is a ${breed}.`;
}

console.log(breedOf()); // 'undefined is a undefined.'

var cat = 'alfred';
var breed = 'scottish fold';

console.log(breedOf()); // 'alfred is a scottish fold.'
```

## Scope

Lexical scoping is kind of like people having their own backpacks, if we were to think of functions as people. Variables would be the things we put into our backpacks. Each function gets its own backpack and has access to only the things in its own backpack.

```
function myNameIs() {
  var name = 'Rebecca';
	console.log(`my name is ${name}.`)
}

function myNameIsStill() {
  console.log(`my name is still ${name}.`
}

myNameIs(); //=> 'my name is Rebecca.'
myNameIsStill(); //=> 'my name is still undefined.'
```
`myNameIsStill()` returns undefined because it does not have access to ```var name```. That variable lives outside of the function's scope. However, if I were to move that variable to the global scope, both of my functions--```myNameIs``` and ```myNameIsStill``` would have access to my name variable. The reason for this is that functions have access to all variables within its own scope along with the variable environment it's contained in. Both of the example functions live in the global scope.

```
var name = 'Rebecca';

function myNameIs() {
	console.log(`my name is ${name}.`)
}

function myNameIsStill() {
  console.log(`my name is still ${name}.`)
}

myNameIs(); //=> 'my name is Rebecca.'
myNameIsStill(); //=> 'my name is still Rebecca.'
```
## const vs let
1. ```const``` cannot be reassigned. ```let``` can be reassigned.

A common misconcspetion about ```const``` is that its value is immutable. This is not true! ```const``` is simply used when the variable is not to be rebound, meaning that if the value is an object, we cannot reassign the value to be an array. Additionally, we cannot overwrite the original assignment of ```const```, though we can certainly add to its datatype. ```let```, on the other hand, may be both reassigned and rebound.

```
let array = [1, 2, 3];

array = ['a', 'b', 'c'];
console.log(array); // ['a', 'b', 'c']

array = 'hello world';
console.log(array); // 'hello world'


const obj = {firstName: 'christopher', lastName: 'jenkins'};

obj = {firstName: 'rebecca', lastName: 'hong'};
console.log(obj); // SyntaxError: Assignment to constant variable: obj

obj.type = 'human'
console.log(obj); // { firstName: 'christopher', lastName: 'jenkins', type: 'human' }
```

2. ```const``` must be initialized upon declaration. It cannot be declared without an assignment, while let can.
```
const unassigned; // SyntaxError: Unexpected token
let unassigned; //=> undefined
```

## named() vs var named = function () {}
Function declarations are always hoisted, while function expressions (named and/or anonymous functions saved to a variable) are not.

```
unnamedFunc(); //=> 'TypeError: unnamedFunc is not a function'
namedFunc(); //=> 'TypeError: namedFunc is not a function'
unnamedNonArrow(); //=> 'TypeError: unnamedNonArrow is not a function'
declaredFunc(); // 'my function declaration prints because it's hoisted.'

const unnamedFunc = () => {
  console.log('does this anonymous arrow function saved to a const print?')
}

const namedFunc = function myFunc() {
  console.log('named saved to variable.')
}

const unnamedNonArrow = function() {
  console.log('anonymous non-arrow function saved to variable.')
}

function declaredFunc() {
  console.log('my function declaration prints because it's hoisted.')
}
```
The first three examples are saved to a `const`. They are known as function expressions. Because function expressions are not hoisted, invoking them will result in a `TypeError`.

### () => 'Arrow functions'

```
var turtle = {
  name: 'wen',
	actions: ['eat', 'sleep', 'crawl'],
	attack: function () {
	  return `${this.name} is hissing!`;
	}
}

turtle.attack(); //=> 'wen is hissing!'
```
In the above example, my `this` value is referring to the object on which my attack() function is invoked--in this case, the turtle object. Thus, my return value's ```${this.name}``` returns ```wen```.
However, my ```this``` value is scoped only one level deep, meaning that if I were to add an inner function as a callback, I would lose the value of ```this``` as my turtle object and the ```this``` would defer to the window object. This is because each new function that's executed creates its own execution context/scope.

```
var turtle = {
  name: 'wen',
	feelings: ['angry', 'hungry', 'tired', 'happy', 'excited'],
	actions: ['eat', 'sleep', 'crawl'],
	attack: function () {
	  this.feelings.forEach(function (feeling) {
		  if (feeling === 'angry') {
			  console.log(`${this.name} is hissing!`);
				console.log(this) // [object Window]
			} else {
			  console.log(`${this.name} is ${feeling}`);
			}
		})
	}
}

turtle.attack();

//  is hissing!
//  is hungry
//  is tired
//  is happy
//  is excited
// => undefined
```
One solution is to use ```.bind```:
```
var turtle = {
  name: 'wen',
	feelings: ['angry', 'hungry', 'tired', 'happy', 'excited'],
	actions: ['eat', 'sleep', 'crawl'],
	attack: function () {
	  this.feelings.forEach(function (feeling) {
		  if (feeling === 'angry') {
			  console.log(`${this.name} is hissing!`);
			} else {
			  console.log(`${this.name} is ${feeling}`);
			}
		}.bind(this)) // binds the `turtle` object to the function so that subsequent `this` values refer to the same turtle object
	}
}

turtle.attack();

//  wen is hissing!
//  wen is hungry
//  wen is tired
//  wen is happy
//  wen is excited
// => undefined
```
Another way to solve the inconsistent `this` scoping is through the use of ES6 arrow functions. With arrow functions, the `this` value of the inner function is inherited from the scope it sits inside. It carries no this value of its own. So automatically, the this value of both the outer function and the inner function will be scoped to the same object. There's no need to use the ```.bind(this)``` after my callback.

```
var turtle = {
  name: 'wen',
	feelings: ['angry', 'hungry', 'tired', 'happy', 'excited'],
	actions: ['eat', 'sleep', 'crawl'],
	attack: function () {
	  this.feelings.forEach((feeling) => {
		  if (feeling === 'angry') {
			  console.log(`${this.name} is hissing!`);
			} else {
			  console.log(`${this.name} is ${feeling}`);
			}
		})
	}
}

turtle.attack();

//  wen is hissing!
//  wen is hungry
//  wen is tired
//  wen is happy
//  wen is excited
// => undefined
```

Lexical scope and hoisting are difficult topics to nail down. However, combing through these concepts with technical posts and code snippets helped me internalize a bit more how to understand what happens to JavaScript code at run time.
