# LeetCode 82. 删除排序链表中的重复元素 II

给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。



示例 1:

```
输入: 1->2->3->3->4->4->5
输出: 1->2->5
```



示例 2:

```
输入: 1->1->1->2->3
输出: 2->3
```





## 解题思路

对于删除链表某些节点的问题，我们通常采用三指针的方法来维护节点中next指针的值，这里我们就不多做分析，先给出代码，后面会给出一些图供大家参考

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null||head.next==null)
            return head;
        
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode pre = dummyHead;
        ListNode cur = pre.next;
        ListNode nxt = cur.next;
        while(nxt!=null){
            if(nxt.val == cur.val){
                while(nxt!=null&&nxt.val == cur.val){
                    nxt = nxt.next;
                }
                pre.next = nxt;
                cur = nxt;
                if(nxt!=null)
                    nxt = nxt.next;
            }else{
                nxt = nxt.next;
                cur  =cur.next;
                pre = pre.next;
            }
        }
        return dummyHead.next;
    }
}

```



下面一张图是刚开始的时候几个指针的位置

![1](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\链表删除\1.png)





由于上面图中cur.val != nxt.val，三个指针同时后移到下图位置

![2](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\链表删除\2.png)

此时cur.val == nxt.val, nxt后移







![3](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\链表删除\3.png)

![4](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\链表删除\4.png)





直到nxt.val == 3的时候，cur.val != nxt.val ，此时修改pre.next，修改完如下图所示

![5](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\链表删除\5.png)



改变nxt和cur

![6](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\链表删除\6.png)



由于上图中cur.val == nxt.val ， nxt继续后移

![7](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\链表删除\7.png)



最后结束时的指针情况

![8](C:\Users\慈凯瑜\iCloudDrive\公众号配图\leetcode\链表删除\8.png)

