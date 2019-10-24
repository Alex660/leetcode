# 验证二叉搜索树（中等）
# 题目描述
![截屏2019-10-23下午7.50.54.png](https://pic.leetcode-cn.com/de8d369231a0de5e170d37a561a16776b615f6ef7412805237d62a57d58a5433-%E6%88%AA%E5%B1%8F2019-10-23%E4%B8%8B%E5%8D%887.50.54.png)
# 题目地址
<https://leetcode-cn.com/problems/validate-binary-search-tree/>
#### 解法一：二叉树的中序遍历基于栈
+ [戳看94.二叉树的中序遍历题解](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/94-er-cha-shu-de-zhong-xu-bian-li-by-alexer-660/)
+ 中序遍历又叫升序遍历
  + 所以仅需判断当前出栈的节点与上一个节点的大小，大于等于则不符合
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
 * @return {boolean}
 */
var isValidBST = function(root) {
    if(root && root.right == null && root.left == null){
        return true;
    }
    var result = [];
    var tmpStack = [];
    var currNode = root;
    var lastNode = '';
    while(currNode != null || tmpStack.length != 0){
        while(currNode != null){
            tmpStack.push(currNode);
            currNode = currNode.left;
        }
        currNode = tmpStack.pop();
        if(lastNode!=='' && currNode.val <= lastNode){
            return false;
        }
        lastNode = currNode.val;
        currNode = currNode.right;
    }
    return true;
};
```
#### 解法二：递归验证
+ [戳看94二叉树的中序遍历指解法二](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/94-er-cha-shu-de-zhong-xu-bian-li-by-alexer-660/)
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
 * @return {boolean}
 */
var isValidBST = function(root) {
    var lastNode = '';
    function pushRoot(root){
        if(root == null){
            return true;
        }
        if(pushRoot(root.left)){
            if(lastNode < root.val || lastNode === ''){
                lastNode = root.val;
                return pushRoot(root.right);
            }  
        }
        return false;
    }
    return pushRoot(root);
};
```