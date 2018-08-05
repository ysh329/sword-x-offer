# 替换空格

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

## 倒序遍历

- in-place  
- O(N)

```cpp
class Solution {
public:
    void replaceSpace(char *str,int length) {
        if(str==NULL || length<=0)
            return;
        
        int space_count = 0;
        for(int sidx = 0; sidx < length; sidx++)
            if(str[sidx]==' ')
                space_count++;
        
        str[space_count*2+length] = '\0';
        for(int sidx = space_count*2+length-1; sidx >= 0; sidx--)
        {
            length--;
            if(str[length]==' ')
            {
                str[sidx] = '0';
                str[sidx-1] = '2';
                str[sidx-2] = '%';
                sidx -= 2;
            }
            else
                str[sidx] = str[length];
        }
        return;
    }
};
```
