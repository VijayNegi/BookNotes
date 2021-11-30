---
alias: 
title: C++ Templates - The Complete Guide 
creation date: 2021-11-27 19:55
type: book 
category: [cpp-templates]
status: in-progress
priority: 3
tags:
author: David Vandevoorde
rating: 4
relates: 
area: 
project:
---
link:: https://learning.oreilly.com/library/view/c-templates-the/9780134778808/contents.xhtml
tags:: 
relations:: 

# C++ Templates - The Complete Guide

**Why Templates**
C++ requires us to declare variables, functions, and most other kinds of entities using specific types. However, a lot of code looks the same for different types
For genericity programmers can do - 
1. Implement the same behavior again and again for each type that needs this behavior.
2. Write general code for a common base type such as `Object` or `void*`.
3. Use special preprocessors.

Drawbacks for each approach - 
1. Implement a behavior again and again -  reinvent the wheel. Making the same mistakes,
2. Write general code for a common base class - lose the benefit of type checking.
3. Use a special preprocessor, - code is replaced by some “stupid text replacement mechanism” that has no idea of scope and types, which might result in strange semantic errors.

> Templates - They are functions or classes that are written for one or more types not yet specified. When you use a template, you pass the types as arguments, explicitly or implicitly. Because templates are language features, you have full support of type checking and scope.

## Book Outline
TMPL- Chapter 1. Function Templates
- **Part 1. The Basics**
    - [Chapter 1. Function Templates](TMPL-%20Chapter%201.%20Function%20Templates.md)
    - Chapter 2. Class Templates
    - Chapter 3. Nontype Template Parameters
    - Chapter 4. Variadic Templates
    - Chapter 5. Tricky Basics
    - Chapter 6. Move Semantics and enable_if<>
    - Chapter 7. By Value or by Reference?
    - Chapter 8. Compile-Time Programming
    - Chapter 9. Using Templates in Practice
    - Chapter 10. Basic Template Terminology
    - Chapter 11. Generic Libraries
- **Part 2. Templates in Depth**.
    - Chapter 12. Fundamentals in Depth
    - Chapter 13. Names in Templates
    - Chapter 14. Instantiation
    - Chapter 15. Template Argument Deduction
    - Chapter 16. Specialization and Overloading
    - Chapter 17. Future Directions
- **Part 3. Templates and Design**
    - Chapter 18. The Polymorphic Power of Templates
    - Chapter 19. Implementing Traits
    - Chapter 20. Overloading on Type Properties
    - Chapter 21. Templates and Inheritance
    - Chapter 22. Bridging Static and Dynamic Polymorphism
    - Chapter 23. Metaprogramming
    - Chapter 24. Typelists
    - Chapter 25. Tuples
    - Chapter 26. Discriminated Unions
    - Chapter 27. Expression Templates
    - Chapter 28. Debugging Templates




## References
- [book website](http://www.tmplbook.com/)