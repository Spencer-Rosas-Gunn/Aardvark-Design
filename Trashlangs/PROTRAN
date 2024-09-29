# Protran
### Concept
Protran (Program Translation) is a programming language that aims to combine the power of Lisp with the performance of Fortran.
### Basics
In Protran, constant values are created with "let", followed by a fortran-style declaration (type :: name = value). Procedure definitions use the "lambda(arguments) begin ... end" syntax, a combination of lisp & Fortran.
```
let int :: zero = 0i;

let int(int) :: square = lambda(int x) int
begin
  return x * x;
end;
```
The syntax structure "fun" defines a procedure (similar to "defun" in lisp, without the "de" convention).
```
fun int :: square(int :: x)
begin
  return x * x;
end;
```
Cells are created with "var" and managed at runtime.
```
var int :: count = 0i;

while count < 100i
begin
  count := count + 1i;
end;
```
All calculations are known at build-time unless they depend on the value inside a cell or read/write to volatile references.
### Tables
The power of Lisp lies in its linked list data structure. Fortran is capable only of modeling basic datatypes, whereas lisp allows arbitrary data to be stored in a given structure. To emulate this capability, the "table" was created, which is a grouping of named and typed values.
```
let type :: linkedlistint_t = table
begin
  int :: data,
  linkedlistint_t@? :: next,
end;
```
These tables (by convention "[name]_t", short for "[name] table") serve as sugar for using multiple values. They are indexed with a slash.
```
let linkedlistint_t :: three = linkedlistint_t
begin
  data = 3,
  next = nil?int,
end;

let linkedlistint_t :: twothree = linkedlistint_t
begin
  data = 2i,
  next = three.@?,
end;

let numthree = twothree/next:unwrap()/data;
```
The colon here serves as a pipe operator, which is useful in complex expressions.
### Unions
Sum types are created with union. They store one state of the states specified, and are named with an "_u" by convention.
```
let type :: iof_u = union
begin
	int :: integer,
	real :: float,
end;
```
They are created with slash syntax.
```
let iof_u :: integer = iof_u/integer(0);
```
### Datatypes
The builtin datatypes are int, real, & nat. These refer to word-sized integer, real, & unsigned values. Additionally, the type fixed is used for fixed-point operations, and is also word sized. The type null can only store the value nil.
```
let int :: integer = 0i;
let real :: real = 3.1415926f;
let nat :: unsigned = 0u;
let fixed :: discreet = 3.1415926x;
let null :: nada = nil;
```
All values of type "null" are known at build-time. The type modifiers "[]", "[n]", "@", "?", "[@]", "[@:S]" represent slices, arrays, pointers, options, view pointers, and sentinel-terminated array pointers.
```
let int[] :: slice = [1, 2, 3];
let int[3] :: array = {1, 2, 3};
let int@ :: pointer = slice[1].@;
let int? :: option = nil?;
let int[@] :: view = slice.[@];
let int[@:2] :: sentinel = slice.[@:2]
```
For list-like types, the syntax "obj[index]" is used to index (slices, arrays, & views). The .@ syntax is used to convert from an lvalue to a reference and .* to do the converse. Options use the ? operator to wrap themselves, with nulls being able to take a type to wrap themselves as.

Since these use an indirection, to mark the underlying data as cells uses "var[]", "var@", & "var[@]" types.
```
let int var[] :: slice = [1, 2, 3];
slice[1] := 0; # [1, 0, 3]

let int var@ :: pointer = slice[1].@;
pointer.* := 2; # [1, 2, 3]

let int var[@] :: view = slice.[@];
view[1] := 0; # [1, 0, 3]
```
Out-of-bounds view indexing is undefined behaviour. Out-of-bounds slice or array indexing is modular. List-like types (except views) have the slash-length syntax to access length.

The slicing operator is provided for list-like types with the following syntax.
```
let int[] :: nums = [1, 2, 3, 4];
let int :: four = nums/length;
let int[] :: backwards = nums[2:4]; # This gets four items starting at index two
# backwards = [3, 4, 1, 2]
```
Bitwidth can be specified after the type name (e.g. `nat8`, `real16`). Numbers without type specifiers coerce to proper datatypes. Number bitwdith specifiers are written `NuBB`, `NiBB`, `NfBB`, or `NxBB` (e.g. `0u32`). The default bitwidth is guaranteed to be bit-castable to and from pointers.
### Metafunctions
To simulate lisp's dynamic data structures, build-time computations can use the type datatype to manipulate datatypes.
```
fun type :: linkedlist_t(type :: T)
begin
  return table
  begin
    T :: data,
    linkedlist_t(T)@? :: next,
  end;
end;
```
### Memory Management
Memory is allocated dynamically with allocators.  Allocators are stored as `nat8[@](nat)` function pointers, but are used with `.new(type, len)` and return `type[@]`. They're coerced from function pointers. The type name is `allocator`.
### Const
The "const" keyword is used to require a build-time known parameter.
```
fun int@ :: make_mem_with(const allocator :: alloc, int :: val)
begin
  let int[@] :: buf = alloc.new(int, 1);
  buf[0] := val;
  return buf[0].@;
end;
```
### Structures
With developments in algorithmic programming, goto statements can often be replaced with conditionals and loops. We've provided syntax for both.
```
if 0 then
begin
  # Doesn't execute
end;
# This branch is optional
else if 0 then
begin
  # Doesn't execute
end;
# This branch is also optional
else then
begin
  # Executes
end;

var int :: counter = 100;
for count > 0 do
begin
  counter := counter + 1;
end;
```
Beyond this, procedural programming can be used to modularize control flow across subroutines and functions as users see fit. The switch structure can be used over unions.
```
let iof_u :: iof = # ...
let int :: val = switch(iof)
begin
	if integer then
	begin
		return integer;
	end;
	else if real then
	begin
		return @round(real);
	end;
end;
```
### Modules & Linkage
Symbols are exported with public and a file is imported as an object with the builtin import procedure. External symbols are linked with imports, and internal symbols with exports. The calling convention specifiers are optional, and the default calling convention is PCC (Protran Calling Convention).
```
import std;

std/print("Hi!");

public fun int :: square(int :: x)
begin
  return x * x;
end;

export("c") fun real :: cube(real :: x)
begin
	return x * x * x;
end;

import("c") fun real :: cbrt(real :: x);
```
### Builtin Procedures
A handful of builtin procedures are provided for common tasks.
```
let int :: val = @as(int, 0); # Do implicit type conversion
let real :: val = @sqrt(3.1415926);
let real :: zero = @sin(0f); # Sin (in radians)
let nat8 volatile[@] :: mmioptr = @bit_cast(nat8 volatile[@], 0xB8000);
let int :: three = @round(3.1415926);
let nat :: four = @abs(-4);
let allocator :: stack = @push; // Memory is freed at the end of the scope
```
### Volatility
Volatile is a type modifier (e.g. int volatile). Volatile values are always runtime-known.
### Packed Types
Packed tables are used to ensure there is no padding between fields, and packed unions are used for untagged unions. Packed unions can be indexed but not switched. The .packed operator can be used to convert normal unions to packed unions.
```
let type :: packedtable_t = table
begin
	real :: x,
	real :: y,
end;

let type :: packedunion_u = union
begin
	real :: float,
	int :: integer,
end;
```
### Case Sensitivity
While most modern character sets support both upper and lower-case, we've chosen to remain case insensitive for backwards-compatibility.
### Volatility
Volatile is a type modifier (e.g. int volatile). Volatile values are always runtime-known.
### Strings
Strings (`"Hi!"`) are data-section memory stored as a pointer to `nat8[]`.