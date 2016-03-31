#2 Chapter 5: Grammar

[Book Source](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20%26%20grammar/ch5.md)

#### Statements & Expressions

`Statements` are sentences
`expressions` are phrases
`operators` are conjunctions/punctuation

#### Expression Side Effects

```js
var a = 42;
++a++;  //ReferenceError. operators 
```

The evalutation is: a++ --> ++42;
Since `++` can't have a side effect directly on a value like `42`.

#### Contextual Rules

When the `{...}` pair is a value that's getting assigned to some variable or operator, it becomes an `object` literal.

```js
// object 

var a = {
	foo: bar()
}


// regular code block, like the block in `if (..) {}`

{
	foo: bar()
}


// JSON format cannot stand alone in the js grammar

{"a": "this is a json data"}; // result an error in console ==> SyntaxError


// JSON-P makes JSON intovalid JS grammar;

foo({"a": "this is a json data"}); // object {"a": "..."} being passed to foo() function
```

**GOTCHA** 

```js
[] + {}; // "[object Object]"

{} + []; // 0
```

**First one** `[] + {}`:

1. `{}` appears in the `+` operator's expression, interpreted as an actual value (an empty `object`). 
2. `[]`  coerced to `""` and `{}` coerced to a `string` `"[object Object]`
3. result is `"[object Object]"`;

**Second one** `{} + []`:

1. `{}` interpreted as a standalone `{}` empty block, which does nothing
2. expression becomes `+ []`
3. which is an _explicitly coerces_ the `[]` to a `number`
4. result is `0`

#### operators precedence

`,` has the lowest precedence.

```js
var a = 42, b;
b = ( a++, a );

a; // 43
b; // 43

// if remove "()"
b = a++, a;

a; // 43;
b; // 42;
```










