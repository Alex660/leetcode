# 最接近的三数之和（中等）
# 题目描述
![截屏2020-02-10上午10.36.14.png](https://pic.leetcode-cn.com/ffc5b100d9c7c1fb5ba3b9f77bd5b9d09d72d97fa359efcc3f077e88f38c50cf-%E6%88%AA%E5%B1%8F2020-02-10%E4%B8%8A%E5%8D%8810.36.14.png)
# 题目地址
<https://leetcode-cn.com/problems/3sum-closest/>
#### 解法：双指针
+ 思路
  + 确定第一个数，在左右指针移动过程中，更新与target差值最小的结果
+ 技巧
  + 排序原数组
    + nums[right] >= nums[left]
    + 确定一个数 x
      + res =  x + nums[left] + nums[right]
    + 当 sum - target < res - target 时
      + res = sum
    + 当 sum == target 时
      + 返回 sum 即为所求
    + 当 sum > target
      + 根据从小到大的排序方式，左右指针不能再增大，只有右指针能够缩小，进而缩小 sum 值
      + right--
    + 当 sum < target
      + 原理同上，只不过先从小的元素累加起
      + left++
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var threeSumClosest = function(nums, target) {
    nums.sort((a,b) => a - b);
    let res = nums[0] + nums[1] + nums[2];
    let n = nums.length;
    for(let i = 0;i < n;i++){
        let left = i + 1;
        let right = n - 1;
        while(left < right){
            let sum = nums[i] + nums[left] + nums[right];
            if(Math.abs(res - target) > Math.abs(sum - target)){
                res = sum;
            }else if(sum > target){
                right--;
            }else if(sum < target){
                left++;
            }else if(sum === target){
                return res;
            }
        }
    }
    return res;
};
```