# 子集 II（中等）
# 题目描述
![截屏2019-12-29上午10.06.59.png](https://pic.leetcode-cn.com/3d9f11e1347fb1afa4b0bbb95c45ae530d9514658fc5d7c9b38657cc81d7fedb-%E6%88%AA%E5%B1%8F2019-12-29%E4%B8%8A%E5%8D%8810.06.59.png)
# 题目地址
<https://leetcode-cn.com/problems/subsets-ii/>
#### 回溯算法系列
+ [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/solution/39-zu-he-zong-he-by-alexer-660/)
+ [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/solution/40-zu-he-zong-he-ii-by-alexer-660/)
+ [46. 全排列](https://leetcode-cn.com/problems/permutations/solution/46-quan-pai-lie-by-alexer-660/)
+ [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/solution/47-quan-pai-lie-ii-by-alexer-660/)
+ [77. 组合](https://leetcode-cn.com/problems/combinations/solution/77-zu-he-by-alexer-660/)
+ [78. 子集](https://leetcode-cn.com/problems/subsets/solution/78-zi-ji-by-alexer-660/)
+ [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/solution/90-zi-ji-ii-by-alexer-660/)
#### 解法一：递归回溯 + 减枝
+ 类似题型
  + [47. 全排列 II - 解法二](https://leetcode-cn.com/problems/permutations-ii/solution/47-quan-pai-lie-ii-by-alexer-660/)
+ 递归代码模板
  + [参看各类算法模板 - 递归一节 - Python&Java版](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsetsWithDup = function(nums) {
    let n = nums.length;
    nums = nums.sort((a,b) => {return a - b});
    let tmpPath = [];
    let res = [];
    let hash = {}
    let backtrack = (tmpPath,start) => {
        res.push(tmpPath);
       for(let i = start;i < n;i++){
        if(hash[i] || (i > 0 && !hash[i-1] && nums[i-1] == nums[i])) continue;
        hash[i] = true;
        tmpPath.push(nums[i]);
        backtrack(tmpPath.slice(),i+1);
        hash[i] = false;
        tmpPath.pop();
       } 
    }
    backtrack(tmpPath,0);
    return res;
};
```
#### 解法二：迭代
+ 类似题型
  + [78. 子集 - 解法二](https://leetcode-cn.com/problems/subsets/solution/78-zi-ji-by-alexer-660/)
+ 举个栗子
  + 按照78-解法二来做的话
  + nums：[1,2,2]
  + 初始化：res = []
    + i = 0
      + [1]
      + len = 1
        + res = [ [],[1] ]
        + **新解的开始索引位置：1 == len**
    + i = 1
      + [2],[1,2]
      + len = 2
        + res = [ [],[1],[2],[1,2]]
        + **新解的开始索引位置：2 == len**
    + i = 2
      + [2],[1,2],[2,2],[1,2,2]
      + len = 4
        + res = [ [],[1],[2],[1,2],[2],[1,2],[2,2],[1,2,2] ]
        + **新解的开始索引位置：4 == len**
+ 关键
  + i = 2 时
  + 此处出现了重复数字，即nums[i] == nums[i-1]
  + 处理重复  
    + 如果像解法一那样，直接跳过nums[i]不进行组合
      + 将会漏掉[2,2]、[1,2,2]这两个解
    + 如果维持不变
      + 则结果集中重复了[2]、[1,2]这两个解
    + 解决办法
      + 我们不像78题那样
        + 重复将新的元素加入到上一个结果集中的每个子集当中去，形成n个新的子集，再全部加入到结果集中去
      + 我们改成将新的元素加入到上一个结果集中最近一次新加入进去的子集当中去
        + 即从上上次结果集的末尾开始遍历上次新加入的结果集，将新元素一一加进去
      + 如栗子
        + i = 1
          + [2],[1,2]
          + len = 2
            + res = [ [],[1],[2],[1,2]]
            + **新解的开始索引位置：2 == len**
        + i = 2
          + 那么从 i = 2处开始遍历结果集，一一加入新元素入遍历的子集当中去
            + [2,2],[1,2,2]
            + [],[1]这个两个就被略过了
              + 从而也避免了重复集合[2],[1,2]的产生
      + 维护一个新解开始位置的变量
        + 即加入之前原数组的长度
        + 内循环组合遍历时，要跳过小于新解开始位置的遍历
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsetsWithDup = function(nums) {
    let n = nums.length;
    nums = nums.sort((a,b) => {return a - b});
    let start = 1;
    let res = [[]];
    for(let i = 0;i < nums.length;i++){
        let len = res.length;
        let resTmp = [];
        for(let j = 0;j < len;j++){
            if(i > 0  && j < start && nums[i-1] == nums[i]) continue;
            resTmp.push(res[j].concat([nums[i]]));
        }
        start = res.length;
        res.push(...resTmp);
    }
    return res;
};
```
#### 解法三：位运算
+ 类似题型
  + [78. 子集 - 解法四](https://leetcode-cn.com/problems/subsets/solution/78-zi-ji-by-alexer-660/)
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsetsWithDup = function(nums) {
    let res = [];
    let len = nums.length;
    nums = nums.sort((a,b) => {return a - b});
    // 2^n 获取所有状态
    let resAll = 1 << len;
    for(let i = 0;i < resAll;i++){
        let subset = [];
        // 当前数组的索引位置
        let j = 0;
        // 移位
        let iCopy = i;
        let illegal = false;
        while(iCopy != 0){
            //判断当前位置是否是1
            if( (iCopy & 1) == 1){
                // 当前是重复数字，且前一位是0，跳过当前数
                if(j > 0 && nums[j] == nums[j-1] && (i >> (j-1) & 1) == 0){
                    illegal = true;
                    break;
                }else{
                    subset.push(nums[j]);
                }
            }
            j++;
            // 右移一位
            iCopy >>= 1;
        }
        !illegal && res.push(subset);
    }
    return res;
};
```
+ 或者这样写
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsetsWithDup = function(nums) {
    let res = [];
    let len = nums.length;
    nums = nums.sort((a,b) => {return a - b});
    let resAll = 1 << len;
    for(let i = 0;i < resAll;i++){
        let subset = [];
        let illegal = false;
        for(let j = 0;j < len;j++){
            if(((i >> j) & 1) == 1){
                if(j > 0 && nums[j] == nums[j-1] && (i >> (j-1) & 1) == 0){
                    illegal = true;
                    break;
                }else{
                    subset.push(nums[j]);
                }
            }
        }
        !illegal && res.push(subset);
    }
    return res;
};
```