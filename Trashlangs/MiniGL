# Z
Z is based on Zig. It is a much more minimal language, provides better low-level control, and has better patterns for error handling.
### Variables
Declare immutable variables with `let` and `var`. `let` is the biggest syntactic difference to Zig. Assignment uses `:=`.
```rust
let zero = 0u32;
var one = 1u32;

let zero_wt: u32 = 0;
var one_wt: u32 = 1;

one := 2;
one_wt := 2;
```
Variable identifiers are pointers to their data's type. There is no distinction between lvalues and pointers.
### Functions
Declare a function with `fn`. Functions can be declared inline, but cannot create lexical closures. Z adds implicit return.
```rust
fn square(x int) int {
	x * x;
}
```
### Basic Types
The basic types are `int`, `uint`, `float`, & `fixed`. They are word-sized but can be bitwidth specified with `iBB`, `uBB`, `fBB`, and `xBB_BB` (integer part first). The default fixed bitwidth is half and half of the word size.

The compile-time types are `module`, `type`, `number`, and `string`. The latter two are the types of literals. To specify a number type, follow it with one of `i u f x` and optionally a bitwidth. Character literals are of type `number`.

The type `any` is used for duck-typed arguments, and the type `void` is used for statements.
```rust
let integer = 0i;
let unsigned = 0u;
let real = 0f;
let fraction = 0x;
```
Write binary or hexidecimal bitfields (coercable to signed or unsigned types) with `0hABCD` and `0b0101`.
### Type Modifiers
Write function types with `(args)ret`. Write slices with `[]T`. Write arrays with `[const uint]T`. Write views with `[*]T`. Write sentinel-terminated vies with `[*:const T]T`. Write vectors with `@vec(const uint, T)`. Write pointers to var/let declared variables with  `*var T` or `*const T`, or use the generic pointer type `*T`.

Functions, slices, views, and sentinel-terminated views are of runtime-known size, so they must be used through an indirection.

Index list-like types with `arr[index]`, which returns a pointer to the relevant item. For all but views, find the length with `.len`. All of them implement generators, but the view generator must manually be ended with `break` or it will create a buffer overflow.

Deference pointers with `.*`, though this normally happens implicitly. There is no reference operator, as there are no lvalues.

Write arrays with `{ item, item, item }`, slices with `[ item, item, item ]`, and vectors with `@vec( item, item, item )`.

