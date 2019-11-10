# 二叉树的最大深度（简单）
# 题目描述
![截屏2019-10-23下午9.14.04.png](https://pic.leetcode-cn.com/6bcdbf43b934e6100850bdeacdf5d8d2cef070de6fcd91ca94fc83585b3a04ba-%E6%88%AA%E5%B1%8F2019-10-23%E4%B8%8B%E5%8D%889.14.04.png)
# 题目地址
<https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/>
#### 解法一：递归 DFS
+ 节点的高度 = Max(左子树的高度，右子树的高度) + 1
+ 以此类推，最后一个左或右节点高度为0 再反过来相加回去即可
+ 时间复杂度：O(n)
+ 空间复杂度 
  + 最坏情况下 O(n) 退化为单链表
  + 最好情况下 O(logn)  为平衡二叉树且高度为logn
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
var maxDepth = function(root) {
    function getDepth(root,leftSubtreeDepth,rightSubtreeDepth){
         if(root != null){
            leftSubtreeDepth = getDepth(root.left);
            rightSubtreeDepth = getDepth(root.right);
            return Math.max(leftSubtreeDepth,rightSubtreeDepth) + 1;   
        }else{
            return 0;
        }
    }
    return getDepth(root);
};
```
#### 解法二：迭代（基于栈）DFS
+ 不断边压栈边出栈
  + 先两边边开始分别都压一个  
  + 并先返回一边 剩下的出栈就都是另一边 即一次只出栈一个节点即可实现
+ 每次出栈取高度的最大值，初始化root根节点高度为1  就不用再加1了
+ 返回更新的高度最终确定比较值
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
var maxDepth = function(root) {
    var tmpStack = [
        {"key":root,"val":1}
    ];
    var depth = 0;
    while(tmpStack.length != 0){
        var currObj = tmpStack.pop();
        var currNode = currObj.key;
        if(currNode != null){
            var currNode_depth = currObj.val;
            depth = Math.max(depth,currNode_depth);
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
#### 解法三：队列 BFS
+ 广度优先遍历 BFS 
  + 此处即二叉树的层次(序)遍历
  + 求最大深度 亦即 求二叉树有几层
+ 广度优先代码
  + 特点："从左到右，从上到下" 
+ 队列
  + 特点："先进先出" 
+ 队列实现广度优先
  + 遍历二叉树节点，依次将当前节点 和它的左右子节点入队，并再一一出队
  + 针对子节点的节点重复上一步操作
  + 刚好符合"先进先出" => "先入队再出队"
  + 数组：push -> shift  
+ 所以二叉树的广度优先即层序遍历用队列实现为
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
    var maxDepth = function(root) {
        if(root == null){
            return 0;
        }
        var tmpQueue = [root];
        var result = [];
        var currNode = null;
        while(tmpQueue.length != 0){ --------------------------(1)
            currNode = tmpQueue.shift();
            result.push(currNode);
            if(node.left != null){
                tmpQueue.push(node.left);
            }
            if(node.right != null){
                tmpQueue.push(node.right);
            }
        }
        return result;
    };
    ``` 
+ 亦可改写为
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
    var maxDepth = function(root) {
        if(root == null){
            return 0;
        }
        var tmpQueue = [root];
        var result = [];
        var currNode = null;
        while(tmpQueue.length != 0){---------------------------(2)
            var size = tmpQueue.length;
            for(var i = 0;i<size;i++){
                currNode = tmpQueue.shift();
                result.push(currNode);
                if(node.left != null){
                    tmpQueue.push(node.left);
                }
                if(node.right != null){
                    tmpQueue.push(node.right);
                }
            }
        }
        return result;
    };
    ``` 
+ 代码分析
  + (1)和(2)的含义是什么？为什么代码段相同
    + 相同点
      + root 根节点先入队、出队 好获得根节点的左、右子树继续操作 
      + 均是出队一个节点再入队1-2个节点 同步操作
    + 不同点
      + (1)是 直接从上到下、从左到右不断入队节点 出队只管按顺序出就对了
      + (2)是 分层出、入队
        + 对于遍历到的任意一个节点从根节点开始 即当前子树时
          + 先出队当前节点，再入队它的左、右子节点
          + <=> ['根节点','根节点的左子节点','根节点的右子节点']
          + 重复 第一步操作
          + <=> ['根节点的左子节点','根节点的右子节点','根节点的左子节点的左子节点',''根节点的左子节点的左子节点的右子节点'']
          + 循环loop
            + 次数 size 即为当前层数有几个节点 
+ 所以由代码分析中不同点(2) 和 前面分析 可知
  + 一层就是一个深度 
  + 在层数遍历前设定一个自加的变量即为所求
+ 题意代码如下
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
var maxDepth = function(root) {
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
+ 此解法类似[102. 二叉树的层次遍历-解法二](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/102-er-cha-shu-de-ceng-ci-bian-li-by-alexer-660/)
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
var maxDepth = function(root) {
    if(root == null){
        return 0;
    }
    var result = 1;
    function helper(root,level){
        if(root != null){
            result = Math.max(result,level);
            if(root.left != null){
                helper(root.left,level+1);
            }
            if(root.right != null){
                helper(root.right,level+1);
            }              
        }
    }
    helper(root,result);
    return result;
};
```