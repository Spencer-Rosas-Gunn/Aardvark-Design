# Tree
Tree is a programming language built around the concept of trees.
### Tree Notation
This is the syntax that will be used to document the state of the program. It's also the syntax that we reccomend debuggers and IDEs use to represent data about the tree.

Each tree is written `(op x y z)` where $x$, $y$, and $z$ are the tree's children. A `_` is placed before the item pointed to by the head.
### Movement
Use `..`, `.`, `->`, & `<-` to move to the previous layer, move to the current node's first child, move to the current node's next sibling, and move to the current node's previous sibling. When the node moved to doesn't exist, one will be created as an empty tree with the `nil` operation.

Use the `=>` and `<=` operators to `->` or `<-` an amount of times equal to that stored in the register. Use `:>` and `<:` to `->` or `<-` an amount of times equal to that stored at the value pointed to by the head.
#### Example
Let's run this program on an exemplary tree.
```
. -> . <- . .. .. -> .. ..
```
The tree at each stage is as follows.
```
START

_(nil
	(nil 1 2 3)
	(+ 4 5
		(* 6 7 8)))

.

(nil
	_(nil 1 2 3)
	(+ 4 5
		(* 6 7 8)))

->

(nil
	(nil 1 2 3)
	_(+ 4 5
		(* 6 7 8)))

.

(nil
	(nil 1 2 3)
	(+ _4 5
		(* 6 7 8)))

<-

(nil
	(nil 1 2 3)
	(+
		_(nil)
		4
		5
		(* 6 7 8)))

.

(nil
	(nil 1 2 3)
	(+
		(nil
			_(nil))
		4
		5
		(* 6 7 8)))

..

(nil
	(nil 1 2 3)
	(+
		_(nil
			(nil))
		4
		5
		(* 6 7 8)))

..

(nil
	(nil 1 2 3)
	_(+
		(nil
			(nil))
		4
		5
		(* 6 7 8)))

->

(nil
	(nil 1 2 3)
	(+
		(nil
			(nil))
		4
		5
		(* 6 7 8))
	_(nil))

..

_(nil
	(nil 1 2 3)
	(+
		(nil
			(nil))
		4
		5
		(* 6 7 8))
	(nil))

..

(nil
	_(nil
	(nil 1 2 3)
	(+
		(nil
			(nil))
		4
		5
		(* 6 7 8))
	(nil)))
```
### Operations
There are two types of operations; variadic and n-ary operations. Variadic operations work on arbitrarily many operands, whereas n-ary operations work on a specified number of operands.

When an operation is seen in source code, it is placed as the head operation of the tree pointed to by the head. If a leaf is pointed to, it will be replaced with an empty tree with the operation. When `,` is seen in source code, it evaluates the current tree.

The unary operations are `!` (logical not), `~` (bitwise not), `-` (negate), `sqrt` (square root), ``. The binary operations are `-` (minus), `/` (divided by), `%` (modulo), `[]` (index tree/string), and `++` (concatenate tree/string). The ternary operation is `if`.

All of these operations, except `if`, evaluate all arguments. `if` evaluates the first argument, and then conditionally evaluates the second or third argument; the second if the first evaluates to a non-`nil` value, and the third if it evaluates to `nil`.

