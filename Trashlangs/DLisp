# Dynamic { Lisp }
Here is the language grammar. All string literals are POSIX regular expressions with standard C escaping rules.
```
expression ::= ( "(" [ "function" | "macro" | "insert" | "defvar" | "scope" ] { expression } ")" ) | ( "'" "^[^\"',`\\(\\)\\s]+$" ) | ( "`" expression ) | ( "," expression ) | "^[^\"',`\\(\\)\\s]+$" | "[\-]?[0-9]" | "\"([^\"\\]|\\.)*\"" | "[\-]?[0-9]\.[0-9]"
```
Dynamic { Lisp } (for short DLisp) is a lisp-derived programming language for use with the DyOS operating system.

DLisp is dynamically scoped and uses F-Expressions.
## Special Operators
The special operators are `function`, `macro`, `insert`, `defvar` & `scope`, and the special syntaxes are `'`, `` ` ``, & `,`.
### Function
The `function` operator takes a list of arguments and a return value. It is self-evaluating. In initial-position of a list, the list evaluates such that each argument passed to the function is evaluated and the result is substituted into the symbol used as the function argument. Any function arguments left unbound are set to `nil`, and extra arguments are added to the `...` list. The `insert` function can be used to insert the contents of that list into code.
```
(let multiply (function (x y) (* x y ((insert ...)))))
(multiply 1) ;; Evaluates to (* 1 nil)
(multiply 1 2) ;; Evaluates to (* 1 2)
(multiply 1 2 3 4) ;; Evaluates to (* 1 2 (3 4))
```
Function parameters are *not* implicitly evaluated. To evaluate a function parameter, the built-in `(eval arg)` function is employed.
### Macro
Macro is identical to `function`, but implicitly calls `insert` on the return value.
```
(let defun ((macro (name args))
	`(let name (lambda (,(insert (eval args)))
		(progn ,(insert ...))))))
```
### Insert
Insert inserts the contents of a list into code dynamically. This can be used for dynamic code generation.
```
(insert `(let x 0)) ;; Evaluates to (let x 0)
```
### Define Variable
The define-variable operator stores the current value of a symbol and caches it to be restored at the end of the current `scope`. It returns the just-defined symbol. If the symbol being defined does not already exist, `defvar` creates it and the symbol exists until the current `scope` completes evaluation.
```
(defvar 'x)
(setq 'x 0)

;; x = 0

(scope (block
	(defvar 'x)
	(setq 'x 1)
	... ;; x = 1
	(0)))

;; x = 0
```
### Scope
The `scope` special operator defines a scope. When evaluated, it evaluates its inner expression and returns it.
```
(defvar 'zero)
(setq 'zero (scope 0))
```
It exists primarily to work with `defvar`. The whole program is implicitly wrapped in a `(scope (block ...))` expression when executed from a file, but this is not true in the REPL.
### Quote
Symbol literals evaluated to their pointed-to value by default, but the `'` operator is used to turn them into symbol *values*, which aren't deferenced until evaluated explicitly.
```
(let cell 'x)

(scope (block
	;; Defines a variable "x"
	(defvar cell)
	;; Overrides cell
	(defvar 'cell)))
```
### Quasiquote
The quasiquote operator does not evaluate its argument, but generates a list with its arguments. The `insert` function, when applied to a quasiquote expression, will insert the expression into code. This happens implicitly for the return value of a `macro`.
```
(let definition `(defvar 'x))
(insert definition) ;; defines x
```
### Comma
The comma operator will produce a compiler error if used outside a quasiquote expression. It evalutes its argument before returning the result, and is evaluated when used as an argument to a quasiquote expression.
```
(defmacro lambda (args)
	`(function (,(insert args)) (progn ,(insert ...))))
```
## Types & Built-In Functions
The datatypes supported by D-Lisp include integers, floating point values, strings, symbols, and singly linked lists.
### Integers
Integers are written to match the regular expression `[\-]?[0-9]+`. They can be manipulated with the binary functions `- / % >> << > < >= <= == !=`, the unary functions `- ~`, and the n-ary functions `+ * & | ^`. They implcitly convert to floats when used in a bulitin function that expects a float.

Note that `> < >= <= == !=` all return `t` or `nil`, not integers.
### Floating Point Values
Floats are written to match the regular expression `[\-]?[0-9+]\.[0-9]+`. They can be manipulated with the binary functions `- / % > < >= <= == !=`, the unary functions `- ~`, and the n-ary functions `+ *`.

Note that `> < >= <= == !=` all return `t` or `nil`, not integers.
### Strings
Strings are written to match `"([^"\\]|\\.)*"` and have the binary `at` function (taking and returning an integer--the first argument is a string),the unary `sizeof` function (returning an integer) and `==`/`!=` functions (returning `t` or `nil`).
### Symbols
Symbols point to dynamically-bound values. They are dereferenced (that is, converted to symbol literals) with the unary `*` operator. Symbols literals are written as identifiers, and evaluate to their pointed-to value.

Symbol literals can be used inside a `(setq symbol value)` expression, which is used for assignment.

Symbols can be compared with `==` and `!=` to see if they point to the same object, which returns `t` or `nil`.
### Lists/Trees
Lists are the fundamental data structure of the language, and you can get the current item with the unary `car` method or the next item with the unary `cdr` method. To create a list, use the binary `(cons first second)` (which works regardless of types).

Note that since the first item of a `cons` cell can itself be a `cons` cell, that means that you can also use this functionality to create a binary-tree data-structure. Trees can be conceptualized as dynamic lists of dynamic lists.

Cons cells can be compared with `==` or `!=` to see if they point to the same two objects, which returns `t` or `nil`.
### Type Errors & Booleans
Type mismatches for these builtin functions return `nil`, which is self-evaluating global constant--as is `t`. This has the nice behaviour that `==` between incompatible types returns `nil`.
## Standard Library
Module management procedures such as `import` and `export` are provided by the standard library, as are convenience macros like `progn`, `lambda`, `defun`, `defmacro`, and `let`.

The contents of the standard library are outside the scope of this document.