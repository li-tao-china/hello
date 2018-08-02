<<<<<<< HEAD
# 与剑指offer相遇的八月31天

day 1  
- 二进制中1的个数  
常规解法  
1、将1 每次 <<1 与 x 进行&操作 记录结果为1的次数   
2、怎么确定 <<1 结束条件 <<1的数值大于x  
科学解法  
1、原数减去1之后最右边的1都会变成0 ， 用减1后的数和原数进行&操作  
    ```
    class Solution {
    public:
        int  NumberOf1(int n) {
            //常规解法
            int count = 0;
            int unsigned flag = 1;
            while(flag){
                
                if(n & flag)
                    count ++;
                flag = flag << 1;
            }
            
            //科学解法
            int count = 0;
            while(n){
                
                count ++;
                n = n & (n-1);
            }

            return count;
        }
    };
    ```
- 试题14  
剪绳子  

动态规划  
1、求一个问题的最优解  
2、整体问题的最优解是依赖各个子问题的最优解  
3、小问题之间还有相互重叠的更小的子问题  
4、从上往下分析问题、从下往上求解问题  

动态规划思路  
1、从
```  
    f(1)=0,  
    f(2)=1,  
    f(3)=2 * 1 = 2,  
    f(4)=2 * 2 = 4,  
    f(5)=2 * 3 = 6,  
    f(6)=3 * 3 = 9,  
    f(7)=3 * 2 * 2 = 12,  
    f(8)=3 * 3 * 2 = 18,   
```  
2、  
```
int maxProductAfterCutting_solution1(int length){

    if(length < 2)
    return 0;
    if(length == 2)
    return 1;
    if(length == 3)
    return 2;

    int* len = new int[length+1];
    len[0]=0;
    len[1]=1;
    len[2]=2;
    len[3]=3;

    int max = 0;
    for(int i = 4; i <= length; i++){
        
        for(int j = 1; j <= i/2; j++){

            len[i] = len[j] * len[i-j];
            if(max < len[i])
                max = len[i];
        }
        len[i] = max;
    }

    max = len[length];
    delete[] len;

    return max;
}
```

贪心算法  
应得到尽可能多的3  
当余数为1时，退3，进4

```
int maxProductAfterCutting_solution1(int length){

    if(length < 2)
    return 0;
    if(length == 2)
    return 1;
    if(length == 3)
    return 2;

    int timesOf3 = length / 3;
    if(length - 3 * timesOf3 == 1)
        timesOf3 --;

    int timesOf2 = (length - 3 * timesOf3) / 2;

    return pow(2,timesOf3) * pow(2,timesOf2);
}
```

=======  
你最喜欢的花是  
玫瑰  
闻它你需要怎做  

尽量憋气, 越久越好  
然后一口呼出来
>>>>>>> d1dc6b3c89d140bf297dbc0cb97c250eaa3ed8ac
