#2 Chapter 2: Values

[Book Source](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch2.md)

#### Arrays

1. `array`s are **numerically** indexed, but they also are objects that can have `string` keys/properties added to them (which don't count toward the `length` of the `array`)

	```javascript
	var a = [];

	a[0] = 1; // added one element to array: a = [1];
	a['foobar'] = 2 // added one property to array 'object': a = [1]; a.foobar = 2;

	a.length;  // 1
	a['foobar']; // 2
	a.foobar; // 2
	```

2. **Gotcha**: if a `string` value intended as a key can be coerced to a standard base-10 `number`, then it's assumed that you wanted to use it as a `number` index rather than as a `string` key!

	```javascript
	var a = [];

	a['13'] = 42; // '13' will ba coerced to a number 13; a = [,,,,...,42];

	a.length; // 14
	```

#### Strings

1. `string`s are **immutable**
		none of `string` methods that alter its contents can modify in-place

	`array`s are quite **mutable** 
	many of methods that change `array` contents actually do modify in-place	
	**EXCEPT**: `push()`, `pop()`, `shift()`,`unshfit()`, `reverse()`, `splice()`

	```javascript
	var a = 'foo';
	var b = ['f', 'o', 'o'];

	a.toUpperCase(); // 'FOO'
	a; // 'foo'

	b.push('!');
	b; // ['f', 'o', 'o', '!'];
	```

2. we can "borrow" non-mutation `array` against out `string`:

	```javascript
	var a = 'foo';

	a.join(); // undefined

	var c = Array.prototype.join.call(a, '-');

	c; // 'f-o-o'
	```

	**Note**: `reverse()` and other in-place mutator methods for `array` do not work on immutable `string`s

	```javascript
	var a = 'foo';

	a.reverse() // undefined

	Array.prototype.reverse.call( a ) // TypeError, returns a string object wrapper

	//correct way
	a.split('').reverse().join('');
	```

#### Numbers

1. Small Decimal Values.

	```javascript
	0.1 + 0.2 === 0.3; //false
	```

	the representations for `0.1` and `0.2` in binary floating-point are not exact, so when they are added, the result is not exactly `0.3`. it's really close: `0.30000000000000004`.

	ES6, `Number.EPSILON` is predefined with this tolerance value.

	```javascript
	//polyfill
	if(!Number.EPSILON){
		Number.EPSILON = Math.pow(2, -52);
	}

	function numbersCloseEnoughToEqual(n1, n2) {
		return Math.abs(n1 - n2) < Number.EPSILON;
	}

	var a = 0.1 + 0.2;
	var b = 0.3;

	numbersCloseEnoughtToEqual (a , b);  // true
	```

2. `null` is an empty value, not an identifier
	`undefined` is a missing value, is an identifier

3. `void` Operator
	Can be a way to get `undefined`, doesn't modify existing value;

	```javascript
	var a = 42;

	console.log(void a, a); //undefined, 42
	```

4. polyfill `Number.isNaN()`

	```javascript
	if(!Number.isNaN){
		Number.isNaN = function (n) {
			return n !== n;
		};
	}
	```

#### Value vs. Reference

Simple values (aka scalar primitives) are always assigned/passed by value-copy: `null`, `undefined`, `string`, `number`, `boolean` and `symbol`.

Compound values -- `object`s (including `array`s, and all boxed object wrappers) and `function`s -- always create a copy of the reference on assignment or passing.

```javascript
var a = 2;
var b = a; // `b` is always a copy of the value in `a`

b++;

a; // 2
b; // 3


var c = [1 ,2 ,3];
var d = c; // `d` is a reference to the shated `[1,2,3]` value
d.push( 4 );
c; // [1 ,2, 3, 4]
d; // [1 ,2, 3, 4]
```

References point to the values themselves and not to eh variables.
You cannot use one reference to change where another reference is pointed

```javascript
var a = [1,2,3];
var b = a ;
a; // [1,2,3]
b; // [1,2,3]

//later
b = [4,5,6] // change b actually changed b to point to [4,5,6]
a; // [1,2,3]; a is unchanged
b; // [4,5,6]

// if you want to modify a by changing b;
// you have to modify b in-place
b.length = 0;
b.push(4,5,6);

a; // [4,5,6]
b; // [4,5,6]
```













