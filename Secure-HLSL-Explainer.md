# Secure HLSL

Secure HLSL is a dialect of HLSL designed to be suitable for web applications. It is source-compatible with most, but not all, existing HLSL code. Secure HLSL removes support for some little-used HLSL features, and adds features aimed at GPGPU applications.

## Goals

- Secure HLSL aspires to provide the same, or stronger, security guarantees than are currently employed by the Web Platform.
- Secure HLSL aspires to be source-compatible with almost all existing HLSL shaders.
- Secure HLSL aspires to be expressive enough to make porting existing CPU applications to the GPU easier.
- Secure HLSL aspires to be as portable as possible, requiring the highest possible level of consistency of behavior across any number of GPUs, Operating Systems, and platform graphics APIs.
- Secure HLSL aspires to enhance HLSL by exposing useful language features that are available in other programming languages.
- Secure HLSL aspires to be as performant as possible within the constraints of the above goals.

## Libraries

The following is a valid Secure HLSL pixel shader (and is also a valid HLSL pixel shader).

    float4 main() : SV_TARGET {
        return float4(1.0f, 1.0f, 1.0f, 1.0f);
    }

When such a shader is supplied to the target API, additional information is supplied to annotate that this shader is intended to be a pixel shader.

In addition, the following is a valid Secure HLSL library (but not a valid HLSL shader).

    vertex float4 main(float4 pos : POSITION) : SV_POSITION {
        return pos;
    }
    pixel float4 main() : SV_TARGET {
        return float4(1.0f, 1.0f, 1.0f, 1.0f);
    }

When such a library is supplied to the target API, no additional information is supplied to annotate which shader stage each function is used with. Instead, this information is described in the source code itself, with the initial token `vertex`,  `pixel`, or `compute`. Therefore, fragment shaders and pixel shaders may share code. Secure HLSL only supports vertex shaders, pixel shaders, and compute shaders; no other shader types are representable in Secure HLSL.

### Preprocessor

