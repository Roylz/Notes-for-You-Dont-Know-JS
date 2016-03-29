#2 Chapter 4: Coercion

[Book Source](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch4.md)

#### Converting Values

1. Javascript coercions always result in on of the scalar primitive values.
e.g. `string`, `number`, or `boolean`.

2. No coercion that results in a complex value.
e.g. `object` or `function`.

3. `"type casting"` ==> occur in **statically typed** languages at compile time.
`"type coercion"` ==> is a run time conversion for **dynamically typed** languages.

4. `explicit coercion` ==> when it is obvious from looking at the code that a type conversion is intentionally occurring.
`implicit coercion` ==> when the type coversion will occur as a less obvious side effect of some other intentional operation

	```javascript
	var a = 42;

	var b = a + ""; // implicit coercion

	var c = String( a );  // explicit coercion
	```

**MY NOTE**: From [Airbnb JS suggestion](https://github.com/airbnb/javascript#type-casting--coercion). explicit coercion is a better way to do the coercion, which is `c`

#### Abstract Value Operations

1. ##### `ToString`

	When any non-`string` value is coerced to a `string` representation, the conversion is handled by the `ToString` absreact operation

	`null` => `"null"`,
	`undefined` => `"undefined"`, 
	`true` => `"true"`, 
	`123` => `"123"`,

	for Object, see `ToNumber` section.

	for Arrays, it's `toString()` is overrideen by displaying each values with `","`:

	```javascript
	var a = [1,2,3];

	a.toString(); // 1,2,3
	```

2. ##### `JSON Stringification`

	For most simple values, JSON stringification behaves basically the same as `toString()` conversions, except that the serialization result is always a _`string`_:

	```js
	JSON.stringify(42); // "42"
	JSON.stringify("42"); // ""42"" (a string with a quoted string value in it)
	JSON.stringify(null); // "null"
	```

	**not** JSON-safe: `undefined`s, `function`s, `symbol`s and `object`s with circular refenrences (try `JSON.stringify() to an `object` with circular reference in it will result an error)

	The `JSON.stringify()` utility will automatically omit those values:

	**`a`**. if such a value is found in an `array`, the value is replaced by `null` (to keep the array position)
	**`b`**. if found as a property of an `object`, that property will simply be excluded.

	```js
	JSON.stringify(undefined); // undefined
	JSON.stringify(function(){}); // undefined

	JSON.stringify([1, undefined, function(){}, 4]); // "[1, null, null, 4]"
	JSON.stringify({a: 2, b: function (){}}); // "{a: 2}"

	```

	`object` has a `toJSON()` method, this method will be called **first** to get a value to use for serialization. And it should return the actual regular value (of whatever type) that's appropriate, and `JSON.stringify()` itself will handle the stringification.

3. ##### ToNumber

	`true` => `1`, `false` => `0`, `undefined` => `NaN`, `null` => `0`

	`Objects(Arrays) -> primitive value equivalent -> number (by calling `ToNumber`)

	To get `primitive value` of an object:

		1. check `valueOf()` method of this object.
		2.a. if `valueOf()` is available and it returns a primitive value, use it.
		2.b. else if `toString()` is available, use it for the coercion
		2.c. else if neither operation can provide a **primitive value**, a `TypeError` is thrown

4. ##### ToBoolean
	
	All of JS's vales can be divided into two categories

		1. values that will become `false` if coerced to `boolean`
		2. everything else (which will obviously become `true`)

	_Falsy values:_

	* `undefined`
	* `null`
	* `false`
	* `+0`, `-0`, and `NaN`
	* `""`

	_truty value:_ **a value is truthy if it's not on the falsy list**

5. ##### Explicit Coercion

	`~x` is roughly the same as `-(x+1)`.
	```js
	~42; // -(42+1) ==> -43;
	```

	Using `~` with `indexOf()` "coerces" (actually just transforms) the value **to be appropraiately `boolean`-coercible:

	```js
	var a = "hello world";

	~a.indexOf("lo"); //  -4  <-- truthy!
	~a.indexOf("ol"); // 0  <-- falsy!

	if(~a.index("lo")){
		// found it!
	}
	```

	```js
	parseInt( 0.000008 );       // 0   ("0" from "0.000008")
	parseInt( 0.0000008 );      // 8   ("8" from "8e-7")
	parseInt( false, 16 );      // 250 ("fa" from "false")
	parseInt( parseInt, 16 );   // 15  ("f" from "function..")

	parseInt( "0x10" );         // 16
	parseInt( "103", 2 );       // 2
	```

6. ##### Implicit Coercion

	`String(a)` invokes `toString()` directly

	```js
	var a = {
		valueOf: function () {
			return 42;
		},
		toString: function (){
			return 4;
		}
	}

	a + ""; // a -> call "ToPrimitive()" -> invoke valueOf() -> 42 + "" -> "42"

	String(a); // a -> call "toString()" -> 4 + "" -> "4"
	```

	`a || b` is **"roughly equivalent"** to `a ? a : b`;
	`a && b` is **"roughly equivalent"** to `a ? b : a`;

	in `a ? a : b` if `a` is a more complex expression (`function`). Then the `a` would possibly by evaluated twice. By contrast, for `a || b`, the `a` expression is evaluted only once. Same as `a && b`

7. ##### LossEquals vs. Strict Equals

	**`==` allows coercion in the quality comparison and `===` disallows coercion.**

	1. **Comparing: `string`s to `number`s**
		```js
		var a = 42;
		var b = "42";

		a === b;    // false
		a == b;     // true
		```
		ES5 spec, clauses 11.9.3.4-5:

		> 1. If Type(x) is Number and Type(y) is String, return the result of the comparison x == ToNumber(y).
		> 2. If Type(x) is String and Type(y) is Number, return the result of the comparison ToNumber(x) == y.

	2. **Comparing: anything to `boolean`**
		```js
		var a = "42";
		var b = true;

		a == b; // false
		```
		clauses 11.9.3.6-7:

		> If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.
		
		> If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).

		Try avoid ever using `==true` or `==false` (aka loose equality with `boolen`s) in the code.

	3. **Comparing: `null`s to `undefined`s**

		clauses 11.9.3.2-3:

		> If x is null and y is undefined, return true.

		> If x is undefined and y is null, return true.

		this means is that `null` and `undefined` can be treated as **indistinguishable** for comaprison purposes, if using `==`.

		```js
		var a = dosomething();

		if (a == null) {
			// ..
		}
		```

		the `a == null` check will pass only if `dosomething()` returns either `null` or `undefined`, and will fail with any other value, even other falsy values like `0`, `false` and `""`.	

	4. **Comparing: `object`s to non-`object`s**

		clauses 11.9.3.8-9:

		> If Type(x) is either String or Number and Type(y) is Object, return the result of the comparison x == ToPrimitive(y).
		
		> If Type(x) is Object and Type(y) is either String or Number, return the result of the comparison ToPrimitive(x) == y.

		There are some values where this is not the case, though, because of other overriding rules in the == algorithm. Consider:

		```js
		var a = null;
		var b = Object( a );    // same as `Object()`
		a == b;                 // false

		var c = undefined;
		var d = Object( c );    // same as `Object()`
		c == d;                 // false

		var e = NaN;
		var f = Object( e );    // same as `new Number( e )`
		e == f;                 // false
		```

		The `null` and `undefined` values cannot be boxed -- they have no object wrapper equivalent -- so `Object(null)` is just like `Object()` in that both just produce a normal object.

		`NaN` can be boxed to its `Number` object wrapper equivalent, but when `==` causes an unboxing, the `NaN == NaN` comparison fails because `NaN` is never equal to itself (see Chapter 2).

		[Comparison Table]( https://github.com/dorey/JavaScript-Equality-Table )

8. ##### Abstract Relational Comparison
	
	```js
	var a = { b: 42 };
	var b = { b: 43 };

	a < b;  // false
	a == b; // false
	a > b;  // false

	a <= b; // true
	a >= b; // true
	```

	the spec says for `a <= b`, it will actually evaluate `b < a` first, and then negate that result. Since `b < a` is also false, the result of `a <= b` is true.

	JS more accurately considers `<=` as "not greater than" (`!(a > b)`, which JS treats as `!(b < a)`). Moreover, `a >= b` is explained by first considering it as `b <= a`, and then applying the same reasoning.


