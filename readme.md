TOC will be added in the final post, since github doesn't recognize [TOC] option...

This file is published as [bfilipek.com/C++17 features](http://www.bfilipek.com/2017/01/cpp17features.html) blog post.

##Intro

**Work in Progress!** I am happy to see your help with the list! :)

If you have code examples, better explanations or any ideas, let me know! I am happy to update the current post so that it has some real value for others.

The plan is to have a list of features with some basic explanation, little example (if possible) and some additional resources, plus a note about availability in compilers. Probably, most of the features might require separate articles or even whole chapters in books, so the list here will be only a jump start.

The list comes from the following resources:

* [SO: What are the new features in C++17?](http://stackoverflow.com/questions/38060436/what-are-the-new-features-in-c17)
* [cppreference.com/C++ compiler support](http://en.cppreference.com/w/cpp/compiler_support).
* [AnthonyCalandra/modern-cpp-features cheat sheet](https://github.com/AnthonyCalandra/modern-cpp-features) - unfortunately it doesn't include all the features of C++17, so this is also why I've started a separate try on that.
* plus other findings and mentions

##Language Features

###New auto rules for direct-list-initialization 
[N3922](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n3922.html)

| GCC: 5.0 | Clang: 3.8 | MSVC: 14.0 |
|---------:|------------|------------|

Fixes some cases with auto type deduction. The full background can be found in [Auto and braced-init-lists](http://open-std.org/JTC1/SC22/WG21/docs/papers/2013/n3681.html), by Ville Voutilainen.

It fixes the problem of deducing `std::initialize_list` like:

```cpp
auto x = foo(); // copy-initialization
auto x{foo}; // direct-initialization, initializes an initializer_list
int x = foo(); // copy-initialization
int x{foo}; // direct-initialization
```

And for the direct initialization, new rules are:

* For a braced-init-list with only a single element, auto deduction will deduce from that entry;
* For a braced-init-list with more than one element, auto deduction will be ill-formed.

Basically, `auto x { 1 };` will be now deduced as `int`, but before it was an initializer list.

###static_assert with no message 
[N3928](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n3928.pdf)

| GCC: 6.0 | Clang: 2.5 | MSVC: 15.0 preview 5|
|---------:|------------|------------|

Self-explanatory. It allows just to have the condition without passing the message, version with the message will also be available. It will be compatible with other asserts like `BOOST_STATIC_ASSERT` (that didn't take any message from the start).


###typename in a template template parameter 

[N4051](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4051.html)

| GCC: 5.0 | Clang: 3.5 | MSVC: 14.0 |
|---------:|------------|------------|

Allows you to use `typename` instead of `class` when declaring a template template parameter. Normal type parameters can use them interchangeably, but template template parameters were restricted to `class`, so this change unifies these forms somewhat.

``` cpp
template <template <typename...> typename Container>
//            used to be invalid ^^^^^^^^
struct foo;

foo<std::vector> my_foo;
```

###Removing trigraphs 
[N4086](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4086.html)

| GCC: 5.1 | Clang: 3.5 | MSVC: not yet |
|---------:|------------|------------|

Removes `??=`, `??(`, `??>`, ... 

Makes the implementation a bit simpler, see [MSDN Trigraphs](https://msdn.microsoft.com/en-us/library/bt0y4awe.aspx)

###Nested namespace definition 
[N4230](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4230.html)

| GCC: 6.0 | Clang: 3.6 | MSVC: 14.3 |
|---------:|------------|------------|

Allows to write:

``` cpp
namespace A::B::C {
   //…
}
```
Rather than:

```cpp
namespace A {
    namespace B {
        namespace C {
            //…
        }
    }
}
```

###Attributes for namespaces and enumerators 
[N4266](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4266.html)

| GCC: 4.9 (namespaces)/ 6 (enums) | Clang: 3.4 | MSVC: 14.0 |
|---------:|------------|------------|

todo...

###u8 character literals 
[N4267](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4267.html)

| GCC: 6.0 | Clang: 3.6 | MSVC: 14.0 |
|---------:|------------|------------|

`u8'U'` character literals.

update needed...

###Allow constant evaluation for all non-type template arguments 
[N4268](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4268.html) 

| GCC: 6.0 | Clang: 3.6 | MSVC: not yet |
|---------:|------------|------------|

todo...

###[Fold Expressions](http://en.cppreference.com/w/cpp/language/fold) 
[N4295](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4295.html)

| GCC: 6.0 | Clang: 3.6 | MSVC: not yet |
|---------:|------------|------------|

More background here in [P0036](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0036r0.pdf)

Articles:

* [C++ Truths: Folding Monadic Functions](http://cpptruths.blogspot.com/2017/01/folding-monadic-functions.html)

###Remove Deprecated Use of the register Keyword 
[P0001R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0001r1.html)

| GCC: 7.0 | Clang: 3.8 | MSVC: not yet |
|---------:|------------|------------|

The `register` keyword was deprecated in the 2011 C++ standard. C++17 tries to clear the standard, so the keyword is now removed.

###Remove Deprecated operator++(bool) 
[P0002R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0002r1.html)

| GCC: 7.0 | Clang: 3.8 | MSVC: not yet |
|---------:|------------|------------|

The ++ operator for `bool` was deprecated in the original 1998 C++ standard, and it is past time to remove it formally. 

###Removing Deprecated Exception Specifications from C++17 
[P0003R5](http://wg21.link/p0003r5)

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

Dynamic exception specifications were deprecated in C++11. This paper formally proposes removing the feature from C++17, while retaining the (still) deprecated `throw()` specification strictly as an alias for `noexcept(true)`.

###Make exception specifications part of the type system 

[P0012R1](http://wg21.link/p0012r1)

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

Previously exception specifications for a function didn't belong to the type of the function, but it will be part of it.

We'll get an error in the case:

```cpp
void (*p)() throw(int);
void (**pp)() noexcept = &p;   // error: cannot convert to pointer to noexcept function
```

###Aggregate initialization of classes with base classes 
[P0017R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0017r1.html)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

If a class was derived from some other type you couldn't use aggregate initialization. But now the restriction is removed.

```cpp
struct base { int a1, a2; };
struct derived : base { int b1; };

derived d1{{1, 2}, 3};      // full explicit initialization
derived d1{{}, 1};          // the base is value initialized
```

To sum up: from the standard:

> An aggregate is an array or a class with:
* no user-provided constructors (including those inherited from a base class),
* no private or protected non-static data members (Clause 11),
* no base classes (Clause 10) and // removed now!
* no virtual functions (10.3), and
* no virtual, private or protected base classes (10.1). 

###Lambda capture of *this 
[P0018R3](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0018r3.html)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

`this` pointer is implicitly captured by lambdas inside member functions. Sometimes, it's useful to capture `this` by value. Such change can reduce problems in multithreading.

###Using attribute namespaces without repetition 
[P0028R4](http://wg21.link/p0028r4)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

todo...

###Dynamic memory allocation for over-aligned data 
[P0035R4](http://wg21.link/p0035r4)

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

todo...

###Unary fold expressions and empty parameter packs 
[P0036R0](http://wg21.link/p0036r0) 

| GCC: 6.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

todo...

###[__has_include](http://en.cppreference.com/w/cpp/preprocessor/include) in preprocessor conditionals 
[P0061R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0061r1.html)

| GCC: 5.0 | Clang: yes | MSVC: not yet |
|---------:|------------|------------|

todo...

###Template argument deduction for class templates 
[P0091R3](http://wg21.link/p0091r3)

| GCC: 7.0 | Clang: not yet | MSVC: not yet |
|---------:|------------|------------|

todo...

###Non-type template parameters with auto type 
[P0127R2](http://wg21.link/p0127r2)

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

todo...

###Guaranteed copy elision 
[P0135R1](http://wg21.link/p0135r1)


| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

Articles:

* [Jonas Devlieghere: Guaranteed Copy Elision](https://jonasdevlieghere.com/guaranteed-copy-elision/)

###New specification for inheriting constructors (DR1941 et al) 
[P0136R1](http://wg21.link/p0136r1)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

todo...

###Direct-list-initialization of enumerations 
[P0138R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0138r2.pdf)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

Allows to initialize enum class with a fixed underlying type:

```cpp
enum class Handle : uint32_t { Invalid = 0 };
Handle h { 42 }; // OK
```
Allows to create 'strong types' that are easy to use...

###Stricter expression evaluation order 
[P0145R3](http://wg21.link/p0145r3)

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

todo...

###constexpr lambda expressions 
[P0170R1](http://wg21.link/p0170r1)


| GCC: 7.0 | Clang: not yet | MSVC: not yet |
|---------:|------------|------------|


todo...

###Differing begin and end types in range-based for 
[P0184R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0184r0.html) 

| GCC: 6.0 | Clang: 3.6 | MSVC: 15.0 Preview 5 |
|---------:|------------|------------|

Changing the definition of range based for from:

```cpp
{
   auto && __range = for-range-initializer;
   for ( auto __begin = begin-expr,
              __end = end-expr;
              __begin != __end;
              ++__begin ) {
        for-range-declaration = *__begin;
        statement
   }
}
```

Into:

```cpp
{
  auto && __range = for-range-initializer;
  auto __begin = begin-expr;
  auto __end = end-expr;
  for ( ; __begin != __end; ++__begin ) {
    for-range-declaration = *__begin;
    statement
  }
}
```

Types of `__begin` and `__end` might be different; only the comparison operator is required. This little change allows Range TS users a better experience.

###[[fallthrough]] attribute 

[P0188R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0188r1.pdf) 

| GCC: 7.0 | Clang: 3.9 | MSVC: 15.0 Preview 4 |
|---------:|------------|------------|

Indicates that a fallthrough in a switch statement is intentional and a warning should not be issued for it. More details in [P0068R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0068r0.pdf).

```cpp
switch (c) {
case 'a':
    f(); // Warning emitted, fallthrough is perhaps a programmer error
case 'b':
    g();
[[fallthrough]]; // Warning suppressed, fallthrough is intentional
case 'c':
    h();
}
```

###[[nodiscard]] attribute 

[P0189R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0189r1.pdf)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

`[[nodiscard]]` is used to stress that the return value of a function is not to be discarded, on pain of a compiler warning. More details in [P0068R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0068r0.pdf).

```cpp
[[nodiscard]] int foo();
void bar() {
    foo(); // Warning emitted, return value of a nodiscard function is discarded
}
```

This attribute can also be applied to types in order to mark all functions which return that type as `[[nodiscard]]`:

```cpp
[[nodiscard]] struct DoNotThrowMeAway{};
DoNotThrowMeAway i_promise();
void oops() {
    i_promise(); // Warning emitted, return value of a nodiscard function is discarded
}
```

###[[maybe_unused]] attribute 

[P0212R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0212r1.pdf)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

Suppresses compiler warnings about unused entities when they are declared with `[[maybe_unused]]`. More details in [P0068R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0068r0.pdf).

```cpp
                 static void impl1() { ... } // Compilers may warn about this
[[maybe_unused]] static void impl2() { ... } // Warning suppressed


void foo() {
                      int x = 42; // Compilers may warn about this
     [[maybe_unused]] int y = 42; // Warning suppressed
}
```




###Ignore unknown attributes 

[P0283R2](http://wg21.link/p0283r2) 

| GCC: Yes | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

Clarifies that implementations should ignore any attribute namespaces which they do not support, as this used to be unspecified. More details in [P0283R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0283r1.pdf).

```cpp
//compilers which don't support MyCompilerSpecificNamespace will ignore this attribute
[[MyCompilerSpecificNamespace::do_special_thing]] 
void foo();
```

###Pack expansions in using-declarations 

[P0195R2](http://wg21.link/p0195r2) 

| GCC: not yet | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

Allows you to inject names with *using-declarations* from all types in a parameter pack.

In order to expose `operator()` from all base classes in a variadic template, we used to have to resort to recursion:

```cpp
template <typename T, typename... Ts>
struct Overloader : T, Overloader<Ts...> {
    using T::operator();
    using Overloader<Ts...>::operator();
    // […]
};

template <typename T> struct Overloader<T> : T {
    using T::operator();
};
```

Now we can simply expand the parameter pack in the *using-declaration*:

```cpp
template <typename... Ts>
struct Overloader : Ts... {
    using Ts::operator()...;
    // […]
};
```

### Decomposition declarations

[P0217R3](http://wg21.link/p0217r3)

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

Helps when using tuples as a return type. It will automatically create variables and `tie` them. More details in [P0144R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0144r0.pdf). Was originally called "structured bindings".

For example:

```cpp
std::tie(a, b, c) = tuple; // a, b, c need to be declared first
```

Now we can write:

```cpp
auto [ a, b, c ] = tuple;
```

Such expressions also work on structs, pairs, and arrays.

Articles:

* [Steve Lorimer, C++17 Structured Bindings](https://skebanga.github.io/structured-bindings/)
* [jrb-programming, Emulating C++17 Structured Bindings in C++14](http://jrb-programming.blogspot.fr/2016/02/emulating-c17-structured-binding-in-c14.html)
* [Simon Brand, Adding C++17 decomposition declaration support to your classes](https://tartanllama.github.io/c++/2016/07/20/structured-bindings/)

###Hexadecimal floating-point literals 

[P0245R1](http://wg21.link/p0245r1)

| GCC: 3.0 | Clang: Yes | MSVC: not yet |
|---------:|------------|------------|

Allows to express some special floating point values, for example, the smallest normal IEEE-754 single precision value is readily written as `0x1.0p-126`.

###init-statements for if and switch 

[P0305R1](http://wg21.link/p0305r1)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

New versions of the if and switch statements for C++: `if (init; condition)` and `switch (init; condition)`. 

This should simplify the code. For example, previously you had to write:

```cpp
{   
    auto val = GetValue();   
    if (val)    
        // on success  
    else   
        // on false... 
}
```

Look, that `val` has a separate scope, without it it will 'leak'.

Now you can write:

```cpp 
if (auto val = GetValue(); val)    
    // on success  
else   
    // on false... 
```

`val` is visible only inside the `if` and `else` statements, so it doesn't 'leak'.

###Inline variables 

[P0386R2](http://wg21.link/p0386r2)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

Previously only methods/functions could be specified as `inline`; now you can do the same with variables.


###DR: Matching of template template-arguments excludes compatible templates 

[P0522R0](http://wg21.link/p0522r0) 

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

todo...

###[std::uncaught_exceptions()](http://en.cppreference.com/w/cpp/error/uncaught_exception) 

[N4259](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4259)

| GCC: 6.0 | Clang: 3.7 | MSVC: 14.0 |
|---------:|------------|------------|

More background in the original paper: [PDF: N4152](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4152) and [GOTW issue 47: Uncaught Exceptions](http://www.gotw.ca/gotw/047.htm).

The function returns the number of uncaught exception objects in the current thread.

###`constexpr` if-statements  

[P0292R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0292r2.html) 

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

The static-if for C++!

```cpp
if constexpr(cond)
     statement1;
else
     statement2;
```

Articles:

* [LoopPerfect Blog, C++17 vs C++14 - Round 1 - if-constexpr](https://www.loopperfect.com/blog/c++-before-and-after-constexpr-if/)
* [SO: constexpr if and static_assert](http://stackoverflow.com/questions/38304847/constexpr-if-and-static-assert)

##Library Features

###Merged: The Parallelism TS, a.k.a. “Parallel STL.”, 

[P0024R2](http://isocpp.org/files/papers/P0024R2.html)

###Merged: The Library Fundamentals 1 TS (most parts)

 [P0220R1](https://isocpp.org/files/papers/p0220r1.html)

###Merged: File System TS, 

[P0218R1](https://isocpp.org/files/papers/P0218r1.html)

###Merged: The Mathematical Special Functions IS, 

[PDF - WG21 P0226R1](https://isocpp.org/files/papers/P0226R1.pdf)

###Improving [std::pair](http://en.cppreference.com/w/cpp/utility/pair) and [std::tuple](http://en.cppreference.com/w/cpp/utility/tuple) 

[N4387](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4387.html) 

###[std::shared_mutex](http://en.cppreference.com/w/cpp/thread/shared_mutex) (untimed) 

[N4508](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4508.html)

