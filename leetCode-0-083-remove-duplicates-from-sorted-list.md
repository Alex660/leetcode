# 删除排序链表中的重复元素（简单）
# 题目描述
![截屏2020-04-02下午3.24.52.png](https://pic.leetcode-cn.com/aa23739809c9669e95d407655f15b5bf923d2aa58e7efbda7fb78f79a3c69256-%E6%88%AA%E5%B1%8F2020-04-02%E4%B8%8B%E5%8D%883.24.52.png)
# 题目地址
<https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/>
#### 链表 
+ [戳看链表各种操作大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
#### 解法：指针遍历
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
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
    let cur = head
    while(cur && cur.next) {
        if(cur.next.val === cur.val) {
            cur.next = cur.next.next
        }
        else{cur = cur.next}
    }
    return head
};
```