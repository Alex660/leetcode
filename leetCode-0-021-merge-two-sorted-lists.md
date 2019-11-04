# 合并两个有序链表（简单）
# 题目描述

![截屏2019-10-20上午9.20.49.png](https://pic.leetcode-cn.com/e8b883bd181265f5c673702b66cd09056811a2c452495a35209cffaf90006068-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.20.49.png)
# 题目地址
<https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/>
# 解法一
![截屏2019-10-16上午10.17.43.png](https://pic.leetcode-cn.com/c0e0eff5be7e6453ff02de83b4febf3f3a58e1447ec42a57d44a36faebe7394b-%E6%88%AA%E5%B1%8F2019-10-16%E4%B8%8A%E5%8D%8810.17.43.png)


>合并两个有序链表：input: 1->2->4 , 1->3->4 output: 1->1->2->3->4->4

#### 解法一：双指针法
+ 时间复杂度：O(a+b) 循环比较两个子问题的次数为 a+b a,b为两个子问题的长度
+ 空间复杂度：O(1) 双指针，常数级别复杂度
+ 分析：第一眼看出来类似数组的归并排序最后一步 将两个排序好的子问题合并成一个大问题即结果
+ 但由于此处是链表有些不同 不过思维是可以利用的 本质是合并两个有序问题 另开一个链表空间存储合并排序的子元素
+ 对于javascirpt来说 得自己原生实现链表 且 需注意 因为是对象 可以利用引用地址传递的方式修改原对象地址的参数而不需每次都递归查询链表的尾结点
+ 根据题意返回合并后的链表 可以使用哨兵结点即利用新增一个莫须有的头结点的next来串联结果链表并返回
+ 返回终止条件 任何一个子问题没有元素后 直接将另一个未空子问题拼接到最后返回即可 因为 子问题均是从小到大排序
+ 代码实现：
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
    // 设置莫须有的头结点作为返回链表的头结点
    var prevHead = new ListNode(-1);
    // 利用js对象地址传递的方式动态更新范围链表的尾结点插入操作
    var prevNode = prevHead;
    // 临界值 
    while (l1 != null && l2 != null) {
        // 每次寻找并仅插入一个尾结点
        if(l1.val <= l2.val){
            // 插入尾结点
            prevNode.next = l1; 
            // 更新指针
            l1 = l1.next
        }else{
            //同上
            prevNode.next = l2;
            l2 = l2.next;
        }
        // js对象引用传递的特性 prevHead 已经更新成加了当前尾结点
        // prevNode 赋值为 返回链表的尾结点对象地址 && 并不修改原地址 循环往复下 只会更新 原链表对象地址的尾结点元素值
        // 当前 prevNode.next 即相当于 原链表的尾部结点地址 类似 原链表.next.next.....next
        prevNode = prevNode.next;
    }
    // 因子问题均有序 剩下不为空结点的链表可以直接追加到 合并好的部分结果链表中
    // 此时的prevNode即为合并好的部分的结果链表的尾结点
    prevNode.next = l1 ? l1 :l2;
    // 返回哨兵结点的next即为所求
    return prevHead.next;
};
```
#### 解法二：分治 
+ 编程技巧：递归 + 原地斩链相连
+ 递归比较查看两个链表哪个元素先小 就斩断此元素位置链条⛓️连接到另一链表上 如此也不需要另外开辟存储空间
+ 斩断后 重连铁链的动作因为要自动非人工 所以需要程序自己调用自己 即为递归
+ 斩断后需要连的结点 通过 return 最小结点 即动态更新 斩断结点位置 
+ 随时连接下一个符合要求的位置（x.next = 求下一个需要连接的结点位置(即程序自动搜索即递归) && x = 下一个需要连接的结点位置）
+ 返回修改后的 l1头结点的链表 或 l2头结点的链表
+ listA listB  merge must return a needful node
+ listA[0] < listB[0] && listA[0] + merge(listA.slice(1),listB)
+ else if listA[0] > listB[0] && listA + merge(listB.slice(1))
+ 示例==> 
+ merge(listA,listB) =>
+ listA[0] -> merge(listA.slice(1),listB)  ==  listA[0] -> listB[0] -> listA[1] -> listB[2] -> listB[n] -> listA[n]
+ // 代码实现
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