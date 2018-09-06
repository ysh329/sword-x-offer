# 构建乘积数组

给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素`B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]`。不能使用除法。

```
b0:   1 a1 a2 a3 a4
b1:  a0  1 a2 a3 a4
b2:  a0 a1  1 a3 a4
b3:  a0 a1 a2  1 a4
b4:  a0 a1 a2 a3  1
```

## 计算每个元素

- 时间复杂度：O(n^2)

```cpp
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        vector<int> B(A.size());
        for(int idxB=0; idxB<A.size(); idxB++) {
            int prod = 1;
            for(int idxA=0; idxA<A.size(); idxA++)
                prod *= idxA==idxB ? 1 : A[idxA];
            B[idxB] = prod;
        }
        return B;
    }
};
```

## 分别计算上/下三角

- 时间复杂度：O(N)

### 分别计算上/下三角1

```cpp
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        vector<int> B(A.size(), 1);
        // 计算下三角
        for(int idx=0, prod=1; idx<A.size(); idx++) {
            prod *= (idx>0) ? A[idx-1] : 1;
            B[idx] = prod;
        }
        // 计算上三角
        for(int idx=A.size()-1, prod=1; idx>=0; idx--) {
            prod *= (idx<A.size()-1) ? A[idx+1] : 1;
            B[idx] *= prod;
        }
        return B;
    }
};
```

### 分别计算上/下三角2

```cpp
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        vector<int> B(A.size(), 1);
        int begin = 1, end = 1, n = A.size();
        for(int i=0; i<n; i++) {
            B[i] *= begin;   //第一次跳过0
            B[n-1-i] *= end; //第一次跳过n-1
            begin *= A[i];   //计算下三角连乘
            end *= A[n-1-i]; //计算上三角连乘
        }
        return B;
    }
};
```

## 动态规划

1. 计算前i - 1个元素的乘积，及后N - i个元素的乘积分别保存在两个数组中。这可以用动态规划实现。  
2. 计算B[i]的值。

```cpp
链接：https://www.nowcoder.com/questionTerminal/94a4d381a68b47b7a8bed86f2975db46
来源：牛客网

import java.util.ArrayList;
public class Solution {
    int[] multiply(int[] A) {
        int len = A.length;
        int forword[] = new int[len];
        int backword[] = new int[len];
        int B[] = new int[len];
        forword[0] = 1;
        backword[0] = 1;
        for(int i = 1;i < len; i++){
            forword[i] = A[i - 1]*forword[i-1];
            backword[i] = A[len - i]*backword[i - 1];
        }
        for(int i = 0; i < len; i++){
            B[i] = forword[i] * backword[len - i -1];
        }
        return B;
    }
}
```

```cpp
作者：BugFree
链接：https://www.nowcoder.com/questionTerminal/94a4d381a68b47b7a8bed86f2975db46
来源：牛客网

public static int [] multiply(int[] A) {
  int len = A.length;
  int[] B = new int[len];
  if (A.length != 0) {
   B[0] = 1;
   // 计算前i - 1个元素的乘积
   for (int i = 1; i < A.length; i++)
    B[i] = A[i - 1] * B[i - 1];
   int temp = 1;
   // 计算后N - i个元素的乘积并连接
   for (int i = len - 2; i >= 0; i--) {
    temp *= A[i + 1];
    B[i] *= temp;
   }
  }
  return B;
    }
```
