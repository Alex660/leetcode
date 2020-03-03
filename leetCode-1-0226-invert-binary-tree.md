# 翻转二叉树（简单）
# 题目描述
![截屏2020-03-01下午10.03.21.png](https://pic.leetcode-cn.com/364cc5922a9fb544ab9888ddf62fb62796cbe15e24c463566826f9dfbe4049ac-%E6%88%AA%E5%B1%8F2020-03-01%E4%B8%8B%E5%8D%8810.03.21.png)
# 题目地址
<https://leetcode-cn.com/problems/invert-binary-tree/>
#### 各类算法模板
+ [递归](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
+ [DFS - 栈](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
+ [BFS - 队列](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
+ [二叉树的前、中、后序遍历 - 解法大全](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/demos/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%B8%89%E5%BA%8F%E9%81%8D%E5%8E%86.md)
#### 解法一：递归
+ 时间复杂度：O(n)
+ 空间复杂度：O(n)
+ 思路
  + 递归交换当前节点的左右节点，当节点为null时返回
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    if(root === null) {
        return null;
    }
    let right = invertTree(root.right);
    let left = invertTree(root.left);
    root.left = right;
    root.right = left;
    return root;
};
```
+ 或者这样写亦可
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    if(root === null) {
        return null;
    }
    [root.left,root.right] = [invertTree(root.right),invertTree(root.left)];
    return root;
};
```
#### 解法二：DFS 栈
+ 思路
  + 深度优先遍历
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    let stack = [root];
    while(stack.length > 0){
        let cur = stack.pop();
        if(cur === null) continue;
        [cur.left,cur.right] = [cur.right,cur.left];
        stack.push(cur.right);
        stack.push(cur.left);
    }
    return root;
};
```
#### 解法三：BFS 队列
+ 时间复杂度：O(n)
+ 空间复杂度：O(n)
+ 思路
  + 广度优先遍历
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    let queue = [root];
    while(queue.length > 0){
        let cur = queue.pop();
        if(cur === null) continue;
        [cur.left,cur.right] = [cur.right,cur.left];
        queue.unshift(cur.left);
        queue.unshift(cur.right);
    }
    return root;
};
```
#### 解法四：前序遍历
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    if(root === null) return null;
    [root.left,root.right] = [root.right,root.left];
    invertTree(root.left);
    invertTree(root.right);
    return root;
};
```
#### 解法五：中序遍历
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    if(root === null) return null;
    invertTree(root.left);
    [root.left,root.right] = [root.right,root.left];
    // 此时的root.left 是上一步的 root.right
    invertTree(root.left);
    return root;
};
```
#### 解法六：后序遍历
```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    if(root === null) return null;
    invertTree(root.left);
    invertTree(root.right);
    [root.left,root.right] = [root.right,root.left];
    return root;
};
```