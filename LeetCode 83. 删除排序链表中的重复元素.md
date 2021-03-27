# LeetCode 83. 删除排序链表中的重复元素

存在一个按升序排列的链表，给你这个链表的头节点 `head` ，请你删除所有重复的元素，使每个元素 **只出现一次** 。

**示例 1：**

```
输入：head = [1,1,2]
输出：[1,2]
```

**示例 2：**

```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```



## 解题思路

这道题与昨天的每日一题非常相似，这道题需要使每个元素只出现一次，因此链表头处的元素一定会被保留，因此不需要定义dummyHead，整个删除过程也只需要维护两个指针即可，相对比较简单，在此也不赘述了，直接看代码

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null || head.next==null)
            return head;
        ListNode cur = head;
        ListNode nxt = head.next;
        while(nxt!=null){
            while( nxt!=null &&  nxt.val == cur.val ){
                nxt = nxt.next;
            }
            cur.next = nxt;
            cur = nxt;
            if(nxt!=null)
                nxt = nxt.next;
        }
        return head;
    }
}
```



## 4. 寻找两个正序数组的中位数

给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位数 。

 

示例 1：

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```



示例 2：

```java
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```




示例 3：

```java
输入：nums1 = [0,0], nums2 = [0,0]
输出：0.00000
```



示例 4：

```java
输入：nums1 = [], nums2 = [1]
输出：1.00000
```




示例 5：

```java
输入：nums1 = [2], nums2 = []
输出：2.00000
```

**进阶：**你能设计一个时间复杂度为 `O(log (m+n))` 的算法解决此问题吗？



## 解题思路

正常来说对于这种题目，由于两个数组有序，我们可以通过维护两个指针，在O(m+n)的时间复杂度找到中位数，想法的思路是非常容易编码实现的，在此我们不给出上述方案的代码。

我们注意到进阶信息里想让我们设计出一种O(log(m+n))的算法。一般来说对数级别的时间复杂度在二分查找中非常常见，且这道题的两个数组都有序，我们能否通过类似二分的思想来做呢？

首先我们给出几个变量

```java
        int len1 = nums1.length;
        int len2 = nums2.length;
        int mid1 = (len1+len2+1)/2;
        int mid2 = (len1+len2+2)/2;
```

由于我们需要找到两个数组"合并"后的中间连个值，也就是两个数组中第mid1和第mid2大的两个元素



我们通过递归的思想来做这道题

> 对于递归，我们只需要控制好递归的目标和边界条件，然后让他自动实现，而我们不需要跳进递归里理解全部的递归逻辑，这一点非常重要

定义我们的目标函数

```java
int find(int[] nums1 ,int start1, int end1,
         int[] nums2, int start2, int end2, int k){
    ...
    ...
    ...
}
```

这个函数的作用是 `在nums1从s1到e1范围中和在nums2从s2到e2范围中，找到第k大的数` 

这道题有一点小技巧，我们一定要保证nums1或者nums2中的一个始终是更长的数组，这样方便我们后续对递归的控制。

先来定义递归结束条件

```java
int find(int[] nums1 ,int s1, int e1,int[] nums2, int s2, int e2, int k){
    	//nums1的可选范围
        int len1 = e1-s1+1;
    	//nums2的可选范围
        int len2 = e2-s2+1;
    	//在这里我们设定要让nums2为更长的数组
        if(len2<len1)
            return find(nums2,start2,end2,nums1,start1,end1,k);
    	//如果len1为0，说明len1已经到头，
    	//找到numsz中下标为s2+k-1就是当前函数的目标值
        if(len1==0)
            return nums2[start2+k-1];
    	//k==1说明我们要在nums1[s1]和nums2[s2]选出一个更小值即可
        if(k==1)
            return Math.min(nums1[start1],nums2[start2]);
    ...
    ...
    ...
}
```



然后我们来定义当前函数中nums1和nums2可以到达的最远位置

```java
        int x1 = start1 + Math.min(k/2,len1)-1;
        int x2 = start2 + Math.min(k/2,len2)-1;
```

由于我们总是让nums1是更短的，因此k/2+s1可能会越界，因此我们这里通过取更小值来获得nums1和nums2的可到达的最远距离



然后我们通过对比x1和x2位置上元素的值，来确定缩小哪个数组的可选范围，也就是后移某个数组的start位置，这里由于nums[x1]和nums[x2]的更大值才可能是我们的要找的值，因此更小的值所在的数组需要后移start指针，也就是下面的代码

```java
        if(nums1[x1]>nums2[x2])
            return find( nums1,start1 , end1 , 
                        nums2 ,  x2+1 ,end2 , k-(x2-start2+1)  );
        else
            return find(nums1 , x1+1 , end1 , 
                        nums2 , start2 , end2 , k-(x1-start1+1));
```

综上我们的递归体就基本定义出来了，我们来看完整的代码

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        int mid1 = (len1+len2+1)/2;
        int mid2 = (len1+len2+2)/2;
        int val1 = find(nums1 , 0 , len1-1 , nums2 , 0 , len2-1 , mid1);
        int val2 = find(nums1 , 0 , len1-1 , nums2 , 0 , len2-1 , mid2);
        return (val1+val2)/2.0;
    }


    public int find(int[] nums1 ,int start1, int end1,
                    int[] nums2, int start2, int end2, int k){
        int len1 = end1-start1+1;
        int len2 = end2-start2+1;
        if(len1>len2)
            return find(nums2,start2,end2,
                        nums1,start1,end1,k);
        if(len1==0)
            return nums2[start2+k-1];
        if(k==1)
            return Math.min(nums1[start1],nums2[start2]);
        int x1 = start1 + Math.min(k/2,len1)-1;
        int x2 = start2 + Math.min(k/2,len2)-1;
        
        
        if(nums1[x1]>nums2[x2])
            //由于start2到x2范围的值配排除
        	//因此要找到第k-(x2-start2+1)大的值即可
            return find( nums1,start1 , end1 , 
                        nums2 ,  x2+1 ,end2 , k-(x2-start2+1)  );
        
        else
            //由于start1到x1范围的值配排除
        	//因此要找到第k-(x1-start1+1)大的值即可
            return find(nums1 , x1+1 , end1 , 
                        nums2 , start2 , end2 , k-(x1-start1+1));
    }
}
```

这样通过类似二分的递归思想，我们就可以求得想要的中位数，且时间复杂度为log(m+n)



