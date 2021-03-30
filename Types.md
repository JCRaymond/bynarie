
# Bynarie Type System

Separate the notions of content and behavior.

## No Content Type Hierarchy

For content, the definition of structs behaves as the definition of concrete types. The structs may implement any number of traits, and structs may contain other structs, but there is no type hierarchy for structs. The reason for this is that content hierarchies are ultimately implemented as containment. For example with the following struct hierarchy in C++,
```c
struct A {
	int x;
	float y;
};

struct B : A {
	long z;
};
```
the ultimate structure of `B` is
```c
struct B {
	int x;
	float y;
	long z;
};
```
This is identical to the structure of
```c
struct B {
	A a;
	long z;
};
```
with the exception of the specification that the first 8 bytes is explicitly treated as an `A` type. Note that this paradigm allows only one of the two: accessing values that would be in the `A` type directly (as is the case with the definition of `B` with `x`, `y`, and `z`), or having some fields clustered together in a sub-struct. In reality, if you have a legitimate reason to have a supertype with content, there's no reason that this data should not be clustered together to indicate it belongs to the supertype. As a result, Bynarie only uses composition to create complex data structures.

### Bynarie Struct Syntax (WIP)
In general, structs are defined as:
```
struct MyStruct(
	field1: Type1
	field2: Type2
	.
	.
	.
)
```
Whenever the number of fields is small, it may be preferable to use the single line syntax:
```
struct MyStruct(field1: Type1, field2: Type2)
```
As an example, 

## Casting

With a type hierarchy, a struct of type `B` can be "cast" into a struct of type `A`, which is possible by treating the size of the data in memory as being the size of `A` instead of the size of `B`. It may seem like forcing composition in structs would remove the ability to do this, but in fact casting ability  follows directly from accessing members of a struct directly. With structs defined as follows,
```
struct A (
	i32 x
	f32 y
)

struct B (
	A super
	i64 z
)
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzM0NTU2NDMyLC0xNTQ1MzcwMTY5XX0=
-->