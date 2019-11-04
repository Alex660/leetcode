# 有效的完全平方数（简单）
# 题目描述
![截屏2019-11-04上午11.14.10.png](https://pic.leetcode-cn.com/b2ba6e5956283bb3aef52d3d13f04f5b70b841363c72286286b8f2cb27f68f2d-%E6%88%AA%E5%B1%8F2019-11-04%E4%B8%8A%E5%8D%8811.14.10.png)
# 题目地址
<https://leetcode-cn.com/problems/valid-perfect-square/solution/>
#### 解法一：二分查找法
+ 时间复杂度：
  + $$O\log(n)$$
+ 类似题型
  1. [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/33-sou-suo-xuan-zhuan-pai-xu-shu-zu-by-alexer-660/)  
  2. [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/solution/69-x-de-ping-fang-gen-by-alexer-660/)
  3. [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/153-xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-de-z-4/) 
  4. [154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/solution/154-xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-de-z-3/)
+ 简便二分法模板，公式不多说
    + java
      ```java
      public int bsearch(int[] a, int n, int value) {
        int low = 0;
        int high = n - 1;

        while (low <= high) {
          int mid = (low + high) / 2;
          if (a[mid] == value) {
            return mid;
          } else if (a[mid] < value) {
            low = mid + 1;
          } else {
            high = mid - 1;
          }
        }

        return -1;
      }
      ```  
    + python
      ```python
      left, right = 0, len(array) - 1 
      while left <= right: 
          mid = (left + right) / 2 
          if array[mid] == target: 
              # find the target!! 
              break or return result 
          elif array[mid] < target: 
              left = mid + 1 
          else: 
              right = mid - 1
      ```   
+ 代码如下         
    ```javascript
    /**
     * @param {number} num
     * @return {boolean}
     */
    var isPerfectSquare = function(num) {
        var low = 0;
        var high = num;
        var tmpSquare = 0;
        while(low <= high){
            var mid = (low+high)>>1;
            tmpSquare = mid*mid;
            if(tmpSquare > num){
                high = mid -1;
            }else if(tmpSquare < num){
                low = mid + 1;
            }else{
                return true;
            }
        }
        return false;
    };
    ```
#### 解法二：牛顿迭代法
+ 参考[69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/solution/69-x-de-ping-fang-gen-by-alexer-660/)
```javascript
/**
 * @param {number} num
 * @return {boolean}
 */
var isPerfectSquare = function(num) {
    if(num == 1){
        return 1;
    }
    var tmp = num;
    while(num*num > tmp){
        num = (num+tmp/num)>>1;
    }
    return num*num == tmp;
};
```
+ 优化+1
  + 上述代码会超时
  + 使用位运算如下 
```javascript
/**
 * @param {number} num
 * @return {boolean}
 */
var isPerfectSquare = function(num) {
    if(num == 1){
        return 1;
    }
    var tmp = num;
    while(num*num > tmp){
        num = (num+tmp/num)/2 | 0;
    }
    return num*num == tmp;
};
```
#### 解法三：数学定理
+ 时间复杂度： 
  + $$O(\sqrt{n})$$
+ 等差数列
  + 任意一个平方数都可以表示出下面的奇数序列和
    + $$1+3+5+7+...+(2N-1) = N^2$$  
```javascript
/**
 * @param {number} num
 * @return {boolean}
 */
var isPerfectSquare = function(num) {
    if(num == 1){
        return 1;
    }
    var tmp = 1;
    while(num > 0){
        num -=tmp;
        tmp +=2;
    }
    return num == 0;
};
```
#### 解法四：暴力法
+ 时间复杂度：
  + $$O(\sqrt{n})$$
+ 迭代判断一个累加的数的平方是否到了num的边界
  + 直到大于等于原值
    + 当它的平方等于num时，即为所求
    + 否则，不是
```javascript
/**
 * @param {number} num
 * @return {boolean}
 */
var isPerfectSquare = function(num) {
    if(num == 1){
        return 1;
    }
    var tmp = 1;
    while(tmp*tmp < num){
        tmp+=1;
    }
    return tmp*tmp == num;
};
```
#### 解法五：位运算
```javascript
/**
 * @param {number} num
 * @return {boolean}
 */
var isPerfectSquare = function(num) {
    var root = 0;
    var bit = 1 << 15;
    while(bit > 0){
        root |= bit;
        if(root > num/root){
            root ^= bit;
        }
        bit >>= 1;
    }
    return root*root == num;
};
```