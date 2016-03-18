#1 Chapter 1: Types

[Book Source](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch1.md)

1. Using type to test a `null` value

	```javascript
	var a = null;

	(!a && typeof a === "object"); //true
	```

2. `function` is a "subtype" of object, it's referred to as a "callabe object" -- an object that has an internal [[call]] property that allows it to be invoked

	```javascript
	typeof function a(){} === "function"; //true

	function b(c, d){}
	b.length // 2, functions are objects, they can have properties
	```

3. safety guard for "undefined"("Undeclared")

	```javascript

	//type check
	if(typeof atob === "undefined") {
		atob = function() {}
	} 

	//object property check, will not have `ReferenceError` thrown
	if(!window.atob) {
		// ..
	}
	```