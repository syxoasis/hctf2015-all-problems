## Description:

*  GeruzoniAnsasu的舍友M大大最近去旁听了编译原理，回来问了一个问题把大家都难住了：为啥处理语法树的时候要有人为规定的优先级？如果优先级重新定义的话……

### 原题文档：
```
Lzy有n个数字a1,a2....an，n-1个符号（+，-，*）
他问***假如运算没了优先级，各种运算顺序下的结果总和对1e9+7取模的结果是多少？
***笑了笑。
ex：1*2+3
（1*2）+3 = 5
 1*（2+3）= 5
结果是10.

输入数据：第一行n表示共有n个数字，随后n个数字，最后符号串。数据范围2<=n<=100,(0≤ai≤109)
输出数据: 结果

sample input：
3
1 1 1
++

sample output:
6

```
--------------
加多点数据:)
问过能不能加入其它的运算符，答：没法加

##  原出题acmer的writeup
```
这是一道简单区间DP问题。
先来分析复杂度O（n^3）,第一层处理数字的个数i，第二层从第几个开始t，第三层以哪个符号为最后结束。
再分类讨论，+，-为一类，*为一类。
第一类：左边的结果*右边符号数的阶乘 + 右边的结果*左边符号数的阶乘
第二类：左边结果*右边结果即可
注意还要乘以组合数，即为答案。
讲得好抽象，不过很多队伍做出来，这里就简要描述。
主要代码部分，c为组合数，f为阶乘数，注意取模
for(int i = 2; i <= n; i++)
{
    for(int t = 1; t + i -1 <= n; t++)
    {
        for(int d = 1; d <=i-1; d++)
        {
            int w = t+d-1;
            char ch = s[w];
            if(ch=='+')
                dp[i][t] = (dp[i][t] + (c[i-2][d-1]*(dp[d][t]*f[i-1-d]%mod + dp[i-d][t+d]*f[d-1]%mod)%mod)%mod)%mod;
            else if(ch=='-')
                dp[i][t] = (dp[i][t] + (c[i-2][d-1]*(dp[d][t]*f[i-1-d]%mod - dp[i-d][t+d]*f[d-1]%mod)%mod)%mod)%mod;
            else
                dp[i][t] = (dp[i][t] + (c[i-2][d-1]*(dp[d][t]*dp[i-d][t+d]%mod)%mod)%mod)%mod;
        }
    }
}
																					
																					By ——Lzy
```
