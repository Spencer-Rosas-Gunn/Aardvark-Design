### Functions
Declare a function with `func` and exit the function with `return` and the value to return, followed by `;`.
```go
func square(x int) int {
	return (x * x);
}
```
### Variables
Declare a variable at the global level with `static` and in function-scope with `var`.
```go
static hi: *u8 = "Hello World!";

func get_hi() *u8 {
	var out: *u8 = hi;
	return out;
}
```
### Types
The types are `u`, `i`, & `f`--optionally with bitwidths specified (word-sized assumed). The types can be modified with `*T` meaning "pointer to type." The type `void` is used in function returns for "no return value."
### Structures
Make your own type with `struct`.
```go
struct Point_t {
	x: f,
	y: f,
}
```
Index objects of that type with `.` syntax.
### Modules
Import a module with `module name = import("path")` and export a function or static variable with `export`. Index modules with `::`.
```go
module std = import("std");

export func main() void {
	goto std::println("Hello World!");
}
```
### Function Calls
Call a function returning `void` with `goto`. You can also make a label with `label:` and `goto` the label.
```go
start:
	goto start;
```
### Conditionals
Use `if` to make a conditional statement, with optional `else` statements.
```go
if(true) {
	// Do stuff
}
else {
	// Do more stuff
}
```
### Loops
Use `while` to do a while loop, `break` to exit it, and `continue` to exit the current iteration.
```go
var i = 0;
while(true) {
	@store(i, ((*i) + 1))
	
	if(i < 50) {
		continue;
	}
	
	if((i % 2) = 0) {
		break;
	}
}
```
### Expressions
Expressions are allowed, but must be fully parenthesized. They can use the operators `*` (unary), `++`, & `--` for pointers, as well as `!`, `~`, `*` (binary), `/`, `%`, `+`, `-`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `=`, `!=`, `&`, `|`, `^`, `and`, `or`, and `xor` for number-like types (note that comparison operations don't return the same number like type as their arguments, but `u1`).