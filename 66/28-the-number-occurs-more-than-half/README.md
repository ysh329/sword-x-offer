# 数组中出现次数超过一半的数字

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

## 排序

- 排序时间：O(nlogn) 
- STL的sort函数在数据量大时采用快排，分段递归排序，一旦分段后的数据小于某个值，就改用插入排序。如果递归层次过深，还会改用堆排序。这样就结合了各类算法的所有优点  
- [关于C++各类排序算法与std::sort性能的比较 - CSDN博客](https://blog.csdn.net/qq_24625045/article/details/49964173)

### 排序1

```cpp
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        if(numbers.empty()) return 0;
        else if(numbers.size()==1) return numbers[0]; //一个元素返回该元素
        
        sort(numbers.begin(), numbers.end());
        int lastIdx = 0, res = 0;
        for(int curIdx = 1; curIdx<numbers.size(); curIdx++) {
            if(numbers[lastIdx]!=numbers[curIdx]) {
                if(curIdx-lastIdx > numbers.size()>>1) {
                    res = numbers[lastIdx];
                    break;
                }
                lastIdx = curIdx;
            }
        }
        if(numbers.begin()==numbers.end()) //所有元素相同
            res = numbers[0];
        return res;
    }
};
```

### 排序2

- 数组排序后，如果符合条件的数存在，则一定是数组中间那个数

```cpp
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        if(numbers.empty()) return 0;
        sort(numbers.begin(), numbers.end());
        int middle = numbers[numbers.size()/2];
        int middle_times = count(numbers.begin(), numbers.end(), middle);
        int res = middle_times <= numbers.size()/2 ? 0 : middle;
        return res;
    }
};
```

## 快速排序思想  

- 若数组排序，则位于数组中间的数字，为出现次数超过数组长度一半的数字，即第n/2大的数字，统计学上的中位数；  
- 随机快速排序思想启发：随机选择一数字，调整数字次序，使比该数小的都在该数左侧，反之右侧；  
- 若该数下标为n/2，则该数为数组中位数，若该数下标大于n/2，则中位数位于该数左边，反之右边。进而可递归查找。

```cpp
class Solution {
    int Partition(vector<int>& nums, int low, int high) {
        int pivot = nums[low];//pivot:枢纽
        //可以是[low,high]区间一个随机数，也可以是三数取中，九数取中，详情见<<大话数据结构>>
        while(low<high) {
            while(low<high && pivot<nums[high]) high--;
            swap(nums[low], nums[high]);
            while(low<high && pivot>=nums[low]) low++;
            swap(nums[low], nums[high]);
        } 
        return low;
    }
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        if(numbers.empty()) return 0;
        int midx = numbers.size()>>1;
        int idx = Partition(numbers, 0, numbers.size()-1);
        int start = 0, end = numbers.size()-1;
        while(idx!=midx) {
            if(idx > midx) {
                end = idx - 1;
                idx = Partition(numbers, start, end);
            }
            else {
                start = idx + 1;
                idx = Partition(numbers, start, end);
            } 
        }
        int res = numbers[midx];
        int times = count(numbers.begin(), numbers.end(), res);
        res = times<=numbers.size()/2 ? 0 : res;
        return res;
    }
};
```

## 数组特点/阵地攻守

作者：cm问前程  
链接：https://www.nowcoder.com/questionTerminal/e8a1b01a2df14cb2b228b30ee6a92163  
来源：牛客网  

采用阵地攻守的思想  
- 第一个数字作为第一个士兵，守阵地，count = 1；
- 遇到相同元素，count++；
- 遇到不相同元素，即为敌人，同归于尽,count--；
- 当遇到count为0的情况，又以新的i值作为守阵地的士兵。
- 继续下去，到最后还留在阵地上的士兵，有可能是主元素。  
- 再加一次循环，记录这个士兵的个数看是否大于数组一半的长度即可。  

```cpp
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        if(numbers.empty()) return 0;// 空数组
        int res = numbers[0], times = 1;// 一个元素数组
        for(int eidx = 1; eidx < numbers.size(); eidx++) {
            if(!times) {
                res = numbers[eidx];
                times = 1;
            }
            else if(numbers[eidx] == res) times++;
            else times--;
        }
        if(res) {
            times = count(numbers.begin(), numbers.end(), res);
            res = times<=numbers.size()/2 ? 0 : res;
        }
        return res;
    }
};
```

## 哈希表

```cpp
class Solution {
public:
    int MoreThanHalfNum_Solution(vector<int> numbers) {
        map<int, int> mp;
        for(int eidx = 0; eidx < numbers.size(); eidx++) {
            ++mp[numbers[eidx]];
            if(mp[numbers[eidx]] > numbers.size()/2)
                return numbers[eidx];
        }
        return 0;
    }
};
```
