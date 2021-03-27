# [LeetCode 54.螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

 

**示例 1：**

![Snipaste_2021-03-15_08-55-41](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\螺旋矩阵\Snipaste_2021-03-15_08-55-41.png)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```





**示例 2：**

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```



## 解题分析

首先这道题的思路并不复杂，但是要不重不漏的进行螺旋输出可能还需要一些功夫

我们先维护四个变量left， right，top，bottom来指示当前未遍历位置的边界

![image-20210315090315667](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210315090315667.png)

根据题意，我们需要先遍历当前矩阵的最左边一行，而这一行的行号都等于top，因此我们不难写出遍历这一行的循环

```java
//用来存放答案
List<Integer> ans = new ArrayList<>();

for(int i = left ; i<=right ; i ++){
	ans.add(matrix[top][i]);
}
```

上方的循环结束后，行号为top的这一行已经加入到了我们的答案之中，因此我们需要调整top的值，top=top+1实现top的下移

![image-20210315090840516](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210315090840516.png)

然后我们再在最后一列从上往下复现我们刚刚的思路，实现right左移，以及后续bottom上移和left右移的遍历过程，就实现了最外面一圈的螺旋获取的过程。

![image-20210315091128996](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210315091128996.png)

![image-20210315091220353](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210315091220353.png)

```java
for(int i = left ; i<=right ; i ++){
    ans.add(matrix[top][i]);
}
top++;
for(int i = top ; i <=bot ; i ++){
    ans.add(matrix[i][right]);
}
right--;
for(int i = right  ; i>=left; i--){
    ans.add(matrix[bot][i]);
}
bot--;
for(int i = bot ; i>=top ;  i --){
    ans.add(matrix[i][left]);
}
left++;
```



但是上述代码只能完成一圈的螺旋遍历，那我们怎样控制遍历的圈数呢？

我们可以通过维护矩阵中未遍历元素的数量控制我们是否要开始下一圈的遍历

定义numEle = row*col表示矩阵中元素总数，然后在我们上述循环中，每进行一次循环就将numEle减一，我们根据numEle的剩余量判断是否进行下一圈的遍历,优化后的代码如下

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> ans = new ArrayList<>();
        if( matrix.length ==0 || matrix[0].length == 0 )
            return ans;
        int row = matrix.length;
        int col = matrix[0].length;
        int left = 0;
        int right = col-1;
        int top = 0;
        int bot = row-1;
        int numEle = row*col;
        while(numEle>=1){
            for(int i = left ; i<=right &&numEle>=1 ; i ++){
                ans.add(matrix[top][i]);
                numEle--;
            }
            top++;
            for(int i = top ; i <=bot && numEle>=1 ; i ++){
                ans.add(matrix[i][right]);
                numEle--;
            }
            right--;
            for(int i = right  ; i>=left && numEle>=1 ; i--){
                ans.add(matrix[bot][i]);
                numEle--;
            }
            bot--;
            for(int i = bot ; i>=top && numEle>=1;  i --){
                ans.add(matrix[i][left]);
                numEle--;
            }
            left++;
        }
        return ans;
    }
}
```

