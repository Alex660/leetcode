# 最小栈（简单）
# 题目描述
![截屏2019-12-17上午10.01.06.png](https://pic.leetcode-cn.com/1c12b0d48e8220bf983fbe4bda317dcc68fc035d603dbd31d183b28699425b1c-%E6%88%AA%E5%B1%8F2019-12-17%E4%B8%8A%E5%8D%8810.01.06.png)
# 题目地址
<https://leetcode-cn.com/problems/min-stack/>
#### 解法一：基栈 + 辅助栈 
+ 时间复杂度：O(1)
+ 空间复杂度：O(n)
+ 思路
  + 额外维护一个最小值栈，初始化为第一个元素，用来保存**历史最小值集合**
  + 入栈
    + 当有更小值入栈时，将当前值入最小栈中
  + 出栈
    + 当出栈值 == 当前最小值时，最小栈的值也要删掉，最小值自热更新为前一步的最小值
  + 取最小值
    + 返回与当前基栈同步的最小栈的栈顶元素即为最小值
```javascript
/**
 * initialize your data structure here.
 */
var MinStack = function() {
    this.stack = [];
    this.min_stack = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
    this.stack.push(x);
    // 初始化 或 当有更小值入栈时，将当前值入最小栈中
    if(this.min_stack.length == 0 || this.min_stack[this.min_stack.length-1] >= x){
        this.min_stack.push(x);
    }
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    // 当出栈值 == 当前最小值时，最小栈的值也要删掉，最小值自热更新为前一步的最小值
    if(this.stack.pop() == this.min_stack[this.min_stack.length-1]){
        this.min_stack.pop();
    }
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.stack[this.stack.length-1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    // 返回与当前基栈同步的最小栈的栈顶元素即为最小值
    return this.min_stack[this.min_stack.length-1];
};

/** 
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```
#### 解法二：基栈
+ 最小值
  + 当前最小值入栈
    + 当遇到更小的一个值时
      + 将更小的值入栈
      + 并更新当前最小值
    + 出栈时
      + 如果出栈值是当前最小值的话，那么它的上一个入栈元素即为上一个最小值
      + 再出栈一次的值即为出栈后更新的最小值了
  + 例如
    + 原入栈值：[3,4,2]
      + Min([3]) = 3
      + Min([3，4]) = 3
      + Min([3，4，2]) = 2
    + 实际入栈：[3,3,4,2,2]
    + 出栈
      + 2 == 2
        + 当前最小值 = 2 (第1个)
        + [3,3,4]
```javascript
/**
 * initialize your data structure here.
 */
var MinStack = function() {
    this.stack = [];
    this.min = Number.MAX_SAFE_INTEGER;
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
    // 当前值比min值更小
    if(this.min >= x){
        // 将上一个min最小值保存入栈
        this.stack.push(this.min);
        // 更新当前最小值
        this.min = x;
    }
    this.stack.push(x)
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    // 如果弹出值 == 当前最小值，那么再弹一次的值为上一个最小值也即出栈后更新的最小值
    if(this.stack.pop() == this.min){
        this.min = this.stack.pop();
    }
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.stack[this.stack.length-1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    return this.min;
};

/** 
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```
#### 解法三：基栈
+ 思路
  + 一个栈
    + 入栈
      + 存储 **差值 = 原入栈值 - 当前最小值**
      + 更新 当前最小值
    + 出栈
      + 返回栈顶差值即可，因不需要求出原本的值
        + 当差值为负数时
          + **更新当前最小值 = 当前最小值 - 差值(负数)**
            + 原入栈值：[3,4,2]
              + Min([3]) = 3
              + Min([3，4]) = 3
              + Min([3，4，2]) = 2
            + 实际入栈：[0,1,-1]
              + 出栈
              + -1 < 0
                + 当前最小值 = 2 - (-1) = 3
                + [3,4]
    + 求栈顶元素
      + **当出栈差值为负数时**
        +  **原入栈值 = 当前最小值**
           + 原入栈值：[3,4,2]
             + Min([3]) = 3
             + Min([3，4]) = 3
             + Min([3，4，2]) = 2
           + 实际入栈：[0,1,-1]
             + 出栈
               + -1 < 0 => return 2
      + **当出栈差值为正数时**
        +  **原入栈值 = 当前最小值 + 差值**
           +  入栈公式的推导
  + 最小值
    + 当前最小值
      + 每次更新即可
```javascript
/**
 * initialize your data structure here.
 */
var MinStack = function() {
    this.stack = [];
    this.min = '';
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
   if(this.stack.length == 0){
       this.min = x;
       this.stack.push(x - this.min);
   }else{
       this.stack.push(x - this.min);
       if(x < this.min){
           this.min = x;
       }
   }
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    if(this.stack.length == 0){
        return;
    }
    let tmpPop = this.stack.pop();
    if(tmpPop < 0){
        this.min = this.min - tmpPop;
    }
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    let top = this.stack[this.stack.length-1];
    if(top < 0){
        return this.min;
    }else{
        return top + this.min;
    }
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    return this.min;
};

/** 
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```
#### 解法四：绑定最小值(链表实现)
+ 思路
  + 出入栈，正常按照顺序
  + 最小值
    + 将每一步的最小值绑定到栈顶元素上
    + 更新
      + 每次入栈更新当前最小值
    + 取值
      + 每次返回栈顶元素的最小值即可
+ 同理
  + 用数组，对象或其它数据结构实现亦可
  + 关键将最小值绑定当前基栈即可
```javascript
/**
 * initialize your data structure here.
 */
class Node{
    constructor(val,min){
        this.val = val;
        this.min = min;
        this.next = null;
    }
}
var MinStack = function() {
    this.head = null;
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
   if(this.head == null){
       this.head = new Node(x,x); 
   }else{
       let currMinNode = new Node(x,Math.min(x,this.head.min));
       currMinNode.next = this.head;
       this.head = currMinNode;
   }
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    if(this.head != null){
        this.head = this.head.next;
    }
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    if(this.head != null){
        return this.head.val;
    }
    return -1;
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    if(this.head != null){
        return this.head.min;
    }
    return -1;
};

/** 
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```