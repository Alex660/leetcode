# 设计循环双端队列（中等）
# 题目描述
![截屏2019-10-20上午9.07.59.png](https://pic.leetcode-cn.com/683e537851fe036e2e283738c79622b7b21e92b45808c90f619194595571b202-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.07.59.png)
# 题目地址
<https://leetcode-cn.com/problems/design-circular-deque/solution/>
# 循环队列图示
![20181002152641772.png](https://pic.leetcode-cn.com/c83c3ca2e95b28e1f8e131c22b593e71b7fa48ccd9295310c32c89ea52ad05fd-20181002152641772.png)
![20181002152709224.png](https://pic.leetcode-cn.com/90bbaf6e98a9e3545483aa283080063601c0be33a07f07adb85c0b44ac8a59b0-20181002152709224.png)
![20181003083027896.png](https://pic.leetcode-cn.com/467656eda43b98a4cfed097a4be438c29e388949062085c438a59573fd253af2-20181003083027896.png)
![20190724210540499.png](https://pic.leetcode-cn.com/81778336b73b6579f891f4e777aa5d1603b95f96dce52c4d34a8cdb788d556a1-20190724210540499.png)

# 解法一
![截屏2019-10-19下午12.13.41.png](https://pic.leetcode-cn.com/5f563c63cb88f258e373c0d744ffb6cd69decf79e39fa638a6ae957307f25a20-%E6%88%AA%E5%B1%8F2019-10-19%E4%B8%8B%E5%8D%8812.13.41.png)

____
## 设计循环双端队列
## 解题知识点准备
#### 队列
+ 只允许一端进行插入即入队，在另一端进行删除的受限的线性表
+ 特点：先进先出
+ 队头：Front  队首
+ 队尾：Rear   队尾
+ 缺点：删除或出队一个元素时间剩余元素要向前移动，例如排队打饭

#### 顺序队列
+ 队头：Front = 0 永远指向队头的元素
+ 队尾：Rear = 0  永远指向队尾元素的下一个元素位置(如队尾元素为队列的size时,Rear == size)
+ 问题：假溢出
```javascript
 <-------------     size == 5       <---------------
      0       1       2       3      4      5   undefined
      ||                                            ||
      \/                                            \/
    front                                          rear
```
+ rear++  不断入队
+ front++ 不断出队

#### 顺序循环队列 Queue
+ 有效解决顺序队列假溢出问题【越界时可以放到空闲存储单元中】
    + 存储空间最大值设置为 QueueSize
    + 以下 QueueSzie 设为 n   队列最大空间长度
    + Queue.front 设为 front 对首指针位置
    + Queue.rear 设为 rear   队尾指针位置
    + 当指针rear或front越界时，转化为对应当头尾指针位置，就可以首尾相连，消除假溢出
        + rear 越界时，利用除法求余运算 % 来实现 
                // 当 rear  == n 时， rear %= n ==> rear == 0
        + front 越界时，同上
                // 当 front == n - 1 时，front = (front+1)%n ==> front == 0 
                // 【用不同数据结构实现时条件不同，如数组出队：front--】
    + 队空
        + front == rear(例如初始化时 0 == 0)
    + 队满
        + rear %= n == front <=> rear == front
        + 由对空可知 两者判断条件重合
    + 区分队空和队满的解决方案
        + <1>：增加标志位
                // 设标识位 flag = 0;
                    // 入队成功 => flag = 1; =>当 front == rear && flag == 1 队满
                    // 出队成功 => flag = 0; =>当 front == rear && flag == 0 队空
        + <2>：少用或增加一个存储单元
                // 队满 (rear = (rear+1)%n) == front
                // 队空 front == rear
    + 入队出队
        + 入队：rear++
        + 出队：front++
        + 入队时判断是否队满、出队时判断是否队空 且操作后均都更新指针位置

#### 循环双端队列
+ 队首可以出入队、队尾亦可以出入队
+ 数组原生实现
    + 入队出队
    + 右边入队：rear++
    + 左边入队：front--
    + 入队时判断是否队满、出队时判断是否队空 且操作后均都更新指针位置
+ 由顺序循环队列分析可知
+ 队满 (rear = (rear+1)%n) == front
+ 队空 front越界时即 front < 0 时，换到队尾去 => (front+n)%n
+ 区分队空和队尾 => 初始化数组时 增加一个单元 比如规定最大存储空间为n 则初始化存储数组 new Array(n+1)
    + 如front == -1时 => front = n-1
+ 文献 java jdk 的实现
    + 首先 n 必须初始化为 n的2次方
        + 2的n次方-1 化为二进制均为 111...1
        + 按位与 上面，则 小于等于 2的n次方-1的 结果还是自己
        + 大于 的则为 0
        + 数组长度为2的n次方，则数组的最大下标为 2的n次方-1
        + => rear == n-1时 rear+1 & n-1 一定 == 1 正好越过一个空位置

    + 右端 入队 如下源码
        ```java
         public void addLast(E e) {
             if (e == null)
                 throw new NullPointerException();
             elements[tail] = e;
             if ( (tail = (tail + 1) & (elements.length - 1)) == head)
                doubleCapacity();
          }
        ```
        + 入队后判断队满
            + 当 n == 8 时，
            + 如果 rear 从 0 开始入队， 连续入队 8 即 n  个元素，入队之前 先更新尾元素的值，再 rear++ 最终 rear == n == 8
            + 由公式 (tail = (tail+1) & (elements.lenght-1) == head)
            + <=> (rear = (rear+1) & (n-1) == front)
            + <=> (rear = (rear+1)%n )
            + rear <= n-1  => (7 & 7) == 7 rear = 7
            + rear > n-1   => ((8+1) & 7) == 1 <=> ((8+1)%8 == 1) rear 跳过尾部索引n-1位置 换到头部开始第二个位置 空出一个位置防止 rear == front 二义性(队空和队满均是此条件) 
            + 即此时 rear == front 为队满 则需要扩容doubleCapacity()
            + 只有 rear+1 后的 rear 与 n-1 按位与时 => 没有越界转换后rear位置为rear+1 越界转换后rear位置为1 空出索引0的位置用来判断是否队满以区别队空
    + 右端 出队 如下源码
        ```java    
        public E pollLast() {
            int t = (tail - 1) & (elements.length - 1);
                E result = elements[t];
                if (result == null)
                    return null;
                elements[t] = null;
                tail = t;
                return result;
        }
        ```
        + 判断队空并更新指针位置
        + 由公式 tail = (tail -1) & (elements.length - 1)
        + <=> (rear = (rear-1) & (n-1) )
        + <=> (当rear-1>0时 rear = (rear-1)%n)
        + <=> (当rear-1<0时 rear = (rear-1+n)%n)
               
            
    + 左端 入队 如下源码
        ```java
         public void addFirst(E e) {
             if (e == null)
                 throw new NullPointerException();
             elements[head = (head - 1) & (elements.length - 1)] = e;
             if (head == tail)
                 doubleCapacity();
         }
        ``` 
        + 入队后判断队满
            + 当 n == 8 时,
            + 如果 front == 0 ，并开始入队一个元素
            + 由公式 (head = (head - 1) & (elements.lenght - 1))
            + <=> (front = (front - 1) & (n-1) == tail)
            + <=> (front = (front - 1+n)%n )
            + front < 0 => (0-1) & 7 == 7 <=> (-1+8)%8 == 7 front越界时 头部跳过索引0的位置 换到尾部数组最大下标位置 
            +当此时 tail即rear == 7 即 front == rear时，队满 则需要扩容doubleCapacity()
            + front >= 0   (1-1) & 7 == 0 <=> (0+8)%8 == 0
    + 左端 出队 如下源码
        ```java
         public E pollFirst() {
             int h = head;
             E result = elements[h]; // Element is null if deque empty
             if (result == null)
                return null;
             elements[h] = null;     // Must null out slot
             head = (h + 1) & (elements.length - 1);
             return result;
         }
        ```
        + 判断队空并更新指针位置
        + 由公式(head = (head + 1) & (elements.length - 1))
        + <=> (front = (front + 1) & (n-1))
        + <=> (front = (front + 1)%n )

    + 左右均不出入队去判断队满
        + 因为不操作队列判断队满之前，出入队操作都更新了指针位置 所以只需判断一个右端或左端队满情况
        + 当且仅当实际开辟容量-1 == 要求存储空间时 队满公式和相应出入队判断队满操作一样 是首尾相连仅差一个空间位置的
        + 因为如果像java里一样做位运算优化操作，则实际开辟容量可能会远远大于要求容量
        + 此时，原队满队空判断公式只能用来更新首尾指针位置索引来用，因为可能开辟空间容量来填充的元素个数已经超过了要求存储空间，还是被判断为没有满

    + 获取队列队首元素 如下源码
        ```java
         public E getFirst() {
             E x = elements[head];
             if (x == null)
                 throw new NoSuchElementException();
             return x;
         }
        ```   
        + 因为队首指针永远指向队列头部元素的位置索引 所以如值有效直接返回即可


    + 获取队列尾元素 如下源码
        ```java
         public E getLast() {
              E x = elements[(tail - 1) & (elements.length - 1)];
              if (x == null)
                   throw new NoSuchElementException();
              return x;
         }
        ```    
        + 因为队尾指针永远指向队列尾元素位置索引的下一个位置索引 
        + 且因之前rear可能越界被衔接到了头部 所以要重新计算它在数组中的位置
        + 如果有效
            +  <=> return ArrDeque[(rear - 1) & n-1]
            +  <=> return ArrDeque[(rear-1+n)%n]
    + 全局双端出入队前判断是否队满
        + 直接判断队满要计算开辟而来的容量空间填充的元素个数是否已经超过要求容量 如下源码
        ```java
        public int size() {
            return (tail - head) & (elements.length - 1);
        }
        ```
        + 即队列空间已有元素个数
        + <=> ( rear - front ) & (n - 1)
        + <=> ( rear - front + n)%n
#### 题目
```javascript
/**
 * Initialize your data structure here. Set the size of the deque to be k.
 * @param {number} k
 */
var MyCircularDeque = function(k) {
    this.ArrDeque = new Array(k+1);
    this.front = 0;
    this.rear = 0;
    this.n = k+1;
    this.k = k;
};

/**
 * Adds an item at the front of Deque. Return true if the operation is successful. 
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertFront = function(value) {
    if(this.isFull('front')){
        return false;
    }else{
        this.front = (--this.front+this.n)%(this.n);
        this.ArrDeque[this.front] = value;
        return true;
    }
};

/**
 * Adds an item at the rear of Deque. Return true if the operation is successful. 
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertLast = function(value) {
    if(this.isFull('last')){
        return false;
    }else{
        this.ArrDeque[this.rear] = value; 
        this.rear = (this.rear+1)%(this.n);
        return true;
    }
};

/**
 * Deletes an item from the front of Deque. Return true if the operation is successful.
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteFront = function() {
    if(this.isEmpty()){
        return false;
    }else{
        this.front = (++this.front)%(this.n);
        return true;
    }
};

/**
 * Deletes an item from the rear of Deque. Return true if the operation is successful.
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteLast = function() {
    if(this.isEmpty()){
        return false
    }else{
        this.rear = (--this.rear+this.n)%(this.n);
        return true;
    }
};

/**
 * Get the front item from the deque.
 * @return {number}
 */
MyCircularDeque.prototype.getFront = function() {
    return this.isEmpty() ? -1 : this.ArrDeque[this.front]
};

/**
 * Get the last item from the deque.
 * @return {number}
 */
MyCircularDeque.prototype.getRear = function() {
    return this.isEmpty() ? -1 : this.ArrDeque[(this.rear-1+this.n)%(this.n)]
};

/**
 * Checks whether the circular deque is empty or not.
 * @return {boolean}
 */
MyCircularDeque.prototype.isEmpty = function() {
    if(this.front == this.rear){
        return true;
    }else{
        return false;
    }
};

/**
 * Checks whether the circular deque is full or not.
 * @return {boolean}
 */
MyCircularDeque.prototype.isFull = function(x) {
    return ( this.rear - this.front + this.n)%(this.n) == this.k;
};

/** 
 * Your MyCircularDeque object will be instantiated and called as such:
 * var obj = new MyCircularDeque(k)
 * var param_1 = obj.insertFront(value)
 * var param_2 = obj.insertLast(value)
 * var param_3 = obj.deleteFront()
 * var param_4 = obj.deleteLast()
 * var param_5 = obj.getFront()
 * var param_6 = obj.getRear()
 * var param_7 = obj.isEmpty()
 * var param_8 = obj.isFull()
 */

//  检测示例
["MyCircularDeque","insertLast","insertLast","insertFront","insertFront","getRear","isFull","deleteLast","insertFront","getFront"]
[[3],[1],[2],[3],[4],[],[],[],[4],[]]
["MyCircularDeque","insertFront","getRear","insertFront","getRear","insertLast","getFront","getRear","getFront","insertLast","deleteLast","getFront"]
[[3],[9],[],[9],[],[5],[],[],[],[8],[],[]]
```

#### 依据java源码优化版
+ 懂你知识点准备
    + (>>>) 运算符  意为 无符号右移
      ![](https://segmentfault.com/img/remote/1460000015213259) 
    + 右移n位 => 抛弃后面n位向右移动n位，其余位补0
    + 变量 temp 的值为 -14 （即二进制的 11111111 11111111 11111111 11110010），
    + 向右移两位后等于 1073741820 （即二进制的 00111111 11111111 11111111 11111100）。
    + （&）按位与运算符
    + 只有两个数的二进制同时为1时，结果才为1，否则为0
    + 例：3 &5  即 00000011 & 00000101 = 00000001 ，所以 3 & 5的值为1。
    + (|) 按位或运算符
    + 只要两个数的二进制一位为1，结果就为1，否则为0
    + 2 | 4 即 00000010 | 00000100 = 00000110 ，所以2 | 4的值为 6 
    + (^) 异或运算符
    + 只有两个数的相应二进制位为"异"时，结果为1，否则为0
    + 2 ^ 4 即 00000010 ^ 00000100 =00000110 ，所以 2 ^ 4 的值为6 
+ 依据上述分析可知  队列空间设置为2的n次方的意义所在，那么如果确保队列容量始终保持为2的n次方呢？如下源码
    ```java
    private void allocateElements(int numElements) {
            int initialCapacity = MIN_INITIAL_CAPACITY;
        // Find the best power of two to hold elements.
            // Tests "<=" because arrays aren't kept full.
            if (numElements >= initialCapacity) {
            initialCapacity = numElements;
            initialCapacity |= (initialCapacity >>>  1);
            initialCapacity |= (initialCapacity >>>  2);
            initialCapacity |= (initialCapacity >>>  4);
            initialCapacity |= (initialCapacity >>>  8);
            initialCapacity |= (initialCapacity >>> 16);
            initialCapacity++;

            if (initialCapacity < 0)   // Too many elements, must back off
                initialCapacity >>>= 1;// Good luck allocating 2 ^ 30 elements
        }
        elements = (E[]) new Object[initialCapacity];
    }
    ```
    + 首先判断指定大小numElements与MIN_INITIAL_CAPACITY的大小关系。
    + 如果小于MIN_INITIAL_CAPACITY，则直接分配大小为MIN_INITIAL_CAPACITY的数组；
    + 如果大于MIN_INITIAL_CAPACITY，则进行无符号右移操作，然后在加1，这样就可以寻找到大于numElements的最小2的幂次方。
    + 原理：无符号右移再进行按位或操作，就是将其低位全部补成1，
    + 然后再自加加一次，就是再向前进一位。这样就能得到其最小的2次幂。之所以需要最多移16位，是为了能够处理大于2^16次方数。
    + 最后再判断值是否小于0，因为如果初始值在int最大值2^31-1和2^30之间，
    + 进行一系列移位操作后将得到int最大值，再加1，则溢出变成负数，所以需要检测临界值，然后再右移1位

    + js中最大整数为Number.MAX_SAFE_INTEGER == 9007199254740991 即2的53次方-1
    + javascript引进
    + BigInt数据类型的目的是比Number数据类型支持的范围更大的整数值。
    + 在对大整数执行数学运算时，以任意精度表示整数的能力尤为重要。使用BigInt，整数溢出将不再是问题。

    + 因为有题意 
    + 所以优化版代码如下

#### 题目优化版
```javascript
/**
 * Initialize your data structure here. Set the size of the deque to be k.
 * @param {number} k
 */
var MyCircularDeque = function(k) {
    var initialCapacity = 8;
    if (k >= initialCapacity) {
        initialCapacity = k;
        initialCapacity |= (initialCapacity >>>  1);
        initialCapacity |= (initialCapacity >>>  2);
        initialCapacity |= (initialCapacity >>>  4);
        initialCapacity |= (initialCapacity >>>  8);
        initialCapacity |= (initialCapacity >>> 16);
        initialCapacity |= (initialCapacity >>> 32);
        initialCapacity++;
 
        if (initialCapacity < 0)   // Too many elements, must back off
            initialCapacity >>>= 1;// Good luck allocating 2 ^ 52 elements
    }
    this.ArrDeque = new Array(initialCapacity);
    this.front = 0;
    this.rear = 0;
    this.n = initialCapacity;
    this.k = k;
};

/**
 * Adds an item at the front of Deque. Return true if the operation is successful. 
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertFront = function(value) {
    if(this.isFull('front')){
        return false;
    }else{
        this.front = (this.front - 1) & (this.n-1);
        this.ArrDeque[this.front] = value;
        return true;
    }
};

/**
 * Adds an item at the rear of Deque. Return true if the operation is successful. 
 * @param {number} value
 * @return {boolean}
 */
MyCircularDeque.prototype.insertLast = function(value) {
    if(this.isFull('last')){
        return false;
    }else{
        this.ArrDeque[this.rear] = value; 
        this.rear = (this.rear+1) & (this.n-1);
        return true;
    }
};

/**
 * Deletes an item from the front of Deque. Return true if the operation is successful.
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteFront = function() {
    if(this.isEmpty()){
        return false;
    }else{
        this.front = (this.front + 1) & (this.n-1);
        return true;
    }
};

/**
 * Deletes an item from the rear of Deque. Return true if the operation is successful.
 * @return {boolean}
 */
MyCircularDeque.prototype.deleteLast = function() {
    if(this.isEmpty()){
        return false
    }else{
        this.rear = (this.rear-1) & (this.n-1);
        return true;
    }
};

/**
 * Get the front item from the deque.
 * @return {number}
 */
MyCircularDeque.prototype.getFront = function() {
    return this.isEmpty() ? -1 : this.ArrDeque[this.front]
};

/**
 * Get the last item from the deque.
 * @return {number}
 */
MyCircularDeque.prototype.getRear = function() {
    return this.isEmpty() ? -1 : this.ArrDeque[(this.rear-1) & (this.n-1)]
};

/**
 * Checks whether the circular deque is empty or not.
 * @return {boolean}
 */
MyCircularDeque.prototype.isEmpty = function() {
    if(this.front == this.rear){
        return true;
    }else{
        return false;
    }
};

/**
 * Checks whether the circular deque is full or not.
 * @return {boolean}
 */
MyCircularDeque.prototype.isFull = function(x) {
    return ((this.rear - this.front) & (this.n-1)) == this.k;
};

/** 
 * Your MyCircularDeque object will be instantiated and called as such:
 * var obj = new MyCircularDeque(k)
 * var param_1 = obj.insertFront(value)
 * var param_2 = obj.insertLast(value)
 * var param_3 = obj.deleteFront()
 * var param_4 = obj.deleteLast()
 * var param_5 = obj.getFront()
 * var param_6 = obj.getRear()
 * var param_7 = obj.isEmpty()
 * var param_8 = obj.isFull()
 */
 ```