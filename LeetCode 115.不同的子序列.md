# LeetCode 115.不同的子序列

给定一个字符串 s 和一个字符串 t ，计算在 s 的子序列中 t 出现的个数。

字符串的一个 子序列 是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）

题目数据保证答案符合 32 位带符号整数范围。

 

示例 1：

```
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下图所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
(上箭头符号 ^ 表示选取的字母)
rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```



示例 2：

```
输入：s = "babgbag", t = "bag"
输出：5
解释：
如下图所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
(上箭头符号 ^ 表示选取的字母)
babgbag
^^ ^
babgbag
^^    ^
babgbag
^    ^^
babgbag
  ^  ^^
babgbag
    ^^^
```



## 记忆化递归

对于字符串s和t来说，我们需要分别维护一个指示下标的变量，我们用idxS和idxT来表示，那么对于这两个变量指向位置的元素来说，会存在相同和不相同两种情况

- 如果当前元素相同，我们可以选择位于idxS位置的元素来构建子序列，也可以选择不使用，因为后面可能存在另外一个相同元素来构成这个子序列，
- 如果当前元素不相同，那么我们必定要右移idxS来寻找一个可能与idxT位置上元素匹配的位置。

根据上面的两种情况，我们先来定义我们的递归函数

```java
public int dfs(String s,String t,int idxS, int idxT){
    //如果走到了t串的尽头，表示可以构成一个子序列
    if(idxT == t.length())
        return 1;
    
    //如果走到了s串的尽头，表示无法构成一个子序列
    if(idxS == s.length())
        return 0;
    
    
    if(ss[idxS] == tt[idxT]){
        //如果两个指针位置上元素相同
        
        //我们可以选择使用当前idxS指向元素构成子序列 
        //也就是dfs(s,t,idxS+1,idxT+1 )
        
        //也可以不使用当前idxS位置上的元素
		//dfs(s ,t ,idxS+1,idxT )
        return dfs(s,t,idxS+1,idxT+1 ) + dfs(s ,t ,idxS+1,idxT );
    }else{   
        //元素不相同的情况
        return dfs(s ,t ,idxS+1,idxT );
    }
}
```

我们整个的递归函数的功能就是：返回s串在idxS开始处可以构成t串在idxT以后的子序列的方案数



其实整个递归过程有大量冗余计算，我们可以通过引入记忆化来消除冗余计算

```java
class Solution {
    public int numDistinct(String s, String t) {
        Map<Integer,Integer> memo = new HashMap<>();
        char[] ss = s.toCharArray();
        char[] tt = t.toCharArray();
        return dfs(ss,tt,0,0,memo);
    }
    public int dfs(char[] ss,char[] tt ,int idxS,int idxT , Map<Integer,Integer> memo){
        if(idxT == tt.length)
            return 1;
        if(idxS==ss.length)
            return 0;
        if(memo.containsKey(idxS*10000+idxT))
            return memo.get(idxS*10000+idxT);

        int sum = 0;

        int temp = 0;
        if(ss[idxS] == tt[idxT]){
            temp = dfs(ss ,tt ,idxS+1,idxT+1 , memo) + dfs(ss ,tt ,idxS+1,idxT , memo);
        }else{
            temp = dfs(ss ,tt ,idxS+1,idxT , memo);
        }
        memo.put(idxS*10000+idxT,temp);
        return temp;
    }
}
```

这里需要注意的是，在给定s和t的情况下，我们的dfs函数的返回值与idxS和idxT都有关系，因此我们在使用HashMap作为备忘录的情况下，需要将idxT也作为备忘录的key的特征（本题中s和t长度都不超过1000，因此我们用10000*lenS+lenT作为备忘录的key）





## 动态规划

在刚刚的记忆化递归的函数中，我们注意到有下列几行代码

```java
if(ss[idxS] == tt[idxT]){
    temp = dfs(ss ,tt ,idxS+1,idxT+1 , memo) + dfs(ss ,tt ,idxS+1,idxT , memo);
}else{
    temp = dfs(ss ,tt ,idxS+1,idxT , memo);
}
```

这里的代码非常像我们在动态规划中的转移方程，我们尝试通过动态规划的思想来解这道题

首先我们定义dp数组 int\[\]\[\] dp = new int\[lenS+1\]\[lenT+1\],这里数组长度为lenS+1和lenT+1的原因和递归的过程相同，我们需要通过判断当前idxS与idxT的值是否等于字符串长度来判断是否构成了子序列

因此我们在初始化的时候，我们需要将dp\[\]\[lenT\]赋值为1，即idxT==lenT的时候表示到可以构成子序列

然后来写一下状态转移方程

- ss[idxS] == tt[idxT]时 ， dp\[idxS\]\[idxT\] = dp\[idxS+1\][idxT+1\] + dp\[idxS+1\]\[idxT\];
- 否则， dp\[idxS\]\[idxT\] =  dp\[idxS+1\]\[idxT\];

在这里我们直接给出代码

```java
class Solution {
    public int numDistinct(String s, String t) {
        char[] ss = s.toCharArray();
        char[] tt = t.toCharArray(); 
        int lenS = ss.length;
        int lenT = tt.length;
        int[][] dp = new int[lenS+1][lenT+1];
        for(int j = 0 ; j < lenS+1 ; j ++){
            dp[j][lenT] = 1;
        }
        for(int i = lenS-1 ; i >=0 ; i --){
            for(int j = lenT -1 ; j>=0 ; j --){
                if(ss[i]==tt[j]){
                    dp[i][j] = dp[i+1][j+1] + dp[i+1][j];
                }else
                    dp[i][j] = dp[i+1][j];
            }
        }
        return dp[0][0];
    }
}
```

其实这道题的动态规划和记忆化递归的思路是高度相似的，都是通过从后往前来计算最终的结果