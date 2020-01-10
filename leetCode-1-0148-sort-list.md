# 排序链表（中等）
# 题目描述
![截屏2020-01-09下午3.28.34.png](https://pic.leetcode-cn.com/75b24990d140db841402780728c4bd9e83fb1f45c5c02c8a95025f299b670240-%E6%88%AA%E5%B1%8F2020-01-09%E4%B8%8B%E5%8D%883.28.34.png)
# 题目地址
<https://leetcode-cn.com/problems/sort-list/>
#### 解法一：归并排序 - 递归
+ [经典排序算法讲解 - 归并排序 - 递归版](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md)
  + 跟上述讲解思路一模一样
+ 摘抄讲解如下
  + 1、**把长度为n的输入序列分成两个长度为n/2的子序列**
  + 2、**对这两个子序列分别采用归并排序**
  + 3、**将两个排序好的子序列合并成一个最终的排序序列**
+ 对应链表
  + 1、用双指针法(快慢指针)寻找链表中间节点
    + [参看链表各种操作大全 - 求链表的中间节点](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/algo/%E9%93%BE%E8%A1%A8_linkedList.md)
      + 奇数个节点找到中点，偶数个节点找到中心左边的节点
      + 注意
        + 找到中点后，要将链表切断，即 mid.next = null
        + 因链表性质，左边子序列取左端点即可
        + 同数组归并一样，只剩一个节点时终止
    + 用于分成左右两边子序列
      + 右边子序列为慢指针的next
  + 2、递归排序左右子序列
  + 3、合并
    + 同数组一样，判断值的大小
    + 不同的是，用哨兵节点链接合并后的链表，返回即可
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
var sortList = function(head) {
    let mergeList = (left,right) => {
        let res = new ListNode(0);
        let pre = res;
        while(left && right){
            if(left.val <= right.val){
                pre.next = left;
                left = left.next;
            }else{
                pre.next = right;
                right = right.next;
            }
            pre = pre.next;
        }
        pre.next = left ? left : right;
        return res.next;
    }
    let mergeSort = (node) => {
        if(!node || !node.next) return node;
        let mid = node;
        let fast = node.next;
        while(fast && fast.next){
            mid = mid.next;
            fast = fast.next.next;
        }
        let rightList = mid.next;
        mid.next = null;
        let left = node;
        let right = rightList;
        return mergeList(mergeSort(left),mergeSort(right));
    }
    return mergeSort(head);
};
```
#### 解法二：归并排序 - 非递归
+ [经典排序算法讲解 - 归并排序 - 非递归版](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/BitOperation%E4%BD%8D%E8%BF%90%E7%AE%97%E3%80%81Bloom%20Filter%E5%B8%83%E9%9A%86%E8%BF%87%E6%BB%A4%E5%99%A8%E3%80%81LRU%20Cache%E7%BC%93%E5%AD%98%E3%80%81Sorting%20algorithm%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95.md)
  + 跟上述讲解思路一模一样
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
var sortList = function(head) {
    // 哨兵节点
    let preHead = new ListNode(0);
    preHead.next = head;
    // 求链表长度
    let n = 0;
    let curr = head;
    while(curr){
        curr = curr.next;
        n++;
    }
    // 分割i长度的链表，返回剩余的链表
    let split = (node,i) => {
        while(i != 1 && node){
            node = node.next;
            i--;
        }
        let rest = node ? node.next : null;
        if(node) node.next = null;
        return rest;
    }
    // 合并
    let merge = (left,right,pre) => {
        let curr = pre;
        while(left && right){
            if(left.val <= right.val){
                curr.next = left;
                left = left.next;
            }else{
                curr.next = right;
                right = right.next;
            }
            curr = curr.next;
        }
        curr.next = left || right;
        while(curr.next) curr = curr.next;
        return curr;
    }
    // 合并 2*i 个
    for(let i = 1;i < n;i *= 2){
        let pre = preHead;
        let curr = preHead.next;
        // 分割左右两部分链表，并合并
        while(curr){
            let left = curr;
            let right = split(left,i);
            curr = split(right,i);
            pre = merge(left,right,pre);
        }
    }
    return preHead.next;
};
```