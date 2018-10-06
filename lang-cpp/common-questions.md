# 问答题

来源：牛客网

## 1.代码分析

分析下面代码有什么问题？

```cpp
void test1()
{
    char string[10];
    char* str1 = "0123456789";
    strcpy( string, str1 );
}
```

字符串str1需要11个字节才能存放下（包括末尾的’\0’），而string只有10个字节的空间，strcpy会导致数组越界。

## 2.代码分析

分析下面代码有什么问题？

```cpp
void test2()
{
    char string[10], str1[10];
    int i;
    for(i=0; i<10; i++)
    {
        str1  = 'a';
    }
    strcpy( string, str1 );
}
```

1.不能通过编译。因为数组名str1为 `char *const` 类型的右值类型，根本不能赋值；  
2.即使想对数组的第一个元素赋值，也要使用 `*str1 = 'a'`;   
3.对字符数组赋值后，使用库函数strcpy进行拷贝操作，strcpy会从源地址一直往后拷贝，直到遇到'\0'为止。所以拷贝的长度是不定的。如果一直没有遇到'\0'导致越界访问非法内存，程序就崩了。

完美修改方案为：

```cpp
void test2()
{
    char string[10], str1[10];
    int i;
    for(i=0; i<9; i++)
    {
        str1[i] = 'a';
    }
    str1[9] = '\0';
    strcpy( string, str1 );
}
```

## 3.代码分析

指出下面代码有什么问题？

```cpp
void test3(char* str1)
{
    if(str1 == NULL) {
        return ;
    }
    char string[10];
    if( strlen( str1 ) <= 10 )
    {
        strcpy( string, str1 );
    }
}
```

知识点：  
- `strlen(p)`：返回字符指针 p 的长度（不计`\0`在内），是C语言风的格字符串函数；  
- `strcpy`：从源地址开始拷贝，直到遇到`\0`为止；
- `sizeof`和`strlen`的区别：`sizeof`的计算包含换行符，`strlen`不包括换行符，所以要小于10，就一个字节的余量。  

问题：  
- 将 str1 拷贝到 string ， string 长度为10，str1长度最多为10（含`\0`），而`strlen`不计`\0`，所以计算 str1 是否装满了，是与 9 比较（排除掉`\0`），而不是 10 。

解决：  
- `if( strlen( str1 ) <= 10 )` 改为 `<10`。

## 4.写出完整版的strcpy函数

如果编写一个标准strcpy函数的总分值为10，下面给出几个不同得分的答案： 

2分 
```cpp
void strcpy( char *strDest, char *strSrc )
{
    while( (*strDest++ = * strSrc++) != '\0' );
}
```

4分 
```cpp
void strcpy( char *strDest, const char *strSrc ) 
//将源字符串加const，表明其为输入参数，加2分
{
    while( (*strDest++ = * strSrc++) != '\0' );
}
```

7分 
```cpp
void strcpy(char *strDest, const char *strSrc) 
{
    //对源地址和目的地址加非0断言，加3分
    assert( (strDest != NULL) && (strSrc != NULL) );
    while( (*strDest++ = * strSrc++) != '\0' );
}
```

10分 
```cpp
//为了实现链式操作，将目的地址返回，加3分！ 
char * strcpy( char *strDest, const char *strSrc ) 
{
    assert( (strDest != NULL) && (strSrc != NULL) );
    char *address = strDest; 
    while( (*strDest++ = * strSrc++) != '\0' ); 
    return address;
}
```

## 5.代码分析

检查下面代码有什么问题？

```cpp
void GetMemory( char *p )
{
    p = (char *) malloc( 100 );
}
void Test( void ) 
{
    char *str = NULL;
    GetMemory( str ); 
    strcpy( str, "hello world" );
    printf( str );
}
```

- 要改变一个变量的值，要传地址，例如你改变`int a`的值，你传`&a`，改变`int *a` 你就指针的地址，也就是二级指针；
- 如若成功申请内存，最后在主函数中需要`free`掉。

解决：

```cpp
//传值调用
void GetMemory( char **p )
{
    *p = (char *) malloc( 100 );
}
//引用调用
void GetMemory_1(char *&p)
{
    p = (char *) malloc (100);
}
```

## 6.代码分析

检查下面代码有什么问题？

```cpp
char *GetMemory( void )
{ 
    char p[] = "hello world"; 
    return p; 
}
void Test( void )
{ 
    char *str = NULL; 
    str = GetMemory(); 
    printf( str ); 
}
```

- `char p[]="hello world";`相当于`char p[12]，strcpy(p," hello world" )`；
- `p`是一个数组名，属于局部变量，存储在栈中；
- `"hello world"` 存储在文字存储区，数组`p`中存储的是 `" hello world"` 的一个副本，当函数结束，`p`被回收，副本也消失了(确切的说`p`指向的栈存储区被取消标记，可能随时被系统修改)，而函数返回的p指向的内容也变得不确定，文字存储区的 `" hello world"` 未改变。

解决：  
- 可以这样修改: 
- 方法1：`char* p= " hello world" ; return p;`，这里 `p` 直接指向文字存储区的 `" hello world"` ，函数按值返回`p`存储的地址，所以有效；   
- 方法2：`static char p[]= " hello world" ; return p;`，`static` 指出数组 `p` 为静态数组存储在内存的data区，函数结束也不会释放，所以有效。

## 7.代码分析

检查下面代码有什么问题？

```cpp
void GetMemory( char **p, int num )
{
    *p = (char *) malloc( num );
}
void Test( void )
{
    char *str = NULL;
    GetMemory( &str, 100 );
    strcpy( str, "hello" ); 
    printf( str ); 
}
```
