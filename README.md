# Javascript style guide

The Javascript style guide used at Procurios is inspired by and laid on top of [the Airbnb style guide](https://github.com/airbnb/javascript/tree/master/es5). We ended up creating our own (forking) because:

1. We aim for synergy between our PHP and JS standards.
2. Many existing style guides are too restrictive for our taste.

Our style guides makes a distinction between `rules` and `recommendation`. A rule must be followed, but it is allowed to break with recommendations if there's a good reason to do so.

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
(function () {
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

### Comparison Operators & Equality

#### Use `===` and `!==` over `==` and `!=`

```js
// bad
if (isCollapsed == true) {
	// do stuff
}

// good
if (isCollapsed === true) {
	// do stuff
}
```

For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

### Blocks

#### Use braces with all multi-line blocks

```js
// bad
if (test)
  return false;

// good
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