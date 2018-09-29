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
