# LeetCode 150.逆波兰表达式求值

根据[ 逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437)，求表达式的值。

有效的算符包括 `+`、`-`、`*`、`/` 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。



示例 1：

```
输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```



示例 2：

```
输入：tokens = ["4","13","5","/","+"]
输出：6
解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
```



示例 3：

```
输入：tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
输出：22
解释：
该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```



## 解题思路

这道题的解法其实非常简单，根据逆波兰表达式的定义，我们只需要定义一个栈来存放数组中的数和运算结果，每次遇到符号就从栈顶取出两个元素进行运算，并将结果放入栈中，最后返回栈顶元素就可以了

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stk = new LinkedList<>();
        for(String token:tokens){
            if( isChar(token) ){
                Integer num2 = stk.pollLast();
                Integer num1 = stk.pollLast();
                Integer ans = 0;
                if( token.equals("+")){
                    ans = num1 + num2;
                }else if(token.equals("-")){
                    ans = num1 - num2;
                }else if(token.equals("*")){
                    ans = num1*num2;
                }if(token.equals("/")){
                    ans = num1/num2;
                }
                stk.addLast(ans);
            }else{
                stk.addLast(Integer.valueOf(token));
            }
        }
        return stk.peekLast();
    }


    public boolean isChar(String token){
        return token.equals("+") || token.equals("-") ||token.equals("*") ||token.equals("/");
    }
}
```





# 加更一道LeetCode 89.格雷编码

格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。

给定一个代表编码总位数的非负整数 n，打印其格雷编码序列。即使有多个不同答案，你也只需要返回其中一种。

格雷编码序列必须以 0 开头。



示例 1:

```
输入: 2
输出: [0,1,3,2]
解释:
00 - 0
01 - 1
11 - 3
10 - 2

对于给定的 n，其格雷编码序列并不唯一。
例如，[0,2,3,1] 也是一个有效的格雷编码序列。

00 - 0
10 - 2
11 - 3
01 - 1
```

示例 2:

```
输入: 0
输出: [0]
解释: 我们定义格雷编码序列必须以 0 开头。
     给定编码总位数为 n 的格雷编码序列，其长度为 2n。当 n = 0 时，长度为 20 = 1。
     因此，当 n = 0 时，其格雷编码序列为 [0]。
```



## 解题分析

对于这道题，最关键一点是理解描述中的“两个连续的数值仅有一个位数的差异”这一句话，我们这道题的整个思路也是根据这句话而来的。首先从示例二中我们可以知道我们的答案中一定包含数字0，我们现已题目中的输入为2的情况开始分析，当输入为2的时候输出为[00,01,11,10],假设我们当前的输入变为了3，其中一定包含输入为2的四个输出结果[000,001,011,010],那剩余的四个数就是我们找到规律的重点，我们先来看010后面的数应该是谁，本来与010仅有一个位数的差异的数有[110,011,000],但是011和000我们已经得到过，因此下一个数必是110,那么110的下一个数又该是谁呢？下一个数一定是以1开头的，不然他只能后退到010，也就是说，我们开头的1可以暂不考虑，我们只需要找到与10仅有一个位数的差异的数，这个数我们在输入为2的情况是已经得到过！没错，如果以10开头，那么剩余的四个数一定是[10,11,01,00],正好与输入2的输出相反！我们来看图你就理解了

![Snipaste_2021-03-20_09-31-57](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\格雷编码\Snipaste_2021-03-20_09-31-57.png)

![Snipaste_2021-03-20_09-34-11](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\格雷编码\Snipaste_2021-03-20_09-34-11.png)



![image-20210320093810931](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210320093810931.png)

![image-20210320093922883](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210320093922883.png)

![image-20210320094130650](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210320094130650.png)

那么此时我们就可以确定，第二个红框内的??的值可以取11（这里也可以取其他值，但是无法找到规律，不好写代码），我们先继续往下看

![image-20210320094350947](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210320094350947.png)

看到这里是不是就已经明白了，其实红框内的数字是与蓝框内数字“对称”，蓝框数字首位变成1就得到了红框内的数字

![image-20210320094625851](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210320094625851.png)

到这里我们可以总结一下规律：假设我们有了N个(N为2的幂数)格雷编码，那么后面N个待求的格雷编码可以由当前N个已经计算出来的格雷编码对称得到，且只需要在原基础上加上N，有了这个，我们一起来写代码吧

```java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> ans = new ArrayList<>();
        int head = 1;
        
        //0是一定存在于首位的
        ans.add(0);
        for(int i = 0 ; i < n ; i ++){
            int x = ans.size();
            for(int j = x-1 ; j>=0 ; j --){
                //从当前N个格雷编码从后往前遍历，并改变一个开头数位就可以了
                ans.add(head + ans.get(j));
            }
            head<<=1;
        }
        return ans;
    }
}
```

