# LeetCode 92.反转链表 II

给你单链表的头节点 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表 。

**示例 1：**

![image-20210318122030657](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210318122030657.png)

```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```



**示例 2：**

```
输入：head = [5], left = 1, right = 1
输出：[5]
```



## 解题分析

对于这种反转链表相关的题目，最常见的方法就是递归和迭代，我们在这里先给出迭代的解法分析

对于这道题来说，我们要反转链表中的一部分，那么我们可以考虑遍历这一部分的同时，将每个节点插入到需要反转位置的头部，来实现反转，对于在链表中间进行反转是比较容易操作的，但是如果从链表头部就需要反转的情况，我们需要不断更新我们的指向头节点的指针，有一定操作难度，因此对于链表的相关问题，笔者更倾向于定义一个虚节点指向链表的原始头节点，这样最后返回的时候只需要返回这个虚节点的next指针就可以了

```java
ListNode dummyHead = new ListNode(0);
dummyHead.next = head;
......
......
return dummyHead.next;
```

通过这一步我们就可以无视到底是从原链表的头部开始反转还是从中段开始反转，通过引入这个虚节点，我们都可以视为在新链表的中段开始反转



因为我们要通过在反转位置的头部插入需要反转的节点，因此首先我们先找到反转部分前的最后一个节点

```java
ListNode p = dummyHead;
for(int i = 1 ; i < left ;  i ++){
	p = p.next;
}
```

这样得到的p就是我们要找到的反转部分前的最后一个节点

由于p.next所指向的值是反转后的最后一个节点，因此我们定义tail来表示这个反转部分的尾节点

```java
ListNode tail = p.next;
```



然后我们再定义一个指向当前需要进行反转的节点的指针

```java
ListNode cur = tail.next;
```

这里需要注意的是，因为我们tail指针指向的这个节点本来就是第一个需要插入的节点，但是他已经处于反转位置的头部，因此我们可以跳过这个节点，直接从他后面的一个节点开始进行插入

```java
for(int i =left+1 ; i<=right ; i ++){
    tail.next = cur.next;
    cur.next = p.next;
    p.next=  cur; 
    cur = tail.next;
}
```



我们在这里用图来看一下第一次循环里发生了什么

在循环体发生之前的指针情况

![image-20210318123923358](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210318123923358.png)





进入循环体内

![image-20210318124048845](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210318124048845.png)



![image-20210318124138739](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210318124138739.png)

![image-20210318124220760](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210318124220760.png)

上面的图看起来有点乱，我们来重新排列一下

![image-20210318124417922](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210318124417922.png)

这样我们就完成了将3这个节点插入反转部位的头部，然后我们后移指针令cur=tail.next，就完成了我们对3这个节点的反转过程

![image-20210318124546869](C:\Users\慈凯瑜\AppData\Roaming\Typora\typora-user-images\image-20210318124546869.png)

然后我们通过for循环来控制其他节点的反转，最后我们直接返回dummyHead.next就是我们最终的结果，完整代码如下

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode p = dummyHead;
        for(int i = 1 ; i < left ;  i ++){
            p = p.next;
        }
        ListNode tail = p.next;
        ListNode cur = tail.next;
        for(int i =left+1 ; i<=right ; i ++){
            tail.next = cur.next;
            cur.next = p.next;
            p.next=  cur; 
            cur = tail.next;
        }
        return dummyHead.next;
    }
}
```



对于链表问题，其实递归也是一种很好的方法，在这里给出labuladong大佬的解题链接，里面包含了很多反转链表类型题的解题思路，写的非常好，笔者就不在此班门弄斧了

https://leetcode-cn.com/problems/reverse-linked-list-ii/solution/bu-bu-chai-jie-ru-he-di-gui-di-fan-zhuan-lian-biao/