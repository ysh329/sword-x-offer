# 旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

## 顺序查找

- O(N)  

```cpp
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int result = 0;
        if(rotateArray.size()<=1)
            return result;
        for(int eidx = 0; eidx < rotateArray.size(); eidx++)
        {
            if(rotateArray[eidx]<=0)
                break;
            else if(eidx>=1 && rotateArray[eidx]<rotateArray[eidx-1])
            {
                result = rotateArray[eidx];
                break;
            }
        }
        return result;
    }
};
```

## 二分查找

- O(log_2 {N})  
- 链接：https://www.nowcoder.com/questionTerminal/9f3231a991af4f55b95579b44b7a01ba  
- 来源：牛客网  

```cpp
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int result = 0;
        if(rotateArray.size()<=1)
            return result;
        
        int start_idx = 0;
        int end_idx = rotateArray.size()-1;
        
        while(start_idx<end_idx)
        {
            int mid_idx = start_idx + (end_idx-start_idx)/2;
            if(rotateArray[mid_idx] > rotateArray[end_idx])
                start_idx = mid_idx+1;
            else if(rotateArray[mid_idx] == rotateArray[end_idx])
                end_idx -= 1;
            else // rotateArray[mid_idx] < rotateArray[end_idx]
                end_idx = mid_idx;
        }
        return rotateArray[start_idx];
    }
};
```
