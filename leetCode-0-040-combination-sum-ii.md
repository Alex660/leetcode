# 组合总和 II（中等）
# 题目描述
![截屏2019-12-25上午7.44.29.png](https://pic.leetcode-cn.com/31a1973bba40c3e97a705c796f5074e3482884c083f5c10d174103ae698e41cf-%E6%88%AA%E5%B1%8F2019-12-25%E4%B8%8A%E5%8D%887.44.29.png)
# 题目地址
<https://leetcode-cn.com/problems/combination-sum-ii/>
#### 回溯算法系列
+ [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/solution/39-zu-he-zong-he-by-alexer-660/)
+ [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/solution/40-zu-he-zong-he-ii-by-alexer-660/)
+ [46. 全排列](https://leetcode-cn.com/problems/permutations/solution/46-quan-pai-lie-by-alexer-660/)
+ [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/solution/47-quan-pai-lie-ii-by-alexer-660/)
+ [77. 组合](https://leetcode-cn.com/problems/combinations/solution/77-zu-he-by-alexer-660/)
+ [78. 子集](https://leetcode-cn.com/problems/subsets/solution/78-zi-ji-by-alexer-660/)
+ [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/solution/90-zi-ji-ii-by-alexer-660/)
#### 解法：递归回溯 + 双重剪枝
+ 递归代码模板
  + [参看各类算法模板 - 递归一节 - Python&Java版](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
+ 类似题型
  + [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/solution/39-zu-he-zong-he-by-alexer-660/)
  + [47. 全排列 II - 解法二](https://leetcode-cn.com/problems/permutations-ii/solution/47-quan-pai-lie-ii-by-alexer-660/)
+ 思路
  + 一重减枝
    + 本题和39题唯一不同的是
      + 39题原数组的每个元素可以重复使用
      + 此题只能使用一次
        + 由39题题解可知
          + **且原数组的单个元素可以重复使用**
            + 意味着下一个for循环中的元素选取，要从前一个元素开始，因为可以重复使用，不然如果跟着for的自增变量i走，会漏掉可能解
              + 将自增变量i传递下去
          + 此题不能重复，意思就是当前自增变量不能传递下去，要传递下一个自增变量 i + 1
  + 二重减枝
    + 和47题一样，既然不能重复当前元素，那么利用排序，将相邻两个相同的元素只取前一个去组合，当前直接跳过，直接进入下一个元素进行组合
+ 题外话
  + break 语句用于跳出循环，即当前整个for循环终止，如有递归里使用for，则直接进入下一个出栈函数中的for循环
  + continue 用于跳过循环中的一个迭代，即跳过当前自增的元素，直接进入下一个元素，当前for循环还在运行中
```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum2 = function(candidates, target) {
    let n = candidates.length;
    let res = [];
    let tmpPath = [];
    candidates = candidates.sort((a,b) => {return a - b})
    let backtrack = (tmpPath,target,start) => {
        if(target == 0){
            res.push(tmpPath);
            return;
        }
        for(let i = start;i < n;i++){
            if(target < candidates[i]) break;
            if(i > start && candidates[i-1] == candidates[i]) continue;
            tmpPath.push(candidates[i]);
            backtrack(tmpPath.slice(),target - candidates[i],i + 1);
            tmpPath.pop();
        }
    }
    backtrack(tmpPath,target,0);
    return res;
};
```