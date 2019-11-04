# 寻找旋转排序数组中的最小值（中等）
# 题目描述
![截屏2019-11-04上午6.09.42.png](https://pic.leetcode-cn.com/ba515330b1d52da824553cc8417e68f18fbb2696e307ff40e504cf84e6bcd82b-%E6%88%AA%E5%B1%8F2019-11-04%E4%B8%8A%E5%8D%886.09.42.png)
# 题目地址
<https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/submissions/>
# 解法：二分查找法
+ 分析
  + 设置low,high左右边界，算出中间数nums[mid]
  + 当nums[mid] > nums[high]时，说明出现了无序的地方在右边
      + low = mid+1 
  + 否则无序点在左侧
      + high = mid  
  + 两边夹逼直到low == high ，剩下的一个元素即为无序点 
+ 类似题型
  1. [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/33-sou-suo-xuan-zhuan-pai-xu-shu-zu-by-alexer-660/)  
  2. [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/solution/69-x-de-ping-fang-gen-by-alexer-660/)
  3. [154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/solution/154-xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-de-z-3/)
+ 归纳解题技巧
  + while(left <  right) 在**循环体外**输出
  + while(left <= right) 在**循环体内**输出
  + n除以2^k可以换成位运算，提升代码性能
    + n>>k
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMin = function(nums) {
    var low = 0;
    var high = nums.length-1;
    while(low < high){
        var mid = (low+high)>>1;
        if(nums[mid] > nums[high]){
            low = mid+1;
        }else{
            high = mid
        }
    }
    return nums[low];
};
```