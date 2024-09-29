# Altran
Here is a "Hello World!" program.
```
let std = import("std")
world->std.print("Hello World!")
```
### File
```
program ::= ["package" IDENTIFIER] expression
```
Each program implicitly begins with `let _start(world :: World_t) :: World_t = ...`, unless it starts with a `package` declaration which declares the current file as a module.
### Type & World
```
expression |= "Type"
expression |= "World"
```
The `World` type is a unique type, so only one value can exist at a time. When a `World` parameter exists to a lambda, the body of that lambda cannot reference an external `World` variable. The `Type` type is the type of types themselves; it can be treated like all other types, but a value of type `Type` can never be defined in terms of a value of type `World`.
### Let Expressions
```
expression |= "let" IDENTIFIER [ "::" expression ] "=" expression expression
```
Altran is an expression-oriented language. The most important type of expression is the `let` expression. Those are expressions of the form $\textrm{let}\space \upsilon = \alpha\space \beta$, and evaluate to $\beta[\upsilon/\al>pha]$--a rule called reverse beta-reduction.
### Lambda Expressions
```
expression |= "λ" IDENTIFIER [expression] [ "::" expression ] "." expression
expression |= expression expression
```
Lambdas are expressions of the form $\lambda \upsilon.\alpha$ (with $\lambda$ being able to be written as `\` for ASCII-compatibility) and are used in application expressions, which are expressions of the form $\alpha\space\beta$. Lambdas are evaluated according to the rule that $(\lambda \upsilon.\alpha)\beta\rightarrow\alpha[\upsilon/\beta]$.

To require a datatype of the argument, the type is placed after the argument (e.g. $\lambda \upsilon\space\Upsilon.\alpha$). The `::` syntax is used to specify the return type (e.g. $\lambda\upsilon\space\Upsilon::\Upsilon.\alpha$).
### Arithmetic Expressions
```
expression |= unary expression
expression |= expression binary expression
expression |= "(" ( unary | binary ) ")"

binary ::= "+" | "-" | "*" | "/" | "%" | "**" | "and" | "or" | "xor" | "=" | "!=" | "~=" | ">" | "<" | ">=" | "<="
unary ::= "not" | "-"
```
Between two values of the same type and of a type equal either to `Nat`, `Int`, or `Real` the binary `+ - * / % **` (add subtract multiply divide modulo power) operators are allowed, as well as the unary `-` (negate) operator (which is not allowed for `Nat`). These return the same type as their arguments Between values of type `Bool` the operators `and or xor not` (and or exclusive-or not) are provided, which also return `Bool`. Between values of types `Nat`, `Int`, & `Real` (including mixing-and-matching the types), the `~= > < >= <=` (roughly-equal greater less greater/equal less/equal) operators are allowed and return booleans. The `=` and `!=` operators are defined for all types and return booleans.

To convert an operator to a function, parenthesize it (e.g. `(=)((+)(2, 2), 4)`).
### Pipe Expressions
```
expression |= expression "→" expression
```
Pipe expressions are of the form $\alpha\rightarrow\beta$ and evaluate to $(\beta\space\alpha)$. The `->` sequence is allowed to be used instead for ASCII-compatibility.
### Structure Expressions
```
expression |= "struct" { IDENTIFIER "::" expression "," } IDENTIFIER "::" expression
expression |= type { IDENTIFIER ":" expression } "," IDENTIFIER ":" expression
```
Structure expressions are used to make product types. For example, a 2D-point could be modeled as a `let Point = struct x :: Real, y :: Real`. We could then make the origin with the syntax `let origin = Point x: 0, y: 0`.

Structure expressions return a value of type `type`.
### Union Expressions
```
expression |= "union" { IDENTIFIER "::" expression "," } IDENTIFIER "::" expression
expression |= expression "::" IDENTIFIER "(" expression ")"
```
Union expressions are used to make sum types. For example, a pet could be modeled as `let Pet = union dog :: Dog, cat :: Cat`. You can then make an object of this type with the syntax `let tesla = Pet::dog(Dog name: "Tesla")`.

Union expressions return a value of type `type`.
### Enum Expressions
```
expression |= "enum" { IDENTIFIER "," } IDENTIFIER
expression |= expression "::" expression
```
Enumeration expressions are used to make enumeration types. For example, `Bool` is defined as `let Bool = enum then, false`. Then, values of these types can be obtained with `Bool::then` and `Bool::false` (though the global constants `true` and `false` make this unneccessary).

Enum expressions return a value of type `type`.
### Function Expressions
```
expression |= "let" IDENTIFIER "(" [ { IDENTIFIER [ "::" expression ] "," } IDENTIFIER [ "::" expression ] ] ")" [ "::" expression ] "=" expression
```
Function expressions are a special type of let expression. Expressions of the form $\textrm{let}\space\upsilon(\alpha)=\beta$ evaluate to $\textrm{let}\space\upsilon=\lambda\alpha.\beta$. The standard `::` syntax is used to specify types.
### Tuples
```
expression |= "(" { expression } ")"
```
Tuples types are created with `(Type, Type)`, indexed with `.` syntax (e.g. `t.0`) with integer literals, and instantiated with `(value, value)`. A tuple containing exactly one type is that type (so `(Int) == Int`), and the empty tuple is unit (`()`).
### Arrays
Arrays are written `"string"` (which is of type `Char[]`) or `[item, item, item]`. Empty arrays aren't allowed without the explicit `Maybe(T[])` type. They are indexed with `.index` and are indexed circularly, meaning that `arr.(index % arr.len) == arr.index`. The length is accessed with `.len`.
### Do-Blocks
```
expression |= "do" "(" expression ")" { expression } "end"
```
Do blocks are used for imperative programming. Basically, blocks of the following form
```
let world2 = do
		std::print("What's your name? ")
		let input = std::input()
		std::print("Hello, $(input)!")
	end
```
Become code of the following form
```
let _1 = std.print(world, "What's your name? ")
let _2 = std.input(_1)
let _3 = std.print(_2.1)
let world2 = _3
```
Basically, they capture the current state of the world and manage its updates imperatively.
### Dot Expressions
```
expression |= expression "." expression
expression |= "import" "(" STRING ")"
```
The dot expression can be used for product-type indexing or for namespace indexing. They can be used on import statements to include libraries, including `std`--the standard library (which is outside the scope of this document).
### If Expressions
```
expression |= "if" expression { ( IDENTIFIER | "false" ) expression } [ "else" expression ]
```
To match over enumerations or unions, the `if` keyword is used. Use `if state result state result else result`. The `then` state represents the `true` state for booleans.
```
let fact(x) = if x = 0
	then 1
	else x * fact(x - 1)
```
For unions, the state name is used and captured as a value.
```
let name(pet :: Pet) = if pet
	dog dog.name
	cat cat.name
```
### Use Expressions
```
expression |= expression expression "use" IDENTIFIER "<-" expression
```
Use the $\textrm{use}\space\upsilon\leftarrow...$ (with $\leftarrow$ optionally approximated as `<-`) syntax after a function call to add a lambda as the first argument. `func() use x <- y` evaluates to `func(\x.y)`.