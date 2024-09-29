# Functionally-Scriptable Object Notation
### Basics
```
program ::= { ( id ":" ( class | expr ) ";" ) | method }
classdef ::= { ( id ":" type ";" ) | method }
method ::= "function" id "(" { id type "," } id type ")" type "=>" expr
type ::= id | "Number" | "String" | "Boolean" | fn | array
fn ::= type "(" { type "," } type ")"
array ::= ( "[" INTEGER "]" type ) | ( "[]" type )
```
Programs are made up of variable declarations, function definitions, and class definitions. Functions are defined as a mapping between one value and another. Variables are identifiers bound to expressions. Classes are a set of functions (methods) and fields (identifier-type pairs). Types are functions of other types, arrays of other types, classes, numbers, strings, or booleans.
### Literals
```
expr |= NUMBER | STRING | "true" | "false"
```
Literals are employed very often in code. They are numbers, strings, or boolean values.
### Array Literals & Index Expressions
```
expr |= "[" { expr ";" } "]"
expr |= expr "." expr
expr |= id | "this"
expr |= expr >>= expr
```
Array literals are indexed with `.index` and are used for arrays of a type.

Objects are indexed with `obj.property`. The `this` object represents the object the current method is being defined over--the global object in functions.

Expressions are mapped with `>>=`.
### Types
Types can be function types (the type of methods is just a function with the first argument being `this`), array types, classes, numbers, strings, or booleans.
### Lambdas
```
expr |= "function" "(" { id type "," } id type ")" type "=>" expr
expr |= expr "(" { expr "," } expr ")"
```
Lambdas are used for inline method definitions, such as in map expressions. They are called just like methods.
### Branching
```
expr |= "if" expr "then" expr "else" expr
```
Branches are done with if-then-else statements, and work like a ternary operator.
### Arithmetic
```
expr |= expr binary expr
expr |= unary expr
```
Arithmetic uses binary and unary operators. Here are the operators, from highest to lowest precedence.
```
a**b
a*b a/b
a+b a-b -a
a&b a|b a^b !a
a>b a<b a>=b a<=b a==b a!=b
```