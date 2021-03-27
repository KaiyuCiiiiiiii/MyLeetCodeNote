# LeetCode 61.旋转链表

给你一个链表的头节点 `head` ，旋转链表，将链表每个节点向右移动 `k` 个位置。

示例 1：

```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]
```




示例 2：

```
输入：head = [0,1,2], k = 4
输出：[2,0,1]
```



## 解题分析

一道非常简单的链表题，假设链表长度为n，我们只需要将前k%n个元素挪到链表最后即可，在这里给出闭合为环的代码，思路就是将链表尾指向链表头形成一个环，然后在第n-k%n个节点处的next指针置为null，返回后面一个节点的引用即可

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if(head==null||head.next==null||k==0)
            return head;
        int length = 1;
        ListNode ptr = head;
        
        //计算链表长度的同时找到链表尾
        while(ptr.next!=null){
            length++;
            ptr = ptr.next;
        }
        //尾节点的next指向头节点形成环
        ptr.next = head;
        
        //计算断开连接的节点位置
        k = length - k%length;
        
        //找到需断开连接的位置
        while(k!=0){
            ptr = ptr.next;
            k--;
        }
        
        //存放答案
        ListNode ans = ptr.next;
        
        //断开连接
        ptr.next = null;
        return ans;
    }
}
```



# LeetCode 155.最小栈

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。

示例:

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```



## 解题思路

这道题在LeetCode上是一道简单题，我们用常规思路来想的话，我们需要两个栈，一个用来存放入栈数，如果当前入栈数比最小值还小的话，我们就把他存入另外一个最小栈，整体编码也非常简单，在这里先不给出代码实现。

如果要求只能用一个栈来实现呢？我们在这里不赘述实现思路，给出具体代码和详细注释，一看便懂

```java
class MinStack {
    Deque<Integer> stk ;
    
    //表示栈中最小值
    int min;
    /** initialize your data structure here. */
    public MinStack(){
        stk = new LinkedList<>();
    }
    
    public void push(int val) {
        //stk为空，加入当前元素后最小值就是val
        if(stk.isEmpty()){
            min = val;
        }
        
        //如果当前值小于栈中最小值
        //我们需要存储当前min值并更新
        if(val<=min){
            //先将当前min存入栈
            stk.addLast(min);
            //更新min
            min = val;
        }
        //在栈中加入val
        stk.addLast(val);
    }
    
    public void pop() {
        //获取并移出栈顶元素
        int top = stk.removeLast();
        //如果栈顶元素等于最小值
        //说明当前栈中最小值被移除
        //需要更新Min
        if(top==min)
            //由于在值为top这个元素入栈的时候
            //我们把当时的min存入栈中
            //此时栈顶元素就是移出top后的最小值
            min = stk.pollLast();
    }
    
    public int top() {
        return stk.peekLast();
    }
    
    public int getMin() {
        return min;
    }
}
```

也就是说，我们代码中的stk栈顶一定存的是之前入栈的元素，而栈顶下面一个可能存的是栈顶元素入栈之前的最小值，因此在入栈和出栈的时候，我们要判断是否需要多对栈顶进行一次操作来维护栈中最小值




