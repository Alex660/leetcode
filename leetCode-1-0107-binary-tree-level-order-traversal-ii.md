# 二叉树的层次遍历 II（简单）
# 题目描述
![截屏2020-05-04下午4.25.11.png](https://pic.leetcode-cn.com/f1c147703cade78c0c7c6c34ddb1d5470abbac584ec00b9199a86dcf7ac05915-%E6%88%AA%E5%B1%8F2020-05-04%E4%B8%8B%E5%8D%884.25.11.png)
# 题目地址
<https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/>
#### 解法一：DFS
+ 类似题型
  + [102. 二叉树的层序遍历 - 解法二](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/102-er-cha-shu-de-ceng-ci-bian-li-by-alexer-660/)
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
var levelOrderBottom = function(root) {
    if(!root || root.length === 0) {
        return []
    }
    let res = []
    let dfs = (curr,lev)  => {
        if(curr) {
            !res[lev] && (res[lev] = [])
            res[lev].push(curr.val)
            if(curr.left) dfs(curr.left,lev+1)
            if(curr.right) dfs(curr.right,lev+1)
        }
    }
    dfs(root,0)
    return res.reverse()
};
```
#### 解法二：BFS
+ 类似题型
  + [102. 二叉树的层序遍历 - 解法一](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/102-er-cha-shu-de-ceng-ci-bian-li-by-alexer-660/)
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
var levelOrderBottom = function(root) {
    if(!root || root.length === 0) {
        return []
    }
    let res = []
    let currNodes = [root]
    while(currNodes.length !== 0) {
        let subRes = []
        let nextSubRes = []
        for(let i = 0;i < currNodes.length;i++) {
            subRes.push(currNodes[i].val)
            if(currNodes[i].left) nextSubRes.push(currNodes[i].left)
            if(currNodes[i].right) nextSubRes.push(currNodes[i].right)
        }
        res.push(subRes)
        currNodes = nextSubRes
    }
    return res.reverse()
};
```