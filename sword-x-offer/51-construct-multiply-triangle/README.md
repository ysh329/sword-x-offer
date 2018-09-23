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

### 动态规划1

1. 计算前i - 1个元素的乘积(下三角)，及后N - i个元素的乘积(上三角)，可用动态规划实现   
2. 计算B[i]的值 

```cpp
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        vector<int> down(A.size(), 1);
        vector<int> up(A.size(), 1);
        vector<int> B(A.size(), 1);
        for(int i=1; i<A.size(); i++) {     //分别跳过上三角的0，下三角的n-1
            down[i] = A[i-1] * down[i-1];   //计算下三角
            up[i] = A[A.size()-i] * up[i-1];//计算上三角
        }
        for(int i=0; i<A.size(); i++)
            B[i] = down[i] * up[A.size()-i-1];
        return B;
    }
};
```

### 动态规划2

```cpp
class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        vector<int> B(A.size(), 1);
        // 计算前i - 1个元素的乘积，相当于计算下三角
        for(int i=1; i<A.size(); i++)
            B[i] = A[i-1] * B[i-1];
        // 计算后N - i个元素的乘积并连接，用up累乘得到上三角
        for(int i=(int)A.size()-2, up=1; i>=0; i--) {
            up *= A[i+1];
            B[i] *= up;
        }
        return B;
    }
};
```
