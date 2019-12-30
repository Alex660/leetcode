# 反转链表（简单）
# 题目描述
![截屏2019-12-19上午9.32.38.png](https://pic.leetcode-cn.com/5e216425adb16842467328f214068d61f84fea1b74429625d38cfd750c67bee7-%E6%88%AA%E5%B1%8F2019-12-19%E4%B8%8A%E5%8D%889.32.38.png)
# 题目地址
<https://leetcode-cn.com/problems/reverse-linked-list/>
## 链表
+ [参看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
#### 解法一：迭代
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
+ 图解
  + ![截屏2019-12-19下午11.03.37.png](https://pic.leetcode-cn.com/88bfee967d158aa1cc94bd56539577f166571e1f63ab3a0510629b4189f4a3c1-%E6%88%AA%E5%B1%8F2019-12-19%E4%B8%8B%E5%8D%8811.03.37.png)
+ 思路
  + 关键
    + 将当前节点的指针指向上一个节点
    + 然后更新当前节点和下一个节点的值即顺移
  + 技巧
    + 设置哨兵节点 null，初始化时将head节点指向null，下一步将next指向head
    + 重复以上动作直到当前节点为尾节点的节点null
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
var reverseList = function(head) {
    let prev = null;
    let curr = head;
    while(curr != null){
        let next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
};
```
#### 解法二：尾递归
+ 思路
  + 其实就是解法一的简化版
    + >prev = curr;  
     curr = next;
  + 此解法将 上面放在递归里返回
    + 同理都是做将当前节点指向前一个节点的操作之后，来顺移更新前一个、当前、和下一个节点的操作
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
var reverseList = function(head) {
    let reverse = (prev,curr) => {
        if(!curr)return prev;
        let next = curr.next;
        curr.next = prev;
        return reverse(curr,next);
    }
    return reverse(null,head);
};
```
#### 解法三：递归
+ 思路
  + 关键是反转操作
    + 当前节点 head，下一个节点 head.next
    + head.next.next = head
      + 此处将原 head.next 指向head，即是反转
    + head.next = null
      + 此处将原 head 指向head.next的指针断开
  + 递归
    + 由编译器函数调用执行栈原理可知
      + **最先调用的函数会在递归过程中最后被执行，而最后调用的会最先执行**
    + 因此此题，**最先返回最后两个节点开始反转操作**
      + 依次从后面两两节点开始反转
+ 图解
  + ![截屏2019-12-20上午8.07.31.png](https://pic.leetcode-cn.com/aa52928915a4e9d142e206901f8d334d767d29b7482e000078380f28d22c5699-%E6%88%AA%E5%B1%8F2019-12-20%E4%B8%8A%E5%8D%888.07.31.png)
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
var reverseList = function(head) {
    // 如果测试用例只有一个节点 或者 递归到了尾节点，返回当前节点 
    if(!head || !head.next) return head;
    // 存储当前节点的下一个节点
    let next = head.next;
    let reverseHead = reverseList(next);
    // 断开 head ，如图闪电⚡️标记处
    head.next = null;
    // 反转，后一个节点连接当前节点
    next.next = head;
    return reverseHead;
};
```
#### 解法四：栈解
+ 思路
  + 既然是反转，那么符合栈先进后出的特点
  + 将原节点依次入栈
  + 出栈时，重新构造链表改变指向
  + 同样设置哨兵节点
    + 最后返回哨兵的next即为所求
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
var reverseList = function(head) {
    let tmp = head;
    let tHead = new ListNode(0);
    let pre = tHead;
    let stack = [];
    while(tmp){
        stack.push(tmp.val);
        tmp = tmp.next;
    }
    while(stack.length != 0){
        pre.next = new ListNode(stack.pop());
        pre = pre.next;
    }
    return tHead.next;
};
```