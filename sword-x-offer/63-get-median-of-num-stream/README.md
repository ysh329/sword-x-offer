# 数据流中的中位数

如何得到一个数据流中的中位数？  
- 如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。  
- 如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。  
- 我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。


## 每次排序

```cpp
链接：https://www.nowcoder.com/questionTerminal/9be0172896bd43948f8a32fb954e1be1
来源：牛客网

class Solution {
public:
    void Insert(int num) {
        data.push_back(num);
        std::sort(data.begin(), data.end());
    }
 
    double GetMedian() {
        unsigned int size = data.size();
        if (size & 1) {
            return data[size >> 1];
        } else {
            int left = data[(size >> 1) - 1];
            int right = data[size >> 1];
            return (static_cast<double>(left) + right) / 2;
        }
    }
private:
    vector<int> data;
};
```

## 简洁cpp

```cpp
链接：https://www.nowcoder.com/questionTerminal/9be0172896bd43948f8a32fb954e1be1
来源：牛客网

class Solution {
public:
    multiset<int> rec;
    void Insert(int num)
    {
        rec.insert(num);
    }
    double GetMedian()
    { 
    auto it1=rec.begin(), it2=it1;
        advance(it1, rec.size()/2);
        advance(it2, rec.size()/2-!(rec.size()%2));
        return (*it1+*it2)/2.0;
    }
};
```

## 最小堆

```cpp
class Solution {
public:
    priority_queue<int, vector<int>, less<int>> p;//排序右半边，让其多一个
    priority_queue<int, vector<int>, greater<int>> q;//排序左半边
    void Insert(int num) {
        //小于最小堆里最小的，那么放入最大堆
        //保证了给最大堆的元素，均小于最小堆的最小元素
        if(p.empty() || num<=p.top()) p.push(num);
        else q.push(num);
        if(p.size() == q.size()+2) {q.push(p.top()); p.pop();}
        if(p.size()+1 == q.size()) {p.push(q.top()); q.pop();}
    }
    double GetMedian() { 
        return q.size()==p.size() ?
               (q.top()+p.top())/2.0 : p.top();
    }
};
```

## 两个堆实现

```cpp
链接：https://www.nowcoder.com/questionTerminal/9be0172896bd43948f8a32fb954e1be1
来源：牛客网

class Solution {
public:
    void Insert(int num)
    {
        count+=1;
        // 元素个数是偶数时,将小顶堆堆顶放入大顶堆
        if(count%2==0){
            big_heap.push(num);
            small_heap.push(big_heap.top());
            big_heap.pop();
        }
        else{
            small_heap.push(num);
            big_heap.push(small_heap.top());
            small_heap.pop();
        }
    }
 
    double GetMedian()
    {
        if(count&0x1){
            return big_heap.top();
        }
        else{
            return double((small_heap.top()+big_heap.top())/2.0);
        }
    }
private:
    int count=0;
    priority_queue<int, vector<int>, less<int>> big_heap;        // 左边一个大顶堆
    priority_queue<int, vector<int>, greater<int>> small_heap;   // 右边一个小顶堆
    // 大顶堆所有元素均小于等于小顶堆的所有元素.
};
```

## 两个堆实现：vector

```cpp
链接：https://www.nowcoder.com/questionTerminal/9be0172896bd43948f8a32fb954e1be1
来源：牛客网

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

## 动手实现堆

链接：https://www.nowcoder.com/questionTerminal/9be0172896bd43948f8a32fb954e1be1
来源：牛客网

思路：构建一棵"平衡二叉搜索树 "。
每个结点左子树均是小于等于其value的值，右子树均大于等于value值。每个子树均按其 “结点数” 调节平衡。 这样根节点一定是中间值中的一个。若结点数为奇数，则返回根节点的值；若结点个数为偶数，则再从根结点左子数或右子数中个数较多的子树中选出最大或最小值既可。

```cpp

struct myTreeNode
{
    int val;
    int count;//以此节点为根的树高
    struct myTreeNode* left;
    struct myTreeNode* right;
    myTreeNode(int v) :
        val(v),
        count(1), left(NULL), right(NULL) {}
 
};
 
myTreeNode *root = NULL;
 
class Solution
{
public:
 
    /*计算以节点为根的树的高度
    */
    int totalCount(myTreeNode* node)
    {
        if (node == NULL)
            return 0;
        else
            return node->count;
    }
 
    //左左
    void rotateLL(myTreeNode* &t)
    {
        myTreeNode* k = t->left;
        myTreeNode* tm = NULL;
        while (k->right != NULL)
        {
            k->count--;
            tm = k;
            k = k->right;
             
        }
        if (k != t->left)
        {
            k->left = t->left;
            tm->right = NULL;
        }
        t->left = NULL;
        k->right = t;
 
 
        t->count = totalCount(t->left) + totalCount(t->right) + 1;
        k->count = totalCount(k->left) + t->count + 1;
 
        t = k;
    }
 
    //右右
    void rotateRR(myTreeNode* &t)
    {
        myTreeNode* k = t->right;
        myTreeNode* tm = NULL;
        while (k->left != NULL) {
            k->count--;
            tm = k;
            k = k->left;
             
        }
        if (k != t->right)
        {
            k->right = t->right;
            tm->left = NULL;
        }
             
        t->right = NULL;
        k->left = t;
 
        t->count = totalCount(t->left) + 1;
        k->count = totalCount(k->right)+ t->count + 1;
        t = k;
    }
 
    //左右
    void rotateLR(myTreeNode* &t)
    {
        rotateRR(t->left);
        rotateLL(t);
    }
 
    //右左
    void rotateRL(myTreeNode* &t)
    {
        rotateLL(t->right);
        rotateRR(t);
    }
 
    //插入
    void insert(myTreeNode* &root, int x)
    {
        if (root == NULL)
        {
            root = new myTreeNode(x);
            return;
        }
         
        if (root->val >= x)
        {
            insert(root->left, x);
            root->count = totalCount(root->left)+ totalCount(root->right) + 1;
            if (2 == totalCount(root->left) - totalCount(root->right))
            {
                if (x < root->left->val)
                {
                    rotateLL(root);
                }
                else
                {
                    rotateLR(root);
                }
            }
        }
        else
        {
            insert(root->right, x);
            root->count = totalCount(root->left)+ totalCount(root->right) + 1;
            if (2 == totalCount(root->right) - totalCount(root->left))
            {
                if (x > root->right->val)
                {
                    rotateRR(root);
                }
                else {
                    rotateRL(root);
                }
            }
        }
 
    }
 
    void deleteTree(myTreeNode* root)
    {
        if (root == NULL)return;
        deleteTree(root->left);
        deleteTree(root->right);
        delete root;
        root = NULL;
    }
     
    void Insert(int num)
    {
        insert(root, num);
    }
 
    double GetMedian()
    {
        int lc = totalCount(root->left), rc = totalCount(root->right);
        if ( lc == rc)
            return root->val;
        else
        {
            bool isLeft = lc > rc ;
            myTreeNode* tmp ;
            if (isLeft)
            {
                tmp = root->left;
                while (tmp->right != NULL)
                {
                    tmp = tmp->right;
                }
            }
            else {
                tmp = root->right;
                while (tmp->left != NULL)
                {
                    tmp = tmp->left;
                }
            }
            return (double)(root->val + tmp->val) / 2.0;
        }
    }
 
};
```
