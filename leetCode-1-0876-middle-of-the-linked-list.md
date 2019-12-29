# 链表的中间结点（简单）
# 题目描述
![截屏2019-12-30上午6.59.11.png](https://pic.leetcode-cn.com/f5d1a68d68d5206e05c6aab46f10983dcdc203656390f568f8798fc6840d67d3-%E6%88%AA%E5%B1%8F2019-12-30%E4%B8%8A%E5%8D%886.59.11.png)
# 题目地址
<https://leetcode-cn.com/problems/middle-of-the-linked-list/>
#### 解法一：双指针
+ [参看链表各种操作大全 - 删掉链表倒数第n个节点 - 解法三](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
+ 思路
  + 最后一个节点的索引位置是中间节点索引位置的两倍距离
  + 因此可以想到用两个指针
    + **快指针始终比慢指针更快一步**
    + 当两个指针其中的快指针走到末尾时，**快指针走过的距离一定是慢指针的两倍**
+ 注意
  + 求第二个中间节点时，fast指针终止条件正常为fast.next
  + 求第一个中间节点时，fast指针终止条件为fast.next.next，多跳一个节点的距离提前终止
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
var middleNode = function(head) {
    let fast = head;
    let slow = head;
    while(fast && fast.next){
        fast = fast.next.next;
        slow = slow.next;
    }
    return slow;
};
```
#### 解法二：两次遍历
+ [参看链表各种操作大全 - 删掉链表倒数第n个节点 - 解法二](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
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
var middleNode = function(head) {
    let len = 0;
    let tmpHead = head;
    while(tmpHead){
        len++;
        tmpHead = tmpHead.next;
    }
    let center = (len >> 1);
    let pos = 0;
    tmpHead = head;
    while(center--){
        tmpHead = tmpHead.next;
    }
    return tmpHead;
};
```
#### 解法三：数组
+ 解法二的简化版
  + 找到了中间节点的位置，那么可以想到利用数组下标取值实现
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
var middleNode = function(head) {
    let len = 0;
    let tmpHead = head;
    let res = [];
    while(tmpHead){
        res[len++] = tmpHead;
        tmpHead = tmpHead.next;
    }
    return res[len >> 1];
};
```