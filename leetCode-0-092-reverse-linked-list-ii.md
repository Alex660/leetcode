# 反转链表 II（中等）
# 题目描述
![截屏2019-12-20上午9.58.20.png](https://pic.leetcode-cn.com/933b9ce4f17e40fd63b82aa502ffefd308e9e3c5d4ada17a2881efc786e2dac8-%E6%88%AA%E5%B1%8F2019-12-20%E4%B8%8A%E5%8D%889.58.20.png)
# 题目地址
<https://leetcode-cn.com/problems/reverse-linked-list-ii/>
## 链表
+ [参看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
#### 解法一：迭代
+ 类似题型
  + [206. 反转链表 - 解法一](https://leetcode-cn.com/problems/reverse-linked-list/solution/206-fan-zhuan-lian-biao-by-alexer-660/)
+ 核心解法就是 206题中的解法一
  + 即将需要反转的 m到n 区间的链表反转，再重新连接首尾即可
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
 * @param {number} m
 * @param {number} n
 * @return {ListNode}
 */
var reverseBetween = function(head, m, n) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let tmpHead = dummy;
    // 找到第m-1个链表节点
    for(let i = 0;i < m - 1;i++){
        tmpHead = tmpHead.next;
    }
    // 206题解法一
    let prev = null;
    let curr = tmpHead.next;
    for(let i = 0;i <= n - m;i++){
        let next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    // 将翻转的部分链表 和 原链表拼接
    tmpHead.next.next = curr;
    tmpHead.next = prev;
    return dummy.next;
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
 * @param {number} m
 * @param {number} n
 * @return {ListNode}
 */
var reverseBetween = function(head, m, n) {
    let preHead = new ListNode(0);
    preHead.next = head;
    let pos = 0;
    let tmpHead = preHead;
    while(pos < m - 1){
        tmpHead = tmpHead.next;
        pos++;
    }
    let pre = null;
    let cur = tmpHead.next;
    while(pos < n){
        let next = cur.next;
        cur.next = pre;
        pre = cur;
        cur = next;
        pos++;
    }
    tmpHead.next.next = cur;
    tmpHead.next = pre;
    return preHead.next;
};
``` 
#### 解法二：迭代II
+ 解法一的 for -> while 版本
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
 * @param {number} m
 * @param {number} n
 * @return {ListNode}
 */
var reverseBetween = function(head, m, n) {
    if(head == null) return null;
    let curr = head,prev = null;
    while(m > 1){
        prev = curr;
        curr = curr.next;
        m--;
        n--;
    }
    let con = prev,tail = curr;
    while(n > 0){
        let next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
        n--;
    }
    if(con != null){
        con.next = prev;
    }else{
        head = prev;
    }
    tail.next = curr;
    return head;
};
```
#### 解法三：迭代III
+ 思路
  + 关键是插入操作，每次将当前尾节点插入到全链表中第一个节点和第二个节点之间
  + 每次只手动更新tail节点，一次插入完成会自动更新第二个节点
  + 因此需要 pre、start、tail三个节点
+ 举个栗子
  + 1->2->3->4->null，m = 2，n = 4
    + 将节点 3 插入到 节点1和节点2 之间
      + 1->3->2->4->null
    + 将节点 4 插入到 节点1和节点3 之间
      + 1->4->3->2->null
+ 图解
  + ![截屏2019-12-22上午8.36.43.png](https://pic.leetcode-cn.com/b4c536ca241616fed0f74869244f90f4165c79ed150284f3d8a1431e648924ae-%E6%88%AA%E5%B1%8F2019-12-22%E4%B8%8A%E5%8D%888.36.43.png)
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
 * @param {number} m
 * @param {number} n
 * @return {ListNode}
 */
var reverseBetween = function(head, m, n) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let tmpHead = dummy;
    for(let i = 0;i < m - 1;i++){
        tmpHead = tmpHead.next;
    }
    let start = tmpHead.next;
    let tail = start.next;
    for(let i = 0;i < n - m;i++){
        start.next = tail.next;
        tail.next = tmpHead.next;
        tmpHead.next = tail;
        tail = start.next;
    }
    return dummy.next;
};
```
#### 解法四：递归
+ 参考题解
  + [步步拆解：如何递归地反转链表的一部分](https://leetcode-cn.com/problems/reverse-linked-list-ii/solution/bu-bu-chai-jie-ru-he-di-gui-di-fan-zhuan-lian-biao/)
+ 题解原型
  + [206. 反转链表 - 解法三](https://leetcode-cn.com/problems/reverse-linked-list/solution/206-fan-zhuan-lian-biao-by-alexer-660/)
  + 解法三
    + 当n为链表长度时
      + 相当于反转链表 从1到n 个节点
        ```javascript
        var reverseList = function(head) {
            // 如果测试用例只有一个节点 或者 递归到了尾节点，返回当前节点 
            if(!head || !head.next) return head;
            // 存储当前节点的下一个节点
            let next = head.next;
            let reverseHead = reverseList(next);
            // 断开 head
            head.next = null;
            // 反转，后一个节点连接当前节点
            next.next = head;
            return reverseHead;
        };
        ``` 
      + 或者这样写
        + 此时 tail 恒为 null
        ```javascript
        var reverseList = function(head,n) {
            if(!head || !head.next) return head;
            let tail = null;
            if(n == 1){
                tail = head.next;
                return head;
            }
            let next = head.next;
            let reverseHead = reverseList(next,n-1);
            head.next = tail;
            next.next = head;
            return reverseHead;
        };
        ``` 
    + 当 n 小于链表长度时
      + 此时 tail 为 第 n+1 个节点
        ```javascript
        var reverseList = function(head,n) {
            if(!head || !head.next) return head;
            let tail = null;
            if(n == 1){
                tail = head.next;
                return head;
            }
            let next = head.next;
            let reverseHead = reverseList(next,n-1);
            head.next = tail;
            next.next = head;
            return reverseHead;
        };
        ``` 
    + 上面两种情况均是默认从第1个节点开始反转，即题意中的 m == 1 时
    + tail 相当于 反转前的头节点反转后不一定是最后一个节点，因为此时n != 链表长度
      + 所以要记录最后一处反转的位置，用以连接被反转的部分
      + 如果是整条链表都反转，head就成了最后一个节点，它的next自然恒为null
    + 图解
      + ![截屏2019-12-22上午11.42.54.png](https://pic.leetcode-cn.com/e6edd91b91d02d42e82084aff83e039f4d7af458c39d767e84457f8bc4953e60-%E6%88%AA%E5%B1%8F2019-12-22%E4%B8%8A%E5%8D%8811.42.54.png)
+ 思路
  + 既然默认m=1，n未知的解法出来了
  + 那么如题，m 未知的话，每次减1递归就好了
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
 * @param {number} m
 * @param {number} n
 * @return {ListNode}
 */
var reverseBetween = function(head, m, n) {
    let nextTail = null;
    let reverseN = (head,n) => {
        if(n == 1){
            nextTail = head.next;
            return head; 
        }
        let last = reverseN(head.next,n-1);
        head.next.next = head;
        head.next = nextTail;
        return last;
    }
    if(m == 1){
        return reverseN(head,n);
    }
    head.next = reverseBetween(head.next,m-1,n-1);
    return head;
};
```