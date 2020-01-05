# 环形链表 II（中等）
# 题目描述
![截屏2020-01-04下午5.43.35.png](https://pic.leetcode-cn.com/8ef994c68092af56a9e30308ccbec734fc27dafdca341322495ecfde3e937f99-%E6%88%AA%E5%B1%8F2020-01-04%E4%B8%8B%E5%8D%885.43.35.png)
![截屏2020-01-04下午5.43.45.png](https://pic.leetcode-cn.com/e12f6973a2faf20331ce336ef5022675e130edaebdecaad3249e586a0ce44db2-%E6%88%AA%E5%B1%8F2020-01-04%E4%B8%8B%E5%8D%885.43.45.png)
# 题目地址
<https://leetcode-cn.com/problems/linked-list-cycle-ii/>
## 链表
+ [参看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
#### 解法一：标记法
+ [思路同-141. 环形链表-解法二](https://leetcode-cn.com/problems/linked-list-cycle/solution/141-huan-xing-lian-biao-by-alexer-660/)
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
var detectCycle = function(head) {
    while(head && head.next){
        if(head.flag){
            return head;
        }else{
            head.flag = 1;
            head = head.next;
        }
    }
    return null;
};
```
#### 解法二：数组判重
+ [思路同-141. 环形链表-解法一](https://leetcode-cn.com/problems/linked-list-cycle/solution/141-huan-xing-lian-biao-by-alexer-660/)
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
var detectCycle = function(head) {
    let res = [];
    while(head != null){
        if(res.includes(head)){
            return head;
        }else{
            res.push(head);
        }
        head = head.next;
    }
    return null;
};
```
#### 解法三：双指针
+ 思路
  + 图解
    + ![截屏2020-01-05下午3.33.09.png](https://pic.leetcode-cn.com/a9c06dc433fdf11b37d6dcee52660e1657472418e3258d6ddae2abb1d6c09bb0-%E6%88%AA%E5%B1%8F2020-01-05%E4%B8%8B%E5%8D%883.33.09.png)
  + 公式
    + S：初始点到环的入口点A的距离
    + m：环的入口点到快慢双指针在环内的相遇点B的距离
    + L：环的周长
    + 如果 S == L - m
      + 那么可以设置两个步数相同的指针分别从，链表入口节点和快慢双指针相遇节点同时出发
        + 当他们第一次相遇时，即是环的入口节点A所在
    + 因此，我们需要证明 S == L - m 
      + 已知快指针的行走距离是慢指针行走距离的两倍
        + 那么他们在环内第一次相遇时
          + 慢指针走过了：S + xL
          + 快指针走过了：S + yL
        + 那么，设C为指针走过的距离
          + C(快) - C(慢) = (y-x)L = nL
          + C(慢) = S + m
        + 因为C(快) == 2C(慢)
        + 所以C(快) - C(慢) == C(慢)
          + S + m = nL
          + S  = nL - m
          + 而L为环的周长 ，n为任意正整数
    + 所以 S == L - m 成立
      + 解即为反证法的操作
  + 判断链表是否有环
    + [思路同-141. 环形链表-解法三](https://leetcode-cn.com/problems/linked-list-cycle/solution/141-huan-xing-lian-biao-by-alexer-660/)
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
var detectCycle = function(head) {
    if(!head || !head.next) return null;
    let slow = head;
    let fast = head;
    let start = head;
     while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            while (start != slow) {
                slow = slow.next;
                start = start.next;
            }
            return slow;
        }
    }
    return null;
};
```