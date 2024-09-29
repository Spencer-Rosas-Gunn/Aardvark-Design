# AOL
```
program ::= { [ "export" ] declaration }
```
AOL (Agent-Oriented Language) is a high-level strongly and statically typed agent-oriented programming language. Here's a "Hello World" program.
```
// The standard library is implemented in pure AOL
let std: Module = import("std")
// The OS library is implemented on a per-OS basis as an OS abstraction layer
let os: Module = import("os")

interface Main
{
	func main() void
}

class TMain implements Main
{
	var counter: Int = 0

	func new(val Int) TMain
	{
		counter := val
	}	

	func Main::main() void
	{
		for(this.counter := 0; this.counter < args.length(); this.counter++)
			std::IOStream::os::TFileStream("/dev/stdout").write("Hello World!")
	}
}
```
### Conceptual Overview
In AOL, there are four primary concepts. The first are standard; variables and procedures. The latter two are classes and interfaces. In standard object-oriented programming, abstractions are partial; a class is meant to hide *some* data behind its interface. In agent-oriented programming, abstractions are complete.

In AOL, a class can never be used directly except to instantiate an interface object. Each class implements some set of interfaces, and is instantiated with the desired interface, a `::`, the class name, and then arguments to it's `new` procedure.
### Variables
```
variable ::= ["let" | "var"] IDENTIFIER [":" type] "=" expression
statement |= variable
statement |= expression ":=" expression
```
Variables are created with the syntax `let name = value` or `var name = value`, with optional `: T` type annotations (which are otherwise inferred from the initialization value). All variables must be initialized. `var`-declared variables can be reassigned with the `:=` syntax. Reassignment is *not* an operator, and cannot be done inside an expression. By convention, variables are named in snake_case.
### Methods
```
method ::= ["func"] IDENTIFIER "(" [ { IDENTIFIER type "," } IDENTIFIER type ] ")" type
```
Methods are created with the syntax `func(args arg_type) return_type { ... }`. Methods are not allowed to exist separately from interfaces and classes. That is, general-purpose procedures are not allowed. By convention, methods are named in camelCase.
### Interfaces
```
interface ::= "interface" [ "(" { IDENTIFIER "," } IDENTIFIER ")" ] IDENTIFIER "{" { method } "}"
```
Interfaces are created with the `interface Name { ... }` syntax. By convention, interfaces are named in PascalCase. Interfaces define types with methods, but don't implement those methods.
### Classes
```
class ::= "class" IDENTIFIER [ "(" { IDENTIFIER "," } IDENTIFIER ")" ] ":" { name "," } name "{" { variable } { [ name "::" ] method "{" { statement | declaration } "}" }
expression |= name "::" name "(" { expression "," } expression ")"
```
Classes implement at least one interface, and can implement methods not connected to any interface for use in modularizing the implementation of other methods (so-called "private" methods). Classes also define a set of *fields*, which contain state that's shared between all methods. They are instantiated with `Interface::TClass(args)` by defining a private `new` method. By convention, their names are prefixed with `T` and in PascalCase.

Every class implicitly implements the `Object` interface.
### Modules
```
declaration |= "let" IDENTIFIER "=" "import" "(" { IDENTIFIER "/" } IDENTIFIER ")"
declaration |= "module" { IDENTIFIER "/" } IDENTIFIER
```
Modules are used to import external libraries. They look like `let` statements, but they aren't traditional variables. The `module` keyword is used to name the current file as a module; otherwise, the current file will be used as the main file. The `/` syntax is used to specify a module as a submodule of another module. The `export` keyword is used before a top-level declaration to export it. `export module` is not allowed.
### Generics
Types are like modules in that they must be compile-time known. They can be used with the `class TName(T)` or `interface Name(T)` syntax to create generic datatypes and do generic programming. This is instantiated with `TName(T)` or `Name(T)` (e.g. `Array(Number)`).
### Method-Calls
```
expression |= expression "." IDENTIFIER "(" [ { expression "," } expression ] ")"
```
The primary way of getting work done is to make a series of method calls. This uses the `object.method(args)` syntax.
### Namespaces
```
name ::= ( [ IDENTIFIER "::" ] name ) | IDENTIFIER
```
Index modules with `::`.
### Method-Bodies
Basic imperative programming mechanisms should be included to implement methods. This section is TBD.