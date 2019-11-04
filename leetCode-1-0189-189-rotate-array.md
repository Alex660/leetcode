# 旋转数组（简单）
# 题目描述
![截屏2019-10-20上午9.15.47.png](https://pic.leetcode-cn.com/27bd1360eedb6c569423d3d94c24fc41862941aae19e6411f1e581edca30c4ed-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.15.47.png)
# 题目地址
<https://leetcode-cn.com/problems/rotate-array/solution/>
# 解法五：
![截屏2019-10-15下午3.50.57.png](https://pic.leetcode-cn.com/0928fc6a8cc2e42dd96858841dfae5aa00fc7bb5f92de10d244e9fcb524f3639-%E6%88%AA%E5%B1%8F2019-10-15%E4%B8%8B%E5%8D%883.50.57.png)

 >旋转数组  
 输入: [1,2,3,4,5,6,7] 和 k = 3  
 输出: [5,6,7,1,2,3,4]  
 解释:  
 向右旋转 1 步: [7,1,2,3,4,5,6]  
 向右旋转 2 步: [6,7,1,2,3,4,5]  
 向右旋转 3 步: [5,6,7,1,2,3,4]



***-----注意----以下解法部分不合题目条件的解法仅供拓展思维-----感谢您的关注，谢谢～***

+ 分析由第一个示例可知 末尾数组元素被换到了最后，其它元素递归向后退，
+ 之后示例是分别在前一步的基础上即改变后的数组上进行同样的操作，就组成了数组旋转的 k 步
#### 解法一：
+ 暴力枚举
+ 时间复杂度 O(n * k) 如果k == n  && n很大 后果严重
+ 空间复杂度 O(1)
```javascript    
var rotate = function(nums, k) {
    var n = nums.length;
    var tmpEnd = 0;
    var tmpPrev = 0;
    for(var i = 0;i<k;i++){
        tmpEnd = nums[n-1];
        for(var r = 0;r<n;r++){
            tmpPrev = nums[r];
            nums[r] = tmpEnd;
            tmpEnd = tmpPrev;
        }
    }
};
```
#### 解法二：
+ 绝地反转
    + 示例： 
    + [1,2,3,4,5,6,7] k = 4  => [4,5,6,7] + [1,2,3] == [4,5,6,7,1,2,3]
    + [1,2,3,4,5,6,7] k = 11 => [4,5,6,7] + [1,2,3] == [4,5,6,7,1,2,3]  
    + [1,2,3,4,5,6,7] k = 5  => [3,4,5,6,7]  +[1,2] == [3,4,5,6,7,1,2]
    + 由示例可以归纳出 k%n == 后面面 n - k%n 个元素 和 前面 k%n 个元素中间交界处 重新组合成一个新的数组的分界点，此点处整体调换两部分的数组即为所求
    ```javascript
    var rotate = function(nums, k) {
        var n = nums.length;
        var reversePoint = n - k%n;
        reversePoint != 0 &&  (nums = nums.slice(reversePoint).concat(nums.slice(0,reversePoint)))
    }
    ```
#### 解法三：
+ 由解法二可知 将后面 k%n 个元素 从原数组删除出来并拼接到 数组前面 即为所求 可以用 splice
    ```javascript
    var rotate = function(nums,k) {
        var n = nums.length && nums.splice(0,0,nums.splice(n-k%n));
    }
    ```

#### 解法四：
+ 由题中示例可知选择 k 步 可以分解每一步，共 k 步，而每一步都是同样在前一数组基础上将最后一个数组元素换到最前面 可用 unshift pop
+ 共k次 即循环k次 unshift 和 pop 操作
```javascript
var rotate = function(nums,k) {
    for(var i = 0;i<k;i++){
        nums.unshift(nums.pop());
    }
}
```
#### 解法五：
+ 原地反转
+ 时间复杂度 O(n) 反转了3次n个元素
+ 空间复杂度 O(1) 没使用额外的空间
+ 由解法二中示例及分析可知 输入k 就等于将 后面 k%n个元素 放在前面 合并 放在后面的 n - k%n 个元素重新拼接起来
+ 要符合题意则需要原地调换
+ 示例： [1,2,3,4,5,6,7] k = 4  => [4,5,6,7] + [1,2,3] == [4,5,6,7,1,2,3]
+       1 2 3 4 5 6 7  => 反转点为 n-1 && 7 6 5 4 3 2 1 => 4 5 6 7 1 2 3 反转点为 k%n-1 分析：上一步前 k%n-1 个元素反转 后面n-k%n 个元素反转
+       [1,2,3,4,5,6,7] k = 12  => [3,4,5,6,7]  +[1,2] == [3,4,5,6,7,1,2]
+       1 2 3 4 5 6 7  => 反转点为 n-1 && 7 6 5 4 3 2 1 => 3 4 5 6 7 1 2 反转点为 k%n-1 分析：上一步前 k%n-1 个元素反转 后面n-k%n 个元素反转
+  k对n求余是因为 k 有可能大于 n，且当旋转次数大于n的倍数时 <=> 旋转 k%n 次
+ 历经两次反转
    + 第一次整体反转
    + 第二次 反转点处 前后同时反转
+ 反转为前后交换元素 用中心收缩法
```javascript
var rotate = function(nums, k) {
    var n = nums.length;
    k %= n;
    if(n == 1){
        return;
    }
    var tmp = 0;
    myReverse(0,n-1);
    myReverse(0,k-1);
    myReverse(k,n-1);
    function myReverse(start,end) {
        while (start < end) {
            tmp = nums[start];
            nums[start] = nums[end];
            nums[end] = tmp;
            start++;
            end--;
        }
    }
}
``` 
#### 解法六：
+ 借助新数组
+ 时间复杂度O(n)
+ 空间复杂度O(n)
+ 由解法二示例可知 通过 k次旋转后 原数组元素所在位置索引i = (i+k)%n 
+ 因此借助新数组动态更新改变后的元素位置 最后再拷贝回去替换原数组的各个元素
```javascript
var rotate = function(nums, k) {
    var n = nums.length;
    var newArr = new Array(n);
    for(var i = 0;i<n;i++){
        newArr[(i+k)%n] = nums[i];
    }    
    for(var r = 0;r<n;r++){
        nums[r] = newArr[r];
    }    
}
``` 