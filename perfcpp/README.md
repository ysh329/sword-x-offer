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

## 2.emplace_back与push_back的区别


- push_back: 在引入右值引用，转移构造函数，转移复制运算符之前，通常使用push_back()向容器中加入一个右值元素（临时对象）的时候，首先会调用构造函数构造这个临时对象，然后需要调用拷贝构造函数将这个临时对象放入容器中。原来的临时变量释放。这样造成的问题是临时变量申请的资源就浪费。引入了右值引用，转移构造函数（请看这里）后，push_back()右值时就会调用构造函数和转移构造函数。 

- emplace_back: 在这上面有进一步优化的空间就是使用emplace_back。在容器尾部添加一个元素，这个元素原地构造，不需要触发拷贝构造和转移构造。而且调用形式更加简洁，直接根据参数初始化临时对象的成员。 
