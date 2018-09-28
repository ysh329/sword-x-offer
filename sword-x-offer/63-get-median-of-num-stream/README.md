# 数据流中的中位数

如何得到一个数据流中的中位数？  
- 如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。  
- 如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。  
- 我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。

## 分析

- 方式一：用两个优先队列来模拟两个堆
   - 用PriorityQueue来构建一个小顶堆和大顶堆  
   - 要求中位数，因而让大顶堆用来存较小的数，从大到小排列；小顶堆存较大的数，从小到大的顺序排序  
   - 显然中位数就是大顶堆的根节点与小顶堆的根节点和的平均数  
   - 插入过程中保证：小顶堆中的元素都大于等于大顶堆中的元素，所以每次塞值，并不是直接塞进去，而是从另一个堆中poll出一个最大（最小）的塞值  
   - 当数据流长度为偶数时，将这个值插入大顶堆中，再将大顶堆中根节点（即最大值）插入到小顶堆中；当数目为奇数的时候，将这个值插入小顶堆中，再讲小顶堆中根节点（即最小值）插入到大顶堆中。这样就可以保证，每次插入新值时，都保证小顶堆中值大于大顶堆中的值，并且都是有序的  
   - 由于第一个数是插入到小顶堆中的，所以在最后取中位数的时候，若是奇数，就从小顶堆中取即可  
   - 当长度为奇数时，中位数就是小顶堆的根节点；为偶数时，中位数为大顶堆和小顶堆两个根节点之和的平均数  
     
如传入的数据为：[5,2,3,4,1,6,7,0,8]，则输出是"5.00 3.50 3.00 3.50 3.00 3.50 4.00 3.50 4.00 "  
        a.那么，第一个数为5，count=0,那么存到小顶堆中，
            步骤是：先存到大顶堆；然后弹出大顶堆root，就是最大值给小顶堆，第一次执行完，就是小顶堆为5，count+1=1；    
            此时若要输出中位数，那么就是5.0，因为直接返回的是小顶堆最小值(第一次塞入到小顶堆中，是从大顶堆中找到最大的给他的)
        b.继续传入一个数为2，那么先存到小顶堆中，将小顶堆最小值弹出给大顶堆，即2，那么这次执行完，小顶堆为5，大顶堆为2，count+1=2
            此时若要输出中位数，因为是偶数，那么取两个头的平均值，即(5+2)/2=3.5(第二次塞入到大顶堆中，是从小顶堆中找到最小的给他的)
        c.继续传入一个数为3，那么此时count为偶数，那么执行第一个if，先存到大顶堆中，大顶堆弹出最大值，那么3>2，就是弹出3，3存到小顶堆中，那么此时小顶堆为3,5，大顶堆为2，count+1=3(第三次塞入到小顶堆中，是从大顶堆中找到最大的给他的)，此时若要输出中位数，因为是奇数，那么取小顶堆的最小值，即3.0  
        d.继续传入一个数为4，先存到小顶堆中，小顶堆此时为3,4，5,弹出最小值为3，给大顶堆。此时大顶堆为3,2,小顶堆为4,5，(第四次塞入到小顶堆中，是从大顶堆中找到最大的给他的)。此时若要输出中位数，因为是偶数，那么取两个头的平均值,即(3+4)/2=3.5  
        e.依次类推。。。  
     
方式三、插入排序，插入到对应的位置
```java
    LinkedList<Integer> data = new LinkedList<Integer>();
    public void Insert(Integer num) {
        for (int i = data.size() - 1; i >= 0 ; i--) {
            if (num >= data.get(i)){
                data.add(i+1,num);
                return;
            }
        }
        data.addFirst(num);
    }
```
    
## 排序

### 排序1：显式排序  

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

## 排序2：隐式排序，基于内部排序的堆结构 multiset

使用基于内部排序的堆结构，在需要计算中位数时，偶数长度直接返回中间两数平均值，奇数长度返回中间的数。这里说一下set和multiset的结构与能力区别。

**结构**

set和multiset会根据特定的排序原则将元素排序。两者不同之处在于，multisets允许元素重复，而set不允许重复。

和所有的标准关联容器类似，sets和multisets通常以平衡二叉树完成。

**能力**

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
        void Insert(int num)
        {
           if(((min.size()+max.size())&1)==0)//偶数时 ，放入最小堆
           {
              if(max.size()>0 && num<max[0])
              {
                // push_heap (_First, _Last),要先在容器中加入数据，再调用push_heap ()
                 max.push_back(num);//先将元素压入容器
                 push_heap(max.begin(),max.end(),less<int>());//调整最大堆
                 num=max[0];//取出最大堆的最大值
                 //pop_heap(_First, _Last)，要先调用pop_heap()再在容器中删除数据
                 pop_heap(max.begin(),max.end(),less<int>());//删除最大堆的最大值
                 max.pop_back(); //在容器中删除
              }
              min.push_back(num);//压入最小堆
              push_heap(min.begin(),min.end(),greater<int>());//调整最小堆
           }
           else//奇数时候，放入最大堆
           {
              if(min.size()>0 && num>min[0])
              {
                // push_heap (_First, _Last),要先在容器中加入数据，再调用push_heap ()
                 min.push_back(num);//先压入最小堆
                 push_heap(min.begin(),min.end(),greater<int>());//调整最小堆
                 num=min[0];//得到最小堆的最小值（堆顶）
                 //pop_heap(_First, _Last)，要先调用pop_heap()再在容器中删除数据
                 pop_heap(min.begin(),min.end(),greater<int>());//删除最小堆的最大值
                 min.pop_back(); //在容器中删除
              }
              max.push_back(num);//压入数字
              push_heap(max.begin(),max.end(),less<int>());//调整最大堆
           }   
        }
        /*获取中位数*/      
        double GetMedian()
        {
            int si敏感词.size()+max.size();
            if(size<=0) //没有元素，抛出异常
                return 0;//throw exception("No numbers are available");
            if((size&1)==0)//偶数时，去平均
                return ((double)(max[0]+min[0])/2);
            else//奇数，去最小堆，因为最小堆数据保持和最大堆一样多，或者比最大堆多1个
                return min[0];
        }
};
```

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
