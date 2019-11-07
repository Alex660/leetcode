# 二叉树的层次遍历（中等）
# 题目描述
![截屏2019-10-28上午11.06.17.png](https://pic.leetcode-cn.com/05bf3d6bf02987c94057a44611f80419605cb65f1d32daa9d798a745fdd4d7f1-%E6%88%AA%E5%B1%8F2019-10-28%E4%B8%8A%E5%8D%8811.06.17.png)
# 题目地址
<https://leetcode-cn.com/problems/binary-tree-level-order-traversal/#/description>
#### 解法一：BFS
+ 本题难度在于广度优先遍历和根据层次返回结果集
  + 广度优先 从左到右
  + 层次封装
    + 此处方法采用直观的对每层的节点进行遍历加入子集数组 循环后加入结果数组
    + 每层的节点要提前放入1个临时数组中方便循环遍历
      + 循环每个节点时，当前节点为当前层，当前节点的左右子节点为下一层的临时数组
      + 递归 两层循环，外层判断是否还有值，内层遍历每层节点数组的每个节点取值入子集
    + 出了内层循环 便成就了一层节点子集 将其加入结果数组
    + 返回结果数组即为所求   
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
 * @return {number[][]}
 */
var levelOrder = function(root) {
    if(!root || root.length == 0){
        return [];
    }
    var result = [];
    var currNodes = [root];
    while(currNodes.length != 0){
        var subResult = [];
        var nextSubResult = [];
        for(var i = 0;i<currNodes.length;i++){
            subResult.push(currNodes[i].val);
            if(currNodes[i].left != null){
                nextSubResult.push(currNodes[i].left);
            }
            if(currNodes[i].right != null){
                nextSubResult.push(currNodes[i].right);
            }
        }
        result.push(subResult);
        currNodes = nextSubResult;
    }
    return result;
};
```
#### 解法二：DFS
+ 深度优先遍历去实现层次遍历 关键还是在于层次 如何层次返回结果集
  + DFS 特点在于深度
  + 每层的节点其深度其实一样
    + 因此每个深度作为一个映射，遍历时遇到同样深度的映射，将值添加进去
  + 遍历结束，返回各个深度(层次)映射的结果数组即为所求 
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
 * @return {number[][]}
 */
var levelOrder = function(root) {
    if(!root || root.length == 0){
        return [];
    }
    var result = [];
    function dfs(currNode,level){
        if(currNode != null){
            (!result[level]) && (result[level] = []);
            result[level].push(currNode.val);
            if(currNode.left != null){
                dfs(currNode.left,level+1);
            }
            if(currNode.right != null){
                dfs(currNode.right,level+1);
            }           
        }
    }
    dfs(root,0);
    return result;
};
```