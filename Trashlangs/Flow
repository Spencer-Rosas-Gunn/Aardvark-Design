# Flow
Flow is a systems programming language that is as near to safe as a turing-complete programming language can be. It is impossible for any procedure to crash the program, it is impossible for pointers to be indeterminate, doubly freed, leaked, or dangling, and all control-flow is explicit.

However, like all Turing-complete languages, a procedure *can* enter an infinite loop and thereby halt program execution. Unfortunately, this is unavoidable due to the halting problem. Additionally, enough stack memory can be allocated that it causes an out of memory error, which is also unavoidable, since Turing machines require the ability to store arbitrary amounts of data--which is what separates them from finite-state automata.
```rust
let std = @import("std")

let str: &static const [*:0]u8 = "Hello World!"
let copy: &dynamic var []u8 = std::strcpy(alloc, str)
```
# Safety System
The Flow safety system is a safety system that introduces zero performance overhead (though it may restrict some performance optimizations) to achieve full thread-safety and memory-safety.
### Runtime
Flow requires four runtime facilities. These are memory allocation, memory deallocation, thread spawning, and thread awaiting. In the language, these use the `new`, `delete`, `async`, and `await` procedures.

When `async` and `await` aren't defined, those keywords aren't usable (though you can still declare `async` functions). When `new` and `delete` aren't defined, `&dynamic` pointers can't be used and `new` expressions aren't allowed. As such, the language is still usable without a runtime environment.

The runtime environment is allowed to, but not required to, provide a `catch` procedure and guard pages to dynamically resize the stack in the case of a stack overflow error.
### Pointer Types
The pointer types are `&static`, `&dynamic`, and `&` (data section, heap, and stack). Dynamic pointers can have various behaviors. Those are `&dynamic` (move) & `&` (borrow). After assigning (including pass to a function) a `&dynamic` pointer to another variable, the old pointer is no longer accessible. `&` pointers can take any pointer without a scope no more local than itself.

