# 合并K个排序链表（困难）
# 题目描述
![截屏2020-01-07上午10.33.42.png](https://pic.leetcode-cn.com/13b74b8aeeb516c4b6857081402c4692f3b6c1d5d281ed74d78d185d74008bc0-%E6%88%AA%E5%B1%8F2020-01-07%E4%B8%8A%E5%8D%8810.33.42.png)
# 题目地址
<https://leetcode-cn.com/problems/merge-k-sorted-lists/>
#### 各类算法模板
+ [递归 + 分治算法模板](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
#### 解法一：双指针法 + 两两合并
+ 时间复杂度：O(kn)(k为链表个数，n为当前两个合并中链表的长度)
+ 空间复杂度：O(1)
+ [思路同 - 21. 合并两个有序链表 - 解法一](https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/21-he-bing-liang-ge-you-xu-lian-biao-by-alexer-660/)
+ 区别
  + 不止两个链表
  + 两两合并
    + 遍历数组，合并两个成新的一个链表再继续合并下一个链表
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
    let mergeTwoLists = (l1,l2) => {
        let preHead = new ListNode(-1)
        let preNode = preHead
        while(l1 && l2){
            if(l1.val <= l2.val){
                preNode.next = l1
                l1 = l1.next
            }else{
                preNode.next = l2
                l2 = l2.next
            }
            preNode = preNode.next
        }
        preNode.next = l1 ? l1 : l2
        return preHead.next
    }
    let n = lists.length
    if(n == 0) return null
    let res = lists[0]
    for(let i = 1;i < n;i++){
        if(lists[i]){
            res = mergeTwoLists(res,lists[i])
        }
    }
    return res
};
```
#### 解法二：递归 + 分治
+ 时间复杂度：O(nlogK)
  + k为链表总数
  + n为合并两个链表所用时间
+ 空间复杂度：O(n)
+ [递归思路同 - 21. 合并两个有序链表 - 解法二](https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/21-he-bing-liang-ge-you-xu-lian-biao-by-alexer-660/)
+ 分治
  + [参考各类算法模板 - 分治模板一处](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
  + 借用官方一张图说明分治合并过程
    ![](https://pic.leetcode-cn.com/6f70a6649d2192cf32af68500915d84b476aa34ec899f98766c038fc9cc54662-image.png)
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
    let n = lists.length;
    if(n == 0) return null;
    let mergeTwoLists = (l1,l2) => {
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        if(l1.val <= l2.val){
            l1.next = mergeTwoLists(l1.next,l2);
            return l1;
        }else{
            l2.next = mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
    let merge = (left,right) => {
        if(left == right) return lists[left];
        let mid = (left + right) >> 1;
        let l1 = merge(left,mid);
        let l2 = merge(mid+1,right);
        return mergeTwoLists(l1,l2);
    }
    return merge(0,n-1);
};
```
#### 解法三：优先级队列
+ 参考大佬的
+ 时间复杂度：O(Nlogk)
  + k为链表数目
  + 弹出操作时，比较操作为O(logK)
  + 找最小值操作为O(1)
  + N为链表元素总数和
+ 空间复杂度：O(n+k)
  + n为创建新链表开销
  + k为额外队列开销
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
    let queue = new PriorityQueue();
    lists.forEach(list => {
        if(list) queue.enqueue(list, list.val)
    });

    let res = new ListNode(-1);
    let cur = res;
    while(!queue.isEmpty()) {
        cur.next = queue.dequeue();
        cur = cur.next;
        if(cur.next) queue.enqueue(cur.next, cur.next.val);
    }
    return res.next;
}

class Node {
	constructor(val, priority) {
		this.val = val;
		this.priority = priority;
	}
}

class PriorityQueue {
	constructor() {
		this.values = [];
	}

	enqueue(val, priority) {
		let node = new Node(val, priority);
		this.values.push(node);
		this.bubbleUp();
	}

	dequeue() {
		let max = this.values[0];
		let end = this.values.pop();
		if(this.values.length) {
			this.values[0] = end;
			this.bubbleDown();
		}
		return max.val;
	}
    
    isEmpty() {
        return !this.values.length;
    }
    
    bubbleUp(index = this.values.length - 1) {
		if(index <= 0) return;
		let parentIndex = Math.floor((index - 1) / 2);
		if(this.values[index].priority <= this.values[parentIndex].priority) {
			[this.values[index], this.values[parentIndex]] = [this.values[parentIndex], this.values[index]];
			this.bubbleUp(parentIndex);
		}
	}
	
	bubbleDown(index = 0, swapIndex = null) {
		let leftIndex = index * 2 + 1,
			rightIndex = index * 2 + 2,
			length = this.values.length;

		if(leftIndex < length) {
			if(this.values[leftIndex].priority <= this.values[index].priority) {
				swapIndex = leftIndex;
			}
		}

		if(rightIndex < length) {
			if((swapIndex === null && this.values[rightIndex].priority <= this.values[index].priority) || (swapIndex !== null && this.values[rightIndex].priority <= this.values[leftIndex].priority)) {
				swapIndex = rightIndex;
			}
		}

		if(swapIndex !== null) {
			[this.values[index], this.values[swapIndex]] = [this.values[swapIndex], this.values[index]];
			this.bubbleDown(swapIndex, null);
		}
	}
};
```
#### 解法四：数组
+ 时间复杂度：O(NlogN)
  + N：节点总数
  + O(N)：遍历开销
  + O(NlogN)：排序(假设稳定)
  + O(N)：创建新的有序链表
+ 空间复杂度：O(N)
  + O(N)：排序
  + O(N)：创建新的有序链表
+ 思路
  + 遍历所有链表元素，将元素值放到一个数组中
  + 排序数组，相当于按优先级排序优先级队列
  + 遍历排好序的数组，重新构造一个新的有序链表即为所求
```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
    if(!lists || lists.length == 0) return null;
    let arr = [];
    let res = new ListNode(0);
    lists.forEach(list => {
        let cur = list;
        while(cur){
            arr.push(cur.val);
            cur = cur.next;
        }
    })
    let cur = res;
    arr.sort((a,b) => a-b).forEach(val => {
        let node = new ListNode(val);
        cur.next = node;
        cur = cur.next;
    })
    return res.next;
};
```