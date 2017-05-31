TOC will be added in the final post, since github doesn't recognize [TOC] option...

This file is published as [bfilipek.com/C++17 features](http://www.bfilipek.com/2017/01/cpp17features.html) blog post.

##Intro

The list is mostly done! Still some descriptions could be improved or more example could be provided.

If you have code examples, better explanations or any ideas, let me know! I am happy to update the current post so that it has some real value for others.

The plan is to have a list of features with some basic explanation, little example (if possible) and some additional resources, plus a note about availability in compilers. Probably, most of the features might require separate articles or even whole chapters in books, so the list here will be only a jump start.

The list comes from the following resources:

* [SO: What are the new features in C++17?](http://stackoverflow.com/questions/38060436/what-are-the-new-features-in-c17)
* [cppreference.com/C++ compiler support](http://en.cppreference.com/w/cpp/compiler_support).
* [AnthonyCalandra/modern-cpp-features cheat sheet](https://github.com/AnthonyCalandra/modern-cpp-features) - unfortunately it doesn't include all the features of C++17, so this is also why I've started a separate try on that.
* plus other findings and mentions

And one of the most important resource: [Working Draft, Standard for Programming Language C++](http://open-std.org/JTC1/SC22/WG21/docs/papers/2016/n4618.pdf)

Plus there's an official list of changes: [P0636r0: Changes between C++14 and C++17 DIS](https://isocpp.org/files/papers/p0636r0.html)

##Language Features

###New auto rules for direct-list-initialization 
[N3922](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n3922.html)

| GCC: 5.0 | Clang: 3.8 | MSVC: 14.0 |
|---------:|------------|------------|

Fixes some cases with auto type deduction. The full background can be found in [Auto and braced-init-lists](http://open-std.org/JTC1/SC22/WG21/docs/papers/2013/n3681.html), by Ville Voutilainen.

It fixes the problem of deducing `std::initializer_list` like:

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

| GCC: 5.1 | Clang: 3.5 | MSVC: Yes |
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

Permits attributes on enumerators and namespaces. More details in [N4196](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4196.html).

```cpp
enum E {
  foobar = 0,
  foobat [[deprecated]] = foobar
};

E e = foobat; // Emits warning

namespace [[deprecated]] old_stuff{
    void legacy();
}

old_stuff::legacy(); // Emits warning
```

###u8 character literals 
[N4267](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4267.html)

| GCC: 6.0 | Clang: 3.6 | MSVC: 14.0 |
|---------:|------------|------------|

> UTF-8 character literal, e.g. `u8'a'`. Such literal has type `char` and the value equal to ISO 10646 code point value of c-char, provided that the code point value is representable with a single UTF-8 code unit. If c-char is not in Basic Latin or C0 Controls Unicode block, the program is ill-formed.

The compiler will report errors if character cannot fit inside `u8` ASCII range.

Reference:

* [cppreference.com/character literal](http://en.cppreference.com/w/cpp/language/character_literal)
* [SO: What is the point of the UTF-8 character literals proposed for C++17?](http://stackoverflow.com/questions/31970111/what-is-the-point-of-the-utf-8-character-literals-proposed-for-c17)

###Allow constant evaluation for all non-type template arguments 
[N4268](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4268.html) 

| GCC: 6.0 | Clang: 3.6 | MSVC: not yet |
|---------:|------------|------------|

Remove syntactic restrictions for pointers, references, and pointers to members that appears as non-type template parameters:

For instance:

```cpp
template<int *p> struct A {};
int n;
A<&n> a; // ok

constexpr int *p() { return &n; }
A<p()> b; // error before C++17
```

###[Fold Expressions](http://en.cppreference.com/w/cpp/language/fold) 
[N4295](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4295.html)

| GCC: 6.0 | Clang: 3.6 | MSVC: not yet |
|---------:|------------|------------|

More background here in [P0036](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0036r0.pdf)

Allows to write compact code with variadic templates without using explicit recursion.

Example:

```cpp
template<typename... Args>
auto SumWithOne(Args... args){
    return (1 + ... + args);
}
```

Articles:

* [C++ Truths: Folding Monadic Functions](http://cpptruths.blogspot.com/2017/01/folding-monadic-functions.html)
* [Simon Brand: Exploding tuples with fold expressions](https://tartanllama.github.io/c++/2016/11/10/exploding-tuples-fold-expressions/)
* [Baptiste Wicht: C++17 Fold Expressions](http://baptiste-wicht.com/posts/2015/05/cpp17-fold-expressions.html)
* [Fold Expressions - ModernesCpp.com](http://www.modernescpp.com/index.php/fold-expressions)

###Unary fold expressions and empty parameter packs 
[P0036R0](http://wg21.link/p0036r0) 

| GCC: 6.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

If the parameter pack is empty then the value of the fold is:

| Operator | Value |
|---------:|------------|
| &&	| true |
| \|\|	| false |
| ,	| void() |

For any operator not listed above, an unary fold expression with an empty parameter pack is ill-formed.

###Remove Deprecated Use of the register Keyword 
[P0001R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0001r1.html)

| GCC: 7.0 | Clang: 3.8 | MSVC: not yet |
|---------:|------------|------------|

The `register` keyword was deprecated in the 2011 C++ standard. C++17 tries to clear the standard, so the keyword is now removed. This keyword is reserved now and might be repurposed in the future revisions. 

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
void (*p)();
void (**pp)() noexcept = &p;   // error: cannot convert to pointer to noexcept function

struct S { typedef void (*p)(); operator p(); };
void (*q)() noexcept = S();   // error: cannot convert to pointer to noexcept function
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

`this` pointer is implicitly captured by lambdas inside member functions (if you use a default capture, like `[&]` or `[=]`). Member variables are always accessed by this pointer.

Example:
```cpp
struct S {
   int x ;
   void f() {
      // The following lambda captures are currently identical
      auto a = [&]() { x = 42 ; } // OK: transformed to (*this).x
      auto b = [=]() { x = 43 ; } // OK: transformed to (*this).x
      a();
      assert( x == 42 );
      b();
      assert( x == 43 );
   }
};
```

Now you can use `*this` when declaring a lambda, for example `auto b = [=, *this]() { x = 43 ; }`. That way `this` is captured by value. Note that the form [&,this] is redundant but accepted for compatibility with ISO C++14.

Capturing by value might be especially important for async invocation, paraller processing.

###Using attribute namespaces without repetition 
[P0028R4](http://wg21.link/p0028r4)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

Other name for this feature was "Using non-standard attributes" in [P0028R3](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0028r3.html) and [PDF: P0028R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0028r2.pdf) (rationale, examples).

Simplifies the case where you want to use multiple attributes, like:

```cpp
void f() {
    [[rpr::kernel, rpr::target(cpu,gpu)]] // repetition
    do-task();
}
```
Proposed change:

```cpp
void f() {
    [[using rpr: kernel, target(cpu,gpu)]]
    do-task();
}
```

That simplification might help when building tools that automatically translate annotated such code into a different programming models.

###Dynamic memory allocation for over-aligned data 
[P0035R4](http://wg21.link/p0035r4)

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

In the following example:

```cpp
class alignas(16) float4 {
	float f[4];
};
float4 *p = new float4[1000];
```

C++11/14 did not specify any mechanism by which over-aligned data can be dynamically allocated correctly (i.e. respecting the alignment of the data). In the example above, not only is an implementation of C++ not required to allocate properly-aligned memory for the array, for practical purposes it is very nearly required to do the allocation incorrectly.

C++17 fixes that hole by introducing additional memory allocation functions that use align parameter:

```cpp
void* operator new(std::size_t, std::align_val_t);
void* operator new[](std::size_t, std::align_val_t);
void operator delete(void*, std::align_val_t);
void operator delete[](void*, std::align_val_t);
void operator delete(void*, std::size_t, std::align_val_t);
void operator delete[](void*, std::size_t, std::align_val_t);
```

###[__has_include](http://en.cppreference.com/w/cpp/preprocessor/include) in preprocessor conditionals 
[P0061R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0061r1.html)

| GCC: 5.0 | Clang: yes | MSVC: not yet |
|---------:|------------|------------|

This feature allows a C++ program to directly, reliably and portably determine whether or not a library header is available for inclusion.

Example: This demonstrates a way to use a library optional facility only if it is available.

```cpp
#if __has_include(<optional>)
#  include <optional>
#  define have_optional 1
#elif __has_include(<experimental/optional>)
#  include <experimental/optional>
#  define have_optional 1
#  define experimental_optional 1
#else
#  define have_optional 0
#endif
```

###Template argument deduction for class templates 
[P0091R3](http://wg21.link/p0091r3)

| GCC: 7.0 | Clang: not yet | MSVC: not yet |
|---------:|------------|------------|

Before C++17, template deduction worked for functions but not for classes.
For instance, the following code was legal:

```cpp
void f(std::pair<int, char>);

f(std::make_pair(42, 'z'));
```

because `std::make_pair` is a _template function_ (so we can perform teplate deduction).
But the following wasn't:

```cpp
void f(std::pair<int, char>);

f(std::pair(42, 'z'));
```

Although it is semantically equivalent. This was not legal because `std::pair` is a _template class_, and template classes could not apply type deduction in their initialization.

So before C++17 one has to write out the types explicitly, even though this does not add any new information:

```cpp
void f(std::pair<int, char>);

f(std::pair<int, char>(42, 'z'));
```

This is fixed in C++17 where template class constructors can deduce type parameters. The syntax for constructing such template classes is therefore consistent with the syntax for constructing non-template classes.

todo: deduction guides.

- [A 4 min episode of C++ Weekly on class template argument type deduction](https://www.youtube.com/watch?v=dEBQL4KPSk8)

- [A 4 min episode of C++ Weekly on deduction guides](https://www.youtube.com/watch?v=-3fVp0U4xi0)

###Non-type template parameters with auto type 
[P0127R2](http://wg21.link/p0127r2)

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

Automatically deduce type on non-type template parameters.

```cpp
template <auto value> void f() { }
f<10>();               // deduces int
```

[Trip report: Summer ISO C++ standards meeting (Oulu) | Sutter’s Mill](https://herbsutter.com/2016/06/30/trip-report-summer-iso-c-standards-meeting-oulu/)

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

More description and reasoning in [P0136R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0136r0.html). Some excerpts below:

An inheriting constructor does not act like any other form of using-declaration. All other using-declarations make some set of declarations visible to name lookup in another context, but an inheriting constructor declaration declares a new constructor  that merely delegates to the original.

This feature changes inheriting constructor declaration from declaring a set of new constructors, to making a set of base class constructors visible in a derived class as if they were derived class constructors. (When such a constructor is used, the additional derived class subobjects will also be implicitly constructed as if by a defaulted default constructor). Put another way: make inheriting a constructor act just like inheriting any other base class member, to the extent possible.

This change does affect the meaning and validity of some programs, but these changes improve the consistency and comprehensibility of C++. 

```cpp
// Hiding works the same as for other member 
// using-declarations in the presence of default arguments 
struct A {
  A(int a, int b = 0);
  void f(int a, int b = 0);
};
struct B : A {
  B(int a);      using A::A;
  void f(int a); using A::f;
};
struct C : A {
  C(int a, int b = 0);      using A::A;
  void f(int a, int b = 0); using A::f;
};

B b(0); // was ok, now ambiguous
b.f(0); // ambiguous (unchanged)

C c(0); // was ambiguous, now ok
c.f(0); // ok (unchanged)

```

```cpp
// Inheriting constructor parameters are no longer copied
struct A { A(const A&) = delete; A(int); };
struct B { B(A); void f(A); };
struct C : B { using B::B; using B::f; };
C c({0}); // was ill-formed, now ok (no copy made)
c.f({0}); // ok (unchanged)
```

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

In a nutshell, given an expression such as `f(a, b, c)`, the order in which the sub-expressions f, a, b, c (which are of arbitrary shapes) are evaluated is left unspecified by the standard.

```cpp
// unspecified behaviour below!
f(i++, i);

v[i] = i++; 

std::map<int, int> m;
m[0] = m.size(); // {{0, 0}} or {{0, 1}} ?
```

Summary of changes:

* Postfix  expressions  are  evaluated  from  left  to  right. This  includes  functions  calls  and  member selection expressions.
* Assignment expressions are evaluated from right to left. This includes compound assignments.
* Operands to shift operators are evaluated from left to right.

Reference:

* [C++ Order of evaluation, cppreference](http://en.cppreference.com/w/cpp/language/eval_order)
* [SO: What are the evaluation order guarantees introduced by C++17?](http://stackoverflow.com/questions/38501587/what-are-the-evaluation-order-guarantees-introduced-by-c17)
* [How compact code can become buggy code: getting caught by the order of evaluations, Fluent C++](http://www.fluentcpp.com/2017/05/09/compact-code-becomes-buggy-code-order-evaluations/)

###constexpr lambda expressions 
[P0170R1](http://wg21.link/p0170r1)


| GCC: 7.0 | Clang: not yet | MSVC: not yet |
|---------:|------------|------------|

consexpr can be used in the context of lambdas.

```cpp
constexpr auto ID = [] (int n)  { return n; };
constexpr int I = ID(3);
static_assert(I == 3);

constexpr int AddEleven(int n) {
  // Initialization of the 'data member' for n can
  // occur within a constant expression since 'n' is
  // of literal type.
  return [n] { return n + 11; }();
}
static_assert(AddEleven(5) == 16);
```

Articles
* [A 5 min episode of Jason Turner's C++ Weekly about constexpr lambdas](https://www.youtube.com/watch?v=kmza9U_niq4)
* [Lambda expression comparison between C++11, C++14 and C++17](https://maitesin.github.io/Lambda_comparison/)

###Different begin and end types in range-based for 
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
[A 4 min video about [[nodiscard]] in Jason Turner's C++ Weekly](https://www.youtube.com/watch?v=l_5PF3GQLKc)

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

[A 3 min video about [[maybe_unused]] in Jason Turner's C++ Weekly](https://www.youtube.com/watch?v=WSPmNL9834U)


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

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
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

Remarks

* Implemented in GCC 7.0, see [this change](https://github.com/gcc-mirror/gcc/commit/caba101ff33a763b444090b9c073bd84972ee552).

###Decomposition declarations

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
    if (condition(val))    
        // on success  
    else   
        // on false... 
}
```

Look, that `val` has a separate scope, without it it will 'leak'.

Now you can write:

```cpp 
if (auto val = GetValue(); condition(val))    
    // on success  
else   
    // on false... 
```

`val` is visible only inside the `if` and `else` statements, so it doesn't 'leak'.
`condition` might be any condition, not only if `val` is true/false.

Examples:

* [C++ Weekly - Ep 21 C++17's `if` and `switch` Init Statements](https://www.youtube.com/watch?v=AiXU5EuLZgc&feature=youtu.be)

###Inline variables 

[P0386R2](http://wg21.link/p0386r2)

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

Previously only methods/functions could be specified as `inline`, now you can do the same with variables, inside a header file.

> A variable declared inline has the same semantics as a function declared inline: it can be defined, identically, in multiple translation units, must be defined in every translation unit in which it is used, and the behavior of the program is as if there is exactly one variable.

```cpp
struct MyClass
{
    static const int sValue;
};

inline int const MyClass::sValue = 777;
```

Or even:

```cpp
struct MyClass
{
    inline static const int sValue = 777;
};
```

Articles

* [SO: What is an inline variable and what is it useful for?](http://stackoverflow.com/questions/38043442/what-is-an-inline-variable-and-what-is-it-useful-for)

###DR: Matching of template template-arguments excludes compatible templates 

[P0522R0](http://wg21.link/p0522r0) 

| GCC: 7.0 | Clang: 4.0 | MSVC: not yet |
|---------:|------------|------------|

This feature resolves [Core issue CWG 150](http://open-std.org/JTC1/SC22/WG21/docs/papers/2015/n4457.html#150).

From the paper:

> This paper allows a template template-parameter to bind to a template argument whenever the template parameter is at least as specialized as the template argument. This implies that any template argument list that can legitimately be applied to the template template-parameter is also applicable to the argument template.

Example:

```cpp
template <template <int> class> void FI();
template <template <auto> class> void FA();
template <auto> struct SA { /* ... */ };
template <int> struct SI { /* ... */ };
FI<SA>();  // OK; error before this paper
FA<SI>();  // error

template <template <typename> class> void FD();
template <typename, typename = int> struct SD { /* ... */ };
FD<SD>();  // OK; error before this paper (CWG 150)
```

(Adapted from the [comment](https://www.reddit.com/r/cpp/comments/5osck2/c_17_features/dcm7eq4/) by [IncongruentModulo1](https://www.reddit.com/user/IncongruentModulo1)) For a useful example, consider something like this:

```cpp
template <template <typename> typename Container>
struct A
{
    Container<int>    m_ints;
    Container<double> m_doubles;
};
```

In C++14 and earlier, `A<std::vector>` wouldn't be valid (ignoring the typename and not class before container) since `std::vector` is declared as:

`template <typename T, typename Allocator = std::allocator<T>>
class vector;`

This change resolves that issue. Before, you would need to declare template `<template <typename...> typename Container>`, which is more permissive and moves the error to a less explicit line (namely the declaration of `m_ints` wherever the `struct A` is implemented / declared, instead of where the struct is instantiated with the wrong template type.

###[std::uncaught_exceptions()](http://en.cppreference.com/w/cpp/error/uncaught_exception) 

[N4259](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4259)

| GCC: 6.0 | Clang: 3.7 | MSVC: 14.0 |
|---------:|------------|------------|

More background in the original paper: [PDF: N4152](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4152) and [GOTW issue 47: Uncaught Exceptions](http://www.gotw.ca/gotw/047.htm).

The function returns the number of uncaught exception objects in the current thread.

This might be useful when implementing proper Scope Guards that works also during stack unwinding.

> A type that wants to know whether its destructor is being run to unwind this object can query uncaught_exceptions
in its constructor and store the result, then query uncaught_exceptions again in its destructor; if the result is different, 
then this destructor is being invoked as part of stack unwinding due to a new exception that was thrown later than the object’s construction

The above quote comes from [PDF: N4152](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4152).

###`constexpr` if-statements  

[P0292R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0292r2.html) 

| GCC: 7.0 | Clang: 3.9 | MSVC: not yet |
|---------:|------------|------------|

The static-if for C++! This allows you to discard branches of an if statement at compile-time based on a constant expression condition.

```cpp
if constexpr(cond)
     statement1; // Discarded if cond is false
else
     statement2; // Discarded if cond is true
```

This removes a lot of the necessity for tag dispatching and SFINAE:

#### SFINAE
```cpp
template <typename T, std::enable_if_t<std::is_arithmetic<T>{}>* = nullptr>
auto get_value(T t) {/*...*/}

template <typename T, std::enable_if_t<!std::is_arithmetic<T>{}>* = nullptr>
auto get_value(T t) {/*...*/}
```

#### Tag dispatching
```cpp
template <typename T>
auto get_value(T t, std::true_type) {/*...*/}

template <typename T>
auto get_value(T t, std::false_type) {/*...*/}

template <typename T>
auto get_value(T t) {
    return get_value(t, std::is_arithmetic<T>{}); 
}
```

#### if constexpr
```cpp
template <typename T>
auto get_value(T t) {
     if constexpr (std::is_arithmetic_v<T>) {
         //...
     }
     else {
         //...
     }
}
```


Articles:

* [LoopPerfect Blog, C++17 vs C++14 - Round 1 - if-constexpr](https://www.loopperfect.com/blog/c++-before-and-after-constexpr-if/)
* [SO: constexpr if and static_assert](http://stackoverflow.com/questions/38304847/constexpr-if-and-static-assert)
* [Simon Brand: Simplifying templates and #ifdefs with if constexpr](https://tartanllama.github.io/c++/2016/12/12/if-constexpr/)

##Library Features

To get more details about library implementation I suggest those links:

* [VS 2015 Update 2’s STL is C++17-so-far Feature Complete](https://blogs.msdn.microsoft.com/vcblog/2016/01/22/vs-2015-update-2s-stl-is-c17-so-far-feature-complete/) - Jan 2016
* [libstdc++, C++ 201z status](https://gcc.gnu.org/onlinedocs/libstdc++/manual/status.html#status.iso.201z)
* [libc++ C++1z Status](http://libcxx.llvm.org/cxx1z_status.html)

This section only mentions some of the most important parts of library changes, it would be too impractical to go into details of every little change.

###Merged: The Library Fundamentals 1 TS (most parts)

[P0220R1](https://isocpp.org/files/papers/p0220r1.html)
 
We get the following items:

* Tuples - [Calling a function with a tuple of arguments](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#tuple.apply)
* Functional Objects - [Searchers](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#func.searchers)
* [Optional objects](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#optional)
* [Class any](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#any)
* [string_view](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#string.view)
* Memory:
 * [Shared-ownership   pointers](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.smartptr)
 * [Class   memory_resource](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource)
 * [Class   memory_resource](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource)
 * [Access   to program-wide memory_resource objects](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.global)
 * [Pool   resource classes](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.pool)
 * [Class   monotonic_buffer_resource](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.monotonic.buffer)
 * [Alias   templates using polymorphic memory resources](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.aliases)
* Algorithms: 
 * [Search](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#alg.search)
 * [Sampling](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#alg.random.sample)
* `shared_ptr` natively handles arrays: see [Merging shared_ptr changes from Library Fundamentals to C++17](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0414r2.html)

The wording from those components comes from Library Fundamentals V2 to ensure the wording includes the latest corrections.

Resources:

* [Marco Arena, string_view odi et amo](https://marcoarena.wordpress.com/2017/01/03/string_view-odi-et-amo/)

###Removal of some deprecated types and functions, including std::auto_ptr, std::random_shuffle, and old function adaptors

[N4190](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0001r1.html)

* Function objects - unary_function/binary_function, ptr_fun(), and mem_fun()/mem_fun_ref()
* Binders - bind1st()/bind2nd()
* auto_ptr
* Random shuffle - random_shuffle(first, last) and random_shuffle(first, last, rng)

###Merged: The Parallelism TS, a.k.a. “Parallel STL.”, 

[P0024R2](http://isocpp.org/files/papers/P0024R2.html)

Articles:

* [Parallel Algorithm of the Standard Template Library - ModernesCpp.com](http://www.modernescpp.com/index.php/parallel-algorithm-of-the-standard-template-library)

###Merged: File System TS, 

[P0218R1](https://isocpp.org/files/papers/P0218r1.html)

###Merged: The Mathematical Special Functions IS, 

[PDF - WG21 P0226R1](https://isocpp.org/files/papers/P0226R1.pdf)

###Improving [std::pair](http://en.cppreference.com/w/cpp/utility/pair) and [std::tuple](http://en.cppreference.com/w/cpp/utility/tuple) 

[N4387](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4387.html) 

###[std::shared_mutex](http://en.cppreference.com/w/cpp/thread/shared_mutex) (untimed) 

[N4508](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4508.html)

###Variant 

[P0088R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0088r2.html)

Variant is a typesafe union that will report errors when you want to access something that's not currently inside the object.

* [cppreference/variant](http://en.cppreference.com/w/cpp/utility/variant)
* [IsoCpp: The Variant Saga: A happy ending?](https://isocpp.org/blog/2015/11/the-variant-saga-a-happy-ending)

###Splicing Maps and Sets

[P0083R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0083r2.pdf)

From Herb Sutter, [Oulu trip report](https://herbsutter.com/2016/06/30/trip-report-summer-iso-c-standards-meeting-oulu/):

> You will now be able to directly move internal nodes from one node-based container directly into another container of the same type. Why is that important? Because it guarantees no memory allocation overhead, no copying of keys or values, and even no exceptions if the container’s comparison function doesn’t throw.
