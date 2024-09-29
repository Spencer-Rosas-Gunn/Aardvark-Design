# NoASM
NoASM (NOt ASseMbler) is a programming language that exists.
### Syntax
```
program ::= { "import" STRING ";" } { fn | static }
fn ::= [ "pub" ] "fn" "@" id "(" args ")" type ":" { body ";" }
static ::= "@" id ":" type "=" expr ";"
args ::= { id ":" type "," } id ":" type
type ::= { "&" } ( ( ( "i" | "u" | "f" ) INTEGER) )
body ::= ( "%" id ":" type "=" expr ) | stmt | ( "#" id ":" )
```
Declare a function with `fn`. Inside a function, you can declare a label with `#label:`, or a variable with `%`. Outside a function, you can declare a variable with `@`. You can use imperatives as well.
### Values
```
val |= [ "-" ] ( INTEGER ( "i" | "u" ) ) | ( INTEGER "." INTEGER "f" )
val |= "@" id
val |= "%" id
val |= STRING

label ::= "#" id
var ::= ( "%" id ) | ( "@" id )
```
### Imperatives
```
stmt |= "return" val

stmt |= "goto" id
stmt |= "if" val label label
stmt |= "store" val val

stmt |= "call" val "(" { val "," } val ")"
```
The `return` imperative returns a value and exits the procedure. The `goto` imperative is used to unconditionally go to a label. The `if` imperative is used for branching. The `store` imperative is used to write a value to a pointer.
### Expressions
```
expr |= "push" type
expr |= "load" val

expr |= "and" val val
expr |= "or" val val
expr |= "xor" val
expr |= "not" val

expr |= "add" val val
expr |= "sub" val val
expr |= "mul" val val
expr |= "div" val val
expr |= "mod" val val
expr |= "neg" val

expr |= "as" type val

expr |= "call" val "(" { val "," } val ")"
```
Expressions are used for computation. Strings are of type `&u8` and are, by convention, null-teriminated.
### Modules
Use `import "path"` to import a module, and use `pub` before a `fn` to export it.
### Hello World
```
import "std";

pub fn main() u64:
	%stdout = call std__IOStream__new("/dev/stdout\0");
	call std__IOStream__write(%stdout, "Hello World!\0");
	
	return 0;
```