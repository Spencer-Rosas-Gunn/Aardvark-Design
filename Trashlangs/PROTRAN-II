# Protran
### Concept
Protran (Program Translation) is a programming language that aims to combine the power of Lisp with the performance of Fortran.
### Basics
In Protran, constant values are created with "let", followed by a type declaration (name: type = value). Procedure definitions use the "lambda(arguments) { ... }" syntax.
```
let zero: int = 0i

let square: int(int) = lambda(x int) int {
  return x * x
}
```
The syntax structure "func" defines a procedure.
```
func square(x int) int {
  // The language has implicit return
  x * x
}
```
Cells are created with "var" and managed at runtime.
```
var count: int = 0i;

while count < 100i {
  count = count + 1i;
}
```
All calculations are known at build-time unless they depend on the value inside a cell or read/write to volatile references.
### Tables
Tables are created for structured data.
```
let linkedlistint_t: type = class {
  data: int,
  next: linkedlistint_t@?,
}
```
These tables (by convention "[name]_t", short for "[name] table") serve as sugar for using multiple values. They are indexed with a dot.
```
let three: linkedlistint_t = linkedlistint_t {
  data = 3,
  next = nil?int,
}

let twothree: linkedlistint_t = linkedlistint_t {
  data = 2i,
  next = three.@?,
}

let numthree = twothree.next.unwrap().data;
```
The dot before `unwrap` serves as a pipe operator, which is useful in complex expressions.
### Unions
Sum types are created with union. They store one state of the states specified, and are named with an "_u" by convention.
```
let iof_u: type = union {
	integer: int,
	real: float,
}
```
They are created with double-colon syntax.
```
let integer: iof_u = iof_u::integer(0);
```
### Datatypes
The builtin datatypes are int, float, & uint. These refer to word-sized integer, real, & unsigned values. Additionally, the type fixed is used for fixed-point operations, and is also word sized. The type null can only store the value nil.
```
let integer: int = 0i;
let real: float = 3.1415926f;
let unsigned: uint = 0u;
let discreet: fixed = 3.1415926x;
let nada: null = nil;
```
All values of type "null" are known at build-time. The type modifiers "[]", "[n]", "@", "?", "[@]", "[@:S]" represent slices, arrays, pointers, options, view pointers, and sentinel-terminated array pointers.
```
let slice: int[] = [1, 2, 3];
let array: int[3] = {1, 2, 3};
let pointer: int* = slice[1].*;
let option: int? = nil?;
let view: int[*] = slice.[*];
let sentinel: int[*:2] = slice.[&:2]
```
For list-like types, the syntax "obj[index]" is used to index (slices, arrays, & views). The .& syntax is used to convert from an lvalue to a reference and .* to do the converse. Options use the ? operator to wrap themselves, with nulls being able to take a type to wrap themselves as.

Since these use an indirection, to mark the underlying data as cells uses "var[]", "var*", & "var[*]" types.
```
let slice: int var[] = [1, 2, 3];
slice[1] := 0; # [1, 0, 3]

let pointer: int var* = slice[1].&;
pointer.* := 2; # [1, 2, 3]

let view: int var[*] = slice.[*];
view[1] := 0; # [1, 0, 3]
```
Out-of-bounds view indexing is undefined behaviour. Out-of-bounds slice or array indexing is modular. List-like types (except views) have the dot-length syntax to access length.

The slicing operator is provided for list-like types with the following syntax.
```
let nums: int[] = [1, 2, 3, 4];
let four: int = nums.length;
let backwards: int[] = nums[2:4]; # backwards = [3, 4, 1, 2]
```
Bitwidth can be specified after the type name (e.g. `uint8`, `float16`). Numbers without type specifiers coerce to proper datatypes. Number bitwdith specifiers are written `NuBB`, `NiBB`, `NfBB`, or `NxBB` (e.g. `0u32`). The default bitwidth is guaranteed to be bit-castable to and from pointers.
### Metafunctions
To simulate lisp's dynamic data structures, build-time computations can use the type datatype to manipulate datatypes.
```
func linkedlist_t(T type) type {
  return class {
    data: T,
    next: linkedlist_t(T)*?,
  }
}
```
### Memory Management
Memory is allocated dynamically with allocators.  Allocators are stored as `uint8[@](uint)` function pointers alongside `void(uint[@])`, but are used with `.new(type, len)` and return `type[@]`. They can also `.delete(ptr)`, which returns `void`. They're made with `@allocator(new, delete)`. The type name is `allocator`.
### Const
The "const" keyword is used to require a build-time known parameter.
```
func make_mem_with(const alloc allocator, val int) int* {
	let buf: int[*] = alloc.new(int, 1)
	buf[0] = val
	return buf[0].&
}
```
### Structures
With developments in algorithmic programming, goto statements can often be replaced with conditionals and loops. We've provided syntax for both.
```
if 0 {
	# Doesn't Execute
}
else if 0 {
	# Doesn't Execute
}
else {
	# Executes
}

var counter: int = 100;
while count > 0 {
  counter++;
}
```
Beyond this, procedural programming can be used to modularize control flow across subroutines and functions as users see fit. The switch structure can be used over unions.
```
let iof: iof_u = # ...
let val: int = switch(iof) {
	if integer { integer }
	else if real { @round(real) }
}
```
### Modules & Linkage
Symbols are exported with public and a file is imported with the builtin import procedure. External symbols are linked with imports, and internal symbols with exports. The calling convention specifiers are optional, and the default calling convention is PCC (Protran Calling Convention).
```
let std = @import("std");

std::print("Hi!");

public func square(x int) int {
	x * x
}

export("c") func cube(x float) float {
	x * x * x
}

import("c") func cbrt(x real) real;
```
### Builtin Procedures
A handful of builtin procedures are provided for common tasks.
```
let val: int = @as(int, 0); # Do implicit type conversion
let val: float = @sqrt(3.1415926);
let zero: float = @sin(0f); # Sin (in radians)
let mmioptr: uint8 volatile[@] = @bit_cast(nat8 volatile[@], 0xB8000);
let three: int = @round(3.1415926);
let four: uint = @abs(-4);
let stack: allocator = @allocator(@push, func(ptr uint8[@]){})
```
### Volatility
Volatile is a type modifier (e.g. int volatile). Volatile values are always runtime-known.
### Packed Types
Packed tables are used to ensure there is no padding between fields, and packed unions are used for untagged unions. Packed unions can be indexed but not switched. The .packed operator can be used to convert normal unions to packed unions.
```
let packedtable_t: type = packed class {
	first: float,
	second: float,
}

let packedunion_u: type = packed union {
	first: float,
	second: int,
}
```
### Case Sensitivity
While most modern character sets support both upper and lower-case, we've chosen to remain case insensitive for backwards-compatibility.
### Volatility
Volatile is a type modifier (e.g. int volatile). Volatile values are always runtime-known.
### Strings
Strings (`"Hi!"`) are data-section memory stored as a pointer to `uint8[]`.
### Type Inference
Function parameters are implicitly generic, and return values/variable types are inferred.