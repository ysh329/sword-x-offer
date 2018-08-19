# 数组中出现次数超过一半的数字

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

## 排序

- 排序时间：O(nlogn) 
- STL的sort函数在数据量大时采用快排，分段递归排序，一旦分段后的数据小于某个值，就改用插入排序。如果递归层次过深，还会改用堆排序。这样就结合了各类算法的所有优点。

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

# partition

```cpp

```
