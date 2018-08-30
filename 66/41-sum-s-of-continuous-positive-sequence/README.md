# 和为S的连续正数序列

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序

## 双指针

```cpp
class Solution {
public:
    vector<vector<int> > FindContinuousSequence(int sum) {
        vector<vector<int>> res;
        if (sum<3) return res;
        int start = 1;
        int end = 2;
        while(start<end && start<=(sum/2)) {
            int cur_sum = (start+end)*(end-start+1)/2;
            if(cur_sum==sum) {
                vector<int> sub_res;
                for(int e = start; e<=end; e++)
                    sub_res.push_back(e);
                res.push_back(sub_res);
                start++;
            }
            else if(cur_sum>sum) start++;
            else end++;//cur_sum<sum
        }
        return res;
    }
};
```

## 等差数列

```java
链接：https://www.nowcoder.com/questionTerminal/c451a3fd84b64cb19485dad758a55ebe
来源：牛客网

import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> ans = new ArrayList<>();
        for (int n = (int) Math.sqrt(2 * sum); n >= 2; n--) {
            if ((n & 1) == 1 && sum % n == 0 || (sum % n) * 2 == n) {
                ArrayList<Integer> list = new ArrayList<>();
                for (int j = 0, k = (sum / n) - (n - 1) / 2; j < n; j++, k++) {
                    list.add(k);
                }
                ans.add(list);
            }
        }
        return ans;
    }
}

```

## 解方程

- (a + b)(b - a + 1) = sum * 2  
- 设x = a + b, y = b - a + 1, y >= 2  
- 枚举y，得x，解出a,b  

```cpp
链接：https://www.nowcoder.com/questionTerminal/c451a3fd84b64cb19485dad758a55ebe
来源：牛客网

typedef vector<int> vi;
typedef vector<vi> vvi;
class Solution {
public:
    vvi FindContinuousSequence(int sum) {
        vvi res;
        sum <<= 1;
        for(int y = 2; y * y <= sum; ++y) if(sum % y == 0){
            int x = sum / y, t = (x - y + 1);//t = x-y+1 = (a+b)-(b-a+1)+1=2a
            if(!(t & 1)){//判断t=2a是否为偶数
                res.push_back(vi());
                vi& v = res[res.size() - 1];//取出res最后一个元素，以引用的方式取出
                t >>= 1;//a=t/2
                for(int a = t; a <= x - t; ++a) v.push_back(a);// b=x-t
            }
        }
        for(int i = 0, j = int(res.size()) - 1; i < j; ++i, --j) swap(res[i], res[j]);
        return res;
    }
};
```

## 其它

```java
链接：https://www.nowcoder.com/questionTerminal/c451a3fd84b64cb19485dad758a55ebe
来源：牛客网

public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
    ArrayList<ArrayList<Integer> > lists = new ArrayList<ArrayList<Integer> >();
    for(int k = 2; k*k < 2*sum; k++){
        double m = ((2.0*sum)/k-k+1)/2.0;
        int temp = (int)m;
        if(m == temp){
            ArrayList<Integer> list = new ArrayList<Integer>();
            for(int i = 0; i < k; i++){
                list.add(i+temp);
            }
            lists.add(0, list);
        }
    }
    return lists;
}
```
