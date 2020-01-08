# 合并两个有序链表（简单）
# 题目描述
![截屏2019-10-20上午9.20.49.png](https://pic.leetcode-cn.com/e8b883bd181265f5c673702b66cd09056811a2c452495a35209cffaf90006068-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.20.49.png)
# 题目地址
<https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/>
#### 各类算法模板
+ [递归算法模板](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
#### 解法一：双指针法
+ 时间复杂度：O(a+b) 循环比较两个子问题的次数为 a+b a,b为两个子问题的长度
+ 空间复杂度：O(1) 双指针，常数级别复杂度
```javascript
/**
    * Definition for singly-linked list.
    * function ListNode(val) {
    *     this.val = val;
    *     this.next = null;
    * }
    */
/**
    * @param {ListNode} l1
    * @param {ListNode} l2
    * @return {ListNode}
    */
var mergeTwoLists = function(l1, l2) {
    var prevHead = new ListNode(-1);
    var prevNode = prevHead;
    while (l1 != null && l2 != null) {
        if(l1.val <= l2.val){
            prevNode.next = l1; 
            l1 = l1.next
        }else{
            prevNode.next = l2;
            l2 = l2.next;
        }
        prevNode = prevNode.next;
    }
    prevNode.next = l1 ? l1 :l2;
    return prevHead.next;
};
```
#### 解法二：递归
+ 时间复杂度：O(n)(n为l1和l2的每个元素的遍历次数和)
+ 空间复杂度：O(n)(n为l1和l2的空间和)
+ 编程技巧：递归 + 原地斩链相连
  + 递归比较查看两个链表哪个元素先小 就斩断此元素位置链条⛓️连接到另一链表上 如此也不需要另外开辟存储空间
  + 斩断后 重连铁链的动作因为要自动非人工 所以需要程序自己调用自己 即为递归
  + 斩断后需要连的结点 通过 return 最小结点 即动态更新 斩断结点位置 
  + 随时连接下一个符合要求的位置（x.next = 求下一个需要连接的结点位置(即程序自动搜索即递归) && x = 下一个需要连接的结点位置）
  + 返回修改后的 l1头结点的链表 或 l2头结点的链表
```javascript  
/**
* Definition for singly-linked list.
* function ListNode(val) {
*     this.val = val;
*     this.next = null;
* }
*/
/**
* @param {ListNode} l1
* @param {ListNode} l2
* @return {ListNode}
*/
var mergeTwoLists = function(l1, l2) {
    if(l1 == null){
        return l2;
    }
    if(l2 == null){
        return l1;
    }
    if(l1.val <= l2.val){
        l1.next = mergeTwoLists(l1.next,l2);
        return l1;
    }else{
        l2.next = mergeTwoLists(l1,l2.next);
        return l2;
    }
}
```