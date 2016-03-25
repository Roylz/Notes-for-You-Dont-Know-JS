#3 Chapter 3: Natives

[Book Source](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch3.md)

#### Internal `[[Class]]`

`[[Class]]` propter cannot be accessed directly, but can generally by revealed indirectly by borrowing the default `Object.prototype.toString()` method called against the value. Examples:

```javascript
Object.prototype.toString.call([1,2,3]); // "[object Array]"

Object.prototype.toString.call(/regex-literal/i); // "[object RegExp]"

// primitives
Object.prototype.toString.call( null ); // "[object Null]"
Object.prototype.toString.call( undefined ); // "[object Undefined]"

Object.prototype.toString.call( "abc" ); // "[object String]"
Object.prototype.toString.call( 42 ); // "[object Number]"
Object.prototype.toString.call( true ); // "[object Boolean]"
```
#### Boxing

```javascript
var a = "abc";
var b = new String( a );
var c = Object( a );

typeof a; // "string"
typeof b; // "object"
typeof c; // "object"

b instanceof String; // true
c instanceof String; // true

Object.prototype.toString.call( b ); // "[object String]"
Object.prototype.toString.call( c ); // "[object String]"
```

#### Unboxing

```javascript
//Use `valueOf()` to Unbox
var a = new String( "abc" );
var b = new Number( 42 );
var c = new Boolean( true );

a.valueOf(); // "abc"
b.valueOf(); // 42
c.valueOf(); // true


//implicitly unbox (coercion)
var a = new String( "abc" );
var b = a + ""; // `b` has the unboxed primitive value "abc"

typeof a; // "object"
typeof b; // "string"
```

#### Array(...)

```javascript
var a new Array( 3 );
a; // [, , ,] with empty slots. (sparse array);
```

Create an array of actual `undefined` values (not just "empty slots"):

```javascript
var a = Array.apply(null, {length: 3});
a; // [undefined, undefined, undefined]
```

Pass a array-like `{length: 3}` parameter to `apply()` function will end to initial the array like this: `Array(undefined, undefined, undefined)`








