# 环形链表（简单）
# 题目描述
![截屏2019-12-30下午7.05.19.png](https://pic.leetcode-cn.com/07103c5a18fc12c40799d0ee062b18ed6b8a38d656f5d3456c74e4f2eaa88463-%E6%88%AA%E5%B1%8F2019-12-30%E4%B8%8B%E5%8D%887.05.19.png)
![截屏2019-12-30下午7.05.33.png](https://pic.leetcode-cn.com/ba29fc198b1867c7a48b0c9ab5d9f5b93237d9fef91ceb6a746e458d60b27c5e-%E6%88%AA%E5%B1%8F2019-12-30%E4%B8%8B%E5%8D%887.05.33.png)
# 题目地址
<https://leetcode-cn.com/problems/linked-list-cycle/>
## 链表
+ [参看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
#### 解法一：数组判重
+ 环中两个节点相遇，说明在不同的时空内会存在相遇的那一刻，过去与未来相遇，自己和前世回眸，即是重复
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
 * @return {boolean}
 */
var hasCycle = function(head) {
    let res = [];
    while(head != null){
        if(res.includes(head)){
            return true;
        }else{
            res.push(head);
        }
        head = head.next;
    }
    return false;
};
```
#### 解法二：标记法
+ 思路
  + 一路遍历，走过的地方，标识走过，当下次再遇到，说明鬼打墙了，在绕圈子！！！妈呀，吓死宝宝👶了，😭
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
 * @return {boolean}
 */
var hasCycle = function(head) {
    while(head && head.next){
        if(head.flag){
            return true;
        }else{
            head.flag = 1;
            head = head.next;
        }
    }
    return false;
};
```
#### 解法三：双指针
+ 思路
  + 在一个圆里，运动快的点在跑了n圈后，一定能相遇运动慢的点
    + 对应链表，就是两个指针重合
  + 如果不是圆，哪怕是非闭合的圆弧，快点一定先到达终点🏁，则说明不存在环
    + 对应链表，就是结尾指针指向null
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
 * @return {boolean}
 */
var hasCycle = function(head) {
    if(!head || !head.next) return false;
    let fast = head.next;
    let slow = head;
    while(fast != slow){
        if(!fast || !fast.next){
            return false;
        }
        fast = fast.next.next;
        slow = slow.next;
    }
    return true;
};
```