# 滑动窗口的最大值

- 给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。  
- 例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}；
- 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

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
