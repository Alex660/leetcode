# x的平方根（简单）
# 题目描述
# 题目地址
<https://leetcode-cn.com/problems/sqrtx/submissions/>
#### 解法一：二分查找
+ 话不多说直接上简便二分法模板，公式不多说
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
+ 注意
  + 此处代码没有用“/2”而是“>>1”
    + 属于位运算，且更加精确
    + [可参考我的总结](https://github.com/Alex660/Algorithms-and-data-structures/tree/master/theoreticalKnowledge)  
```javascript
/**
 * @param {number} x
 * @return {number}
 */
var mySqrt = function(x) {
    if(x == 0 || x ==1){
        return x;
    }
    var left = 1;
    var right = x;
    while(left <= right){
       var middle = left + ((right-left)>>1);
       if(middle*middle == x){
           return middle;
       }else if(middle*middle > x){
           right = middle-1;
       }else{
           left = middle+1;
       }
    }
    return right;
};
```
#### 解法二：牛顿迭代法
+ 公式：x= (x+tmp_x/x)/2
+ [戳看公式参考文献](https://www.zhihu.com/question/20690553)
+ [英文文献](https://www.beyond3d.com/content/articles/8/)
```javascript
/**
 * @param {number} x
 * @return {number}
 */
var mySqrt = function(x) {
    if(x == 0 || x ==1){
        return x;
    }
    var tmp = x;
    function sqrt(x){
        var sqrtx = (x+tmp/x)/2;
        if(sqrtx == x){
            return parseInt(x);
        }else{
            return sqrt(sqrtx);
        }
    }
    return sqrt(x);
};
```
+ 还有这样写
    ```javascript
    /**
     * @param {number} x
     * @return {number}
     */
    var mySqrt = function(x) {
        var tmp = x;
        while (x*x > tmp)
            x = ((x + tmp/x) / 2) | 0;
        return x;  
    };
    ```
+ 当然也可以这样写
  + 不过这个会超时！ 
  ```javascript
    /**
     * @param {number} x
    * @return {number}
    */
    var mySqrt = function(x) {
        if(x == 0 || x ==1){
            return x;
        }
        var tmp = x;
        while(x*x > tmp){
            x = (x+tmp/x)>>1;
        }
        return x;
    };
  ```