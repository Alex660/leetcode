#  K 个一组翻转链表（困难）
# 题目描述
![截屏2019-12-19上午7.49.49.png](https://pic.leetcode-cn.com/bc15a6141124406b6242f0a9f48a3f3cb7ff9fc4b4254ba2f21f1e576e6632f0-%E6%88%AA%E5%B1%8F2019-12-19%E4%B8%8A%E5%8D%887.49.49.png)
# 题目地址
<https://leetcode-cn.com/problems/reverse-nodes-in-k-group/>
## 链表
+ [参看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
#### 解法一：迭代
+ [思路同206. 反转链表 - 解法一](https://leetcode-cn.com/problems/reverse-linked-list/solution/206-fan-zhuan-lian-biao-by-alexer-660/)
+ 区别
  + 限制k个
    + 用计数实现，实时更新链表需要反转部分的头、尾节点
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
 * @param {number} k
 * @return {ListNode}
 */
var reverseKGroup = function(head, k) {
    let cur = head;
    let count = 0;
    // 求k个待反转元素的首节点和尾节点
    while(cur != null && count != k){
        cur = cur.next;
        count++;
    }
    // 足够k个节点，去反转
    if(count == k){
        // 递归链接后续k个反转的链表头节点
        cur = reverseKGroup(cur,k);
        while(count != 0){
            count--;
            // 反转链表
            let tmp = head.next;
            head.next = cur;
            cur = head;
            head = tmp;
        }
        head = cur;
    }
    return head;
};
```
#### 解法二：递归 II
+ 同解法一
+ 区别
  + while改成for
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
 * @param {number} k
 * @return {ListNode}
 */
var reverseKGroup = function(head, k) {
    if(!head) return null;
    // 反转链表
    let reverse = (a,b) => {
        let pre;
        let cur = a;
        let next = b;
        while(cur != b){
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
    // 反转区间a-b的k个待反转的元素
    let a = head;
    let b = head;
    for(let i = 0;i < k;i++){
        // 不足k个，不需要反转
        if(!b) return head;
        b = b.next;
    }
    // 反转前k个元素
    let newHead = reverse(a,b);
    // 递归链接后续反转链表
    a.next = reverseKGroup(b,k);
    return newHead;
};
```
#### 解法三：栈解
+ [思路同206. 反转链表 - 解法四](https://leetcode-cn.com/problems/reverse-linked-list/solution/206-fan-zhuan-lian-biao-by-alexer-660/)
+ 区别
  + 反转k个
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
 * @param {number} k
 * @return {ListNode}
 */
var reverseKGroup = function(head, k) {
    let stack = [];
    let preHead = new ListNode(0);
    let pre = preHead;
    // 循环链接后续反转链表
    while(true){
        let count = 0;
        let tmp = head;
        while(tmp && count < k){
            stack.push(tmp);
            tmp = tmp.next;
            count++;
        }
        // 不够k个，直接链接剩下链表返回
        if(count != k){
            pre.next = head;
            break;
        }
        // 出栈即是反转
        while(stack.length > 0){
            pre.next = stack.pop();
            pre = pre.next;
        }
        pre.next = tmp;
        head = tmp;
    }
    return preHead.next;
};
```