---
alias: 
title: Chapter 1.3 Multiple Template Parameters 
creation date: 2021-11-29 10:47
type: note
category: []
status:
priority:
tags:
author:
rating: 
relates: 
area: 
project:
---
link:: 
tags:: 
relations:: [TMPL- Chapter 1. Function Templates](TMPL-%20Chapter%201.%20Function%20Templates.md)

[<- BACK TO BOOK ](C++%20Templates%20-%20The%20Complete%20Guide.md)
[<- Chapter 1. Function Templates](TMPL-%20Chapter%201.%20Function%20Templates.md)

# Chapter 1.3 Multiple Template Parameters



***Template parameters***, which are declared in angle brackets before the function template name:

```CPP
template<typename T>        // T is template parameter
```

***Call parameters***, which are declared in parentheses after the function template name:

```CPP
T max (T a, T b)            // a and b are call parameters
```

## Multiple template parameters and Return Type:

```cpp
template<typename T1, typename T2>
T1 max (T1 a, T2 b)
{
    return b < a ? a : b; }
…
auto m = ::max(4, 7.2);        // OK, but type of first argument defines return type
```

*issues with above :*  If you use one of the parameter types as **return type,** the argument for the other parameter might get converted to this type, regardless of the caller’s intention. Thus, the return type depends on the call argument order.

C++ provides different ways to deal with this problem:

1. Introduce a third template parameter for the return type.

2. Let the compiler find out the return type.

3. Declare the return type to be the “common type” of the two parameter types.

### Template Parameters for Return Types

Template Parameters can be specified explicitly  or deduced by compiler.

> In cases when there is no connection between template and call parameters and when template parameters cannot be determined, you must specify the template argument explicitly with the call.

```cpp
template<typename T1, typename T2, typename RT>
RT max (T1 a, T2 b);

::max<int,double,double>(4, 7.2);        // OK, but tedious

// if order is changed
template<typename RT, typename T1, typename T2>
RT max (T1 a, T2 b);

::max<double>(4, 7.2)        //OK: return type is double, T1 and T2 are deduced
```

In general, you must specify all the argument types up to the last argument type that cannot be determined implicitly

> this variation doesnt give any advantage so ,  it’s a good idea to **keep it simple** and use the one-parameter version.

### Deducing the Return Type

If a return type depends on template parameters, the simplest and **best approach to deduce the return type is to let the compiler find out.** Since **C++14**, this is possible by simply not declaring any return type (i.e. as  `auto`):

```cpp
template<typename T1, typename T2>
auto max (T1 a, T2 b)
{
  return b < a ? a : b;
}
```

In fact, the use of `auto` for the return type without a corresponding *trailing return type* (which would be introduced with a `->` at the end) indicates that:

- actual return type must be deduced from the return statements in the function body. 
- Therefore, the code must be available and multiple return statements have to match.



In **C++11** we can benefit from the fact that the ***trailing return type*** syntax allows us to use the call parameters. That is, we can *declare* that the return type is derived from what `operator?:` yields:

```cpp
template<typename T1, typename T2>
auto max (T1 a, T2 b) -> decltype(b<a?a:b) // more or less making the implementation of the function part of its declaration.
{
  return b < a ? a : b;
}
```

 

#### What if `T` happens to be reference ?

Then it might happen that the return type is a reference type, because under some conditions `T` might be a reference. 

For this reason you should return the type *decayed* from `T`, which looks as follows:

```cpp
#include <type_traits>
template<typename T1, typename T2>
auto max (T1 a, T2 b) -> typename std::decay<decltype(true?a:b)>::type
{ 
  return b < a ? a : b;
}
```

Here, the type trait `std::decay<>` is used, which returns the resulting type in a member `type`. (defined in `<type_trait>` ) 
Because the member `type` is a type, you have to qualify the expression with `typename` to access it.



> Note that an initialization of type `auto` always decays.

This also applies to return values when the return type is just `auto`. 

```cpp
int i = 42;
int const& ir = i;        // ir refers to
i auto a = ir;            // a is declared as new object of type int i.e. decayed type of i
```

### Return Type as Common Type

Since **C++11**, the C++ standard library provides a means to specify choosing “the more general type.” 
`std::common_type<>::type` yields the “common type” of two (or more) different types passed as template arguments.

```cpp
#include <type_traits>
template<typename T1, typename T2>
std::common_type_t<T1,T2> max (T1 a, T2 b)
{
  return b < a ? a : b;
}
```

`std::common_type` is a *type trait*, defined in `<type_traits>`, which yields a structure having a `type` member for the resulting type

```cpp
typename std::common_type<T1,T2>::type    //since C++11
```
However, since C++14 you can simplify the usage of traits like this by appending `_t` to the trait name and skipping `typename` and `::type`

```cpp
std::common_type_t<T1,T2>   // equivalent since C++14
```

**Note that `std::common_type<>` also decays**