Pointer types are (in order of weakest to strongest) `& &dynamic &static`. Stronger pointer types can decay to weaker ones, but not vice-versa. When pointer type is being inferred, the weakest permissible pointer is used.
```rust
// Note that "**" here is the array multiplication operator
var one: &dynamic var [100]i32 = try new {0i32} ** 100
var two: &dynamic var [100]i32 = try new {0i32} ** 100

// This initialization moves one
var alpha: &dynamic var [100]i32 = one
// This assignment borrows two
var beta: ?&local var [100]i32 = null
buf := two
```
Pointers can specify the mutability of the pointed-to type--either as `const` or `var`. Variables declared with `let` can only be referenced with `const` pointers, and variables with `var` with `var`. Generic pointers (that don't specify mutability) can point to either sort of variable, and can't be assigned through.

<!-- The purpose for the &const T/&T distinction is that since &const is guaranteed not to be mutated, it can be optimized better than &T--which could be an immutable reference to a mutable value -->
Unlike most languages with automatic memory management facilities, Flow does not implicitly perform allocations. To dynamically allocate memory, use the `new` keyword followed by the value to initialize the new memory buffer with. The type being allocated is inferred. New expressions return a result type, which is an error in the case of out of memory errors.

Deallocation is, however, implicit. The deallocation rules are simple. `&local` variables are freed whenever the scope they were allocated in ends. They can be dynamically allocated with the `push` syntax, which is just like `new` but works on the stack. `&move` variables are freed whenever they reach the end of a scope without being moved-from. `&borrow` variables are not owned by the borrower, and are therefore not freed by the borrower. `&static` variables exist in the data-section, and are never freed.
### Asynchronous Functions
Use the `async` keyword to declare a function that can be called on its own thread. Threaded functions cannot read from external `var` declared variables, they cannot read from `&T` or `&var T` pointers, and they cannot write to external state. They can read from external `let` declared variables and `&const` pointers.

To call a function on a new thread, use the `async` keyword before calling it. That will return an `async T` type. Whenever you need the return value, use the `await` keyword.
```rust
async func square(x int) int { x * x }

let four: int = square(2)

let will_four: async i32 = async square(2)

let four2: int = await will_four
```
These rules are, in general, far too restrictive to facilitate many useful tasks. As such, the `unsafe` keyword can be used to break the threading rules, and the standard library provides generic types to abstract over `unsafe` and ensure thread safety.
```rust
var val: int = 0

async func get_mut() int {
	return unsafe { val }
}
```
### Result Types
Result types are created with the `!T` syntax, or the `F!T` syntax to specify the error type. The default error type is `error`. The `error` type is the global builtin `enum error: uint`. The only value in that enumeration by default is `out_of_memory`.

From a procedure returning `F!T`, return a value of type `F` with `throw val` and a value of type `T` with `return val`. In all other contexts, result types are coerced from one of the types they're made from. To bubble errors with a result type use `try val`. To catch an error and get a value, use `try val catch(error err) result`.
### Destruction
When a value is destroyed, any owned (`&dynamic`) references it holds will also be destroyed.
# Type System
The Flow type system is an algebraic, polymorphic type system.
### Compile-Time Types
An expression is compile-time known if it does not rely on calls to function that are not compile-time computable (i.e. that contain runtime-known expressions), does not rely on a value that is not compile-time known, does not rely on runtime-known expressions, and is not mutable.
### Basic Types
The basic types provided by the language are `int`, `uint`, `float`, `fixed`, & `ufixed`. These types represent signed, unsigned, floating-point, signed fixed-point, and unsigned fixed point numeric values. They can be bitwidth specified with `iBB`, `uBB`, `fBB`, `ixBB_BB`, & `uxBB_BB` (fixed points can specify the integer and fractional parts with seperate bitwidths).

Literals of these types are written with the `i u f x ux iBB uBB fBB xBB uxBB` suffixes for those above types, respectively. Literals without those types can coerce, where needed.
### Type Modifiers
Type modifiers are split into two genres; the dynamic and static types. Dynamic types must be used indirectly, i.e. through a pointer, since their size is not known at compile-time. Static types can be used directly.

Dynamic types include `[]T`, `[*]T`, & `[*:v]T`. These are slices, views, and sentinel terminated view. Views can only be indexed in `unsafe`, because bounds checking is not possible. These types can be indexed with `val[index]`, and the `v` value is a compile-time known value of type `T`. These are written `{ ... }`, `[*]{ ... }`, and `[*:v]{ ... }` respectively (which are implicitly `push`ed unless used in a `new` expression). The first and last implement generators. String literals can coerce to `&static []u8`, `&static [*:0]u8`, or `&static [*]u8` as needed.

Static types include `?T` (sugar for `void!T`), `[n]T` (where `n` is a compile-time known `uint`), and `*T` (a raw pointer to `T`--can only be dereferenced in `unsafe`).
### Classes
Flow is not an object-oriented language, it is procedural. However, Flow has adopted the `class` keyword for readability. Classes are of the type `type`, which must be compile-time known.
```rust
let Point_t = class {
	x int,
	y int,
}
```
### Unions
Sum types are created with `union`.
```rust
let Pet_t = union {
	dog Dog_t,
	cat Cat_t,
}
```
They can be pattern matched with `if` statements.
```rust
let name = if pet
	dog dog.name
	cat cat.name
```
Pattern matching only needs to be exhaustive when used as an expression.
### Interfaces
Interfaces are created with the `interface` keyword, and are of the type `type`. They can use the `Self` type.
```rust
let IHasIntX = interface {
	func getx(self Self) int
}
```
Interfaces are implemented with `impl IInterface for Type_t`.
```rust
impl IHasIntX for Point_t {
	func getx(self Self) int {
		self.x
	}
}
```
A type can define and implement its own anonymous interface with `impl Type_t`.
```rust
impl Point_t {
	func len(self Self) float {
		@sqrt(self.x * self.x + self.y * self.y)
	}
}
```
You can implement a type multiple times to add methods extrusively, which is useful for extending included libraries--including the standard library.

To get a dynamically typed reference to an object implementing an interface, use `virtual &IInterface`.
```
let point = Point_t { x: 0, y: 0 }
let has_int_x: virtual &dynamic IHasIntX = &point
```
### Enumerations
Enumerations are created with `enum`.
```rust
let MyBool_t = enum {
	then,
	false,
}
```
Enumeration states are accessed with `type::state`.
```rust
let my_true = MyBool_t::then
```
To define the underlying integer type of an `enum` use `()`.
```rust
let MyBool_t = enum(u1) {
	then,
	false,
}
```
To define an enumeration to which states can later be added, the `var enum` syntax is used. They must always use an `else` branch when being matched against.
```rust
let MyError_t = enum(uint) {
	out_of_memory = 1,
}
```
Enumeration values start at zero and add one to the previous value unless specifically given a value with `=`.

The builtin type `bool` is sugar for `enum(u1) { then, false }` and `true`/`false` are global constants equal to `bool::then` and `bool::false`. The builtin type `error` is sugar for `enum(uint) { out_of_memory = 1, }`.
### Modules
`module` is the type returned by `@import`, and must be compile-time known. It is indexed with `::`. Compile-time manipulation of `module` values is done instead of a formal build system.
```rust
let arch: module =
	if @import("platform.fl")::arch == "x64" then
	{
		@import("x64.fl")
	}
	else if @import("platform.fl")::arch == "aarch64" then
	{
		@import("aarch64.fl")
	}
	else
	{
		@compileError("Architecture not found!")
	}
```
### Generators
Implement generators with a `for` method and the `yield` keyword.
```rust
let std = @import("std")

func ListWrapper_t(T type) type {
	let Out_t = class {
		data &dynamic []T,
	}

	impl Out_t {
		func for(self Self) std::ArrayList(T) {
			let out = std::ArrayList(T)::init()

			for item in self.data
			{
				out.push(yield item)
			}

			out
		}
	}

	Out_t
}
```
Many basic type modifiers implement generators, which behave like a map function.
### Function Pointers
Function pointers are of type `return_t(arg_t)`.
```rust
func square(x int) int {
	return x * x
}

let sptr: &static int(int) = square
```
### Closures
Lambdas use the `=>` or `func` syntax. When inline functions or lamdbas capture external lexical variables, they create a *closure*. Closures are referenced with `virtual &return_t(arg_t)`.
```rust
let const_f = func(x int) int {
	(y int) => x
}

let one: virtual &static int(int) = const_f(1)
```
### Vectors
Vectors use the generic `@vector(type, len)` syntax to get the type, and `@vector(item, item, item)` to produce values.
```rust
let vec3: @vector(int, 3) = @vector(1, 2, 3)
```
Vectors have the `**` vector multiplication operator and the `++` concatenation operator.
### Tuples
Tuple types are written `(Type, Type)`. They are returned with `return val, val`, broken with `break val, val`, continued with `continue val, val`, and otherwise constructed with `(val, val)`.
```rust
func double(x int) (int, int) {
	return (x, x)
}
```
### Implicit Generics & Inferred Types
Function arguments of pointer type implicitly make their memory management mode generic, and function return values can infer the reference type. If argument types aren't specified at all, they'll be generic. If the return type isn't specified at all, it'll be inferred. Variables can have inferred types.
```rust
let integer = 0i

func id(x) { x }

func id_int_ref(x &int) &int {
	return x
}
```
# Imperatives
The language provides various imperative constructs for convenience.
### While
The while loop uses the syntax `while COND do ...`. For iterative loops, the `while var NAME = VAL, COND (NEXT) do ...` syntax is used. You can also use `while var NAME = VAL, COND do ...` or `while COND (NEXT) do ...`.
```rust
var i = 0
while i < 100 do
	i := i + 1

while var i = 0, i < 100 do
	i := i + 1

var i = 0
while i < 100 (i := i + 1) do
	something()

while var i = 0, i < 100 (i := i + 1) do
	something()
```
### Break & Continue
The `break` keyword can be used to exit a for-loop (when the for loop is used as an expression, you must provide a value to the break), a while-loop, or a pattern match (when used as an expression, you must provide a value to the break). `continue` can be used to exit a for-loop iteration (when the yielded value is used, a value must be provided), a while-loop iteration, or a pattern match arm (if a next arm exists).
```rust
let zero = if true
	then continue
	else break 0

let one = for item in { 1, 2, 3 } do
	continue 1

let nums = for item in { 1, 2, 3 } do
	break { 4, 5, 6 }

while true do
	break

while var i = 0, i < 100 (i := i + 1) do
	continue
```
### Return
To early-exit the current procedure, use the `return` keyword.
```rust
func early_returns(b bool, x int) {
	if b then return x

	0
}
```
# Expressions
### Operators
Expression can use the `*` operator on pointers (dereference), the `&` operator on lvalues (to pointer), and the `[index]` operator on indexable types. One level of dereferencing is implicit in expressions. Arrays can do array multiplication with `**`. Numeric types support the `+ - * / % ** > < >= <=` operators. Integer types support the `>> << & | ^ ~` operators. Booleans support the `not and or xor ?:` operators. All types implement `:=` (for lvalues), `==`, and `!=` implicitly.
### Builtin-Functions
The builtin `@sqrt` function is used for square roots and `@dot` is used for dot-products. The `@as(result_t, arg)` function is used for type coercion. In `unsafe` the `@out(port, val)`, `@int(no, args)`, `@syscall(no, args)`, and `@bit_cast(result_t, arg)` procedures are added for low-level operations. In unsafe, the `undefined` keyword can be used to produce arbitrary indeterminate values. The `unreachable` keyword can be used to specify a control path whose execution is undefined behavior.
### Function Calls
You can call functions or methods with `fname(args)` or `obj.method(args)`.