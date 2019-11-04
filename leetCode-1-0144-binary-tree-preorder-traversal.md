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
#### 解法二：模拟栈
+ [戳看94.二叉树的中序遍历题解](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/94-er-cha-shu-de-zhong-xu-bian-li-by-alexer-660/)
+ 简析
    + 栈：后进先出
      + 通过出栈依次获取所求中序序列 
    + 中序：方向从左到右遍历，当前根节点位于前【根-左-右】
      + 为符合栈的特点 && 节省遍历时间  所以选择边遍历边入栈临界点后出栈
        +  先入栈完整二叉树的根节点 root 及 当前节点的左子树的左子节点
        +  出栈临界点
           + 先出栈完整二叉树的根节点root，第二遍循环时***正序***继续出栈左子树的节点(包括每一个子树的根节点)
           + 再只有当前节点 没有左子节点 && 只有右子节点时出栈
             + 有左子节点时：
               + 即当前子节点为当前子树的右子节点（递归：从最右下到右上）
                 + 更新当前根节点入栈且没有左子树时出栈
                 + 有左子树从头递归调用
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
    var tmpStack = [];
    var currNode = root;
    while( currNode != null || tmpStack.length !=0 ){
        while(currNode != null){
            result.push(currNode.val);
            tmpStack.push(currNode);
            currNode = currNode.left;
        }
        currNode = tmpStack.pop();
        currNode = currNode.right;
    }
    return result;
};
```