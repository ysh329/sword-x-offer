# 孩子们的游戏(圆圈中最后剩下的数)

每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。

其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,
然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。

请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)

## 模拟

### 模拟1

- 环状链表

```cpp
略
```

### 模拟2

```cpp
class Solution {
public:
    int LastRemaining_Solution(int n, int m) {
        if(n==1) return 0;
        else if(m==1) return n-1;
        else if(n<1 || m<1) return -1;
        
        vector<int> circle;
        for(int cidx=0; cidx<n; cidx++)
            circle.emplace_back(cidx);
        
        int m_cnt = 0, cidx = -1;
        while(circle.size()>1) {
            m_cnt++;
            cidx = (cidx+1==circle.size()) ? 0 : cidx+1;
            if(m_cnt==m) {
                circle.erase(circle.begin()+cidx);
                m_cnt = 0;
                cidx = (cidx==0) ? circle.size()-1 : cidx-1;
            }
        }
        return circle[0];
    }
};
```

### 模拟3

- list容器+其迭代器实现圆形链表

```cpp
class Solution {
public:
    int LastRemaining_Solution(int n, int m) {//n为人数
        if(n<1 || m<1) return -1;
        list<int> circle;
        for(int i=0; i<n; i++) circle.emplace_back(i);
        list<int>::iterator current = circle.begin();
        while(circle.size() > 1) {
            for(int i=1; i<m; i++) {//走m-1步到达第m个数处 
                ++current;
                if(current==circle.end())
                    current = circle.begin();
            }
            list<int>::iterator next = ++current;
            if(next==circle.end())
                next = circle.begin();
            --current;
            circle.erase(current);
            current = next;
        }
        return *current;//对迭代器取值，等价于对指针取值
    }
};
```

## 找规律/公式法

### 思路1：倒推

参考：约瑟夫环——公式法（递推公式） - CSDN博客  
https://blog.csdn.net/u011500062/article/details/72855826

这个思路不太好理解，最后一个人的下标必然为0，则可以倒推出2个人的时候，胜利者的下标，进而递推/循环得到3个人的时候，以此类推，得到递推式。

```markdown
f(1,3)：只有1个人了，那个人就是获胜者，他的下标位置是0
f(2,3)=(f(1,3)+3)%2=3%2=1：在有2个人的时候，胜利者的下标位置为1
f(3,3)=(f(2,3)+3)%3=4%3=1：在有3个人的时候，胜利者的下标位置为1
f(4,3)=(f(3,3)+3)%4=4%4=0：在有4个人的时候，胜利者的下标位置为0
……
f(11,3)=6
```

进而找出规律，通项为：  
- `n>1, f(n,m) = {f(n-1,m) + m} % n`  
- `n=1, f(n,m) = 0`  

### 思路2：规律计算

该方法的另一个思路是，考虑第一个出圈人的下标为`k=(m-1)%n`，下一个k+1为新的起点，可以确定的是有n个人报数m的最后一个是f(n,m)与n-1个人报数m最后一个f'(n-1,m)的结果是一样的，即有`f(n,m)=f(n-1,m)`，即该映射长度为n-1的，从0到n-2的序列：
```shell
      old index         new index
(k+1)+0      = k+1   ->   0
(k+1)+1      = k+2   ->   1
(k+1)+2      = k+3   ->   2
...
...
(k+1)+n-2k-3 = n-1   ->   n-k-2
(k+1)+n-2k-2 = 0     ->   n-k-1
(k+1)+n-2k-1 = 1     ->   n-k
...
...
(k+1)-3      = k-2   ->   n-3
(k+1)-2      = k-1   ->   n-2
```
把旧的下标x映射为新下标的过程定义为p，有p(x)=(x-(k+1))%n。该映射的逆映射是p^(-1)(x)=(x+(k+1))%n。但这个逆映射的计算没理解是怎么算出来的。

由于映射前后有同样的形式，即都是从0开始的序列，因而可以用函数f表示，记为f(n-1,m)。根据映射规则，映射前的序列中最后剩下的数字f'(n-1,m)=p^(-1)[f(n-1,m)]=[f(n-1,m)+k+1]%n，把k=(m-1)%n代入后得到f(n,m)=f'(n-1,m)=[f(n-1,m)+m]%n。

最终可得到一个递归式，要得到n个数字的序列中剩下的数字，只需要得到n-1个数字的序列中最后剩下的数字。以此类推，当n=1时，即序列中开始只有一个数字，其下标为0，那么显然可以推出前一个，这种关系表示为：

- f(n,m) = 0, n=1  
- f(n,m) = {f(n-1,m) + m} % n, n>1  

### 递归

```cpp
class Solution {
public:
    int LastRemaining_Solution(unsigned int n, unsigned int m) {
        if(n==0) return -1; //学生数为0异常
        if(n==1) return 0; //学生数为1返回第1个
        else return (LastRemaining_Solution(n-1, m) + m) % n;
    }
};
```

### 非递归

```cpp
class Solution {
public:
    int LastRemaining_Solution(int n, int m) {
        if(n<1 || m<1) return -1;
        int last = 0;
        for(int i=2; i<=n; i++)
            last = (last+m)%i;
        return last;
    }
};
```
