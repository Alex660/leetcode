# 搜索旋转排序数组（中等）
# 题目描述
![截屏2019-11-03上午9.31.26.png](https://pic.leetcode-cn.com/4623b2238fd545c70b7c07c4e75a2ab1920ca0c9120af280727db209814c3885-%E6%88%AA%E5%B1%8F2019-11-03%E4%B8%8A%E5%8D%889.31.26.png)
# 题目地址
<https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/>
#### 解法：二分查找法
+ 题目要求时间复杂度为O(logn),故想到二分查找法
  + 首先看下二分法的模版
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
  + 思路分析
    + 此题要求求出所给值在数组中的索引位置，如不存在，则返回-1
    + 此题数组有些特别，如[4,5,6,7,0,1,2]，为旋转数组
      + 众所周知，二分法是在排序好的数组上进行二分查找的：或单调递增、或单调递减
      + 但此题当出现旋转数组时
        1. 找到分界下标，分成两个有序数组
        2. 判断目标值在哪个有序数据范围内，做二分查找
      + 代码中，我们仍然以mid作为动态分解下标
      + 但与传统单调(递增/递减)数组不同的是，不能再用target直接与mid比较了
        + 因为在旋转数组中，target 小于mid，但不一定大于nums[0]，因为它不一定在左边了，正如target大于mid时，也不一定大于nums[0]，因为有可能nums[mid]正好位于旋转位置，即nums[mid] < nums[0],整体趋势成为了开口向下的抛物线。
        + 所以，旋转数组中，解题关键在于确定target值在哪个有序数组的一边内
          + 在mid 右边，low = mid+1
          + 在mid 左边,high = mid 
        + 遍历完成时，当low和high相遇时，数组只剩一个元素，如果nums[low]/nums[high]  == target时，low/high即为所求索引值
    + **进一步分析**
      + 由上述可知
        + nums : 0~mid~high
        + low:0
        + high = nums.lenght-1、mid = (low+high)/2 
        + 当target在[mid+1,high]中时，low = mid +1
        + 当target在[0,mid]中时，high = low
          + 当[0,mid] 单调即升序时，nums[0] <= nums[mid] 
            + 当target > nums[mid] || target < nums[0]时
              + low = mid+1
            + 否则
              + high = mid
          + 当[0,mid] 不单调时（如[4,5,6,0,1,2,3]中等[4~0]），nums[0] > nums[mid]
            + 当target < nums[0] && target > nums[mid]（为什么且，因为上面说了大于mid的元素不一定就在mid右边，如target == 5时，这是旋转数组的特性，所以取交集）,
              + low = mid+1
            + 否则
              + high = mid  
        + 直到low==high时间，即**两边夹逼**只剩一个数组元素没有比较时，判断是否等于target，是返回low/high，否返回-1
  + 类似题型
    [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/153-xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-de-z-4/)   
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    var low = 0;
    var high = nums.length - 1;
    while(low < high){
        var mid = (low+high)>>1;
        if( target < nums[0] && target > nums[mid]){
            low = mid+1;
        }
        else if(nums[0] <= nums[mid] && (target > nums[mid] || target < nums[0])) {
            low = mid+1;
        }
        else{
            high = mid;
        }
    }
    return low == high && nums[low] == target ? low : -1;
};
```
+ 优化+1
  + 如上述分析和代码，整体判断条件三个，且
    + nums[0] > nums[mid] && target < nums[0] && target > nums[mid]
      + 为数组旋转区域时，代码里没写，是因为target大于nums[mid] 又小于第一个元素，则此夹逼的范围数组内肯定不是单调(递增/递减)的，只有nums[0] > nums[mid] 时会出现，也就是旋转数组的旋转节点存在。
      + 所以代码里第一步判断省略了一步，但是不能只写nums[0] > nums[mid]，因为只写这个，后面两个不一定能成立，因为不能确定target的值，值才能确定范围，范围不能确定值。
      + low = mid + 1 
    + nums[0] <= nums[mid] && (target < nums[0] || target > nums[mid])
      + low = mid + 1 
    + else 其它所有情况 
      + high = mid 
    + 设
      + a = nums[0] > nums[mid]
      + b = target < nums[0]
      + c = target > nums[mid]
    + 由代码可知
      +  a为真、b为真、c为真时
        + low = mid + 1  
      +  a为假、b为真**或**c为真时
        + low = mid + 1  
    + 当a为真时，有没有可能b、c一个为真一个为假呢？
      + 不可能的，只要a为真，b和c有一个为真，则剩下的一个一定为真 
    + 但当a为假时，
      + b和c有可能都为假
      + 但我们只需要其中一种为真的情况
  + 结合位运算：异或运算符(^)的性质可知
    + 三个为真或一个为真时，整体为真
    + 其余情况为假
    + 所以代码可以改写为 
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    var low = 0;
    var high = nums.length - 1;
    while(low < high){
        var mid = (low+high)>>1;
        if( (nums[0] > nums[mid]) ^ (target > nums[mid]) ^ (target < nums[0]) ) {
            low = mid+1;
        }
        else{
            high = mid;
        }
    }
    return low == high && nums[low] == target ? low : -1;
};
``` 
+ 优化+2【参考大佬的】 
  ```java
    int search(vector<int>& nums, int target) {
        int lo = 0, hi = nums.size();
        while (lo < hi) {
            int mid = (lo + hi) / 2;
            
            double num = (nums[mid] < nums[0]) == (target < nums[0])
                    ? nums[mid]
                    : target < nums[0] ? -INFINITY : INFINITY;
                    
            if (num < target)
                lo = mid + 1;
            else if (num > target)
                hi = mid;
            else
                return mid;
        }
        return -1;
    }
  ```