# 四数之和（中等）
# 题目描述
![截屏2020-02-10下午3.59.32.png](https://pic.leetcode-cn.com/7d8885fcffae9df0c0371131e112a60d3ee44758f1ad059031ec7463945eb84b-%E6%88%AA%E5%B1%8F2020-02-10%E4%B8%8B%E5%8D%883.59.32.png)
# 题目地址
<https://leetcode-cn.com/problems/4sum/>
#### 解法：双指针
+ 类似题型解法
  + [16. 最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/solution/16-zui-jie-jin-de-san-shu-zhi-he-by-alexer-660/)
+ 思路
  + 与16题类似，16是固定第一个数，另外两个数用左右指针
  + 此题固定两个数，另外两个数同样用移动的双指针寻找，更新比较与 target 的大小
    + 固定的两个数用双循环
  + 注意剪枝和去重
```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[][]}
 */
var fourSum = function(nums, target) {
    let res = [];
    nums.sort((a,b) => a - b);
    let n = nums.length;
    for(let i = 0;i < n - 3;i++){
        if(i > 0 && nums[i] === nums[i-1]) continue;
        if(nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target) break;
        if(nums[i] + nums[n-1] + nums[n-2] + nums[n-3] < target) continue;
        for(let j = i + 1;j < n - 2;j++){
            if(j - i > 1 && nums[j] === nums[j-1]) continue;
            if(nums[i] + nums[j] + nums[j+1] + nums[j+2] > target) break;
            if(nums[i] + nums[j] + nums[n-1] + nums[n-2] < target) continue;
            let left = j + 1;
            let right = n - 1;
            while(left < right){
                let tmpRes = nums[i] + nums[j] + nums[left] + nums[right];
                if(tmpRes === target){
                    res.push([nums[i],nums[j],nums[left],nums[right]]);
                    while(left < right && nums[left] === nums[left + 1]) left++;
                    while(left < right && nums[right] === nums[right - 1]) right--;
                    left++;
                    right--;
                }else if(tmpRes > target){
                    right--;
                }else{
                    left++;
                }
            }
        }
    }
    return res;
};
```