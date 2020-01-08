# 对链表进行插入排序（中等）
# 题目描述
![](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)
![截屏2020-01-08下午7.41.14.png](https://pic.leetcode-cn.com/fbd3fd394981f57c76ff5dfa98fdf7dc9c74406d18f407e579f42ae6a164d6d2-%E6%88%AA%E5%B1%8F2020-01-08%E4%B8%8B%E5%8D%887.41.14.png)
# 题目地址
<https://leetcode-cn.com/problems/insertion-sort-list/>
#### 解法：从左往右插入排序
+ [经典排序算法讲解 - 插入排序](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md)
+ 由上述讲解可知
  + 如果此题是双链表，那么很容易根据前驱模仿插入排序实现，即从右往左插入实现
  + 此题是单链表，只能每次从开头开始依次比较
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var insertionSortList = function(head) {
    // 边界条件
    if(head === null) return head;
    // 哨兵节点
    let preHead = new ListNode(0);
    // 将要移动重新插入链表中的元素
    let curr = head;
    // 插入链表中的前驱位置 插入pre和pre.next之间
    let pre = preHead;
    // 下一个将要移动插入的元素
    let next = null;
    // 遍历
    while(curr){
        // 保存下一个将要插入的元素
        next = curr.next;
        // 寻找插入的前驱位置
        while(pre.next && pre.next.val < curr.val){
            pre = pre.next;
        }
        // 插入
        curr.next = pre.next;
        pre.next = curr;
        // 从头或者从左到右开始遍历
        pre = preHead;
        // 下一个
        curr = next;
    }
    return preHead.next;
};
```