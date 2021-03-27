# [LeetCode 59.螺旋矩阵Ⅱ](https://leetcode-cn.com/problems/spiral-matrix/)

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

 

**示例 1：**

![Snipaste_2021-03-15_08-55-41](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\螺旋矩阵\Snipaste_2021-03-15_08-55-41.png)

```
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
```





**示例 2：**

```
输入：n = 1
输出：[[1]]
```



## 解题分析

今天这道题与昨天的那道题高度相似，只要我们控制好螺旋遍历的路径，就可以解决这道题

![image-20210315090315667](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210315090315667.png)

根据题意，我们需要先遍历当前矩阵的最左边一行，而这一行的行号都等于top，因此我们不难写出遍历这一行的循环

```java
//用于维护当前应该放入的值
int count = 1;

for(int i = left ; i<=right ; i ++){
	ans[top][i] = count++;
}
```

上方的循环结束后，行号为top的这一行已经加入到了我们的答案之中，因此我们需要调整top的值，top=top+1实现top的下移

![image-20210315090840516](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210315090840516.png)

然后我们再在最后一列从上往下复现我们刚刚的思路，实现right左移，以及后续bottom上移和left右移的遍历过程，就实现了最外面一圈的螺旋获取的过程。

![image-20210315091128996](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210315091128996.png)

![image-20210315091220353](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210315091220353.png)

```java
for(int i = left ; i<=right && count<=n*n ; i ++){
    ans[top][i] = count++;
}
top++;

for(int i = top ; i<=bot && count<= n*n ; i ++){
    ans[i][right] = count++;
}
right--;

for(int i = right ; i>=left && count<=n*n ; i --){
    ans[bot][i] = count++;
}
bot --;

for(int i = bot ; i >= top && count<= n*n ; i --){
    ans[i][left] = count++;
}
left++;
```



但是上述代码只能完成一圈的螺旋遍历，那我们怎样控制遍历的圈数呢？

可以参考我们昨天的文章，思路都是一样的啦，有兴趣的同学去瞄一眼昨天的文章，在这里直接给出这道题的完整代码

```java
class Solution {
    public int[][] generateMatrix(int n) {
        if(n==0)
            return new int[n][];
        int row = n-1;
        int col = n-1;
        int left=0;
        int right = col;
        int top = 0;
        int bot = row;
        int count = 1;
        int[][] ans = new int[n][n];
        while(count<= n*n){

            for(int i = left ; i<=right && count<=n*n ; i ++){
                ans[top][i] = count++;
            }
            top++;

            for(int i = top ; i<=bot && count<= n*n ; i ++){
                ans[i][right] = count++;
            }
            right--;

            for(int i = right ; i>=left && count<=n*n ; i --){
                ans[bot][i] = count++;
            }
            bot --;

            for(int i = bot ; i >= top && count<= n*n ; i --){
                ans[i][left] = count++;
            }
            left++;

        }
        return ans;
    }
}
```