The n-ary operations are `+` (add), `*` (multiply), & `...` (evaluate all children).
#### Example
Let's evaluate the following program.
```
. -> - -> % .. if
```
On an exemplary tree.
```
START

_(nil
	t
	(nil 2)
	(nil 15 3))

.

(nil
	_t
	(nil 2)
	(nil 15 3))

->

(nil
	t
	_(nil 2)
	(nil 15 3))
	
-

(nil
	t
	_(- 2)
	(nil 15 3))

->

(nil
	t
	(- 2)
	_(nil 15 3))

%

(nil
	t
	(- 2)
	_(% 15 3))

..

_(nil
	t
	(- 2)
	(% 15 3))

if

_(if
	t
	(- 2)
	(% 15 3))

,

-2
```
### Macros
Macros are a series of instructions that can be called into. To define a macro, use `fn name () { ... }`. Call it with `name`. If a macro is defined inside another macro, it's only available inside that macro. Use `ret` to end macro execution. It ends implicitly at `}`.
##### Example
Let's run this program.
```
fn dnr () {
	.
	->
	ret
}

dnr ..
```
On an exemplary tree. Here, we'll add the current state of the program at each step.
```
START	dnr ..

_(nil 1 2)

dnr		. -> ret

_(nil 1 2)

	.	-> ret

	(nil _1 2)

	->	ret

	(nil 1 _2)

	ret	..

	(nil 1 _2)

..

_(nil 1 2)
```
Once you `ret` outside of a macro, the program stops.
### Values
The values are integers (e.g. `12` or `6`), floating point values (e.g. `52.12` or `3.14`), or strings (e.g. `"heya"` or `"bya"`). `nil` is a null-operation, which does nothing when evaluated. `t` is a value used as a meaningless non-nil value.
### Allocation/Deallocation
Use `new` in code to add a new child node as a next sibling of the current node, and use `delete` to destroy the current node and move to the left.
### Store/Load
Use `load` to read the value pointed to, and `store` to assign that into another node.
### Language Extensions
Use `@` to begin a language extension clause, followed by a string for what language gets the extension, and a `{ ... }` block containing the language extension's content.
### Quotes
Use `'` to quote a macro name. This will assign the macro name to the pointed to cell as a leaf. When evaluated, this executes the macro.
### Modules
Use `import "path"` to include another source file by pasting its source into the current one.
### Comments
Use `#` to declare a line as a comment and have it ignored by the interpeter.
# KeyDB
KeyDB is a language extension for debugging, with a name parodying GDB.
### Breakpoints
Use this to make a breakpoint.
```
@ "keydb break" {
	"Breakpoint name!"
}
```
You don't have to specify a string, it's optional.
### Errors
Use this to make the program crash.
```
@ "keydb throw" {
	"Printed error!"
}
```
Again, the string is optional.
### Usage
Use `i` to step through the program, `j` to call the current macro, and `k` to enter view mode and view the current state of the program. In view mode, use `k` to exit, `i` to expand macro by one layer, and `j`/`l` to move left/right through the macros to examine their innards. Use spacebar while a macro is selected to expand it, but not other macro.

The tree will be shown at all times, though it will be partially obscured by view mode. The output console will be at the bottom. Even when not in view mode, the current, last, and next instruction will be shown at the top.
# FileIO
Use this command to get a file handle from the string in the register and place it into the FileIO register.
```
@ "fileio get_stream" {}
```
Use this command to read the file in the register and assign it to the head's pointed to location.
```
@ "fileio read" {}
```
Use this command to write to the filestream with whatever value is currently pointed to.
```
@ "fileio write" {}
```
### Hello World
Here's a hello world with FileIO.
```
"/dev/stdout"

@ "fileio get_stream" {}

"Hello World!"

@ "fileio write" {}
```
# Proof of Turing Completeness
Let's write a tip VM using relative addressing. See https://esolangs.org/wiki/Tip#Specification for a proof of tip's turing completeness.
```
# a * b
def mul {
	# Compute (a * b)
	->
	new
	<-
	load
	->
	->
	.
	store
	..
	<-
	load
	->
	.
	store
	..
	*
	,
	load
	delete
	<-
	<-
}

# goto *(a * b)
def tip {
	mul
	=>
}
```
Now just take a memory tape and run `tip` repeatedly to interpret the program.
# Examples
### Multiplication
This is a macro to multiply the current and next values into the register. This is a copy of part of the tip VM from above.
```
def mul {
	# Compute (a * b)
	->
	new
	<-
	load
	->
	->
	.
	store
	..
	<-
	load
	->
	.
	store
	..
	*
	,
	load
	delete
	<-
	<-
}
```