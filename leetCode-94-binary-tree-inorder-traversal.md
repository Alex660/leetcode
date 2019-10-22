# 二叉树的中序遍历（中等）
# 题目描述
![截屏2019-10-22上午9.35.37.png](https://pic.leetcode-cn.com/f71cfda4727039d147e65f213eab2346213fc38cb3393f33bf10da016c93171b-%E6%88%AA%E5%B1%8F2019-10-22%E4%B8%8A%E5%8D%889.35.37.png)
#### 解法一：递归
+ 时间复杂度：O(n)
  + 递归函数：T(n) = 2*T(n/2)+1
  + 每个节点最多会被访问两次，所以遍历操作的时间复杂度，跟节点的个数成正比
+ 空间复杂度：O(logn) 
  + 此为平均情况下 因为是 分叉查询
  + 最坏情况为 O(n)
+ 关键在于推出递推公式
    >前序遍历的递推公式：  
    preOrder(r) = print r->preOrder(r->left)->preOrder(r->right)

    >中序遍历的递推公式：  
    inOrder(r) = inOrder(r->left)->print r->inOrder(r->right)  

    >后序遍历的递推公式：  
    postOrder(r) = postOrder(r->left)->postOrder(r->right)->print r

+ 根据题意 把 print 换成 push存储操作即可

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
var inorderTraversal = function(root) {
    var result = [];
    function pushRoot(root){
        if(root != null){
            if(root.left != null){
                pushRoot(root.left);
            }
            result.push(root.val);
            if(root.right !=null){
                pushRoot(root.right);
            }
        }
    }
    pushRoot(root);
    return result;
};
```
#### 解法二：基于栈的遍历
+ 时间复杂度：O(n)
+ 空间复杂度：O(n)
+ 分析
  + 二叉树中序遍历的特点
    + 对于任意一个节点 先取它的左子节点 再取它本身 最后取它的右节点 如解法一递推公式
    + 左子节点+根节点+右子节点
  + 栈的特点  
    + 先进后出 
  + 结合题意
    1. 设***当前根节点***为 curr = root 
    2. 遍历时，遍历到的每个子节点都相当于 当前根节点 且 由公式可知方向是从左往右
         + 所以将当前根节点 curr(第一遍时为最上根节点root)***入栈***(可能是左子节点，也可能是右子节点)
         + 和它的全部左子树全部***入栈***
         + 遍历条件 curr != null
         + 栈不为空 ----- <2.4>
    3. 出栈
         + ***出栈***一个节点tmpNode(为所有左子节点中逆序中的一个) 
           + 【出栈的左子节点可能为当前子树的左子节点或根节点或右子节点】-----<3.1.1>
           + 第一遍时为最底层的左子节点，
           + 其余遍可能为节点的左子节点或节点的右子节点
         + 且临时将此节点作为根节点
         + 更新当前根节点 curr = tmpNode
         + 判断当前作为临时根节点的左子节点有没有右子节点即右子树 
           + 有就更新 curr = tmpNode.right
             + 重新从第 2 步循环 ***入栈***右子节点
               + 到第 3 步后就会***出栈***右子节点 -----<3.4.1.1.1>
           + 没有就直接***出栈***之前的root根节点和所有左子树的子节点中***一个***节点
             + 【出栈的左子节点可能为当前子树的左子节点或根节点】
             + 继续更新当前根节点为其右子节点并从第1步继续循环
               + 循环条件为 <2.4> 
    4. 返回代码中所有按顺序(-----<3.1.1> 和 <3.4.1.1.1>)出栈的节点即为所求。
  + 简析
    + 栈：后进先出
      + 通过出栈依次获取所求中序序列 
    + 中序：方向从左到右遍历，当前根节点位于中间【左-根-右】
      + 为符合栈的特点 && 节省遍历时间  所以选择边遍历边入栈到临界点后出栈
        +  先入栈当前节点的所有左子节点
        +  出栈临界点
           + 先出栈左子树最左下边的子节点，第二遍循环时逆序继续出栈左子树的根节点
           + 再只有当前节点 没有左右子节点 时 出栈
             + 即当前子节点为当前子树的根节点
               + 到了此时说明到了根节点则下一步要更新当前根子节点的父节点入栈
                 + 当前根子节点的父节点当作了根节点，当前根子节点当作了左子节点
               + 到了此时说明到了根节点则下一步要更新右子节点入栈
                 + 入栈后的子节点有左子节点则继续模拟递归
                 + 没有则直接入栈后出栈 并循环更新右子树（如有继续循环判断有无左子树）入栈出栈
             + 分析可知此种情况只有当此节点为左右子节点且子节点没有子节点时出现
        + '递归'分解自下至上
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
var inorderTraversal = function(root) {
    var result = [];
    var tmpStack = [];
    var currNode = root;
    while(currNode != null || tmpStack.length !=0){
        while(currNode != null){
            tmpStack.push(currNode);
            currNode = currNode.left;
        }
        currNode = tmpStack.pop();
        result.push(currNode.val);
        currNode = currNode.right;
    }
    return result;
};
```
#### 解法三：二叉线索树
> 有点小复杂，待更，预计本周内，敬请期待，谢谢～