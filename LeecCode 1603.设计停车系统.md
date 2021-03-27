# LeetCode 341.扁平化嵌套列表迭代器

给你一个嵌套的整型列表。请你设计一个迭代器，使其能够遍历这个整型列表中的所有整数。

列表中的每一项或者为一个整数，或者是另一个列表。其中列表的元素也可能是整数或是其他列表。

 

示例 1:

```
输入: [[1,1],2,[1,1]]
输出: [1,1,2,1,1]
解释: 通过重复调用 next 直到 hasNext 返回 false，next 返回的元素的顺序应该是: [1,1,2,1,1]。
```


示例 2:

```
输入: [1,[4,[6]]]
输出: [1,4,6]
解释: 通过重复调用 next 直到 hasNext 返回 false，next 返回的元素的顺序应该是: [1,4,6]。
```



## 解题思路

首先我们来看一下输入数据的定义

```java
public interface NestedInteger {

    // @return true if this NestedInteger holds a single integer, rather than a nested list.
    public boolean isInteger();

    // @return the single integer that this NestedInteger holds, if it holds a single integer
    // Return null if this NestedInteger holds a nested list
    public Integer getInteger();

    // @return the nested list that this NestedInteger holds, if it holds a nested list
    // Return null if this NestedInteger holds a single integer
    public List<NestedInteger> getList();
}
```

在这道题中输入的是一个List<NestedInteger>,然后每一个NestedInteger可能是一个整数，也可能是另外一个 List<NestedInteger>，因此我们可以考虑在初始化目标迭代器的时候，就把所有的NestedInteger以整数的形式添加到另外一个List中，在后续调用next和hasNext的时候我们直接在得到的List中进行操作即可

```java
public class NestedIterator implements Iterator<Integer> {
    List<NestedInteger> list;
    List<Integer> res;
    public NestedIterator(List<NestedInteger> nestedList) {
        this.list = nestedList;
        this.res = new ArrayList<>();
        
        //对list中的元素进行dfs
        dfs(list);
    }

    @Override
    public Integer next() {
        return res.remove(0);
    }

    @Override
    public boolean hasNext() {
        return !res.isEmpty();
    }

    //本道题的核心
    public void dfs(List<NestedInteger> l1){
        //对l1中的每个元素进行遍历
        for(NestedInteger l2:l1){
            //如果当前元素是Integer类型，则直接加入res
            if(l2.isInteger()){
                this.res.add(l2.getInteger());
            }else{
                //当前l2是一个List，我们先对l2中的元素dfs添加到res中
                dfs(l2.getList());
            }
        }
    }
}
```





