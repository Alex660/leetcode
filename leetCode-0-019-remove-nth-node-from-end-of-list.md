# 删除链表的倒数第N个节点（中等）
# 题目描述
![截屏2019-12-29下午5.12.10.png](https://pic.leetcode-cn.com/55e6bebb7c75d8be2670a283752ce818a749979eda4ae7d2106b2d04d22f9198-%E6%88%AA%E5%B1%8F2019-12-29%E4%B8%8B%E5%8D%885.12.10.png)
# 题目地址
<https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/>
#### 解法一：单链表的插入、删除、查找、反转等经典操作
+ [参看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
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
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    // 反转链表
    let prev = null;
    let curr = head;
    var reverse = (prev,curr) => {
        while(curr){
            let next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev
    }
    let newHead = reverse(prev,curr);
    // 边界条件处理
    if(n == 1){
        return reverse(null,newHead.next);
    }
    // 查询节点
    let findNodeByIndex = (currSearchTmp,n) => {
        let pos = 1;
        while(currSearchTmp && pos != n){
            currSearchTmp = currSearchTmp.next
            pos++
        }
        return currSearchTmp
    }
    let currSearch = findNodeByIndex(newHead,n);
    // 查找前一个节点
    let pre = findNodeByIndex(newHead,n-1);
    // 删除节点
    pre.next = currSearch.next;
    let prevNeed = null;
    let currNeed = newHead;
    // 反转回来
    return reverse(prevNeed,currNeed)
};
```
#### 解法二：两次遍历
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
+ 思路
  + 删除正数第n个节点
    + 遍历链表，直到第n-1个节点，将第n-1个节点指向第n个节点即完成了删除操作
    + 通过索引位置自减，直到0，就可以找到
  + 删除倒数第n个节点
    + 如解法一
      + 我们通过反转链表来删除第n个节点，即是删除了倒数第n个节点
        + 查找节点
          + 通过遍历位置得到
    + 此处不需反转
    + 举个栗子
      + 1 -> 2 -> 3 -> 4 -> 5
      + 删除倒数第2个节点
      + 等价于
        + 删除正数第4(5 - 2 + 1)个节点
    + **因此此题转换成了删除正数第(L - n + 1)个节点**
      + L为链表长度，需要通过遍历算得
      + 然后解法同正数的相同即可
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
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let preHead = new ListNode(-1);
    preHead.next = head;
    let len = 0;
    let first = head;
    while(first){
        len++;
        first = first.next;
    }
    len -= n;
    first = preHead;
    while(len != 0){
        len--;
        first = first.next;
    }
    first.next = first.next.next;
    return preHead.next;
};
```
#### 解法三：双指针
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
+ 思路
  + 设快慢两个指针
    + 1、快指针先移n个节点
    + 2、快、慢指针一起移动，使得两指针之间一直保持n个节点
      + 2.1、当指针到达链表底了，将慢指针的指针指向它的下下一个指针
      + 2.2、慢指针的下一个指针即为要删除的节点
  + 边界
    + 当要删除的节点是head时，需要设置哨兵节点，即增设一个位于当前头部节点的前一个值为null的哨兵节点
      + 最后返回哨兵节点的next即为所求
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
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let preHead = new ListNode(-1);
    preHead.next = head;
    let fast = preHead;
    let slow = preHead;
    while(n != 0){
        fast = fast.next;
        n--;
    }
    while(fast.next != null){
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;
    return preHead.next;
};
```