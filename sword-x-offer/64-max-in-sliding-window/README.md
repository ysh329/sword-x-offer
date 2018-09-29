# 滑动窗口的最大值

- 给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。  
- 例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}；
- 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

```cpp
class Solution {
public:
    vector<int> maxInWindows(const vector<int>& num, unsigned int size) {
        vector<int> res;
        if(num.size()<size || size<=0)
            return res;
        //1. 初始化找到当前size最大
        /*
        res.push_back(num[0]);
        for(int eidx = 0; eidx < size; eidx++)
            if(num[eidx]>res[0])
                res[0] = num[eidx];
        */
        //2. 开始cur_num的[size+1, num.size()]的遍历
        for(int cur_eidx = size; cur_eidx <= num.size(); cur_eidx++) {
            //2.1 内层遍历size次，[cur_num, cur_num+size]内的最大，存入res中
            int cur_size_max = num[cur_eidx-size];
            for(int in_eidx = cur_eidx-size; in_eidx < cur_eidx; in_eidx++)
                if(num[in_eidx] > cur_size_max)
                    cur_size_max = num[in_eidx];
            res.push_back(cur_size_max);
        }
        //3. 返回res
        return res;
    }
};
```
