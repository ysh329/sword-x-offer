# 数据流中的中位数

如何得到一个数据流中的中位数？  
- 如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。  
- 如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。  
- 我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。
    
## 排序

### 排序1：插入排序

插入时比较

```cpp
class Solution {
private:
    vector<int> a;
public:
    void Insert(int num) {
        for(auto i=a.begin(); i!=a.end(); i++) {
            if(num>*i) {
                a.insert(i, num);
                return;
            }
        }
        a.emplace_back(num);
    }

    double GetMedian() {
        if(a.empty()) return 0.0;
        return (a.size()&1==1)?
            a[a.size()/2]:
            (a[a.size()/2]+a[a.size()/2-1]) / 2.0;
    }
};
```

### 排序2：显式排序  

通过每次插入数据后进行排序，最终取数组中位数

```cpp
class Solution {
public:
    vector<int> data;
    void Insert(int num) {
        data.push_back(num);
        sort(data.begin(), data.end());
    }
    double GetMedian() {
        unsigned int size = data.size();
        if(size & 1)
            return data[size >> 1];
        else {
            int left = data[(size >> 1) - 1];
            int right = data[size >> 1];
            return (static_cast<double>(left) + right) / 2;
        }
    }
};
```

## 排序3：隐式排序，基于内部排序的堆结构 multiset

使用基于内部排序的堆结构，在需要计算中位数时，偶数长度直接返回中间两数平均值，奇数长度返回中间的数。这里说一下set和multiset的结构与能力区别。

**set与multiset的结构**

set和multiset会根据特定的排序原则将元素排序。两者不同之处在于，multisets允许元素重复，而set不允许重复。

和所有的标准关联容器类似，sets和multisets通常以平衡二叉树完成。

**set与multiset的能力**

自动排序的主要优点在于使二叉树搜寻元素具有良好的性能，在其搜索函数算法具有对数复杂度。但是自动排序也造成了一个限制，不能直接改变元素值，因为这样会打乱原有的顺序，要改变元素的值，必须先删除旧元素，再插入新元素。所以sets和multisets具有以下特点：

- 不提供直接用来存取元素的任何操作元素；  
- 通过迭代器进行元素的存取。

```cpp
class Solution {
public:
    multiset<int> rec;
    void Insert(int num) {
        rec.insert(num);
    }
    double GetMedian() { 
        auto it1 = rec.begin(), it2 = it1;
        advance(it1, rec.size()/2);
        advance(it2, rec.size()/2-!(rec.size()%2)); // 偶数长度返回it前一个，奇数长度it1与it2一样
        return (*it1+*it2)/2.0;
    }
};
```
补充：std::advance函数  
- template< class InputIt, class Distance >  
- constexpr void advance( InputIt& it, Distance n );  
增加给定的迭代器 it 以 n 个元素的步长。若 n 为负，则迭代器自减。该情况下， InputIt 必须满足双向迭代器 (BidirectionalIterator) 的要求，否则行为未定义。  

