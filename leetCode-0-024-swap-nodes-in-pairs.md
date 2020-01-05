# 两两交换链表中的节点（中等）
# 题目描述
![截屏2019-12-18上午9.46.28.png](https://pic.leetcode-cn.com/027bbdec30c32d0e88c160a52f615303c72a2ce5f0d0b3976ffa682937e37717-%E6%88%AA%E5%B1%8F2019-12-18%E4%B8%8A%E5%8D%889.46.28.png)
# 题目地址
<https://leetcode-cn.com/problems/swap-nodes-in-pairs/>
## 链表
+ [参看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
#### 解法一：非递归
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
+ 思路
  + 添加一个哨兵节点
  + 三个节点外加一个哨兵节点之间作指针指向变换操作
+ 图解
  + ![截屏2019-12-19上午7.16.40.png](https://pic.leetcode-cn.com/84031b11de4ccd16d020a2f4a727db2df1d86e94b91e60948a2c18f24c19cdcb-%E6%88%AA%E5%B1%8F2019-12-19%E4%B8%8A%E5%8D%887.16.40.png)
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
var swapPairs = function(head) {
    let thead = new ListNode(0);
    thead.next = head;
    let tmp = thead;
    while(tmp.next != null && tmp.next.next != null){
        let start = tmp.next;
        let end = start.next;
        tmp.next = end;
        start.next = end.next;
        end.next = start;
        tmp = start;
    }
    return thead.next;
};
```
#### 解法二：递归
+ 时间复杂度：O(n)
+ 空间复杂度：O(n)
+ 思路
  + 终止
    + 同解法一：至少三个节点之间才可以互换
    + 只有一个节点或没有节点，返回此节点
  + 交换
    + 设需要交换的两个节点为head、next
    + head -> next -> c -> ...
    + head -> c -> ... && next -> head
    + next -> head -> c -> ...
      + head 连接( -> )后面交换完成的子链表
      + next 连接( -> )head 完成交换
    + 对子链表重复上述过程即可
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
var swapPairs = function(head) {
    if(head == null || head.next == null){
        return head;
    }
    // 获得第 2 个节点
    let next = head.next;
    // next.next = head.next.next
    // 第1个节点指向第 3 个节点，并从第3个节点开始递归
    head.next = swapPairs(next.next);
    // 第2个节点指向第 1 个节点
    next.next = head;
    // 或者 [head.next,next.next] = [swapPairs(next.next),head]
    return next;
};
```