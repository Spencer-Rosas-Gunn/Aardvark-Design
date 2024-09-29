# Mulisp
This is the specification for Mulisp.
## Syntactic Forms
The following is the grammar of Mulisp.
```
expression ::= ["," | "`"] ( ( "(" (expression | special_operator) { expression } ")" ) | literal | (["'" [","]] symbol) | "...")
```
More tersely,
```
e::=[","|"`"](("("(e|s){e}")")|l|(["'"[","]]y)|"...")
```
## Special Operators & Syntax
The special operators are `(deflex Symbol_t)`, `(defdyn Symbol_t)`, `(function List_t ANY)`, `(defconst Symbol_t ANY)` & `(lexscope ...)`. Additionally, the special syntactic elements `` ' ` , ... `` are provided. The list argument to `function` must be a list only of `Symbol_t`.
### Define Lexical Symbol
The `deflex` operator creates a symbol bound to the current lexical scope. The symbol and its value are defined inside the `lexscope` in which the `deflex` was used. The symbol is not exposed to called procedures.

When evaluated, it returns the defined symbol.
### Define Dynamic Symbol
The `defdyn` operator creates a symbol bound to the current dynamic scope. The symbol and its value are defined until the `lexscope` in which the `defdyn` was used is entirely evaluated. The symbol is, therefore, exposed to called procedures.

When evaluated, it returns the defined symbol.
### Function
The `function` operator defines a mapping from the argument symbols to a return-value. Expressions of the form `((function ARGDEFS RET) ARGS)` evaluate to `RET[ARGDEFS/ARGS]`, with term-by-term substitution. If any defined arguments aren't passed, they will be `nil`. Extra arguments are added to the `...` list, which can be referenced by the `RET` expression.
### Define Constant
The `defconst` operator defines an immutable global symbol. It cannot be used inside `lexscope`, and it returns the symbol it defined.
### Lexical Scope
The `lexscope` operator defines a lexical scope. When evaluated, it evaluates each of its terms in sequence, modifying the lexical scope as required, and returning the result of the final expression it contains.

