# lang-cpp

## Code Style


Google C++ Style Guide
http://google.github.io/styleguide/cppguide.html

## Feature

c++11新特性--Lambda表达式 - CSDN博客
https://blog.csdn.net/taoyanqi8932/article/details/52541312

## Performance

- [6 个技巧，提升 C++11 的 vector 性能 | 开源中国社区](https://www.oschina.net/translate/6-tips-supercharge-cpp-11-vector-performance?lang=chs&page=1#)
- [c++ - Appending a vector to a vector | Stack Overflow](https://stackoverflow.com/questions/2551775/appending-a-vector-to-a-vector)
- [fenbf/AwesomePerfCpp: A curated list of awesome C/C++ performance optimization resources: talks, articles, books, libraries, tools, sites, blogs. Inspired by awesome.](https://github.com/fenbf/AwesomePerfCpp)

## common questions

### C++与C的空指针

- NULL，C中定义：((void*)0)  
- NULL，C++中定义：0  
- nullptr，在C++中表示空指针  

### 1.指针与引用的区别  

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

### 2.emplace_back与push_back的区别

#### 2.1 Why emplace_back is faster than push_back?

- `push_back` takes a container element and copies/moves it into the container.  
- `emplace_back` takes arbitrary arguments and constructs from those a new container element. But if you pass a single argument that's already of element type to `emplace_back`, you'll just use the copy/move constructor anyway.

#### 2.2 push_back vs emplace_back

I'm a bit confused regarding the difference between `push_back` and `emplace_back`.
```cpp
void emplace_back(Type&& _Val);
void push_back(const Type& _Val);
void push_back(Type&& _Val);
```
As there is a `push_back` overload taking a rvalue reference I don't quite see what the purpose of `emplace_back` becomes?

#### 3.3 When would I use push_back instead of emplace_back?

C++11 vectors have the new function emplace_back. Unlike push_back, which relies on compiler optimizations to avoid copies, emplace_back uses perfect forwarding to send the arguments directly to the constructor to create an object in-place. It seems to me that emplace_back does everything push_back can do, but some of the time it will do it better (but never worse).

What reason do I have to use push_back?

##### A1

`push_back` always allows the use of uniform initialization, which I'm very fond of. For instance:
```cpp
struct aggregate {
    int foo;
    int bar;
};

std::vector<aggregate> v;
v.push_back({ 42, 121 });
On the other hand, v.emplace_back({ 42, 121 }); will not work.
```
#### A2

The real primary difference has to do with implicit vs. explicit constructors. Consider the case where we have a single argument that we want to pass to push_back or emplace_back.

```cpp
std::vector<T> v;
v.push_back(x);
v.emplace_back(x);
```

After your optimizing compiler gets its hands on this, there is no difference between these two statements in terms of generated code. The traditional wisdom is that push_back will construct a temporary object, which will then get moved into v whereas emplace_back will forward the argument along and construct it directly in place with no copies or moves. This may be true based on the code as written in standard libraries, but it makes the mistaken assumption that the optimizing compiler's job is to generate the code you wrote. The optimizing compiler's job is actually to generate the code you would have written if you were an expert on platform-specific optimizations and did not care about maintainability, just performance.

The actual difference between these two statements is that the more powerful emplace_back will call any type of constructor out there, whereas the more cautious push_back will call only constructors that are implicit. Implicit constructors are supposed to be safe. If you can implicitly construct a U from a T, you are saying that U can hold all of the information in T with no loss. It is safe in pretty much any situation to pass a T and no one will mind if you make it a U instead. A good example of an implicit constructor is the conversion from std::uint32_t to  std::uint64_t. A bad example of an implicit conversion is double to std::uint8_t.

We want to be cautious in our programming. We do not want to use powerful features because the more powerful the feature, the easier it is to accidentally do something incorrect or unexpected. If you intend to call explicit constructors, then you need the power of emplace_back. If you want to call only implicit constructors, stick with the safety of push_back.

###### An example

```cpp
std::vector<std::unique_ptr<T>> v;
T a;
v.emplace_back(std::addressof(a)); // compiles
v.push_back(std::addressof(a)); // fails to compile
```

std::unique_ptr<T> has an explicit constructor from T *. Because emplace_back can call explicit constructors, passing a non-owning pointer compiles just fine. However, when v goes out of scope, the destructor will attempt to call delete on that pointer, which was not allocated by new because it is just a stack object. This leads to undefined behavior.

This is not just invented code. This was a real production bug I encountered. The code was std::vector<T *>, but it owned the contents. As part of the migration to C++11, I correctly changed T * to std::unique_ptr<T> to indicate that the vector owned its memory. However, I was basing these changes off my understanding in 2012, during which I thought "emplace_back does everything push_back can do and more, so why would I ever use push_back?", so I also changed the push_back to emplace_back.

Had I instead left the code as using the safer push_back, I would have instantly caught this long-standing bug and it would have been viewed as a success of upgrading to C++11. Instead, I masked the bug and didn't find it until months later.