参考：  
- [std::advance - cppreference.com](https://zh.cppreference.com/w/cpp/iterator/advance)  
- [【C++ STL】Set和Multiset - Memset - 博客园](https://www.cnblogs.com/ChinaHook/p/6985444.html)

## 两个堆实现

### 两个堆实现1

- 新元素小于最小堆里最小的，放入最大堆，保证给最大堆的元素均小于最小堆的最小元素；
- 总元素个数为偶数，中位数为最大堆和最小堆顶部元素的平均值；
- 总元素个数为奇数，中位数为最小堆顶部元素。

```cpp
class Solution {
private:
    priority_queue<int, vector<int>, less<int>> p;    //排序右半边，让其多一个
    priority_queue<int, vector<int>, greater<int>> q; //排序左半边
public:
    void Insert(int num) {
        // 小于最小堆里最小的，那么放入最大堆。保证了给最大堆的元素，均小于最小堆的最小元素
        if(p.empty() || num<=p.top()) p.push(num);
        else q.push(num);
        // 平衡两个堆的元素个数
        if(p.size() == q.size()+2) {q.push(p.top()); p.pop();}
        if(p.size()+1 == q.size()) {p.push(q.top()); q.pop();}
    }
    double GetMedian() { 
        return q.size()==p.size() ?
               (q.top()+p.top())/2.0 : p.top();
    }
};
```

### 两个堆实现2

```cpp
class Solution {
private: // 大顶堆所有元素均小于等于小顶堆的所有元素.
    int count = 0;
    priority_queue<int, vector<int>, less<int>> big_heap;        // 左边一个大顶堆
    priority_queue<int, vector<int>, greater<int>> small_heap;   // 右边一个小顶堆
public:
    void Insert(int num) {
        count++;
        if((count & 1) != 1) { // 总元素个数为偶数，将小顶堆堆顶放入大顶堆
            big_heap.push(num);
            small_heap.push(big_heap.top());
            big_heap.pop();
        }
        else {
            small_heap.push(num);
            big_heap.push(small_heap.top());
            small_heap.pop();
        }
    }
    double GetMedian() {
        return (count & 0x1) ?
               big_heap.top() :
               double((small_heap.top()+big_heap.top())/2.0);
    }
};
```

### 两个堆实现3：动态数组

```cpp
class Solution {
private:
    vector<int> min;
    vector<int> max;
public:
    void Insert(int num) {
        if(((min.size()+max.size())&1) == 0) { //总长为偶数，放入最小堆
            if(max.size() && num<max[0]) {
                max.push_back(num);
                push_heap(max.begin(), max.end(), less<int>());
                num = max[0];
                pop_heap(max.begin(), max.end(), less<int>());
                max.pop_back();
            }
            min.push_back(num);
            push_heap(min.begin(), min.end(), greater<int>());
        }
        else {
            if(min.size() && num>min[0]) { //奇数，放入最大堆
                min.push_back(num);
                push_heap(min.begin(), min.end(), greater<int>());
                num = min[0];
                pop_heap(min.begin(), min.end(), greater<int>());
                min.pop_back();
            }
            max.push_back(num);
            push_heap(max.begin(), max.end(), less<int>());
        }
    }
    double GetMedian() {
        int size = min.size() + max.size();
        if(size <= 0) return 0;
        if((size&1) == 0) return ((double)(max[0]+min[0]) / 2);
        return min[0];
    }
};
```

### 两个堆实现4：动手实现堆

- 构建一棵平衡二叉搜索树：每个节点大于等于其左孩子，小于等于其右孩子。每个子树按其 “结点数” 调节平衡  
- 因此，根节点一定是中间值中的一个：若总结点数为奇数个，则返回根节点的值；若为偶数个，则再从根结点左子树或右子树中个数较多的子树中选出最大或最小值既可，与根节点加和后平均  

```cpp

struct myTreeNode
{
    int val;
    int count; //以此节点为根的树高
    struct myTreeNode* left;
    struct myTreeNode* right;
    myTreeNode(int v) :
        val(v),
        count(1), left(nullptr), right(nullptr) {}
 
};

myTreeNode *root = NULL;
 
class Solution
{
public:
 
    /*计算以节点为根的树的高度*/
    int totalCount(myTreeNode* node) {
        if(!node) return 0;
        else return node->count;
    }
 
    //左左
    void rotateLL(myTreeNode* &t) {
        myTreeNode* k = t->left;
        myTreeNode* tm = nullptr;
        while(k->right) {
            k->count--;
            tm = k;
            k = k->right;             
        }
        if(k != t->left) {
            k->left = t->left;
            tm->right = nullptr;
        }
        t->left = nullptr;
        k->right = t;
        
        t->count = totalCount(t->left) + totalCount(t->right) + 1;
        k->count = totalCount(k->left) + t->count + 1;
 
        t = k;
    }
    //右右
    void rotateRR(myTreeNode* &t) {
        myTreeNode* k = t->right;
        myTreeNode* tm = NULL;
        while(k->left) {
            k->count--;
            tm = k;
            k = k->left;
        }
        if (k != t->right) {
            k->right = t->right;
            tm->left = NULL;
        }
             
        t->right = nullptr;
        k->left = t;
 
        t->count = totalCount(t->left) + 1;
        k->count = totalCount(k->right)+ t->count + 1;
        t = k;
    }
 
    //左右
    void rotateLR(myTreeNode* &t) {
        rotateRR(t->left);
        rotateLL(t);
    }
 
    //右左
    void rotateRL(myTreeNode* &t) {
        rotateLL(t->right);
        rotateRR(t);
    }
 
    //插入
    void insert(myTreeNode* &root, int x) {
        if(!root) {
            root = new myTreeNode(x);
            return;
        }
         
        if(root->val >= x) {
            insert(root->left, x);
            root->count = totalCount(root->left) + totalCount(root->right) + 1;
            if(2 == totalCount(root->left)-totalCount(root->right)) {
                if (x < root->left->val)
                    rotateLL(root);
                else
                    rotateLR(root);
            }
        }
        else {
            insert(root->right, x);
            root->count = totalCount(root->left)+ totalCount(root->right) + 1;
            if(2 == totalCount(root->right) - totalCount(root->left)) {
                if (x > root->right->val) 
                    rotateRR(root);
                else
                    rotateRL(root);
            }
        }
    }
 
    void deleteTree(myTreeNode* root) {
        if(!root) return;
        deleteTree(root->left);
        deleteTree(root->right);
        delete root;
        root = nullptr;
    }
    
    void Insert(int num) {
        insert(root, num);
    }
 
    double GetMedian() {
        int lc = totalCount(root->left), rc = totalCount(root->right);
        if(lc == rc)
            return root->val;
        else {
            bool isLeft = lc > rc ;
            myTreeNode* tmp ;
            if(isLeft) {
                tmp = root->left;
                while (tmp->right)
                    tmp = tmp->right;
            }
            else {
                tmp = root->right;
                while (tmp->left)
                    tmp = tmp->left;
            }
            return (double)(root->val + tmp->val) / 2.0;
        }
    }
};
```
