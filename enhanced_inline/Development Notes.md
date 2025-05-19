# Enhanced Inline Development Notes





## Two Primary Issues to Tackle

- It is essential that we are able to disambiguate between inline expansion and external linkage. Link Time Code Generation (LTCG) may perform inline expansion on functions that are not marked `inline`. We need language syntax to express inline expansion for these cases.
- We need a flexible approach to inlining aggressiveness that is future proof, but easy to use. Currently most programs use three macros or attributes: noinline, inline, and always_inline. These are already supported by the major compilers. But, what if we want more granularity in the future?



## Linkage Keywords

We currently have:

 `extern int foo();` - External linkage.

`static int foo();` - Internal linkage, which restricts its visibility to the current translation unit.

`inline int foo();` - Relaxed external linkage, allowing identical definitions in multiple translation units as long as the One Definition Rule is followed. The compiiler is required to merge repeated functions.

### Additional linkage cases:

- Functions declared without specifiers (`int foo();`) have external linkage by default.
- Functions/variables in anonymous namespaces (`namespace { int foo(); }`) have internal linkage, similar to `static`.
- `constexpr` functions are implicitly `inline`.



## Expansion







## Syntax for Inline Expansion vs Inline Linkage



- Must allow backwards compatibility in the sense that code still runs and builds, which means `inline` meaning linkage cannot be changed.



The parenthetical argument value for `inline` should only refer to expansion to make the language easier to understand.



## Inline Expansion at Link-Time

Link-Time Code Generation may perform inline expansion, so functions not marked `inline` may be inlined by the compiler. `-flto` on GCC.

https://stackoverflow.com/questions/5987020/can-the-linker-inline-functions

This means that we really need a way to express our desire for inline expansion even in contexts where we omit the `inline` keyword.





Consider `[[gnu::pure]]` and `[[gnu::const]]` functions as well.





## Attribute Syntax

I don't think we want attribute syntax for inline expansion qualifiers. The reason for this is that attributes cannot take template arguments like `noexcept`. And, there are major compile time ramifications for making a function inline in all cases. We want to programatically enable inlining. Perhaps this can be accomplished with C++26 reflection, if we can programatically add attributes. Is this possible? Could the be done through template parameters?







GCC bug concerning inlining heuristics and ignoring linking inline on function: https://gcc.gnu.org/bugzilla/show_bug.cgi?id=93008



