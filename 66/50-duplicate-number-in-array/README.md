# 数组中重复的数字

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

## 哈希表

### 哈希表1

- 时间复杂度:O(n), 空间复杂度：O(n)

```cpp
class Solution {
public:
    // Parameters:
    //        numbers:     an array of integers
    //        length:      the length of array numbers
    //        duplication: (Output) the duplicated number in the array number
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    bool duplicate(int numbers[], int length, int* duplication) {
        // 空串,1字符，2字符
        // 有重复，无重复，全重复
        bool found = false;
        if(numbers==NULL || length<=1) return found;
        int *num_dict = (int*)calloc(length, sizeof(int));
        for(int nidx = 0; nidx < length; nidx++) {
            int num = numbers[nidx];
            if(num>length-1) break;//不满足元素的值域在[0,n-1]
            if((++num_dict[num])>=2) {
                *duplication = num;
                found = true;
                break;
            }
        }
        return found;
    }
};
```

### 哈希表2

- 时间复杂度:O(n), 空间复杂度：O(n)

```cpp
class Solution {
public:
    bool duplicate(int numbers[], int length, int* duplication) {
        vector<bool> dict(length, false);
        for(int nidx = 0; nidx < length; nidx++) {
            if(dict[numbers[nidx]]==true) {
                *duplication = numbers[nidx];
                return true;
            }
            dict[numbers[nidx]] = true;
        }
        return false;
    }
};
```

### 哈希表3

- 时间复杂度:O(n), 空间复杂度：O(1)  
- 考虑到数组有效元素的值均在`[0, length-1]`的这一特性  
- 将数组下标和值作为哈希表的key-value  
- 确保`nidx==numbers[nidx]`,否则交换`swap(numbers[nidx], numbers[numbers[nidx]])`  
- 若当前遍历到的第nidx个元素的值与数组numbers[numbers[nidx]]相同，即`numbers[nidx]==numbers[numbers[nidx]]`,则说明有重复

```cpp
class Solution {
public:
    bool duplicate(int numbers[], int length, int* duplication) {
        if(length<=0 || numbers==NULL) return false;
        for(int nidx=0; nidx<length; nidx++) {
            if(numbers[nidx]<0 || length-1<numbers[nidx]) return false;
            while(numbers[nidx]!=nidx) {
                if(numbers[nidx]==numbers[numbers[nidx]]) {
                    *duplication = numbers[nidx];
                    return true;
                }
                swap(numbers[nidx], numbers[numbers[nidx]]);
            }
        }
        return false;
    }
};
```
