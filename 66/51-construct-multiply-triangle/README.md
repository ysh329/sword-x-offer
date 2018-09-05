# 构建乘积数组

给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

## O(n^2)

```cpp
/*
b0:   1 a1 a2 a3 a4
b1:  a0  1 a2 a3 a4
b2:  a0 a1  1 a3 a4
b3:  a0 a1 a2  1 a4
b4:  a0 a1 a2 a3  1
*/ 

class Solution {
public:
    vector<int> multiply(const vector<int>& A) {
        vector<int> res(A.size());
        for(int idxB=0; idxB<A.size(); idxB++) {
            int prod = 1;
            for(int idxA=0; idxA<A.size(); idxA++)
                prod *= idxA==idxB ? 1 : A[idxA];
            res[idxB] = prod;
        }
        return res;
    }
};
```
