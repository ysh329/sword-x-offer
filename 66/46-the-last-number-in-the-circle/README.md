# 孩子们的游戏(圆圈中最后剩下的数)

每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。

其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,
然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。

请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)

## 模拟

### 模拟1

- 环状链表

```cpp
public class Solution {  
    // 构造循环链表结构
    public static class Node {
        private int value;
        private Node pre;
        private Node next;
        public Node(int value) {
            this.value = value;
            this.pre = null;
            this.next = null;
        }
    }
    public int LastRemaining_Solution(int n, int m) {
        if (m == 0 && n == 0) {
            return -1;
        }
        Node head = new Node(0);
        Node cur = head;
        Node last = null;
        for(int i = 1; i < n; i++) {
            last = new Node(i);
            last.pre = cur;
            cur.next = last;
            cur = last;
        }
        cur = head;
        last.next = head;
        head.pre = last;
        int count = n;
        while (count != 1) {
            for (int i = 0; i < m - 1; i++) {
                cur = cur.next;
            }
            cur.pre.next = cur.next;
            cur.next.pre = cur.pre;
            cur = cur.next;
            count--;
        }
        return cur.value;
    }
}
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

## 找规律

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

- 找出规律，通项为：`f(n,m) = {f(n-1,m)+m} % n`

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
