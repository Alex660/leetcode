# 二叉树的最小深度（简单）
# 题目描述
![截屏2020-03-18下午9.09.24.png](https://pic.leetcode-cn.com/e3ff04f389ec1b12e90ca775f0768ed44802e5824867793fb003de258304d861-%E6%88%AA%E5%B1%8F2020-03-18%E4%B8%8B%E5%8D%889.09.24.png)
# 题目地址
<https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/>
#### 各类算法模板
+ [递归 + DFS + BFS 算法模板](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
#### 解法一：递归 DFS
+ [思路同 104. 二叉树的最大深度 - 解法一](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/104-er-cha-shu-de-zui-da-shen-du-by-alexer-660/)
+ 区别
  + 求最小深度时，有特殊情况
    + 当根节点只有一个左子节点或右子节点时，最小深度为1，不包括根节点自己
  + 类似
    + ```root.left === null || root.right === null```
    + 此时，如104解法一，```leftSubDepth```和```rightSubDepth```必定有一个为0
      + 如果还用```Math.min(leftSubDepth,rightSubDepth) + 1 ```来算的话，会算得```0+1=1```，此时就相当于把当前```rooot```节点算进去了，这显然是不和题意的
      + 因此要特殊处理，把其中一个不等于0的算进去才对，即
        + ```leftSubDepth + rightSubDepth + 1```
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
 * @return {number}
 */
var minDepth = function(root) {
    let getDepth = (root,leftSubDepth,rightSubDepth) => {
        if(root) {
            leftSubDepth = getDepth(root.left)
            rightSubDepth = getDepth(root.right)
            return root.left && root.right ? Math.min(leftSubDepth,rightSubDepth) + 1 : leftSubDepth + rightSubDepth + 1;
        }
        return 0
    }
    return getDepth(root)
};
```
#### 解法二：迭代+栈+DFS
+ [思路同 104. 二叉树的最大深度 - 解法二](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/104-er-cha-shu-de-zui-da-shen-du-by-alexer-660/)
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
 * @return {number}
 */
var minDepth = function(root) {
  if(!root) return 0;
  var tmpStack = [
    {"key":root,"val":1}
  ];
  var depth = Number.MAX_SAFE_INTEGER;
  while(tmpStack.length != 0){
    var currObj = tmpStack.pop();
    var currNode = currObj.key;
    if(currNode != null){
      var currNode_depth = currObj.val;
      !currNode.left && !currNode.right && (depth = Math.min(depth,currNode_depth));
      if(currNode.left != null){
        tmpStack.push({"key":currNode.left,"val":currNode_depth + 1});
      }
      if(currNode.right != null){
        tmpStack.push({"key":currNode.right,"val":currNode_depth + 1});
      }
    }
  }
  return depth;
};
```
#### 解法三：BFS
+ [思路同 104. 二叉树的最大深度 - 解法三](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/104-er-cha-shu-de-zui-da-shen-du-by-alexer-660/)
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
 * @return {number}
 */
var minDepth = function(root) {
  if(root == null){
    return 0;
  }
  var tmpQueue = [root];
  var depth = 0;
  var currNode = null;
  while(tmpQueue.length != 0){
    var size = tmpQueue.length; 
    depth++; 
    while(size >0){
      currNode = tmpQueue.shift();
      if(!currNode.left && !currNode.right){ 
        return depth
      };
      if(currNode.left != null){
        tmpQueue.push(currNode.left);
      }
      if(currNode.right != null){
        tmpQueue.push(currNode.right);
      }
      size--;
    }      
  }
  return depth;
};
```
#### 解法四：递归DFS
+ [思路同 104. 二叉树的最大深度 - 解法四](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/104-er-cha-shu-de-zui-da-shen-du-by-alexer-660/)
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
 * @return {number}
 */
var minDepth = function(root) {
  if(root == null){
    return 0;
  }
  var result = Number.MAX_SAFE_INTEGER;
  function helper(root,level){
    if(root != null){
      !root.left && !root.right && (result = Math.min(result,level));
      if(root.left != null){
        helper(root.left,level+1);
      }
      if(root.right != null){
        helper(root.right,level+1);
      }              
    }
  }
  helper(root,1);
  return result;
};
```