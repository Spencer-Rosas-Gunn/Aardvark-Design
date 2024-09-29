# D-Lisp II
D-Lisp II is a dynamically scoped f-expression based lisp based on a syntactic variation over JMC lisp with f-expressions added to improve language extensibility.
# Special Operators
By convention, special operators have names that begin with `_`. They should never be used in code directly, but should be abstracted over with macros.

The language provides a set of three special operators (which are like keywords in other languages), those being `_defvar _scope _function`. These special operators are self-evaluating symbol objects that have special behaviour when evaluated in initial position of a list. Special operators are global constants, and cannot be dynamically bound or reassigned.

It also provides five special symbols, those being `` ( ' ` , ) ``.
### Defvar/Scope/End
In ultralisp, there is no `let` construct. Rather, variables are dynamically bound. To begin a new dynamic binding in the current scope and optionally initialize it, use `_defvar`. To declare a new scope, use `_scope`. There is always a global scope you can `_defvar` into.

At the end of a scope, all variables that were created with `_defvar` will be destroyed if they had no previous value, or returned to the value they were before the dynamic binding occurred.
```
(_defvar 'x 0)

(_defvar 'one (_scope
	(_defvar 'x 1)
	;; Here, x = 1
	x))

;; Here, x = 0
(_defvar 'zero x)
```
### Function
The `_function` operator takes a list of symbols and provides a return-expression. In an expression of the form `(fobj x y z)` where `fobj` was created with an expression of the form `(_function (???) ???)`, each symbol in the linked list will be dynamically bound within the function, which forms its own scope, to the value in the same position in the arguments list.

Function arguments are *not* evaluated implicitly. You must explicitly evaluate them within the function.

Parameters not given a value will be `nil`. Extra arguments are added to the `...` list.

When the function finishes execution, the return value will be placed into code.
```
(_defvar 'square (_function ('x)
	`,(* (eval x) (eval x))))

(square 2) ;; 4
```
### Quote
The quote special form takes a symbol literal and turns it into a symbol value, instead of evaluating the pointed-to value (the default behaviour for symbol literals).
```
(_defvar 'x 'y)

(_scope
	;; Binds "y" to zero
	(defvar x 0)
	;; Binds "x" to zero
	(defvar 'x 0))
```
### Quasiquote/Comma
The quasiquote special form takes an expression and refrains from evaluating it, instead storing it as a code object which is allowed to store special operators. Within a quasiquote, the `,` special operator can be used to evaluate a sub-expression without evaluating the rest of the expression.

The built-in `insert` function can be used to insert an expression into code dynamically. It is implicitly called for the return value of functions.
```
(_defvar 'defzero
	;; Evaluates to `((defvar 'zero 0))
	`((_defvar 'zero ,(- 2 2))))

;; Binds "zero" to "0"
(insert defzero)
```