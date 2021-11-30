---
alias: 
title: Chapter 1.2 Template Argument Deduction 
creation date: 2021-11-29 00:36
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
relations:: [Chapter 1. Function Templates](TMPL-%20Chapter%201.%20Function%20Templates.md)

[<- BACK TO BOOK ](C++%20Templates%20-%20The%20Complete%20Guide.md)
[<- Chapter 1. Function Templates](TMPL-%20Chapter%201.%20Function%20Templates.md)

# Chapter 1.2 Template Argument Deduction

>  ***template argument deduction*** allows us to call function templates with syntax identical to that of calling an ordinary function: We do not have to explicitly specify the types corresponding to the template parameters.
- the template parameters are determined by the arguments we pass.
- `T` might only be “part” of the type. see code below

```cpp
template<typename T>
T max (T const& a, T const& b) // if we pass `int`, again `T` is deduced as `int`, 
{                              // because the function parameters match for `int const&`.
    return b < a ? a : b;
}
```

## Type Conversions During Type Deduction

**Automatic type conversions are limited during type deduction**

Based on Call parameters

| by Reference                                                                   | By Value                                                                                       |
| ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| Even trivial conversions do not apply to type deduction                        | Only trivial conversions that *decay* are supported                                            |
| Two arguments declared with the same template parameter `T` must match exactly | For two arguments declared with the same template parameter `T` the *decayed* types must match |
|                                                                                | Qualifications with `const` or `volatile` are ignored.                                         |
|                                                                                | References convert to the referenced type                                                      |
|                                                                                | Raw arrays or functions convert to the corresponding pointer type.                             |
|                                                                                |                                                                                                |



```CPP
template<typename T>
T max (T a, T b);
// …
int const c = 42;
max(i, c);            // OK: T is deduced as int
max(c, c);            // OK: T is deduced as int
int& ir = i;
max(i, ir);          // OK: T is deduced as int
int arr[4];
foo(&i, arr);        // OK: T is deduced as int*

// However, the following are errors:
max(4, 7.2);        // ERROR: T can be deduced as int or double
std::string s;
foo("hello", s);    //ERROR: T can be deduced as char const[6] or std::string
```



### Three ways to handle type deduction errors:

1. Cast the arguments so that they both match:
     ```CPP
     max(static_cast<double>(4), 7.2) 
     ```

2. Specify (or qualify) explicitly the type of `T` to prevent the compiler from attempting type deduction
    ```CPP
    max<double>(4, 7.2);
    ```

3. Specify that the parameters may have different types.

    ```CPP
    template<typename T1, typename T2>
    T1 max (T1 a, T2 b
    ```

    

## Type Deduction for Default Arguments

Note also that type deduction does not work for default call arguments.

```cpp
template<typename T>
void f(T = "");				// default call argument: empty string
// …

f(1);        // OK: deduced T to be int, so that it calls f<int>(1)
f();         // ERROR: cannot deduce T

```

To support this case, you also have to declare a default argument for the template parameter

```CPP
template<typename T = std::string>		// default template argument
void f(T = "");
//…
f();        // OK
```

