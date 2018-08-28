# 数组中只出现一次的数字

一个整型数组里除了两个数字之外，其他的数字都出现了偶数次。请写程序找出这两个只出现一次的数字。

```cpp
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data, int* num1, int* num2) {
        map<int, int> mp;
        for(int idx=0; idx<data.size(); idx++)
            mp[data[idx]]++;
        for(map<int, int>::iterator iter = mp.begin();
            iter != mp.end(); ++iter)
            if(iter->second==1) {
                if(!*num1) *num1 = iter->first;
                else if(!*num2) { *num2 = iter->first; break;}
            }
        return;
    }
};
```