When it's done evaluating, the lexical and dynamic scopes are returned to the state they were in before `lexscope` began evaluation.
### Quote
The quote operator `'` is used before either a `,` expression that evaluates to a symbol or a symbol literal. Normally, when either of those things are evaluated the symbol will be resolved; that is, the value of the symbol in the current lexical or dynamic scope will be looked up and returned. However, the `` ' `` operator prevents this resolution, instead evaluating to the symbol object itself.
### Quasiquote
The quasiquote operator `` ` `` is used to prevent evaluation of its argument. It returns its argument as code.
### Comma (Evaluate)
The comma operator `,` is used before a list to evaluate it inside a quasi-quoted expression.
### Ellipsis (Variadic Arguments)
The elipsis `...` is used to access excess arguments passed to a `function`. Excess arguments are placed, in order, as a list, into `...`. When `...` is evaluated, all arguments are placed directly into code, not as values in a list.
## Data Types
All types implicitly implement `==`, `!=`, and `:=` methods.
### Scalar Types
The scalar types include integers, floating point values, complex numbers, and imaginary arguments. These are `Integer_t`, `Number_t`, `Complex_t`, & `Imaginary_t`. They are written `100`, `10.0` or `1.0e1`, `10+0i` (no spaces), & `100i`.

The operations supported between scalar types are `+ - * / % :~ :& :| :^ << >> || ~=` which are add, subtract, multiply, divide, modulo, bitwise not, bitwise and, bitwise or, bitwise xor, bitshift left, bitshift right, absolute value (becomes `Number_t` for all listed types besides `Integer_t`, which becomes itself), and roughly equals. The operators fold the list of arguments, so `(1 - 2 3)` is equivalent to `(1 - (2 - 3))`. These operations are all methods when binary, but the unary operations are functions not method.

Bitwise not will return the not of its argument when given one, and will otherwise apply element-wise to the list passed to it (so `(:~ 1 2 3)` is equivalent to `((:~ 1) (:~ 2) (:~ 3))`).

Unary `-` negates the argument.

Between integers and numbers, the operators `> < >= <=` are also provided.
### Symbols
Symbols are of type `Symbol_t`, and have the method `*`, which is used to dereference them. When applied to one argument, it returns the value of the symbol. Otherwise, it applies element-wise to its arguments. The symbol `t` is a symbol such that `(t = 't)` (that is, `(defconst 't 't)`).
### Text Types
The text types are `String_t` and `Character_t`, written `"..."` and `"..."c` respectively. 

Strings have the function `(at String_t Integer_t)` which does a bounds-checked index into a string, `(insert String_t Integer_t Character_t)` which inserts the character at the index, `(insert String_t Integer_t String_t)` which inserts the string at the index, `(erase String_t Integer_t)` which erases the character at the index, `(erase String_t Integer_t Integer_t)` which erases the second integer amount of characters starting at the first integer, `(++ String_t String_t)` which concatenates two strings, `(++ String_t Character_t)` which adds a character to a string, `(replace String_t Integer_t String_t)` which replaces the characters starting at the index with the second string, `(find_first String_t String_t)` which returns the index where the second is first found in the first string, `(find String_t String_t)` which returns a list of indicies where the second string appears in the first, `(starts_with String_t String_t)` which checks if the former string begins with the latter, `(ends_with String_t String_t)` which checks if the former string ends with the latter, `(contains String_t String_t)` which checks if one string contains another, `(substr String_t Integer_t Integer_t)` which slices the string from first index with the second integer amount of characters, and `(eval String_t)` which parses a string as an expression and evaluates it.
### Sequence Structures
The data-structures include `List_t` and `Code_t` (which is a `List_t` made from quasiquote instructions that inserts itself into code when returned from a function). Lists are written with `(expr expr expr)`.

They have the `(next List_t)` function to get the next item, and the `(++ List_t List_t)` concatenation operator. They also have the `(at List_t Integer_t)` index operator.
### Functions
Functions are of type `Function_t` and are made with the `function` special operator.
### Modules
Modules are of type `Module_t`. They are obtained with `(import Symbol_t)` (which is compile-time computed) and indexed with `(:: Module_t Symbol_t)`. They are implicitly generated from the global declarations of a file.

The function `(use Module_t)` brings a module into the global namespace.

The standard library is included with `(use (import 'std))` and provides a set of convenience macros like `let` and `defvar` to ease programming.
### Booleans
Booleans are of type `Bool_t` and are created with the `=` and `!=` operators. Moreover, they can be manipulated with `and`, `or`, `xor`, & `not` functions (the `not` function is unary, and applies element-wise when given multiple arguments). The binary operators between them are methods (e.g. `(not (true and false) or (true xor true))`).

The boolean values are `nil` and `t`.
## Objects & Classes
The mOS (meta-object system) is the simple object system used by the language. Methods are called with `(object Symbol_t ...)` syntax without quoting the symbol, and members are accessed with `(object Symbol_t)` syntax, without quoting the symbol.

Make an object with `(object (Symbol_t ANY) (Symbol_t ANY) (Symbol_t ANY) ...)`. The `meta` field is used to refer to a metaobject, which stores the methods used by the object as functions whose first argument is the object itself.

The value `nil` is created with `(defconst 'nil (object))`.
## Exceptions
When a type error is created with a builtin function, or a `(throw expr)` expression is evaluated, an exception is returned up the function stack until a `(try expr)` block is reached. When it is, the error value is handed over to the next `(catch (err) ...)` block (where `...` is a `lexscope` returned if the expression fails). Once that completes, control flow returns to the `try` expression and returns the caught value.
## Conditions
The builtin function `(cond Bool_t first second)` is used for booleans. NOTE: `if` is usually a special operator, but `if` can be made as a macro with ``(defconst 'if (lambda (bool first second) (cond bool `first `second)))``; therefore, it is not needed as a special operator.
#  Example
Here is basic program to compute the factorial of a number.
```
(defconst 'defun (function (name args)
	`(defconst ',name ,args (lexscope ,...))))
(defun if (bool first second)
	(cond bool `first `second)))

(defun fact (x)					;; fact(x) := x == 0 ? 1 : x * fact(x - 1)
	(if (x == 0)
		1						;; 0! = 1
		(x * (fact (x - 1)))))	;; x! = x * (x - 1)!
```