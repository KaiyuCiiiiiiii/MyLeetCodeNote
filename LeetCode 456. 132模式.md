# LeetCode 456. 132模式

给定一个整数序列：a1, a2, ..., an，一个132模式的子序列 ai, aj, ak 被定义为：当 i < j < k 时，ai < ak < aj。设计一个算法，当给定有 n 个数字的序列时，验证这个序列中是否含有132模式的子序列。

示例1:

```
输入: [1, 2, 3, 4]

输出: False

解释: 序列中不存在132模式的子序列。
```



示例 2:

```
输入: [3, 1, 4, 2]

输出: True

解释: 序列中有 1 个132模式的子序列： [1, 4, 2].
```




示例 3:

```
输入: [-1, 3, 2, 0]

输出: True

解释: 序列中有 3 个132模式的的子序列: [-1, 3, 2], [-1, 3, 0] 和 [-1, 2, 0].
```





## 解题分析

首先理解什么是132模式很重要，一个需要注意的地方是132模式中并不要求i，j，k连续。那么对于这种需要找到下一个更大（或更小）的元素，我们需要使用单调栈来做这道题

在这道题中，我们从后往前遍历数组，并按条件存入单调栈。由于是从后往前遍历，也就是我们先要找到132模式中的''2"，以及我们需要记录这个‘‘2‘’的最大值，这样我们才能更容易找到''1''。为了记录这个''2''的最大值，我们通过维护一个int maxRight来记录''2''的最大值。

然后我们来看一下单调栈的出栈条件，我们为了找到一个更大的"3"，因此我们的单调栈的出栈条件就是nums[i]>stk.peekLast()，也就是说比当前值小的栈顶元素依次出栈(也就是如果右侧元素小，则依次出栈)，出栈的元素会影响maxRight的值，因此我们每次出栈的过程还要来维护maxRight的值。

最后如果当前元素比maxRight小，则说明必然存在一个132模式的子序列，直接返回true

```java
class Solution {
    public boolean find132pattern(int[] nums) {
        int maxRight = Integer.MIN_VALUE;
        int n = nums.length;
        Deque<Integer> stk = new LinkedList<>();
        for(int i = n-1 ; i >=0 ; i --){
            //当前值小于maxRight
            //也就是num[i] 为模式中的'1'
            //直接返回true
            if(nums[i]<maxRight){
                return true;
            }
            //当前元素大于栈顶元素，则栈顶元素依次出栈
            //当前栈内存储的都是i右侧位置
            while(!stk.isEmpty() && nums[i]>stk.peekLast()  ){
                //出栈需要维护maxRight的值
                //也就是维护"2"的最大值
                maxRight = stk.pollLast();
            }
            //说明nums[i]为“3”
            if(nums[i]>maxRight){
                stk.addLast(nums[i]);
            }
        }
        return false;
    }
}
```

