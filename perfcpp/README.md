# PerfCPP

- [6 个技巧，提升 C++11 的 vector 性能 | 开源中国社区](https://www.oschina.net/translate/6-tips-supercharge-cpp-11-vector-performance?lang=chs&page=1#)
- [c++ - Appending a vector to a vector | Stack Overflow](https://stackoverflow.com/questions/2551775/appending-a-vector-to-a-vector)
- [fenbf/AwesomePerfCpp: A curated list of awesome C/C++ performance optimization resources: talks, articles, books, libraries, tools, sites, blogs. Inspired by awesome.](https://github.com/fenbf/AwesomePerfCpp)



## 1.指针与引用的区别  

[What are the differences between a pointer variable and a reference variable in C++? | Stack Overflow](https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable-in?rq=1)

1. A pointer can be re-assigned any number of times while a reference cannot be re-assigned after binding.  
2. Pointers can point nowhere (NULL), whereas a reference always refers to an object.  
3. You can't take the address of a reference like you can with pointers.  
4. There's no "reference arithmetic" (but you can take the address of an object pointed by a reference and do pointer arithmetic on it as in &obj + 5).  

To clarify a misconception:

The C++ standard is very careful to avoid dictating how a compiler may implement references, but every C++ compiler implements references as pointers. That is, a declaration such as:
```cpp
int &ri = i;
```
if it's not optimized away entirely, allocates the same amount of storage as a pointer, and places the address of i into that storage.
So, a pointer and a reference both use the same amount of memory.

As a general rule,

Use references in function parameters and return types to provide useful and self-documenting interfaces.
Use pointers for implementing algorithms and data structures.