Strings can coerce to slices, arrays, vectors, and (optionally sentinel-terminated) views of any integer type. The character encoding used is implementation-defined. We recommend ASCII, UTF-8, UTF-16, & UTF-32 for `i8`, `u8`, `u16`, & `u32` respectively. The character-literal encoding and string-literal encoding will always match.
### Structures
Create `type` literals for product and sum types with `struct` and `union` respectively. Structures are word-aligned, but this can be diabled with `packed struct`. Use `packed union` to remove tagging. This all works just as in Zig, save for `union`/`union(enum)` being replaced with `packed union`/`union`.
```rust
let Point_t: type = struct {
	x: float,
	y: float,

	pub fn len(self: *Point_t) float {
		sqrt(x * x + y * y);
	}
};

let Pet_t: type = union {
	dog: Dog_t,
	cat: Cat_t,

	pub fn name(self: *Pet_t) []u8 {
		match(self) {
		.dog {
			self.name
		}
		.cat {
			self.name
		}
		};
	}
};
```
### Match, If, Else, & While
Use `match` for pattern-matching, and use `if`-`else` for conditions. Optional binding is supported. Use `while` for iteration, with optional iteration behaviour.
```rust
let name = match(pet) {
	// Use states to check for a typical type
	.dog {
		// Inside the block, the value behaves as its state
		pet.name
	}

	// Use "else" to handle other states
	.else {
		"Doge"
	}
}

let ternary = if(2 + 2 == 3) {
	false
} else if(2 + 2 == 4) {
	true
} else {
	false
}

var i = 0;
while(i < 100) {
	i += 1;
	do_stuff(i);
}

var i = 0:
while(i < 100) : (i += 1) {
	do_stuff(i);
}
```
In an `if` statement, `break` exits the condition (not allowed when used as an expression) & `continue` moves to the next condition. In a `while` loop, `break` ends the loop and `continue` moves to the next iteration. In a `match` statement, `break` exists the match (not allowed when used as an expression) and `continue` falls through to the next case (not allowed in the last/else case when used as an expression).
### For
Use `for` methods to create generators with `yield`. The `return`ed value is returned from the iteration. List-like types implement generators that do not return a value.
```rust
let Point_t = struct {
	x: float,
	y: float,

	pub fn for(self: *Point_t) Point_t {
		let out_x = yield self.x;
		let out_y = yield self.y;
		
		return Point_t {
			x: out_x,
			y: out_y,
		}
	}
};

let switched = for(let val in Point_t { x: 1, y: 2, }) {
	3 - val
};
```
When not used as an expression, `break` ends for-loop iteration early (deferred statements from the generator still execute) and `continue` takes a value to yield.
### Inline
Use `inline` before a for or while loop to unfurl the loop at compile time. `inline` while loops can happen with the variable declaration, as shown below.
```rust
inline for(let item in [int, uint, float]) {
	arena.new(item);
}

inline var x = 0;
while(x < 100) : (x += 1) {
	arena.new(types[x]);
}
```
### Low-Level Functions
Use `@asm(op, args)` to insert an assembly instruction. This is done instead of a full inline-assembly mechanism. The `@out`, `@int`, & `@syscall` operations are included as built-in functions (to avoid the need for inline assembly). Use `@mov(reg)` syntax to read from a register and `@mov(reg, val)` to write to one. Said register will not be used to emulate variables in the current procedure or any called inline procedures.
### Module Management
Use `@import(path string) module&` for module management. Index modules with `.`. Compile a module by dereferencing.
```rust
let std = @import("std").*;
let os = @import("os"); // .* is implicit with .

let array = std.ArrayList_t(u8, os::allocator, {1, 2, 3});
```
### Notes
The language has no UB by default, though some behaviour (e.g. divide-by-zero) may be architecture specific. Use the `@optimize` directive to specify optimizations for a scope. Optimizations cascade.
```rust
@optimize("fast-float-arithmetic")

fn square(x float) float {
	@optimize("safe-float-arithmetic")
	x * x
}
```
### Builtin-Functions
Use `@typeof` to get a type from a variable, and use `@sizeof` to get bytewidth of a type or variable. Use the implicit `.@typeof()` method for static reflection, with `.fields` and `.methods` properties as compile-time generators for use with `inline for`.
### Type Casts
Use `@bit_cast(target type, val any)` for bitwise-casting, and `as` for type coersion.
### Compile-Time
Use `const` for compile-time values (like Zig `comptime`).
### Use
Use `use args <- {}` to put lambdas that should've been the last argument outside of a function call.
```rust
fallible
.try()
use(val u8) {
	return err.new(val);
}
.catch()
use(error [*:0]u8) {
	err.throw(error);
}
```
# Examples
### Runtime
Most runtime facilities are dynamic allocation, dynamic typing, & exception handling. Here is an example of all of them.
```rust
let os = @import("os");
let std = @import("std");

let alloc = std.alloc(os.mmap)

let val: *std.dyn = std.dyn.new(alloc, 0u8);
val.set("Hi" as [*:0]u8);

val.get([*:0]u8).try()
use(hi [*:0]u8) void {
	os.getf("/dev/stdout").write(hi);
};

fn divide(top u8, bot u8, const err std.error_handler) err.fallible(u8) {
	if(bot == 0) {
		return err.throw(u8, std.errors.div_by_zero);
	}

	return err.new(0u8);
}

divide(1, 2, os.error_handler)
.try()
use(hope u8) void {
	os.getf("/dev/stdout").write("Success!");
}
.catch()
use(error os.error_handler.exception(std.error)) void {
	os.getf("/dev/stderr").write(error);
};
```