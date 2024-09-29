# OX-EML
Objective-[eXtension] to Expression-Markdown Language.
## Special Operators (Keywords)
### Var
Use `var` to make a dynamically scoped variable and, optionally, initialize it.
```js
(@var x 0)
```
### Function
Use `function` to declare a function, anonymous or otherwise. Functions can have any number of statements, and form a dynamic scope.
```js
(@var square (@function (x) (x * x)))
(@function square(x) (x * x))
```
### If
Use `if` to perform ternary conditional branching.
```js
(@function fact(x)
	(@if (x == 0)
		(1)
		(x * (fact (x - 1)))))
```
### While
Use `while` for a while loop.
```js
(@function fact(x)
	(@var count 1)
	(@var out 1)
	(@while (count < x)
		(count := (count + 1))
		(out := (out * count))
	out))
```
### Set & Class
Use `set` to set a class, and `class` to make a class--anonymous or otherwise.
```js
(@class Point
	(@function len() (sqrt ((this.x * this.x) + (this.y * this.y)))))

(@var Point2 (@class
	(@function len() (sqrt ((this.x * this.x) + (this.y * this.y)))))

(@function mknewpt(x)
	(@set x Point))
```
### Block
Use `block` to make a subscope.
```js
(@function @square_if_not_two(x)
	(@if (x == 2) (block
		(@var out 0)
		out)
	(block
		(@var out (x * x))
		out)))
```
### .
Use `this.field` to access a field of the current object. Each object is a *record* with various values, save for `Integer`, `String`, `Class`, & `Function` which are opaque classes. Note that values aside from `this` cannot be so indexed, since their fields are *private*.
## Evaluation Rules
Special operators are annotated with `@`. Numeric sequences are interpreted as `Number` objects. String literals are interpreted as `String` objects.

For an expression, all arguments are first evaluated. Then, if the expression is a special operator, that operator does whatever its function is.

If it's a function, the function is called on the arguments. Extra arguments are ignored and too few arguments substitute the remaining arguments with `nil`.

If it's an object, the second value must be a symbol and will be used to index the class of the object for a method to dispatch, where `this` will be the current object.

Otherwise, the expression throws a type error.
## Symbols
Symbols are written as identifiers or with `#"..."` (like in Smalltalk). They are used as reference-types, and to prevent their implicit dereference the `'` notation can be used (e.g. `'x`), just like in lisp-derived languages. To dereference after that, the `,` notation can be used (e.g. `,ptr`).

Symbols can be reassigned with `:=` & compared with `==`. Special operators and opaque classes are escaped with `@` and cannot be reassigned. The class `Nil` is an opque class written with `nil`, and is used as a sentinel value for type errors.
## Opaque Classes
### Number
Numbers have the methods `+ - * / >> << & | ^ and or xor < > <= >=` with one another. These are all binary, but `-` can be unary. Additionally, the unary `~ not` are provided. Numbers are used as booleans as well, with nonzero numbers being truthy. Other types are falsey, as is `0`.
### String
Strings have the binary `++` concatenation method, the `to_int` method to attempt conversion to an integer (returning `""` on failure), the `at` method that takes an integer and returns an integer with UTF-8 character encoding. 
### Class
Classes have no methods.
### Function
Functions have no methods.
### Nil
Nil has no methods.