# 全排列 II（中等）
# 题目描述
![截屏2019-12-24下午7.19.14.png](https://pic.leetcode-cn.com/bd7bd968898d6db539001c4a1167d8147ab239627f915b3f272c500ae9f78d06-%E6%88%AA%E5%B1%8F2019-12-24%E4%B8%8B%E5%8D%887.19.14.png)
# 题目地址
<https://leetcode-cn.com/problems/permutations-ii/>
#### 回溯算法系列
+ [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/solution/39-zu-he-zong-he-by-alexer-660/)
+ [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/solution/40-zu-he-zong-he-ii-by-alexer-660/)
+ [46. 全排列](https://leetcode-cn.com/problems/permutations/solution/46-quan-pai-lie-by-alexer-660/)
+ [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/solution/47-quan-pai-lie-ii-by-alexer-660/)
+ [77. 组合](https://leetcode-cn.com/problems/combinations/solution/77-zu-he-by-alexer-660/)
+ [78. 子集](https://leetcode-cn.com/problems/subsets/solution/78-zi-ji-by-alexer-660/)
+ [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/solution/90-zi-ji-ii-by-alexer-660/)
#### 解法一：递归回溯
+ 类似题型
  + [46. 全排列](https://leetcode-cn.com/problems/permutations/solution/46-quan-pai-lie-by-alexer-660/)
+ 递归代码模板
  + [参看各类算法模板 - 递归一节 - Python&Java版](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
+ 超时
  + 和46题解法几乎一模一样
  + 唯一不同的就是此题可能有重复组合
    + 在结果时判断重复的组合不再次进入
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permuteUnique = function(nums) {
    let n = nums.length;
    let res = [];
    let tmpPath = [];
    let hash = {};
    let backtrack = (tmpPath) => {
        if(tmpPath.length == n && JSON.stringify(res).indexOf(JSON.stringify(tmpPath)) == -1){
            res.push(tmpPath);
            return;
        }
        for(let i = 0;i < n;i++){
            if(!hash[i+'-'+nums[i]]){
                hash[i+'-'+nums[i]] = true;
                tmpPath.push(nums[i]);
                backtrack(tmpPath.slice());
                hash[i+'-'+nums[i]] = false;
                tmpPath.pop();
            }
        }
    }
    backtrack(tmpPath);
    return res;
};
```
#### 解法二：递归回溯 + 减枝
+ 重复问题
  + [1,1,2]
    + i = 0 => [ [1,1,2],***[1,2,1]*** ]
    + i = 1 => [ ***[1,2,1]*** ]
    + i = 2 => [ [2,1,1] ]
  + 结果
    + [ [1,1,2],[1,2,1],[2,1,1]] 
+ 剪枝
  + 排序原数组
  + 相邻两个相同的元素，跳过当前元素，从下一个元素开始组合
    + 从源头减少重复数组进入 res
    + 例如[1,1,2]
      + 当i =  1时， nums[i-1] == nums[i]，此时不组合，即i = 1时的组合[1,2,1]不进入结果
      + **也说明了排序的意义所在**
+ 区别
  + 相比解法一，此题从源头判重，重复则子组合都无需进行组合
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permuteUnique = function(nums) {
    let n = nums.length;
    nums = nums.sort((a,b) => {return a - b});
    let res = [];
    let tmpPath = [];
    let hash = {};
    let backtrack = (tmpPath) => {
        if(tmpPath.length == n){
            res.push(tmpPath);
            return;
        }
        for(let i = 0;i < n;i++){
            if(hash[i] || (i > 0 && !hash[i-1] && nums[i-1] == nums[i])) continue;
            hash[i] = true;
            tmpPath.push(nums[i]);
            backtrack(tmpPath.slice());
            hash[i] = false;
            tmpPath.pop();
        }
    }
    backtrack(tmpPath);
    return res;
};
```