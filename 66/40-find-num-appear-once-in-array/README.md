# 数组中只出现一次的数字

一个整型数组里除了两个数字之外，其他的数字都出现了偶数次。请写程序找出这两个只出现一次的数字。

## 哈希表

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

## set

```cpp
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
        set<int> s;
        for(int idx=0; idx<data.size(); idx++) {
            if(s.find(data[idx]) == s.end()) s.insert(data[idx]);
            else s.erase(data[idx]);
        }
        set<int>::iterator iter = s.begin();
        *num1 = *iter; *num2 = *(++iter);
        return;
    }
};
```

## 异或

```cpp
链接：https://www.nowcoder.com/questionTerminal/e02fdb54d7524710a7d664d082bb7811
来源：牛客网

class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
        int diff = accumulate(data.begin(), data.end(), 0, bit_xor<int>());
        diff &= -diff;  //即找到最右边1-bit
        *num1 = 0; *num2 = 0;
        for(auto c:data) {
            if((c&diff) == 0) *num1 ^= c;
            else *num2 ^ =c;
        }
    }
};
```

```cpp
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data, int* num1, int *num2) {
        if(data.empty()) return;
        int x = 0;
        for(auto i : data) x ^= i;
        int n1 = (~(~x + 1)) + 1; // 关键所在！n1是分组依据，x - (x & (x-1))估计还更易读点儿
        *num1 = *num2 = 0;
        for(auto i : data) {
            if(i & n1) *num1 ^= i;
            else *num2 ^= i;
        }
    }
};
```
