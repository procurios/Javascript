# Javascript style guide

The Javascript style guide used at Procurios is inspired by and laid on top of [the Airbnb style guide](https://github.com/airbnb/javascript/tree/master/es5). We ended up creating our own (forking) because:

1. We aim for synergy between our PHP and JS standards.
2. Many existing style guides are too restrictive for our taste.

Our style guides makes a distinction between `rules` and `recommendation`. A rule must be followed, but it is allowed to break with recommendations if there's a good reason to do so.

## Table of Contents

- [Rules](#)
	- [Strings](#strings)
		- [Quotes](#quotes)
	- [Functions](#functions)
		- [Declaration](#declaration)
		- [Never declare a function in a non-function block](#never-declare-a-function-in-a-non-function-block)
	- [Variables](#variables)
		- [Always use var](#always-use-var)
		- [One var declaration per variable](#one-var-declaration-per-variable)
	- [Comparison Operators & Equality](#comparison-operators--equality)
		- [Use === and !== over == and !=](#use--and--over--and-)
	- [Blocks](#blocks)
		- [Use braces with all multi-line blocks](#use-braces-with-all-multi-line-blocks)
		- [Put else on the same line as your if block's closing brace](#put-else-on-the-same-line-as-your-if-blocks-closing-brace)
	- [Comments](#comments)
		- [Use /** ... */ for multi-line comments](#use----for-multi-line-comments)
		- [Use // for single line comments](#use--for-single-line-comments)
	- [Whitespace](#whitespace)
		- [Use tabs to indent your code](#use-tabs-to-indent-your-code)
		- [Place 1 space before the leading brace.](#place-1-space-before-the-leading-brace)
		- [Place 1 space before opening parenthesis](#place-1-space-before-opening-parenthesis)
		- [Set off operators with spaces](#set-off-operators-with-spaces)
	- [Commas](#commas)
		- [Don't use leading commas](#dont-use-leading-commas)
		- [Don't place an additional trailing comma](#dont-place-an-additional-trailing-comma)
	- [Semicolons](#semicolons)
		- [Never leave out semicolons](#never-leave-out-semicolons)
	- [Naming](#naming)
		- [Use camelCase when naming variables, objects and functions](#use-camelcase-when-naming-variables-objects-and-functions)
		- [Use PascalCase when naming instances, constructors or classes](#use-pascalcase-when-naming-instances-constructors-or-classes)
		- [When saving a reference to this use _this](#when-saving-a-reference-to-this-use-_this)
	- [Modules (requirejs)](#modules-requirejs)
- [Recommendations](#recommendations)
	- [Objects](#objects)
		- [Creation](#creation)
	- [Arrays](#arrays)
		- [Creation](#creation-1)
		- [Copying](#copying)
		- [Converting](#converting)
	- [Functions](#functions)
		- [Don't name a parameter arguments](#dont-name-a-parameter-arguments)
	- [Variables](#variables)
		- [Declare unassigned variables last](#declare-unassigned-variables-last)
		- [Assign variables at the top of their scope](#assign-variables-at-the-top-of-their-scope)
	- [Comparison Operators & Equality](#comparison-operators--equality-1)
		- [Use shortcuts](#use-shortcuts)
	- [Whitespace](#whitespace)
		- [Use indentation when making long method chains](#use-indentation-when-making-long-method-chains)
	- [Type Casting & Coercion](#type-casting--coercion)
		- [Perform type coercion at the beginning of the statement.](#perform-type-coercion-at-the-beginning-of-the-statement)
		- [Be careful when using bitshift operations](#be-careful-when-using-bitshift-operations)
	- [Naming](#naming)
		- [Be descriptive with your naming](#be-descriptive-with-your-naming)
		- [Name your functions](#name-your-functions)
	- [Constructors](#constructors)
		- [Assign methods to the prototype object](#assign-methods-to-the-prototype-object)
		- [return this to help with method chaining](#return-this-to-help-with-method-chaining)

## Rules

### Strings

#### Quotes
Use single quotes (`''`) for strings. Double quotes are allowed if the string contains a quote that would have to be escaped.

```js
// bad
var name = "Bob Parr";

// good
var name = 'Bob Parr';

// bad
var fullName = "Bob " + this.lastName;

// good
var fullName = 'Bob ' + this.lastName;

// bad
var greeting = "Hi Bob! How is the weather today?";

// good
var greeting = "Hi Bob! How's the weather today?";
```

[↑ back to top](#table-of-contents)

### Functions

#### Declaration

This is how we write function expressions. Note the space before the parentheses that emphasizes the difference between a function call and its definition.

```js
// anonymous function expression
var anonymous = function () {
	return true;
};

// named function expression
var named = function named () {
	return true;
};

// immediately-invoked function expression (IIFE)
;(function () {
	console.log('Welcome to the Internet. Please follow me.');
})();
```

#### Never declare a function in a non-function block

Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

```js
// bad
if (currentUser) {
	function test() {
		console.log('Nope.');
	}
}

// good
var test;
if (currentUser) {
	test = function test() {
		console.log('Yup.');
	};
}
```

[↑ back to top](#table-of-contents)

### Variables

#### Always use `var`

Always use var to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace.

```js
// bad
Car = new Car();

// good
var Car = new Car();
```

#### One `var` declaration per variable

Use one var declaration per variable. It's easier to add new variable declarations this way, and you never have to worry about swapping out a ; for a , or introducing punctuation-only diffs.

```js
// bad
var items = getItems(),
	goSportsTeam = true,
	dragonball = 'z';

// bad (compare to above, and try to spot the mistake)
var items = getItems(),
	goSportsTeam = true;
	dragonball = 'z';

// good
var items = getItems();
var goSportsTeam = true;
var dragonball = 'z';
```

[↑ back to top](#table-of-contents)

### Comparison Operators & Equality

#### Use `===` and `!==` over `==` and `!=`

```js
// bad
if (a == b) {
	// do stuff
}

// good
if (a === b) {
	// do stuff
}
```

For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

[↑ back to top](#table-of-contents)

### Blocks

#### Use braces with all multi-line blocks

```js
// bad
if (test)
  return false;

// bad
if (test) return false;

// good
if (test) {
	return false;
}

// bad
function () { return false; }

// good
function () {
	return false;
}
```

#### Put `else` on the same line as your `if` block's closing brace

```js
// bad
if (test) {
	thing1();
	thing2();
}
else {
	thing3();
}

// good
if (test) {
	thing1();
	thing2();
} else {
	thing3();
}
```

[↑ back to top](#table-of-contents)

### Comments

#### Use `/** ... */` for multi-line comments

Include a description, specify types and values for all parameters and return values [using JSDoc](http://usejsdoc.org/).

```js
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make (tag) {
	// ...stuff...
	return element;
}

// good
/**
 * make() returns a new element
 * based on the passed in tag name
 *
 * @param {String} tag
 * @return {Element} element
 */
function make (tag) {
	// ...stuff...
	return element;
}
```

#### Use `//` for single line comments

```js
// bad
function getType () {
	console.log('fetching type...');
    /** set the default type to 'no type' */
	var type = this._type || 'no type';

	return type;
}

// good
function getType () {
	console.log('fetching type...');

	// set the default type to 'no type'
	var type = this._type || 'no type';

	return type;
}
```

[↑ back to top](#table-of-contents)

### Whitespace

#### Use tabs to indent your code

```js
// bad
function () {
∙∙∙∙var name;
}

// bad
function () {
∙var name;
}

// good
function () {
	var name;
}
```

#### Place 1 space before the leading brace.

```js
// bad
function test (){
  console.log('test');
}

// good
function test () {
  console.log('test');
}
```

#### Place 1 space before opening parenthesis

```js
// bad
if(isJedi) {
  fight ();
}

// good
if (isJedi) {
  fight();
}

// bad
function fight() {
  console.log ('Swooosh!');
}

// good
function fight () {
  console.log('Swooosh!');
}
```

#### Set off operators with spaces

```js
// bad
var x=y+5;

// good
var x = y + 5;
```

[↑ back to top](#table-of-contents)

### Commas

#### *Don't* use leading commas

```js
// bad
var story = [
	once
	,upon
	,aTime
];

// good
var story = [
	once,
	upon,
	aTime
];

// bad
var hero = {
	firstName: 'Bob'
	,lastName: 'Parr'
	,heroName: 'Mr. Incredible'
	,superPower: 'strength'
};

// good
var hero = {
	firstName: 'Bob',
	lastName: 'Parr',
	heroName: 'Mr. Incredible',
	superPower: 'strength'
};
```

#### *Don't* place an additional trailing comma

```js
// bad
var hero = {
	firstName: 'Kevin',
	lastName: 'Flynn',
};

var heroes = [
	'Batman',
	'Superman',
];

// good
var hero = {
	firstName: 'Kevin',
	lastName: 'Flynn'
};

var heroes = [
	'Batman',
	'Superman'
];
```

This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

> Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

[↑ back to top](#table-of-contents)

### Semicolons

#### Never leave out semicolons

```js
// bad
(function() {
	var name = 'Skywalker'
	return name
})()

// good
(function() {
	var name = 'Skywalker';
	return name;
})();

// good (guards against the function becoming an argument when two files with IIFEs are concatenated)
;(function() {
	var name = 'Skywalker';
	return name;
})();
```

[Read more](http://stackoverflow.com/a/7365214/1712802) about guarding IIFEs.

[↑ back to top](#table-of-contents)

### Naming

#### Use camelCase when naming variables, objects and functions

```js
// bad
var OBJEcttsssss = {};
var this_is_my_object = {};
var o = {};
function c () {}

// good
var thisIsMyObject = {};
function thisIsMyFunction () {}
```

#### Use PascalCase when naming instances, constructors or classes

```js
// bad
function user (options) {
	this.name = options.name;
}

var user = new user({
	name: 'nope'
});

// good
function User (options) {
	this.name = options.name;
}

var User = new User({
	name: 'yup'
});
```

#### When saving a reference to this use _this

```js
// bad
function () {
	var self = this;
}

// bad
function () {
  var that = this;
}

// good
function () {
  var _this = this;
}
```

[↑ back to top](#table-of-contents)

### Modules (requirejs)

* The file should be named with PascalCase, and match the name of the single export.
* Always declare `'use strict';` at the top of the module.
* Wrap your module in a IIFE if it contains dynamic dependencies.

Example:

```js
;(function () {
	'use strict';

	var dependencies = [];

	if (!('classList' in document.createElement('p'))) {
		dependencies.push('path/to/classList.min.js');
	}

	define(dependencies,
		/**
		 * @returns {ExampleModule}
		 */
		function () {
			var ExampleModule = function () {
				// stuff
			}

			return ExampleModule;
		}
	);
})();
```

## Recommendations

### Objects

#### Creation
Use the literal syntax for object creation.

```js
// bad
var item = new Object();

// good
var item = {};
```

`{}` has the same return value as `new Object()`, but has the following advantages:

 - `new Object()` can be shadowed (and might have a different return value than expected)
 - `{}` is shorter (less typing, KISS)
 - `{}` can be partially optimized at parse time

[↑ back to top](#table-of-contents)

### Arrays

#### Creation
Use the literal syntax for array creation.

```js
// bad
var items = new Array();

// good
var items = [];
```

Using `[]` has the following advantages:

- It's [much faster](http://jsperf.com/literal-vs-new-23).
- It can't be shadowed (always has the same return value).
- Less typing.

#### Copying
When you need to copy an array use `Array#slice`. It's [fast](http://jsperf.com/converting-arguments-to-an-array/7).

```js
var len = items.length;
var itemsCopy = [];
var i;

// bad
for (i = 0; i < len; i++) {
	itemsCopy[i] = items[i];
}

// good
itemsCopy = items.slice();
```

#### Converting
To convert an array-like object to an array, use `Array#slice`.

```js
function trigger () {
	var args = Array.prototype.slice.call(arguments);
	...
}
```

[↑ back to top](#table-of-contents)

### Functions

#### Don't name a parameter `arguments`

This will take precedence over the `arguments` object that is given to every function scope.

```js
// bad
function nope (name, options, arguments) {
	// ...stuff...
}

// good
function yup (name, options, args) {
	// ...stuff...
}
```

[↑ back to top](#table-of-contents)

### Variables

#### Declare unassigned variables last

This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

```js
// bad
var i, len, dragonball,
	items = getItems(),
	goSportsTeam = true;

// bad
var i;
var items = getItems();
var dragonball;
var goSportsTeam = true;
var len;

// good
var items = getItems();
var goSportsTeam = true;
var dragonball;
var length;
var i;
```

#### Assign variables at the top of their scope

This helps avoid issues with variable declaration and assignment hoisting related issues.

```js
// bad
function () {
	test();
	console.log('doing stuff..');

	//..other stuff..

	var name = getName();

	if (name === 'test') {
		return false;
	}

	return name;
}

// good
function () {
	var name = getName();

	test();
	console.log('doing stuff..');

	//..other stuff..

	if (name === 'test') {
		return false;
	}

	return name;
}

// bad - unnecessary function call
function () {
	var name = getName();

	if (!arguments.length) {
		return false;
	}

	this.setFirstName(name);

	return true;
}

// good
function() {
	var name;

	if (!arguments.length) {
		return false;
	}

	name = getName();
	this.setFirstName(name);

	return true;
}
```

[↑ back to top](#table-of-contents)

### Comparison Operators & Equality

#### Use shortcuts

```js
// bad
if (name !== '') {
	// ...stuff...
}

// good
if (name) {
	// ...stuff...
}

// bad
if (collection.length > 0) {
	// ...stuff...
}

// good
if (collection.length) {
	// ...stuff...
}
```

[↑ back to top](#table-of-contents)

### Whitespace

#### Use indentation when making long method chains
Use a leading dot, which emphasizes that the line is a method call, not a new statement.

```js
// bad
var DraftOrderLine = (OrderLineApi.getOrderLine()).setPrice(1.00).setAmount(1200).setProductId(1).setDescription('Foobar');

// good
var DraftOrderLine = (OrderLineApi.getOrderLine())
	.setPrice(1.00)
	.setAmount(1200)
	.setProductId(1)
	.setDescription('Foobar');
```

[↑ back to top](#table-of-contents)

### Type Casting & Coercion

#### Perform type coercion at the beginning of the statement.

```js
// bad
var totalScore = this.reviewScore + '';

// good
var totalScore = '' + this.reviewScore;

// bad
var totalScore = '' + this.reviewScore + ' total score';

// good
var totalScore = this.reviewScore + ' total score';
```

#### Be careful when using bitshift operations

Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. Discussion. Largest signed 32-bit Int is 2,147,483,647:

```js
2147483647 >> 0 //=> 2147483647
2147483648 >> 0 //=> -2147483648
2147483649 >> 0 //=> -2147483647
```

[↑ back to top](#table-of-contents)

### Naming

#### Be descriptive with your naming

```js
// bad
function q () {
  // ...stuff...
}

// good
function query () {
  // ..stuff..
}
```

#### Name your functions

This is helpful for stack traces.

```js
// bad
var log = function (msg) {
	console.log(msg);
};

// good
var log = function log (msg) {
	console.log(msg);
};
```

[↑ back to top](#table-of-contents)

### Constructors

#### Assign methods to the prototype object

... instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

```js
function Jedi () {
	console.log('new jedi');
}

// bad
Jedi.prototype = {
	fight: function fight () {
		console.log('fighting');
	},
	block: function block () {
		console.log('blocking');
	}
};

// good
Jedi.prototype.fight = function fight () {
	console.log('fighting');
};

Jedi.prototype.block = function block () {
	console.log('blocking');
};
```

#### return `this` to help with method chaining

```js
// bad
Jedi.prototype.jump = function () {
	this.jumping = true;
    return true;
};

Jedi.prototype.setHeight = function (height) {
	this.height = height;
};

var Luke = new Jedi();
Luke.jump(); // => true
Luke.setHeight(20); // => undefined

// good
Jedi.prototype.jump = function () {
	this.jumping = true;
	return this;
};

Jedi.prototype.setHeight = function (height) {
	this.height = height;
	return this;
};

var Luke = new Jedi();

Luke.jump()
	.setHeight(20);
```