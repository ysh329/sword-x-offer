# 滑动窗口的最大值

- 给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。  
- 例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}；
- 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

## 两层循环

```cpp
class Solution {
public:
    vector<int> maxInWindows(const vector<int>& num, unsigned int size) {
        if(size<=0 || num.empty() || num.size()<size) return vector<int>();
        vector<int> res(int(num.size())-size+1, 0);
        for(int start = 0;  start <= int(num.size())-size; start++) {
            int max = num[start];
            for(int idx = 1; idx < size; idx++)
                max = (max < num[start+idx]) ? num[start+idx] : max;
            res[start] = max;
        }
        return res;
    }
};
```

## 单调队列

单调队列，O(n)

```cpp
class Solution {
public:
    vector<int> maxInWindows(const vector<int>& a, unsigned int k){
        vector<int> res;
        deque<int> s;
        for(unsigned int i = 0; i < a.size(); ++i){
            while(s.size() && a[s.back()] <= a[i]) s.pop_back();
            while(s.size() && i - s.front() + 1 > k) s.pop_front();
            s.push_back(i);
            if(k && i + 1 >= k) res.push_back(a[s.front()]);
        }
        return res;
    }
};
```
补充：
- std::deque是双端队列，可以高效的在头尾两端插入和删除元素，在std::deque两端插入和删除并不会使其它元素的指针或引用失效。在接口上和std::vector相似。  
- 与vector相反，deque中的元素并非连续存储：典型的实现是使用一个单独分配的固定大小数组的序列。std::deque的存储空间会自动按需扩大和缩小。扩大std::deque比扩大std::vector要便宜，因为它不涉及到现有元素复制到新的内存位置。

- 双端队列（Double-ended queue，缩写为Deque）是一个大小可以动态变化（Dynamic size）且可以在两端扩展或收缩的顺序容器。顺序容器中的元素按照严格的线性顺序排序。可以通过元素在序列中的位置访问对应的元素。  
- 不同的库可能会按不同的方式来实现双端队列，通常实现为某种形式的动态数组。但不管通过哪种方式，双端队列都允许通过随机迭代器直接访问各个元素，且内部的存储空间会按需求自动地扩展或收缩。容器实际分配的内存数超过容纳当前所有有效元素所需的，因为额外的内存将被未来增长的部分所使用。就因为这点，当插入元素时，容器不需要太频繁地分配内存。  
- 因此，双端队列提供了类似向量（std::vector）的功能，且不仅可以在容器末尾，还可以在容器开头高效地插入或删除元素。但是，相比向量，双端队列不保证内部的元素是按连续的存储空间存储的，因此，不允许对指针直接做偏移操作来直接访问元素。在内部，双端队列与向量的工作方式完全不同：向量使用单数组数据结构，在元素增加的过程中，需要偶尔的内存重分配，而双端队列中的元素被零散地保存在不同的存储块中，容器内部会保存一些必要的数据使得可以以恒定时间及一个统一的顺序接口直接访问任意元素。因此，双端队列的内部实现比向量的稍稍复杂一点，但这也使得它在一些特定环境下可以更高效地增长，特别是对于非常长的序列，内存重分配的代价是及其高昂的。对于大量涉及在除了起始或末尾以外的其它任意位置插入或删除元素的操作，相比列表（std::list）及正向列表（std::forward_list），deque 所表现出的性能是极差的，且操作前后的迭代器、引用的一致性较低。


## 双端队列

时间复杂度o（n），空间复杂度为o（n）
/*思路就是采用双端队列，队列中的头节点保存的数据比后面的要大。
      比如当前假如的数据比队尾的数字大，说明当前这个数字最起码在从现在起到后面的过程中可能是最大值
      ，而之前队尾的数字不可能最大了，所以要删除队尾元素。
      此外，还要判断队头的元素是否超过size长度，由于存储的是下标，所以可以计算得到；
      特别说明，我们在双端队列中保存的数字是传入的向量的下标；
*/

```cpp
class Solution {
public:
    //

    vector<int> maxInWindows(const vector<int>& num, unsigned int size)
    {
        vector<int> vec;
        if(num.size()<=0 || num.size()<size ||size<=0) return vec;//处理特殊情况
        deque<int> dq;
        //处理前size个数据，因为这个时候不需要输出最大值；
        for(unsigned int i=0;i<size;i++)
        {
            //假如当前的元素比队列队尾的元素大，说明之前加入的这些元素不可能是最大值了。因为当前的这个数字比之前加入队列的更晚
            while(!dq.empty()&&num[i]>=num[dq.back()])
                dq.pop_back();//弹出比当前小的元素下标
            dq.push_back(i);//队尾压入当前下标
        }
        //处理size往后的元素，这时候需要输出滑动窗口的最大值
        for(unsigned int i=size;i<num.size();i++)
        {
            vec.push_back(num[dq.front()]);
            while(!dq.empty()&&num[i]>=num[dq.back()])
                dq.pop_back();
            if(!dq.empty() && dq.front()<=(int)(i-size))//判断队头的下标是否超出size大小，如果超过，要删除队头元素
                dq.pop_front();//删除队头元素
            dq.push_back(i);//将当前下标压入队尾，因为可能在未来是最大值
        }
        vec.push_back(num[dq.front()]);//最后还要压入一次
        return vec;
    }
};
```
