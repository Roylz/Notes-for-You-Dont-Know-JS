# Chapter 2: `this` All Makes Sense Now!

[Chaprter One Source](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20%26%20object%20prototypes/ch2.md)

#### Call-site (how the function is called)

**call-site**: where a function is called from

**call-stack**: the stack of functions that have been called to get us to the current momement in execution

```js
//demonstrate call-stack and call-site

function baz() {
    // call-stack is: `baz`
    // so, our call-site is in the global scope

    console.log( "baz" );
    bar(); // <-- call-site for `bar`
}

function bar() {
    // call-stack is: `baz` -> `bar`
    // so, our call-site is in `baz`

    console.log( "bar" );
    foo(); // <-- call-site for `foo`
}

function foo() {
    // call-stack is: `baz` -> `bar` -> `foo`
    // so, our call-site is in `bar`

    console.log( "foo" );
}

baz(); // <-- call-site for `baz`
```
#### `new` Binding

When a function is invoked with `new` in front of it, otherwise known as a constructor call, the following things are done automatically:

1. a brand new object is created (aka, constructed) out of thin air
2. _the newly constructed object_ is `[[prototype]]`-linked
3. the newly constructed object is set as the `this` binding for that function call
4. unless the function returns its own alternate **object**, the `new`-invoked function call will automatically return the newly constructed object.

#### Determining `this`

Now, we can summarize the rules for determining `this` from a function call's call-site, in their order of precedence. _Ask these questions in this order_, and stop when the first rule applies.

Is the function called with `new` (**new binding**)? If so, `this` is the newly constructed object.

`var bar = new foo()`

Is the function called with `call` or `apply` (**explicit binding**), even hidden inside a `bind` hard binding? If so, `this` is the explicitly specified object.

`var bar = foo.call( obj2 )`

Is the function called with a context (**implicit binding**), otherwise known as an owning or containing object? If so, `this` is that context object.

`var bar = obj1.foo()`

Otherwise, default the `this` (**default binding**). If in `strict mode`, pick `undefined`, otherwise pick the `global` object.

`var bar = foo()`


**safer `this`**:

Be carefull of accidental/unintentional invoking of the _default binding_ rule. In cases where you want to "safely" ignore a `this` binding, a "DMZ"(de-militatized zone) object like `ø = Object.create(null)` is a good placeholder value that protects the `global` object from unintended side-effects.

**NOTE**:

```js
var a = Object.create(null); // you do not inherit anything. this is completely blank object.

var b = {}  // is same with following initialize

var b = Object.create(Object.prototype); // you inherit all object properties
```





















