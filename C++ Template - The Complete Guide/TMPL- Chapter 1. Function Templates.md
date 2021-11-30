---
alias: 
title: Chapter 1. Function Templates 
creation date: 2021-11-28 20:04
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
relations:: [C++ Templates - The Complete Guide](C++%20Templates%20-%20The%20Complete%20Guide.md)

[<- BACK TO BOOK ](C++%20Templates%20-%20The%20Complete%20Guide.md)

# Chapter 1. Function Templates



> Function templates are functions that are parameterized so that they represent a family of functions.

 - Function templates provide a functional behavior that can be called for different types. In other words, a function template represents a family of functions.
 - Undetermined elements are parameterized.

## Defining a Template

```cpp
template<typename T>  
T max (T a, T b)  
{  
 // if b < a then yield a else yield b  
 return b < a ? a : b;  
}
```

For function parameters  `a`  and `b`, the type of these parameters is left open as ***template parameter*** `T`.

Syntax for template parameters - 

```cpp
template< comma-separated-list-of-parameters >
```

- list of parameters is `typename T`
- The keyword `typename` introduces a ***type parameter***.
- The *type parameter* represents an arbitrary type that is determined by the caller when the caller calls the function. 
- You can use any type (fundamental type, class, and so on) as long as it provides the operations that the template uses.
- For historical reasons, you can also use the keyword `class` instead of `typename` to define a type parameter. 
    - The keyword `typename` came relatively late in the evolution of the C++98 standard. 
    - Semantically there is no difference. But it can be misleading.

```cpp
template<class T>
T max (T a, T b)
{
    return b < a ? a : b;
}
```

## Using the Template

```cpp
 int i = 42;
std::cout << "max(7,i):      " << ::max(7,i) << ’\n’; // :: This is to ensure that our max() template 
																											// is found in the global namespace.
double f1 = 3.4; double f2 = -6.7;
std::cout << "max(f1,f2):    " << ::max(f1,f2) << ’\n’;
std::string s1 = "mathematics"; std::string s2 = "math";
std::cout << "max(s1,s2):    " << ::max(s1,s2) << ’\n’;
// output
max(7,i): 42
max(f1,f2): 3.4
max(s1,s2): mathematics

```

- Templates aren’t compiled into single entities that can handle any type. Instead, different entities are generated from the template for every type for which the template is used

- > The process of replacing template parameters by concrete types is called instantiation. It results in an instance of a template.

- Note that the mere use of a function template can trigger such an instantiation process. There is no need for the programmer to request the instantiation separately

- Note also that `void` is a valid template argument provided the resulting code is valid

```cpp
template<typename T>
T foo(T*)
{ }

void* vp = nullptr;
foo(vp);                            // OK: deduces void foo(void*)
```

## Two Phase Translation

- An attempt to instantiate a template for a type that doesn’t support all the operations used within it will result in a compile-time error

```cpp
std::complex<float> c1, c2;        // doesn’t provide operator <
…
::max(c1,c2);                     // ERROR at compile time
```

Thus, templates are “compiled” in two phases:

1. At ***definition time***, the template code itself is checked for correctness ignoring the template parameters. This includes:
    1. Syntax errors are discovered, such as missing semicolons.
    2. Using unknown names (type names, function names, …) that don’t depend on template parameters are discovered.
    3. Static assertions that don’t depend on template parameters are checked.

2. At ***instantiation time***, the template code is checked (again) to ensure that all code is valid. That is, now especially, all parts that depend on template parameters are double-checked.

> Anything that is not involving template parameters is checked at **definition time** and rest is checked at **instantiation time**.

```cpp
template<typename T>
void foo(T t)
{
   undeclared();        // first-phase compile-time error if undeclared() unknown
   undeclared(t);        // second-phase compile-time error if undeclared(T) unknown
   static_assert(sizeof(int) > 10,          // always fails if sizeof(int)<=10
                 "int too small");
   static_assert(sizeof(T) > 10,            //fails if instantiated for T with size <=10
                 "T too small");
}
```

### Compiling and Linking

Two-phase translation leads to an **important problem** 
When instantiation of function template is triggered, a compiler will (at some point) need to see that template’s definition. 
This breaks the usual compile and link distinction for ordinary functions, when the declaration of a function is sufficient to compile its use.  
For the moment, let’s take the simplest approach: **Implement each template inside a header file.**

## [1.2 Template Argument Deduction](TMPL-%20Chapter%201.2%20Template%20Argument%20Deduction.md)

## [1.3 Multiple Template Parameters](TMPL-%20Chapter%201.3%20Multiple%20Template%20Parameters.md)