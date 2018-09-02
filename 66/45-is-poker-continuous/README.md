# 扑克牌顺子

LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...

他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。

LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。

```cpp
class Solution {
public:
    bool IsContinuous(vector<int>& numbers) {
        bool res = false;
        if(numbers.size()!=5) return res;
        sort(numbers.begin(), numbers.end());
        bool repeat = false;
        int count0 = 0, split = 0;
        for(int i = 0; i < numbers.size(); i++) {
            if(i>=1 && 
               numbers[i-1]!=0 && 
               numbers[i-1]==numbers[i]) { // 重复跳出
               repeat = true;
                break;
            }
            if(numbers[i]==0) count0++; // 大小王
            else if(i>=1 && 
                    numbers[i-1]!=0 &&
                    numbers[i]-numbers[i-1] != 1)// 不连续
                split += numbers[i] - numbers[i-1] - 1;
        }
        res = (!repeat && split<=count0) ? true : res;
        return res;
    }
};
```

```cpp
class Solution {
public:
    bool IsContinuous( vector<int> numbers ) {
        bool result = false;
        if(numbers.empty() || numbers.size()!=5) return result;
        // 先排序
        sort(numbers.begin(), numbers.end());
        int count0 = 0;
        int repeat = 0;
        int split = 0;
        for(int eidx=0; eidx<numbers.size()-1; eidx++) {
            // 1.统计0的个数
            if(numbers[eidx]==0)
                count0++;
            // 2.是否有除0外重复的数字
            else if(eidx>=1 && numbers[eidx]==numbers[eidx-1]) {
                repeat = 1;
                break;
            }
            else if(numbers[eidx]!=0 && 
                    numbers[eidx]!=1 && 
                    numbers[eidx]-1==numbers[eidx])
                continue;
            // 3.不连续的空挡个数
            else if(numbers[eidx]!=0 && 
                    numbers[eidx]!=1 && 
                    numbers[eidx]-1!=numbers[eidx])
                split++;
        }
        return result;
    }
};
```
