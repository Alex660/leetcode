# 二叉树的前序遍历(中等)
# 题目描述
![截屏2019-10-22下午3.49.54.png](https://pic.leetcode-cn.com/d835a9ee7508351968b3e378b78a2a18114ef1ffcf234715ef4fefbfd96a06ae-%E6%88%AA%E5%B1%8F2019-10-22%E4%B8%8B%E5%8D%883.49.54.png)
# 题目地址
<https://leetcode-cn.com/problems/binary-tree-preorder-traversal/>
#### 解法一：递归
+ 关键在于推出递推公式
    >前序遍历的递推公式：  
    preOrder(r) = print r->preOrder(r->left)->preOrder(r->right)

    >中序遍历的递推公式：  
    inOrder(r) = inOrder(r->left)->print r->inOrder(r->right)  

    >后序遍历的递推公式：  
    postOrder(r) = postOrder(r->left)->postOrder(r->right)->print r
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
var preorderTraversal = function(root) {
    var result = [];
    function pushRoot(node){
        if(node != null){
            result.push(node.val);
            if(node.left != null){
                pushRoot(node.left);
            }
            if(node.right != null){
                pushRoot(node.right);
            } 
        }
    }
    pushRoot(root);
    return result;
};
```