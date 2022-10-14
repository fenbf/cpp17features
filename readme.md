# All Major C++17 Features You Should Know

The ISO Committee accepted and published the C++17 Standard in December 2017. In this mega-long article, I’ve built (with your help!) a list of **all major features** of the new standard.

Please have a look and see what we get!

-   [Language Features](#language-features)
    -   [New auto rules for direct-list-initialization](#new-auto-rules-for-direct-list-initialization)
    -   [static\_assert with no message](#staticassert-with-no-message)
    -   [typename in a template template parameter](#typename-in-a-template-template-parameter)
    -   [Removing trigraphs](#removing-trigraphs)
    -   [Nested namespace definition](#nested-namespace-definition)
    -   [Attributes for namespaces and enumerators](#attributes-for-namespaces-and-enumerators)
    -   [u8 character literals](#u8-character-literals)
    -   [Allow constant evaluation for all non-type template  arguments](#allow-constant-evaluation-for-all-non-type-template-arguments)
    -   [Fold Expressions](#fold-expressions)
    -   [Unary fold expressions and empty parameter packs](#unary-fold-expressions-and-empty-parameter-packs)
    -   [Remove Deprecated Use of the register Keyword](#remove-deprecated-use-of-the-register-keyword)
    -   [Remove Deprecated operator++(bool)](#remove-deprecated-operatorbool)
    -   [Removing Deprecated Exception Specifications from C++17](#removing-deprecated-exception-specifications-from-c17)
    -   [Make exception specifications part of the type system](#make-exception-specifications-part-of-the-type-system)
    -   [Aggregate initialization of classes with base classes](#aggregate-initialization-of-classes-with-base-classes)
    -   [Lambda capture of \*this](#lambda-capture-of-this)
    -   [Using attribute namespaces without repetition](#using-attribute-namespaces-without-repetition)
    -   [Dynamic memory allocation for over-aligned data](#dynamic-memory-allocation-for-over-aligned-data)
    -   [\_\_has\_include in preprocessor conditionals](#hasinclude-in-preprocessor-conditionals)
    -   [Template argument deduction for class templates](#template-argument-deduction-for-class-templates)
    -   [Non-type template parameters with auto type](#non-type-template-parameters-with-auto-type)
    -   [Guaranteed copy elision](#guaranteed-copy-elision)
    -   [New specification for inheriting constructors (DR1941 et al)](#new-specification-for-inheriting-constructors-dr1941-et-al)
    -   [Direct-list-initialization of enumerations](#direct-list-initialization-of-enumerations)
    -   [Stricter expression evaluation order](#stricter-expression-evaluation-order)
    -   [constexpr lambda expressions](#constexpr-lambda-expressions)
    -   [Different begin and end types in range-based for](#different-begin-and-end-types-in-range-based-for)
    -   [\[\[fallthrough\]\] attribute](#fallthrough-attribute)
    -   [\[\[nodiscard\]\] attribute](#nodiscard-attribute)
    -   [\[\[maybe\_unused\]\] attribute](#maybeunused-attribute)
    -   [Ignore unknown attributes](#ignore-unknown-attributes)
    -   [Pack expansions in using-declarations](#pack-expansions-in-using-declarations)
    -   [Structured Binding Declarations](#structured-binding-declarations)
    -   [Hexadecimal floating-point literals](#hexadecimal-floating-point-literals)
    -   [init-statements for if and switch](#init-statements-for-if-and-switch)
    -   [Inline variables](#inline-variables)
    -   [DR: Matching of template template-arguments excludes compatible templates](#dr-matching-of-template-template-arguments-excludes-compatible-templates)
    -   [`std::uncaught_exceptions()`](#stduncaughtexceptions)
    -   [constexpr if-statements](#constexpr-if-statements)
        -   [SFINAE](#sfinae)
        -   [Tag dispatching](#tag-dispatching)
        -   [if constexpr](#if-constexpr)
-   [Library Features](#library-features)
    -   [Merged: The Library Fundamentals 1 TS (most parts)](#merged-the-library-fundamentals-1-ts-most-parts)
    -   [Removal of some deprecated types and functions, including std::auto\_ptr, std::random\_shuffle, and old function adaptors](#removal-of-some-deprecated-types-and-functions-including-stdautoptr-stdrandomshuffle-and-old-function-adaptors)
    -   [Merged: The Parallelism TS, a.k.a. “Parallel STL.”,](#merged-the-parallelism-ts-aka-parallel-stl)
    -   [Merged: File System TS,](#merged-file-system-ts)
    -   [Merged: The Mathematical Special Functions IS,](#merged-the-mathematical-special-functions-is)
    -   [Improving std::pair and std::tuple](#improving-stdpair-and-stdtuple)
    -   [std::shared\_mutex (untimed)](#stdsharedmutex-untimed)
    -   [Variant](#variant)
    -   [Splicing Maps and Sets](#splicing-maps-and-sets)
    -   [Elementary string conversion](#elementary-string-conversion)
    -   [`std::apply`](#stdapply)
    -   [`std::invoke`](#stdinvoke)
-   [Contributors](#contributors)
-   [Summary](#summary)

Intro
-----

**Updated**: This post was updated on 10th October 2022.

If you have code examples, better explanations or any ideas, let me know! I am happy to update the current post so that it has some real value for others.

The plan is to have a list of features with some basic explanation, little example (if possible) and some additional resources, plus a note about availability in compilers. Probably, most of the features might require separate articles or even whole chapters in books, so the list here will be only a jump start.

**See this** GitHub repo: [github/fenbf/cpp17features](https://github.com/fenbf/cpp17features). Add a pull request to update the content.

The feature list comes from the following resources:

-   [SO: What are the new features in C++17?](http://stackoverflow.com/questions/38060436/what-are-the-new-features-in-c17)
-   [cppreference.com/C++ compiler support](http://en.cppreference.com/w/cpp/compiler_support).
-   [AnthonyCalandra/modern-cpp-features cheat sheet](https://github.com/AnthonyCalandra/modern-cpp-features) - unfortunately it doesn’t include all the features of C++17.
-   plus other findings and mentions

And one of the most important resource: [N4659, 2017-03-21, **Draft, Standard for Programming Language C++**](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/n4659.pdf) - from [isocpp.org](https://isocpp.org/).

Plus, there’s an official list of changes: [P0636r0: Changes between C++14 and C++17 DIS](https://isocpp.org/files/papers/p0636r0.html)

Also, you can grab my list of concise descriptions of all of the C++17 - It’s a one-page reference card:

I also have a more detailed series:

1.  [Fixes and deprecation](https://www.cppstories.com/2017/05/cpp17-details-fixes-deprecation.html)
2.  [Language clarification](https://www.cppstories.com/2017/06/cpp17-details-clarifications.html)
3.  [Templates](https://www.cppstories.com/2017/06/cpp17-details-templates.htm)
4.  [Attributes](https://www.cppstories.com/2017/07/cpp17-in-details-attributes.html)
5.  [Simplification](https://www.cppstories.com/2017/07/cpp17-details-simplifications.html)
6.  [Library changes - Filesystem](https://www.cppstories.com/2017/08/cpp17-details-filesystem.html)
7.  [Library changes - Parallel
    STL](https://www.cppstories.com/2017/08/cpp17-details-parallel.html)
8.  [Library changes - Utils](https://www.cppstories.com/2017/09/cpp17-details-utils.html)
9.  [Wrap up, Bonus](https://www.cppstories.com/2017/09/c17-in-detail-summary-bonus.html) -
    with a free ebook! :)

And another cool article: 

* [17 Smaller but Handy C++17 Features - C++ Stories](https://www.cppstories.com/2019/08/17smallercpp17features/)

[![](../../images/2017-01-09-c-17-features-cpp17indetail.png)](https://leanpub.com/cpp17indetail)

Resources about C++17 STL:

-   [**C++17 In Detail**](https://leanpub.com/cpp17) by Bartek!

## Language Features

### New auto rules for direct-list-initialization

[N3922](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n3922.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 5.0</th>
<th>Clang: 3.8</th>
<th>MSVC: 14.0</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Fixes some cases with auto type deduction. The full background can be found in [Auto and braced-init-lists](http://open-std.org/JTC1/SC22/WG21/docs/papers/2013/n3681.html), by Ville Voutilainen.

It fixes the problem of deducing `std::initializer_list` like:

```cpp
auto x = foo(); // copy-initialization
auto x{foo}; // direct-initialization, initializes an initializer_list
int x = foo(); // copy-initialization
int x{foo}; // direct-initialization
```

And for the direct initialization, new rules are:

-   For a braced-init-list with only a single element, auto deduction will deduce from that entry;
-   For a braced-init-list with more than one element, auto deduction will be ill-formed.

Basically, `auto x { 1 };` will be now deduced as `int`, but before it was an initializer list.

### `static_assert` with no message

[N3928](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n3928.pdf)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 6.0</th>
<th>Clang: 2.5</th>
<th>MSVC: 15.0 preview 5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Self-explanatory. It allows having the condition without passing the message, version with the message will also be available. It will be compatible with other asserts like `BOOST_STATIC_ASSERT` (that didn’t take any message from the start).

### `typename` in a `template template` parameter

[N4051](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4051.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 5.0</th>
<th>Clang: 3.5</th>
<th>MSVC: 14.0</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Allows you to use `typename` instead of `class` when declaring a template template parameter. Normal type parameters can use them interchangeably, but template template parameters were restricted to `class`, so this change unifies these forms somewhat.

```cpp
template <template <typename...> typename Container>
//            used to be invalid ^^^^^^^^
struct foo;

foo<std::vector> my_foo;
```    

### Removing trigraphs

[N4086](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4086.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 5.1</th>
<th>Clang: 3.5</th>
<th>MSVC: Yes</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Removes `??=`, `??(`, `??>`, ...

Makes the implementation a bit simpler, see [MSDN Trigraphs](https://msdn.microsoft.com/en-us/library/bt0y4awe.aspx)

### Nested namespace definition

[N4230](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4230.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 6.0</th>
<th>Clang: 3.6</th>
<th>MSVC: 14.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Allows writing:

```cpp
namespace A::B::C {
   // ...
}
```

Rather than:

```cpp
namespace A {
    namespace B {
        namespace C {
            // ...
        }
    }
}
```

### Attributes for namespaces and enumerators

[N4266](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4266.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 4.9 (namespaces)/ 6 (enums)</th>
<th>Clang: 3.4</th>
<th>MSVC: 14.0</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

### `u8` character literals

[N4267](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4267.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 6.0</th>
<th>Clang: 3.6</th>
<th>MSVC: 14.0</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

> UTF-8 character literal, e.g. `u8'a'`. Such literal has type `char` and the value equal to ISO 10646 code point value of c-char, provided that the code point value is representable with a single UTF-8 code unit. If c-char is not in Basic Latin or C0 Controls Unicode block, the program is ill-formed.

The compiler will report errors if a character cannot fit inside `u8` ASCII range.

Reference:

-   [cppreference.com/character literal](http://en.cppreference.com/w/cpp/language/character_literal)
-   [SO: What is the point of the UTF-8 character literals proposed for C++17?](http://stackoverflow.com/questions/31970111/what-is-the-point-of-the-utf-8-character-literals-proposed-for-c17)

### Allow constant evaluation for all non-type template arguments

[N4268](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4268.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 6.0</th>
<th>Clang: 3.6</th>
<th>MSVC: VS 2017 15.5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Remove syntactic restrictions for pointers, references, and pointers to members that appear as non-type template parameters:

For instance:

```cpp
template<int *p> struct A {};
int n;
A<&n> a; // ok

constexpr int *p() { return &n; }
A<p()> b; // error before C++17
```    

### Fold Expressions

[N4295](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4295.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 6.0</th>
<th>Clang: 3.6</th>
<th>MSVC: VS 2017 15.5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

More background here in [P0036](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0036r0.pdf)

Allows writing compact code with variadic templates without using explicit recursion.

Example:

```cpp
template<typename... Args>
auto SumWithOne(Args... args){
    return (1 + ... + args);
}
```    

Articles:

-   [Bartek’s coding blog: C++17 in details: Templates](https://www.cppstories.com/2017/06/cpp17-details-templates.html#fold-expressions)
-   [C++ Truths: Folding Monadic Functions](http://cpptruths.blogspot.com/2017/01/folding-monadic-functions.html)
-   [Simon Brand: Exploding tuples with fold expressions](https://tartanllama.github.io/c++/2016/11/10/exploding-tuples-fold-expressions/)
-   [Baptiste Wicht: C++17 Fold Expressions](http://baptiste-wicht.com/posts/2015/05/cpp17-fold-expressions.html)
-   [Fold Expressions - ModernesCpp.com](http://www.modernescpp.com/index.php/fold-expressions)

### Unary fold expressions and empty parameter packs

[P0036R0](http://wg21.link/p0036r0)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 6.0</th>
<th>Clang: 3.9</th>
<th>MSVC: VS 2017 15.5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

If the parameter pack is empty then the value of the fold is:

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th style="text-align: right;">Operator</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: right;">&amp;&amp;</td>
<td>true</td>
</tr>
<tr class="even">
<td style="text-align: right;">||</td>
<td>false</td>
</tr>
<tr class="odd">
<td style="text-align: right;">,</td>
<td>void()</td>
</tr>
</tbody>
</table>{{</rawhtml>}}

For any operator not listed above, an unary fold expression with an empty parameter pack is ill-formed.

### Remove Deprecated Use of the `register` Keyword

[P0001R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0001r1.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.8</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

The `register` keyword was deprecated in the 2011 C++ standard. C++17 tries to clear the standard, so the keyword is now removed. This keyword is reserved now and might be repurposed in future revisions.

### Remove Deprecated `operator++(bool)`

[P0002R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0002r1.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.8</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

The `++operator` for `bool` was deprecated in the original 1998 C++ standard, and it is past time to remove it formally.

### Removing Deprecated Exception Specifications from C++17

[P0003R5](http://wg21.link/p0003r5)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: VS 2017 15.0</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Dynamic exception specifications were deprecated in C++11. This paper formally proposes removing the feature from C++17, while retaining the (still) deprecated `throw()` specification strictly as an alias for `noexcept(true)`.

### Make exception specifications part of the type system

[P0012R1](http://wg21.link/p0012r1)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: VS 2017 15.5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Previously exception specifications for a function didn’t belong to the type of the function, but it will be part of it.

We’ll get an error in the case:

```cpp
void (*p)();
void (**pp)() noexcept = &p;   // error: cannot convert to pointer to noexcept function

struct S { typedef void (*p)(); operator p(); };
void (*q)() noexcept = S();   // error: cannot convert to pointer to noexcept function
```

### Aggregate initialization of classes with base classes

[P0017R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0017r1.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: VS 2017 15.7</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

If a class was derived from some other type you couldn’t use aggregate initialization. But now the restriction is removed.

```cpp
struct base { int a1, a2; };
struct derived : base { int b1; };

derived d1{{1, 2}, 3};      // full explicit initialization
derived d1{{}, 1};          // the base is value initialized
```

To sum up: from the standard:

> An aggregate is an array or a class with:  
> \* no user-provided constructors (including those inherited from a
> base class),  
> \* no private or protected non-static data members (Clause 11),  
> \* no base classes (Clause 10) and // removed now!  
> \* no virtual functions (10.3), and  
> \* no virtual, private or protected base classes (10.1).

See more in [Five tricky topics for data members in C++20 - C++ Stories](https://www.cppstories.com/2022/five-topics-data-members-cpp20/#1-changing-status-of-aggregates) - "Changing status of aggregates"

### Lambda capture of `*this`

[P0018R3](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0018r3.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

Now you can use `*this` when declaring a lambda, for example `auto b = [=, *this]() { x = 43 ; }`. That way `this` is captured by value. Note that the form `[&,this]` is redundant but accepted for compatibility with ISO C++14.

Capturing by value might be especially important for async invocation, paraller processing.

See more at [C++ Lambdas, Threads, std::async and Parallel Algorithms - C++ Stories](https://www.cppstories.com/2020/05/lambdas-async.html/#capturing-this)

### Using attribute namespaces without repetition

[P0028R4](http://wg21.link/p0028r4)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Other name for this feature was “Using non-standard attributes” in [P0028R3](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0028r3.html) and [PDF: P0028R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0028r2.pdf) (rationale, examples).

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

### Dynamic memory allocation for over-aligned data

[P0035R4](http://wg21.link/p0035r4)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: VS 2017 15.5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

See more in [New new() - The C++17's Alignment Parameter for Operator new() - C++ Stories](https://www.cppstories.com/2019/08/newnew-align/).

### `__has_include` in preprocessor conditionals

[P0061R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0061r1.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 5.0</th>
<th>Clang: yes</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

### Template argument deduction for class templates

[P0091R3](http://wg21.link/p0091r3)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 5</th>
<th>MSVC: VS 2017 15.7</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Before C++17, template deduction worked for functions but not for classes. For instance, the following code was legal:

```cpp
void f(std::pair<int, char>);

f(std::make_pair(42, 'z'));
```

because `std::make_pair` is a *template function* (so we can perform template deduction). But the following wasn’t:

```cpp
void f(std::pair<int, char>);

f(std::pair(42, 'z'));
```

Although it is semantically equivalent. This was not legal because `std::pair` is a *template class*, and template classes could not apply type deduction in their initialization.

So before C++17 one has to write out the types explicitly, even though this does not add any new information:

```cpp
void f(std::pair<int, char>);

f(std::pair<int, char>(42, 'z'));
```

This is fixed in C++17 where template class constructors can deduce type parameters. The syntax for constructing such template classes is therefore consistent with the syntax for constructing non-template classes.

See more in:

-   [Bartek’s coding blog: C++17 in details: Templates](https://www.cppstories.com/2017/06/cpp17-details-templates.html#template-argument-deduction-for-class-templates)
-   [A 4 min episode of C++ Weekly on class template argument type deduction](https://www.youtube.com/watch?v=dEBQL4KPSk8)
-   [A 4 min episode of C++ Weekly on deduction guides](https://www.youtube.com/watch?v=-3fVp0U4xi0)
-   [Modern C++ Features - Class Template Argument Deduction -](https://arne-mertz.de/2017/06/class-template-argument-deduction/)
-   [Deducing your intentions | Andrzej's C++ blog](https://akrzemi1.wordpress.com/2018/12/09/deducing-your-intentions/)
-   [\_Contra\_ CTAD – Arthur O'Dwyer – Stuff mostly about C++](https://quuxplusone.github.io/blog/2018/12/09/wctad/)

### Non-type template parameters with auto type

[P0127R2](http://wg21.link/p0127r2)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: VS 2017 15.7</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Automatically deduce type on non-type template parameters.

```cpp
template <auto value> void f() { }
f<10>();               // deduces int
```

[Trip report: Summer ISO C++ standards meeting (Oulu) | Sutter’s
Mill](https://herbsutter.com/2016/06/30/trip-report-summer-iso-c-standards-meeting-oulu/)

### Guaranteed copy elision

[P0135R1](http://wg21.link/p0135r1)

Copy elision for temporary objects, not for Named RVO.

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: VS 2017 15.6</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

```cpp
// based on P0135R0
struct NonMoveable 
{
  NonMoveable(int);
  // no copy or move constructor:
  NonMoveable(const NonMoveable&) = delete;
  NonMoveable(NonMoveable&&) = delete;

  std::array<int, 1024> arr;
};

NonMoveable make() 
{
  return NonMoveable(42);
}

// construct the object:
auto largeNonMovableObj = make();
```

Articles:

-   [Bartek’s coding blog: C++17 in details: language clarifications](https://www.cppstories.com/2017/06/cpp17-details-clarifications.html#guaranteed-copy-elision)
-   [Jonas Devlieghere: Guaranteed Copy Elision](https://jonasdevlieghere.com/guaranteed-copy-elision/)
-  [Guaranteed Copy Elision Does Not Elide Copies - C++ Team Blog](https://devblogs.microsoft.com/cppblog/guaranteed-copy-elision-does-not-elide-copies/)

### New specification for inheriting constructors (DR1941 et al)

[P0136R1](http://wg21.link/p0136r1)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: VS 2017 15.7</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

More description and reasoning in [P0136R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0136r0.html). Some excerpts below:

> An inheriting constructor does not act like any other form of using-declaration. All other using-declarations make some set of declarations visible to name lookup in another context, but an inheriting constructor declaration declares a new constructor that merely delegates to the original.

This feature changes inheriting constructor declaration from declaring a set of new constructors, to making a set of base class constructors visible in a derived class as if they were derived class constructors. (When such a constructor is used, the additional derived class subobjects will also be implicitly constructed as if by a defaulted default constructor). Put another way: make inheriting a constructor act just like inheriting any other base class member, to the extent
possible.

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

// Inheriting constructor parameters are no longer copied
struct A { A(const A&) = delete; A(int); };
struct B { B(A); void f(A); };
struct C : B { using B::B; using B::f; };
C c({0}); // was ill-formed, now ok (no copy made)
c.f({0}); // ok (unchanged)
```

### Direct-list-initialization of enumerations

[P0138R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0138r2.pdf)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Allows the initialization of an enum class with a fixed underlying type:

```cpp
enum class Handle : uint32_t { Invalid = 0 };
Handle h { 42 }; // OK
```

Allows creating `strong types` that are easy to use...

### Stricter expression evaluation order

[P0145R3](http://wg21.link/p0145r3)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: VS 2017 15.7</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

In a nutshell, given an expression such as `f(a, b, c)`, the order in which the sub-expressions f, a, b, c (which are of arbitrary shapes) are evaluated is left unspecified by the standard.

```cpp
// unspecified behaviour below!
f(i++, i);

v[i] = i++;

std::map<int, int> m;
m[0] = m.size(); // {{0, 0}} or {{0, 1}} ?
```

Summary of changes:

-   Postfix expressions are evaluated from left to right. This includes functions calls and member selection expressions.
-   Assignment expressions are evaluated from right to left. This includes compound assignments.
-   Operands to shift operators are evaluated from left to right.

See more in: [Stricter Expression Evaluation Order in C++17 - C++ Stories](https://www.cppstories.com/2021/evaluation-order-cpp17/)

Reference:

-   [Bartek’s coding blog: C++17 in details: language clarifications](https://www.cppstories.com/2017/06/cpp17-details-clarifications.html#stricter-expression-evaluation-order)
-   [C++ Order of evaluation, cppreference](http://en.cppreference.com/w/cpp/language/eval_order)
-   [SO: What are the evaluation order guarantees introduced by C++17?](http://stackoverflow.com/questions/38501587/what-are-the-evaluation-order-guarantees-introduced-by-c17)
-   [How compact code can become buggy code: getting caught by the order of evaluations, Fluent C++](http://www.fluentcpp.com/2017/05/09/compact-code-becomes-buggy-code-order-evaluations/)

### `constexpr` lambda expressions

[P0170R1](http://wg21.link/p0170r1)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 5</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

`constexpr` can be used in the context of lambdas.

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

-   [A 5 min episode of Jason Turner’s C++ Weekly about constexpr lambdas](https://www.youtube.com/watch?v=kmza9U_niq4)
-   [Lambda expression comparison between C++11, C++14 and C++17](https://maitesin.github.io/Lambda_comparison/)

### Different begin and end types in range-based for

[P0184R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0184r0.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 6.0</th>
<th>Clang: 3.6</th>
<th>MSVC: 15.0 Preview 5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

### `[[fallthrough]]` attribute

[P0188R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0188r1.pdf)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: 15.0 Preview 4</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Indicates that a fallthrough in a switch statement is intentional and a warning should not be issued for it. More details in [P0068R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0068r0.pdf).

```cpp
switch (c) {
case 'a':
    f(); // Warning emitted, fallthrough is perhaps a programmer error
case 'b':
    g();
[[fallthrough]]; // Warning suppressed, fallthrough is intentional
case 'c':
    h();
}
```

See more in [C++17 in details: Attributes - C++ Stories](https://www.cppstories.com/2017/07/cpp17-in-details-attributes/)

### `[[nodiscard]]` attribute

[P0189R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0189r1.pdf)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

Articles:

-   [Bartek’s coding blog: Enforcing code contracts with \[\[nodiscard\]\]](https://www.cppstories.com/2017/11/nodiscard.html)
-   [A 4 min video about nodiscard in Jason Turner’s C++ Weekly](https://www.youtube.com/watch?v=l_5PF3GQLKc)

### `[[maybe_unused]]` attribute

[P0212R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0212r1.pdf)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Suppresses compiler warnings about unused entities when they are declared with `[[maybe_unused]]`. More details in [P0068R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0068r0.pdf).

```cpp
                 static void impl1() { ... } // Compilers may warn about this
[[maybe_unused]] static void impl2() { ... } // Warning suppressed

void foo() {
                      int x = 42; // Compilers may warn about this
     [[maybe_unused]] int y = 42; // Warning suppressed
}
```

[A 3 min video about maybe\_unused in Jason Turner’s C++ Weekly](https://www.youtube.com/watch?v=WSPmNL9834U)

### Ignore unknown attributes

[P0283R2](http://wg21.link/p0283r2)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: Yes</th>
<th>Clang: 3.9</th>
<th>MSVC: VS 2017 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Clarifies that implementations should ignore any attribute namespaces which they do not support, as this used to be unspecified. More details in [P0283R1](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0283r1.pdf).

```cpp
//compilers which don't support MyCompilerSpecificNamespace will ignore this attribute
[[MyCompilerSpecificNamespace::do_special_thing]]
void foo();
```

### Pack expansions in using-declarations

[P0195R2](http://wg21.link/p0195r2)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: VS 2017 15.7</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

See more in [2 Lines Of Code and 3 C++17 Features - The overload Pattern - C++ Stories](https://www.cppstories.com/2019/02/2lines3featuresoverload.html/)

Remarks

-   Implemented in GCC 7.0, see [this change](https://github.com/gcc-mirror/gcc/commit/caba101ff33a763b444090b9c073bd84972ee552).

### Structured Binding Declarations

[P0217R3](http://wg21.link/p0217r3)  
[P0615R0: Renaming for structured
bindings](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/p0615r0.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Helps when using tuples as a return type. It will automatically create variables and `tie` them. More details in [P0144R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0144r0.pdf).

The name “Decomposition Declaration” was also used, but finally the standard agrees to use “Structured Binding Declarations” (section 11.5)

For example:

```cpp
int a = 0;
double b = 0.0;
long c = 0;
std::tie(a, b, c) = tuple; // a, b, c need to be declared first
```

Now we can write:

```cpp
auto [ a, b, c ] = tuple;
```

Such expressions also work on structs, pairs, and arrays.

Articles:

-   [Steve Lorimer, C++17 Structured Bindings](https://skebanga.github.io/structured-bindings/)
-   [jrb-programming, Emulating C++17 Structured Bindings in C++14](http://jrb-programming.blogspot.fr/2016/02/emulating-c17-structured-binding-in-c14.html)
-   [Simon Brand, Adding C++17 decomposition declaration support to your classes](https://tartanllama.github.io/c++/2016/07/20/structured-bindings/)

### Hexadecimal floating-point literals

[P0245R1](http://wg21.link/p0245r1)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 3.0</th>
<th>Clang: Yes</th>
<th>MSVC: VS 2017 15.3t</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

Allows expressing some special floating point values, for example, the smallest normal IEEE-754 single precision value is readily written as `0x1.0p-126`.

### init-statements for if and switch

[P0305R1](http://wg21.link/p0305r1)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

New versions of the if and switch statements for C++:

`if (init; condition)` and `switch (init; condition)`.

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

Look, that `val` has a separate scope, without it it will ‘leak’.

Now you can write:

```cpp
if (auto val = GetValue(); condition(val))
    // on success
else
    // on false...
```

`val` is visible only inside the `if` and `else` statements, so it doesn’t ‘leak’. `condition` might be any condition, not only if `val` is true/false.

Examples:

-   [C++ Weekly - Ep 21 C++17’s `if` and `switch` Init Statements](https://www.youtube.com/watch?v=AiXU5EuLZgc&feature=youtu.be)

### Inline variables

[P0386R2](http://wg21.link/p0386r2)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: VS 2017 15.5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

-   [17 Smaller but Handy C++17 Features - C++ Stories](https://www.cppstories.com/2019/08/17smallercpp17features/#2-inline-variables) - inline variables
-   [SO: What is an inline variable and what is it useful for?](http://stackoverflow.com/questions/38043442/what-is-an-inline-variable-and-what-is-it-useful-for)

### DR: Matching of template template-arguments excludes compatible templates

[P0522R0](http://wg21.link/p0522r0)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 4.0</th>
<th>MSVC: VS 2017 15.5</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

(Adapted from the [comment](https://www.reddit.com/r/cpp/comments/5osck2/c_17_features/dcm7eq4/) by [IncongruentModulo1](https://www.reddit.com/user/IncongruentModulo1)) 

For a useful example, consider something like this:

```cpp
template <template <typename> typename Container>
struct A
{
    Container<int>    m_ints;
    Container<double> m_doubles;
};
```

In C++14 and earlier, `A<std::vector>` wouldn’t be valid (ignoring the typename and not class before container) since `std::vector` is declared as:

`template <typename T, typename Allocator = std::allocator<T>> class vector;`

This change resolves that issue. Before, you would need to declare template `<template <typename...> typename Container>`, which is more permissive and moves the error to a less explicit line (namely the declaration of `m_ints` wherever the `struct A` is implemented /declared, instead of where the struct is instantiated with the wrong template type.

### `std::uncaught_exceptions()`

[N4259](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4259)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 6.0</th>
<th>Clang: 3.7</th>
<th>MSVC: 14.0</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

More background in the original paper: [PDF: N4152](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4152) and [GOTW issue 47: Uncaught Exceptions](http://www.gotw.ca/gotw/047.htm).

The function returns the number of uncaught exception objects in the current thread.

This might be useful when implementing proper Scope Guards that works also during stack unwinding.

> A type that wants to know whether its destructor is being run to unwind this object can query uncaught\_exceptions  in its constructor and store the result, then query uncaught\_exceptions again in its destructor; if the result is different,  then this destructor is being invoked as part of stack unwinding due to a new exception that was thrown later than the object’s construction

The above quote comes from [PDF: N4152](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2014/n4152).

### `constexpr` if-statements

[P0292R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0292r2.html)

{{<rawhtml>}}<table>
<thead>
<tr class="header">
<th>GCC: 7.0</th>
<th>Clang: 3.9</th>
<th>MSVC: 15.3</th>
</tr>
</thead>
<tbody>
</tbody>
</table>{{</rawhtml>}}

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

-   [Bartek’s coding blog: Simplify code with ‘if constexpr’ in C++17](https://www.cppstories.com/2018/03/ifconstexpr.html)
-   [LoopPerfect Blog, C++17 vs C++14 - Round 1 - if-constexpr](https://medium.com/@LoopPerfect/c-17-vs-c-14-if-constexpr-b518982bb1e2)
-   [SO: constexpr if and `static_assert`](http://stackoverflow.com/questions/38304847/constexpr-if-and-static-assert)
-   [Simon Brand: Simplifying templates and \#ifdefs with if constexpr](https://tartanllama.github.io/c++/2016/12/12/if-constexpr/)

Library Features
----------------

To get more details about library implementation I suggest those links:

-   [VS 2015 Update 2’s STL is C++17-so-far Feature Complete](https://blogs.msdn.microsoft.com/vcblog/2016/01/22/vs-2015-update-2s-stl-is-c17-so-far-feature-complete/) - Jan 2016
-   [libstdc++, C++ 201z status](https://gcc.gnu.org/onlinedocs/libstdc++/manual/status.html#status.iso.201z)
-   [libc++ C++1z Status](http://libcxx.llvm.org/cxx1z_status.html)

This section only mentions some of the most important parts of library changes, it would be too impractical to go into details of every little change.

### Merged: The Library Fundamentals 1 TS (most parts)

[P0220R1](https://isocpp.org/files/papers/p0220r1.html)

We get the following items:

-   Tuples - [Calling a function with a tuple of arguments](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#tuple.apply)
-   Functional Objects - [Searchers](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#func.searchers)
-   [Optional objects](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#optional) - `std::optional`!
-   [Class any](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#any) - `std::any`!
-   [`std::string_view`](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#string.view)
-   Memory:  
    -   [Shared-ownership pointers](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.smartptr)
    -   [Class memory_resource](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource)
    -   [Access to program-wide memory\_resource objects](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.global)
    -   [Pool resource classes](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.pool)
    -   [Class monotonic\_buffer\_resource](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.monotonic.buffer)
    -   [Alias templates using polymorphic memory
        resources](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#memory.resource.aliases)
-   Algorithms:  
    -   [Search](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#alg.search)
    -   [Sampling](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4562.html#alg.random.sample)
-   `shared_ptr` natively handles arrays: see [Merging shared\_ptr  changes from Library Fundamentals to  C++17](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0414r2.html)

The wording from those components comes from Library Fundamentals V2 to ensure the wording includes the latest corrections.

Resources:

- [Using C++17 std::optional - C++ Stories](https://www.cppstories.com/2018/05/using-optional/)
- [C++Stories: Standard Library Utilities](https://www.cppstories.com/2017/09/cpp17-details-utils.html)
- [Marco Arena, `string_view` odi et amo](https://marcoarena.wordpress.com/2017/01/03/string_view-odi-et-amo/)

### Removal of some deprecated types and functions, including `std::auto_ptr`, `std::random_shuffle`, and old function adaptors

[N4190](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0001r1.html)

-   Function objects - unary\_function/binary\_function, ptr\_fun(), and  mem\_fun()/mem\_fun\_ref()
-   Binders - bind1st()/bind2nd()
-   **auto\_ptr**
-   Random shuffle - random\_shuffle(first, last) and random\_shuffle(first, last, rng)

### Merged: The Parallelism TS, a.k.a. “Parallel STL.”,

[P0024R2](http://isocpp.org/files/papers/P0024R2.html)

Parralel versions/overloads of most of std algorithms. Plus a few new
algorithms, like reduce, transform\_reduce, for\_each.

```cpp
std::vector<int> v = genLargeVector();

// standard sequential sort
std::sort(v.begin(), v.end());

// explicitly sequential sort
std::sort(std::seq, v.begin(), v.end());

// permitting parallel execution
std::sort(std::par, v.begin(), v.end());

// permitting vectorization as well
std::sort(std::par_unseq, v.begin(), v.end());
```

Articles:

-   [Bartek’s coding blog: C++17 in details: Parallel Algorithms](https://www.cppstories.com/2017/08/cpp17-details-parallel.html)
-   [Parallel Algorithm of the Standard Template Library -  ModernesCpp.com](http://www.modernescpp.com/index.php/parallel-algorithm-of-the-standard-template-library)

### Merged: File System TS,

[P0218R1](https://isocpp.org/files/papers/P0218r1.html)

```cpp
namespace fs = std::filesystem;

fs::path pathToShow(/* ... */);
cout << "exists() = " << fs::exists(pathToShow) << "\n"
     << "root_name() = " << pathToShow.root_name() << "\n"
     << "root_path() = " << pathToShow.root_path() << "\n"
     << "relative_path() = " << pathToShow.relative_path() << "\n"
     << "parent_path() = " << pathToShow.parent_path() << "\n"
     << "filename() = " << pathToShow.filename() << "\n"
     << "stem() = " << pathToShow.stem() << "\n"
     << "extension() = " << pathToShow.extension() << "\n";
```

Articles:

-   [Bartek’s coding blog: C++17 in details:
    Filesystem](https://www.cppstories.com/2017/08/cpp17-details-filesystem.html)

### Merged: The Mathematical Special Functions IS,

[PDF - WG21 P0226R1](https://isocpp.org/files/papers/P0226R1.pdf)

### Improving [std::pair](http://en.cppreference.com/w/cpp/utility/pair) and [std::tuple](http://en.cppreference.com/w/cpp/utility/tuple)

[N4387](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4387.html)

### [`std::shared_mutex`](http://en.cppreference.com/w/cpp/thread/shared_mutex) (untimed)

[N4508](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/n4508.html)

### `std::variant`

[P0088R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0088r2.html)

Variant is a typesafe union that will report errors when you want to access something that’s not currently inside the object.

```cpp
std::variant<std::string, int> v { "Hello A Quite Long String" };
// v allocates some memory for the string
v = 10; // we call destructor for the string!
// no memory leak
```

Notes:

-   Variant is not allowed to allocate additional (dynamic) memory.
-   A variant is not permitted to hold references, arrays, or the type `void`.
-   A variant is default initialized with the value of its first alternative.
-   If the first alternative type is not default constructible, then the variant must use std::monostate as the first alternative

Have a look at more example in a separate article:  

[Everything You Need to Know About std::variant from C++17 - C++ Stories](https://www.cppstories.com/2018/06/variant/)

### Splicing Maps and Sets

[P0083R2](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0083r2.pdf)

From Herb Sutter, [Oulu trip report](https://herbsutter.com/2016/06/30/trip-report-summer-iso-c-standards-meeting-oulu/):

> You will now be able to directly move internal nodes from one node-based container directly into another container of the same type. Why is that important? Because it guarantees no memory allocation overhead, no copying of keys or values, and even no exceptions if the container’s comparison function doesn’t throw.

New functions:

* `std::map::extract`
* `std::map::merge`
* `std::set::extract`
* `std::set::merge`

### Elementary string conversion

`from_chars` and `to_chars` which are low-level, and offers the best possible performance

The new conversion routines are:

* non-throwing
* non-allocating
* no locale support
* memory safety
* error reporting gives additional information about the conversion outcome
* bound checked
* explicit round-trip guarantees - you can use to_chars and from_charsto convert the number back and forth, and it will give you the exact binary representations. This is not guaranteed by other routines like printf/sscanf/itoa, etc.

Simple example:

```cpp
std::string str { "xxxxxxxx" };
const int value = 1986;
std::to_chars(str.data(), str.data() + str.size(), value);

// str is "1986xxxx"
```

more in:

* [How to Use The Newest C++ String Conversion Routines - std::from\_chars - C++ Stories](https://www.cppstories.com/2018/12/fromchars/)
* [How to Convert Numbers into Text with std::to\_chars in C++17 - C++ Stories](https://www.cppstories.com/2019/11/tochars/)

### `std::apply`

A handy helper for `std::tuple`. It takes a tuple and a callable object and then invokes this callable with parameters fetched from the tuple.

```cpp
#include <iostream>
#include <tuple>
 
int sum(int a, int b, int c) { 
    return a + b + c; 
}

void print(std::string_view a, std::string_view b) {
    std::cout << "(" << a << ", " << b << ")\n";
} 

int main() {
    std::tuple numbers {1, 2, 3};
    std::cout << std::apply(sum, numbers) << '\n';

    std::tuple strs {"Hello", "World"};
    std::apply(print, strs);
}
```

See more in [C++ Templates: How to Iterate through std::tuple: std::apply and More - C++ Stories](https://www.cppstories.com/2022/tuple-iteration-apply/)

### `std::invoke`

With `std::invoke` you get access to INVOKE expression that was defined in the Standard since C++11 (or even in C++0x, TR1), but wasn’t exposed outside.

It can handle the following callables:

* function objects: like `func(arguments...)`
* pointers to member functions `(obj.*funcPtr)(arguments...)`
* pointer to member data `obj.*pdata`

In C++20 it was improved and marked with `constexpr`.

See more in [C++20 Ranges, Projections, std::invoke and if constexpr - C++ Stories](https://www.cppstories.com/2020/10/understanding-invoke.html/)

Contributors
------------

This is a place for you to be mentioned!

Contributors:

-   [Simon Brand](https://tartanllama.github.io/)
-   [Jonathan Boccara, Fluent{C++}](http://www.fluentcpp.com/)
-   [Marek Kurdej](https://twitter.com/Kurdej)
-   suggestions from r/cpp thread:  [`c_17_features`](https://www.reddit.com/r/cpp/comments/5osck2/c_17_features/)

Summary
-------

Thanks for all the support with the list!

There are still items that should be updated, but the list is mostly done.

If you want to read more about C++17, have a look at my book

[![](../../images/2017-01-09-c-17-features-cpp17indetail.png)](https://leanpub.com/cpp17indetail/c/fall22)

[C++17 In Detail, -17% coupon code](https://leanpub.com/cpp17indetail/c/fall22)