The `#include` preprocessor directive in HLSL is not present in Secure HLSL because no direct filesystem access is available on the Web. However, the [remaining preprocessor directives](https://msdn.microsoft.com/en-us/library/bb943993(v=vs.85).aspx) available in HLSL are also available in Secure HLSL.

## Types

Secure HLSL supports the [types available to HLSL](https://msdn.microsoft.com/en-us/library/bb509587(v=vs.85).aspx), with some exceptions described below.

### Memory Spaces

All variables in Secure HLSL are associated with a memory space. Secure HLSL includes four memory spaces:

- `thread`. By default, local variables are associated with this memory space.
- `constant`. Variables inside a `Buffer` or `StructuredBuffer` object, or a `cbuffer` or `tbuffer` block are associated with this memory space.
- `device`. Variables inside a `RWBuffer` or a `RWStructuredBuffer`, or a  object are associated with this memory space.
- `groupshared`. Local variables may be associated with this memory space instead of the `thread` memory space. Variables associated with this memory space are shared across all the threads in a thread group.

### Built-In Types

In Secure HLSL, textures and samplers are never combined into a single object. The `texture` HLSL type is not present in Secure HLSL, nor is the `sampler` HLSL type present in Secure HLSL.

The following HLSL types are not present in Secure HLSL:

- `StreamOutputObject`
- `AppendStructuredBuffer`
- `ConsumeStructuredBuffer`
- `ByteAddressBuffer`
- `RWByteAddressBuffer`
- strings

In addition to the above types, author-specified classes and interfaces are also not present in Secure HLSL. Instead, Secure HLSL has protocols (described below).

Buffer blocks may be used in Secure HLSL as they are in HLSL.

### Variables

All variables live for the lifetime of the program. This applies to all variables, even `thread` variables. This means that the allocation of every variable occurs before the program begins executing, and the deallocation of every variable occurs after the program completes executing. At the location of a variable's declaration in the source program, the storage associated with the variable will be zero-filled (or `null`-filled, depending on the type). Then, the variable's initializer will run, and the result of the initializer will be assigned into the storage associated with the variable.

This is possible because Secure HLSL does not support recursion. Therefore, local variables simply get global storage. Local variables are initialized at the point of their declaration. Hence, the following is a valid program, which will exhibit the same behavior on every Secure HLSL implementation:

    thread int* foo()
    {
        int x = 42;
        return &x;
    }
    int bar()
    {
        thread int* p = foo();
        thread int* q = foo();
        *p = 53;
        return *q; // Returns 53.
    }
    int baz()
    {
        thread int* p = foo();
        ^p = 53;
        foo();
        return ^p; // Returns 42.
    }

All shader stage outputs are initialized to 0 before the shader stage begins executing. A variable is considered a shader stage output if it has an associated semantic and a write to this semantic occurs anywhere within the possible execution paths within the shader stage's entry point.

### Safe Pointers

In addition to the types already supported by HLSL, Secure HLSL supports safe pointers. A safe pointer is similar to a pointer in C or C++, with the restriction that it will always point to a valid object, or it will point to `null`. In this context, a "valid" object is an object which may be referred to without using pointers. The type of a pointer includes the memory space of the variable to which it is pointing.

    int x;
    thread int* y = &x;

Pointers may be nested arbitrarily deeply:

    struct Foo {
        int z;
    }
    RWStructuredBuffer<Foo> theFoos : register(u0);
    float4 main() : SV_TARGET {
        device Foo* pointerToTheInterestingFoo = &theFoos[17];
        device int* pointerToTheInterestingZ = &((*pointerToTheInterestingFoo).z);
        thread device int** pointerToThePointerToTheInterestingZ = &pointerToTheInterestingZ;
        ...
    }

Any pointer may also point to `null`, and all pointers are initialized to `null` in their declaration.

    thread int* x; // x points to null.
    device float* y = null; // y also points to null.

### Type-Safe Array References

Similar to how `Buffer`, `StructuredBuffer`, `RWBuffer` and `RWStructuredBuffer` exist for `device` and `constant` variables, an array reference type exists for `local` and `groupshared` variables. An array reference is similar to an array, except the length of the array is not known at compile-time. At runtime, array references carry a pointer to the base of the array and the array's length. This allows accesses to the array to be bounds-checked.

An array reference can be created using the `@` operator:

    int array[42];
    thread int[] arrayRef = @array;

The length of an array reference may not be known at compile-time.

    Buffer<int> dataThatIsNotKnownAtCompileTime : register(t0);
    thread int[] figureOutWhichArrayReferenceToUse(thread int[] a, thread int[] b, int x) {
        if (dataThatIsNotKnownAtCompileTime[0] == x)
            return a;
        else
            return b;
    }
    float4 main() : SV_TARGET {
        int a[27];
        int b[52];
        thread int[] arrayReference = figureOutWhichArrayReferenceToUse(@a, @b, ...);
        // The length of arrayReference is not known at compile-time.
        ...
    }

Both arrays and array references can be loaded from and stored to using `operator[]`:

    int x = array[i];
    int y = arrayRef[i];

Both arrays and array references know their length:

    uint arrayLength = array.length;
    uint arrayRefLength = arrayRef.length;

Given an array or array reference, it's possible to get a pointer to one of its elements:

    thread int* ptr1 = &array[i];
    thread int* ptr2 = &arrayRef[i];

A pointer is like an array reference with one element. It's possible to perform this conversion:

    thread int* pointer = ...;
    thread int[] ref = @ptr1;
    ref[0] // Equivalent to *ptr1.
    ref.length // 0 if ptr1 was null, 1 if ptr1 was not null.

Similarly, using `@` on a non-pointer value results in a reference of length 1:

    int x;
    thread int[] ref = @x;
    ref[0] // Aliases x.
    ref.length // Returns 1.

It's not legal to use `@` on an array reference:

    thread int[] ref;
    thread int[][] refref = @ref; // Error!

## Security

### Safe Pointers

Pointers may not be compared, casted, incremented, or used as an input or output of any arithmetic computation. Therefore, the only way to create a pointer is to use the `&` operator (of a valid variable), use the keyword `null`, or omit an initializer for the pointer variable. This means that all pointers either point to `null` or point to a valid variable at the time they are created. Pointers are assignable, but only from another pointer of the same type.

Because variables live for the lifetime of the program, every pointer points to a valid variable at the time it is created, and it will always point to a valid variable. Because assigning to a pointer requires another valid pointer, all pointers will always either point to null or to a valid variable.

### Out-of-Bounds Accesses

If a program accesses an array, array reference, `Buffer`, `StructuredBuffer`, `RWBuffer`, `RWStructuredBuffer`, vector, or matrix out of bounds, the program must behave as if the following steps were followed:

1. All previous read and write operations of this thread must complete in the order they were submitted
2. All outputs of the current shader stage must be populated with 0s.
3. The shader stage immediately completes without performing any more writes
4. The next shader stage proceeds (as if the erroneous stage completed successfully)

Arrays, vectors, and matrices have lengths known at compile-time, so a compiler can emit bounds checks against immediate values. Array references, `Buffer`s, `StructuredBuffer`s, `RWBuffer`s, and `RWStructuredBuffer`s hold their length at runtime, so a compiler can emit bounds checks against these runtime values.

The above steps are also followed in the event of dereferencing a null pointer or accessing a null array reference (with any index).

## Generics

Secure HLSL supports generic types using a simple syntax. Secure HLSL's generics are designed to integrate cleanly into the compiler pipeline:

- Generics have unambiguous syntax.
- Generic functions can be type checked before they are instantiated.

Semantic errors inside generic functions show up once regardless of the number of times the generic function is instantiated.

This is a simple generic function:

    T identity<T>(T value)
    {
        T tmp = value;
        return tmp;
    }

Secure HLSL's structs are also allowed to be generic:

    // Not generic.
    struct Foo {
        int x;
        double y;
    }
    // Generic.
    struct Bar<T, U> {
        T x;
        U y;
    }

`typedef` can be used to create generic types:

    typedef Bar<T> = Foo<T, T>;

Type parameters can also be constant expressions. For example:

    void initializeArray<T, uint length>(thread vector<T, length>* array, T value)
    {
        for (uint i = length; i--;)
            (*array)[i] = value;
    }

Constant expressions passed as type arguments must obey a very narrow definition of constantness. Only literals and references to other constant parameters qualify.

Type parameters may also be specialized. For example,

    struct vector<T, uint length>;
    struct vector<T, 2> {
        T x;
        T y;
    }
    struct vector<T, 3> {
        T x;
        T y;
        T z;
    }
    ...
    vector<int, 2> foo; // foo has "x" and "y" members.
    vector<int, 17> bar; // Error! No specialization provided for "length" == 17.

Secure HLSL is guaranteed to compile generics by instantiation. This is observable, since functions can return pointers to their locals. Here is an example of this phenomenon:

    thread int^ allocate<uint>()
    {
        int x;
        return &x;
    }

## Protocols

Protocols enable generic functions to work with data of generic type. Because a function must be type-checkable before instantiation, the following would not be legal:

    int bar(int) { ... }
    double bar(double) { ... }
    
    T foo<T>(T value)
    {
        return bar(value); // Error!
    }

The call to `bar` doesn't type check because the compiler cannot know that `foo<T>` will be instantiated with `T = int` or `T = double`. Protocols enable the programmer to tell the compiler what to expect of a type variable:

    int bar(int) { ... }
    double bar(double) { ... }
    
    protocol SupportsBar {
        SupportsBar bar(SupportsBar);
    }
    
    T foo<T:SupportsBar>(T value)
    {
        return bar(value);
    }
    
    int x = foo(42);
    double y = foo(4.2);

Protocols have automatic relationships to one another based on what functions they contain:

    protocol Foo {
        void foo(Foo);
    }
    protocol Bar {
        void foo(Bar);
        void bar(Bar);
    }
    void fuzz<T:Foo>(T x) { ... }
    void buzz<T:Bar>(T x)
    {
        fuzz(x); // OK, because Bar is more specific than Foo.
    }

Protocols can also mix other protocols in explicitly. Like in the example above, the example below relies on the fact that `Bar` is a more specific protocol than `Foo`. However, this example declares this explicitly (`protocol Bar : Foo`) instead of relying on the language to infer it:

    protocol Foo {
        void foo(Foo);
    }
    protocol Bar : Foo {
        void bar(Bar);
    }
    void fuzz<T:Foo>(T x) { ... }
    void buzz<T:Bar>(T x)
    {
        fuzz(x); // OK, because Bar is more specific than Foo.
    }

## Operator Overloading

Many Secure HLSL operations desugar to calls to functions. Those functions are called *operator overloads* and are declared using syntax that involves the keyword `operator`. The following operations result in calls to operator overloads:

- Numerical operators (`+`, `-`, `*`, `/`, etc.).
- Increment and decrement (`++`, `--`).
- Casting (`type(value)`).
- Accessing values in arrays (`[]`, `[]=`, `&[]`).
- Accessing fields (`.field`, `.field=`, `&.field`).

Secure HLSL operator overloading is designed to synthesize many operators for you:

- Read-modify-write operators like `+=` are desugared to a load of the load value, the underlying operator (like `+`), and a store of the new value. It's not possible to override `+=`, `-=`, etc.
- `x++` and `++x` both call the same operator overload. `operator++` takes the old value and returns a new one; for example the built-in `++` for `int` could be written as: `int operator++(int value) { return value + 1; }`.
- `operator==` can be overloaded, but `!=` is automatically synthesized and cannot be overloaded.

Some operators and overloads are restricted:

- `!`, `&&`, and `||` are built-in operations on the `bool` type.
- Self-casts (`T(T)`) are always the identity function.
- Casts with no arguments (`T()`) always return the default value for that type. Every type has a default value (`0`, `null`, or the equivalent for each field).

Cast overloading allows for supporting conversions between types and for creating constructors for custom types. Here is an example of cast overloading being used to create a constructor:

    struct Complex<T> {
        T real;
        T imag;
    }
    operator<T> Complex<T>(T real, T imag)
    {
        Complex<T> result;
        result.real = real;
        result.imag = imag;
        return result;
    }
    
    Complex<float> i = Complex<float>(0, 1);

Secure HLSL supports accessor overloading as part of the operator overloading syntax. This gives the programmer broad powers. For example:

    struct Foo {
        int x;
        int y;
    }
    int operator.sum(Foo foo)
    {
        return foo.x + foo.y;
    }

It's possible to say `foo.sum` to call the `operator.sum` function. Both getters and setters can be provided:

    struct Foo {
        int value;
    }
    double operator.doubleValue(Foo value)
    {
        return double(value.value);
    }
    Foo operator.doubleValue=(Foo value, double doubleValue)
    {
        value.value = int(doubleValue);
        return value;
    }

Providing both getter and setter overloads makes `doubleValue` behave almost as if it was a field of `Foo`. For example, it's possible to say:

    Foo foo;
    foo.value = 42;
    foo.doubleValue *= 2; // Now foo.value is 84

It's also possible to provide an address-getting overload called an *ander*:

    struct Foo {
        int value;
    }
    thread int^ operator&.valueAlias(thread Foo^ foo)
    {
        return &foo->value;
    }

Providing just this overload for a pointer type in every address space gives the same power as overloading getters and setters. Additionally, it makes it possible to `&foo.valueAlias`.

The same overloading power is provided for array accesses. For example:

    struct Vector<T> {
        T x;
        T y;
    }
    thread T^ operator&[](thread T^ ptr, uint index)
    {
        return index ? &ptr->y : &ptr->x;
    }

Alternatively, it's possible to overload getters and setters (`operator[]` and `operator[]=`).

## Function Overload Resolution

Secure HLSL automatically selects the most specific overload if given multiple choices. For example:

    void foo(int);    // 1
    void foo(double); // 2
    void foo<T>(T);   // 3

    foo(1); // calls 1
    foo(1.); // calls 2
    foo(1u); // calls 3

Functions are ranked by how specific they are; function A is more specific than function B if A's parameter types can be used as argument types for a call to B but not vice-versa. If one function is more specific than all others, then this function is selected, otherwise a type error is issued.

## Binding Model

Secure HLSL's binding model matches HLSL's binding model as closely as possible. However, Secure HLSL is designed to work with a three-tier binding model, where a shader stage has access to a collection of resource sets, and each resource set may hold a collection of resources.

These concepts map to Direct3D's concept of descriptor tables inside the root descriptor. Therefore, the existing binding model of HLSL is used as if the collection of resource sets corresponds to the root descriptor, and a single resource set corresponds to a descriptor table.

## Concurrency

There are two situations where multiple GPU GPU threads associated with a single draw call may read and write the same data concurrently.

- UAV accesses
- Variables associated with the `groupshared` memory space

For both of these situations, the order of reads and writes across different threads is undefined. Within a single thread, the order of reads and writes is defined as the order they occur in the source program. Operations from multiple threads may be interleaved or concurrent.

The standard library includes barrier functions and atomic functions for order operations between threads.

## Logical Mode

Secure HLSL is designed to be run on all modern popular 3D graphics APIs existing today. However, some of these APIs have additional requirements that programs must conform to. As such, Secure HLSL includes a "Logical Mode" which conforms to the requirements of these languages. Any shader supplied to any WebGPU API must meet these Logical Mode restrictions. (As the platform APIs evolve and mature, our hope is that the presence of Logical Mode can be dropped from Secure HLSL, and future versions of WebGPU may be able to accept programs that do not meet these Logical Mode descriptions.)

The requirements of Logical Mode are:

- None of the following types may transitively hold pointers or array references:
    - `Buffer`
    - `StructuredBuffer`
    - `RWBuffer`
    - `RWStructuredBuffer`
    - Arrays
    - Array references
    - Vectors
    - Matrices
    - Buffer blocks (`cbuffer`, `tbuffer`)
- Pointers and array references must never be assigned to
- Functions whose return types transitively include a pointer or an array reference must only have a single return point
- Ternary expressions must not return a pointer or array reference

## Additional Limitations

The following additional limitations may be placed on a Secure HLSL program:

- `device`, `constant`,  and `groupshared` pointers cannot point to data that may have pointers in it. This safety check is not done as part of the normal type system checks. It's performed only after instantiation. This check is always performed for every shader.

## Standard Library

HLSL's intrinsic functions are included in Secure HLSL, with the following exceptions:

- If an argument to any built-in function is `NaN`, and the function returns a floating point value, the function returns the first `NaN` argument, duplicated out to any array cells if necessary, without any side effects. If the function returns a non-floating point value, it returns 0 or false or `null`, duplicated out to any array cells if necessary, without any side effects. 
- Denormalized floats must not exist. If a denormalized float may arise from some calculation or memory load, it is immediately flushed to 0 by the runtime.
- `clamp()`: if `min > max`, the result is `min`.
- The following functions must not be present inside divergent control flow, including loops:
    - `GroupMemoryBarrierWithGroupSync()`
    - `ddx()`
    - `ddy()`
- The following functions are not supported:
    - `abort()`
    - `CheckAccessFullyMapped()`
    - `errorf()`
    - `printf()`
