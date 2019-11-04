# 寻找旋转排序数组中的最小值 II（困难）
# 题目描述
![截屏2019-11-04上午9.51.46.png](https://pic.leetcode-cn.com/4c8ad961c4aa6bc3b164eac010365380038163761893c7c504385f44dcb2c305-%E6%88%AA%E5%B1%8F2019-11-04%E4%B8%8A%E5%8D%889.51.46.png)
# 题目地址
<https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/solution/>


#### 解法：二分查找法
+ 相关题型
  + [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/153-xun-zhao-xuan-zhuan-pai-xu-shu-zu-zhong-de-z-4/) 
+ 分析
  + 跟153题解题思路基本一模一样
    + 设置low,high左右边界，算出中间数nums[mid]
    + 当nums[mid] > nums[high]时，说明出现了无序的地方在右边
        + low = mid+1 
    + 当nums[mid] == nums[high]时，说明遇到了重复元素，无法判断mid在哪一边的排序数组中
      + 区别是上一题数组没有重复元素
        + 此时我们只有有两种选择，不然还遍不遍历了
          + 理论证明
            + 当最小值只有一个时
              + 因为nums[mid] = nums[high]，所以nums[high]一定不是数组的最小值
                + 最小值出现在[mid,high-1]
                  + high--，一定不会越过最小值
                  + low++,可能会越过最小值
                + 最小值出现在[0,mid]
                  + high--，一定不会越过最小值
                  + low++,可能会越过最小值
            + 当最小值不止一个时
              + 因为nums[mid] = nums[high]，所以nums[high]可能是数组的最小值
                + 最小值出现在[mid,high]
                  + high--，一定不会越过最小值，因为不止一个，在[mid,high-1]中还有重复的最小值
                  + low++,可能会越过最小值
                + 最小值出现在[0,mid]
                  + high--，一定不会越过最小值
                  + low++,可能会越过最小值 
          + 示例证明
            + 当最小值在左侧时
              + low++会刚好被越过，最后夹逼剩一个元素一定是不是这个被越过当最小值
                + 例如[6,1,6,6,6] 
              + high--则不会
                + 例如 [6,1,6,6,6]、[6,6,6,0,6]，均成立
            + 当最小值在右侧时
              + low++不会越过
                + 例如[6,6,6,1,6]
              + high--  
                + 例如 [6,1,6,6,6]、[6,6,6,0,6]，均成立
        + 因此综上所述：high-- 
    + 否则无序点在左侧
        + high = mid  
    + 两边夹逼直到low == high ，剩下的一个元素即为无序点  
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
        }else if(nums[mid] == nums[high]){
            high--;
        }else{
            high = mid
        }
    }
    return nums[low];
};
```