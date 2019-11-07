# 在每个树行中找最大值（中等）
# 题目描述
![截屏2019-11-07上午4.25.54.png](https://pic.leetcode-cn.com/afb01ff0962415ffe4a44af7b1636d2c5fb7ce5d9c14726439929786a91915a9-%E6%88%AA%E5%B1%8F2019-11-07%E4%B8%8A%E5%8D%884.25.54.png)
# 题目地址
<https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/>
#### 解法一：BFS
+ 类似题型解法
  + [102. 二叉树的层次遍历-解法一](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/102-er-cha-shu-de-ceng-ci-bian-li-by-alexer-660/) 
+ 本质和102题一模一样
+ 只是本题取的是每层的最大值
  + 因此只需要每次更新当前层遍历元素的最大值即可
  + 合并解即为所求
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
 * @return {number[]}
 */
var largestValues = function(root) {
    if(!root || root.length == 0){
        return [];
    }
    var result = [];
    var leverMax = 0;
    var currNodes = [root];
    while(currNodes.length != 0){
        var subresultMax = -Infinity;
        var nextSubresult = [];
        for(var i = 0;i<currNodes.length;i++){
            var currNode = currNodes[i];
            subresultMax = Math.max(subresultMax,currNode.val);
            if(currNode.left != null){
                nextSubresult.push(currNode.left);
            }
            if(currNode.right != null){
                nextSubresult.push(currNode.right);
            } 
        }
        result.push(subresultMax);
        currNodes = nextSubresult;
    }
    return result;
};
```
#### 解法二：DFS
+ 类似题型解法
  + [102. 二叉树的层次遍历-解法二](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/102-er-cha-shu-de-ceng-ci-bian-li-by-alexer-660/) 
+ 本质和102题一模一样
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
 * @return {number[]}
 */
var largestValues = function(root) {
    if(!root || root.length == 0){
        return [];
    }
    var result = [];
    function dfs(currNode,level){
        if(currNode != null){
            if(result[level] == undefined){
                result[level] = currNode.val;
            }else{
                result[level] = Math.max(currNode.val,result[level]);
            }
            if(currNode.left != null){
                dfs(currNode.left,level+1);
            }
            if(currNode.right != null){
                dfs(currNode.right,level+1)
            }
        }
    }
    dfs(root,0);
    return result;
};
```