# 删除排序链表中的重复元素 II（中等）
# 题目描述
![截屏2020-04-12上午9.47.48.png](https://pic.leetcode-cn.com/e4928240ec2f2c4d55d91923a8e6d6676bf8b86eea9fc38247045d9b3d73b2f0-%E6%88%AA%E5%B1%8F2020-04-12%E4%B8%8A%E5%8D%889.47.48.png)
# 题目地址
<https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/>
#### 链表 
+ [戳看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
#### 类似题型
+ [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/solution/83-shan-chu-pai-xu-lian-biao-zhong-de-zhong-fu--15/)
#### 解法一：快慢指针
+ 思路
  + 快指针：跳过重复数，记录下一个前面没有重复数的节点位置
  + 慢指针：标记重复数字出现起点，根据链表特性，负责与下一个快指针相连
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
var deleteDuplicates = function(head) {
    let dummy = new ListNode(-1)
    dummy.next = head
    let fast = dummy.next
    let slow = dummy
    while(fast) {
        if(fast.next && fast.val === fast.next.val) {
            let sameVal = fast.val
            while(fast && sameVal === fast.val) {
                fast = fast.next
            }
        }else{
            slow.next = fast
            slow = fast
            fast = fast.next
        }
    }
    slow.next = fast
    return dummy.next
};
```
+ 也可以这样写
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
var deleteDuplicates = function(head) {
    let dummy = new ListNode(-1)
    dummy.next = head
    let fast = dummy.next
    let slow = dummy
    while(fast) {
        while(fast.next && fast.val === fast.next.val) {
            fast = fast.next
        }
        if(slow.next === fast) slow = slow.next;
        else slow.next = fast.next;
        fast = fast.next
    }
    return dummy.next
};
```
#### 解法二：递归
+ 思路
  + 将解法一断点位置保存在递归函数栈中，去拼接前后符合题意节点
+ [参看各类算法模板 - 递归一节 - Python&Java版](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
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
var deleteDuplicates = function(head) {
    if(!head) return null
    if(head.next && head.val === head.next.val) {
        while (head.next && head.val === head.next.val) {
            head = head.next
        }
        return deleteDuplicates(head.next)
    }else {
        head.next = deleteDuplicates(head.next)
    }
    return head
};
```