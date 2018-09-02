# 扑克牌顺子

LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...

他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。

LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。

## 分析

```cpp
class Solution {
public:
    bool IsContinuous(vector<int>& num) {
        bool res = false;
        if(num.size()!=5) return res;
        sort(num.begin(), num.end());
        bool repeat = false;
        int count0 = 0;
        for(int i=0; i<num.size(); i++) {
            if(num[i]==0) count0++; //大小王
            else if() ; //牌面值异常
            else if() ; //重复
            else if() ; //不连续
            else continue; // i=0,连续，num[i-1]=0
        }
        res = (!repeat && count0>=0) ? true : res;
        return res;
    }
};
```

## 常规解法

### 常规解法1

```cpp
class Solution {
public:
    bool IsContinuous(vector<int>& a) {
        bool res = false;
        if(a.size()!=5) return res;
        sort(a.begin(), a.end());
        bool repeat = false;
        int count0 = 0, split = 0;
        for(int i=0; i<a.size(); i++) {
            if(a[i]<0 || 13<a[i]) {repeat=true; break;} //牌面值异常
            else if(a[i]==0) count0++; //大小王(结尾判断最多4个王)
            else if(i>0 && //重复
                    a[i-1]!=0 &&
                    a[i-1]==a[i]) {
                repeat=true; break;
            }
            else if(i>0 && //不连续
                    a[i-1]!=0 &&
                    a[i-1]!=a[i]-1)
                split += (a[i]-a[i-1]-1);
            else continue; // i=0,连续，a[i-1]=0
        }
        res = (!repeat && count0>=split && count0<5) ? true : res; //最多4个王
        return res;
    }
};
```

### 常规解法2

```cpp
class Solution {
public:
    bool IsContinuous(vector<int>& a) {
		if(a.size()!=5) return false;
        sort(a.begin(), a.end());
        int cnt0 = 0, cntNeed = 0;
        for(int i = 0; i < 5; i++) {
			if(a[i]<0 || 13<a[i]) return false; //牌面值异常
            else if(a[i] == 0) ++cnt0; //大小王
			else if(i + 1 < 5 ) {
                if(a[i + 1] == a[i]) return false; //重复
                cntNeed += a[i + 1] - a[i] - 1; //非0，不连续
            } else continue; //连续,i=4
        }
        if(cntNeed > cnt0) return false;
        return true;
    }
};
```

### 常规解法3

```cpp
class Solution {
public:
    bool IsContinuous(vector<int>& a) {
        if(a.size() != 5) return false;
        int min = 14, max = -1, flag = 0;
        for(int i = 0; i < a.size(); i++) {
            if(a[i]<0 || a[i]>13) return false; //异常牌面值
            if(a[i] == 0) continue; //大小王
            if(((flag >> a[i]) & 1) == 1) return false; //检查重复
            flag |= (1 << a[i]); //以异或的二进制形式存储
            max = (a[i]>max) ? a[i] : max;
            min = (a[i]<min) ? a[i] : min;
            if(max-min >= 5) return false;
        }
        return true;
    }
};
```

### 常规解法4

参考：马客(Mark) | 牛客网  

1. sort  
2. 0 的个数<5 && 正数互不相同 && 正数最大最小值相差<5

`(a[4]-a[count0])<5` 这句，用来判断count0和不连续牌个数是否相符的！若连续，必然有第一个非0(或0)与最大的数`相差<5`

```cpp
class Solution {
public:
    bool IsContinuous(vector<int>& a) {
        if(a.size() != 5) return false;
        sort(a.begin(), a.end());
        int count0 = 0;
        while(count0<5 && a[count0]==0) ++count0;
        for(int i=count0; i<5; ++i) if(i && a[i]==a[i - 1]) return false;//从第2张牌查重复
        return count0<5 && (a[4]-a[count0])<5; //大小王张数最多4,第一个非王的牌面与最后一张牌面王差距在5以内
    }
};
```
