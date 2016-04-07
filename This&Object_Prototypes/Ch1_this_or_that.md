# Chapter 1: `this` Or That?

[Book Source](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch1.md)

`this` is not an author-time binding but a runtime binding.

```js
function foo() {
	var a = 2;
	this.bar();
}	

function bar() {
	console.log(this.a);
}

foo();  // undefined
``` 
**Explaination:**
	
* this is attempting to use `this` to create a bridge between the lexical scopes of `foo()` and `bar()`.

* so that `bar()` has access to the variable `a` in the inner scope of `foo()`

* **No bridge is possible**

* You cannot use a `this` reference to look something up in a lexical scope.
