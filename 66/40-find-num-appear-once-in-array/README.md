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

### 异或1

```cpp
class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
        if(data.size()<2) return;
        int xor_res = 0;
        for(auto e: data) xor_res^=e;
        int idxOf1 = FindIdxOf1(xor_res);
        *num1 = *num2 = 0;
        for(auto e: data) {
            if((e>>idxOf1)&1 == 1) *num1^=e;
            else *num2^=e;
        } return;
    }
    int FindIdxOf1(int num) {
        int idx = 0;
        while((num&1)==0 && idx<8*sizeof(int)) {//注意(num&1)==0要带括号，==优先级高于&
            idx++; num >>= idx;
        } return idx;
    }
};
```

### 异或2

- 正数的反码与原码相同，负数的二进制形式为反码，即对该数的原码除符号位外各位取反后+1  
- 数n与其负数-n做与运算，结果一定为1，

```cpp
链接：https://www.nowcoder.com/questionTerminal/e02fdb54d7524710a7d664d082bb7811
来源：牛客网

class Solution {
public:
    void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
        int diff = accumulate(data.begin(), data.end(), 0, bit_xor<int>());
        diff &= -diff;  //即找到最右边1-bit，正数的反码与原码相同，负数的二进制形式为反码，即对该数的原码除符号位外各位取反后+1
        *num1 = 0; *num2 = 0;
        for(auto c:data) {
            if((c&diff) == 0) *num1 ^= c;
            else *num2 ^=c;
        }
    }
};
```

### 异或3

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

## 其他

```cpp
链接：https://www.nowcoder.com/questionTerminal/e02fdb54d7524710a7d664d082bb7811
来源：牛客网

/**
     * 数组中有两个出现一次的数字，其他数字都出现两次，找出这两个数字
     * @param array
     * @param num1
     * @param num2
     */
    public static void findNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        if(array == null || array.length <= 1){
            num1[0] = num2[0] = 0;
            return;
        }
        int len = array.length, index = 0, sum = 0;
        for(int i = 0; i < len; i++){
            sum ^= array[i];
        }
        for(index = 0; index < 32; index++){
            if((sum & (1 << index)) != 0) break;
        }
        for(int i = 0; i < len; i++){
            if((array[i] & (1 << index))!=0){
                num2[0] ^= array[i];
            }else{
                num1[0] ^= array[i];
            }
        }
    }
/**
     * 数组a中只有一个数出现一次，其他数都出现了2次，找出这个数字
     * @param a
     * @return
     */
    public static int find1From2(int[] a){
        int len = a.length, res = 0;
        for(int i = 0; i < len; i++){
            res = res ^ a[i];
        }
        return res;
    }
/**
     * 数组a中只有一个数出现一次，其他数字都出现了3次，找出这个数字
     * @param a
     * @return
     */
    public static int find1From3(int[] a){
        int[] bits = new int[32];
        int len = a.length;
        for(int i = 0; i < len; i++){
            for(int j = 0; j < 32; j++){
                bits[j] = bits[j] + ( (a[i]>>>j) & 1);
            }
        }
        int res = 0;
        for(int i = 0; i < 32; i++){
            if(bits[i] % 3 !=0){
                res = res | (1 << i);
            }
        }
        return res;
    }
```
