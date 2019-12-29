# 子集（中等）
# 题目描述
![截屏2019-10-26上午9.41.46.png](https://pic.leetcode-cn.com/7aeebe470cfd972cab153df995927e0cc2383adab5c667b0296010cd201d0675-%E6%88%AA%E5%B1%8F2019-10-26%E4%B8%8A%E5%8D%889.41.46.png)
# 题目地址
<https://leetcode-cn.com/problems/subsets/>
#### 回溯算法系列
+ [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/solution/39-zu-he-zong-he-by-alexer-660/)
+ [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/solution/40-zu-he-zong-he-ii-by-alexer-660/)
+ [46. 全排列](https://leetcode-cn.com/problems/permutations/solution/46-quan-pai-lie-by-alexer-660/)
+ [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/solution/47-quan-pai-lie-ii-by-alexer-660/)
+ [77. 组合](https://leetcode-cn.com/problems/combinations/solution/77-zu-he-by-alexer-660/)
+ [78. 子集](https://leetcode-cn.com/problems/subsets/solution/78-zi-ji-by-alexer-660/)
+ [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/solution/90-zi-ji-ii-by-alexer-660/)
#### 解法一：递归回溯
+ 代码写法
  + [参看各类算法模板 - 递归一节 - Python&Java版](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
+ 类似题型
  + [46. 全排列](https://leetcode-cn.com/problems/permutations/solution/46-quan-pai-lie-by-alexer-660/)
+ 思路简析
  + 这里相比之前几道类似的题，res处不用条件
    + 因为题意是，所有可能的子集，连 [ ] 都不放过
  + 且当前自增变量不能传递下去，要传递下一个自增变量 i + 1
    + 因为并没有说可以重复利用原数组的单个元素去组合子集，所以下一次搜索要从 i + 1 开始，跳过当前元素
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function(nums) {
    let n = nums.length;
    let tmpPath = [];
    let res = [];
    let backtrack = (tmpPath,start) => {
        res.push(tmpPath);
       for(let i = start;i < n;i++){
           tmpPath.push(nums[i]);
           backtrack(tmpPath.slice(),i+1);
           tmpPath.pop();
       } 
    }
    backtrack(tmpPath,0);
    return res;
};
```
+ 或者这样写
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function(nums) {
    var result = [];
    var subsets = [];
    var index = 0;
    var len = nums.length;
    function combinate(subsets,index){
         // terminator
        if(index == len){
            result.push(subsets)
            return;
        }
        // process
        combinate(subsets,index+1);
        subsets.push(nums[index]);
        combinate(subsets.slice(0),index+1);
        // reverse states
        subsets.pop();

        //或
        // process
        // combinate(subsets.slice(0),index+1);
        // subsets.push(nums[index]);
        // combinate(subsets.slice(0),index+1);
    }
    combinate(subsets,0);
    return result;
};
```
#### 解法二：迭代
+ 举个栗子
  + nums：[1,2,3]
  + 初始化：res = []
    + i = 0
      + [1]
        + res = [ [],[1] ]
    + i = 1
      + [2],[1,2]
        + res = [ [],[1],[2],[1,2]]
    + i = 2
      + [3],[1,3],[2,3],[1,2,3]
        + res = [ [],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3] ]
+ 总结
  + 重复将新的元素加入到上一个结果集中的每个子集当中去，形成n个新的子集，再全部加入到结果集中去
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function(nums) {
    let res = [[]];
    for(let i = 0;i < nums.length;i++){
        let len = res.length;
        for(let j = 0;j < len;j++){
            let sub = res[j].slice();
            sub.push(nums[i]);
            res.push(sub);
        }
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
var subsets = function(nums) {
    let res = [[]];
    for(let i = 0;i < nums.length;i++){
        let len = res.length;
        for(let j = 0;j < len;j++){
            //或者这样写
            //res.push([...res[j],nums[i]]);
            res.push(res[j].concat([nums[i]]));
        }
    }
    return res;
};
```
#### 解法三：二叉树的三序遍历
+ [参看 - 三序遍历解法](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/demos/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E4%B8%89%E5%BA%8F%E9%81%8D%E5%8E%86.md)
#### 解法四：位运算
+ 思路
  + 数组中的每个元素，均有两个状态
    + 1、不在结果数组中，用 0 表示
    + 2、在结果数组中，用 1 表示
  + 因此所有状态组合就有2^n 个
  + 举个栗子
    + 1，2，3
    + 0，0，0 => []
    + 0，0，1 => [3]
    + 0，1，0 => [2]
    + 0，1，1 => [2，3]
    + 1，0，0 => [1]
    + 1，0，1 => [1，3]
    + 1，1，0 => [1，2]
    + 1，1，1 => [1，2，3]
  + 求解
    + 遍历判断每个比位是否是1，是1的话加入子集的一部分
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function(nums) {
    let res = [];
    let len = nums.length;
    // 2^n 获取所有状态
    let resAll = 1 << len;
    for(let i = 0;i < resAll;i++){
        let subset = [];
        // 当前数组的索引位置
        let j = 0;
        // 移位
        let iCopy = i;
        while(iCopy != 0){
            //判断当前位置是否是1
            if( (iCopy & 1) == 1){
                subset.push(nums[j]);
            }
            j++;
            // 右移一位
            iCopy >>= 1;
        }
        res.push(subset);
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
var subsets = function(nums) {
    let res = [];
    let len = nums.length;
    let resAll = 1 << len;
    for(let i = 0;i < resAll;i++){
        let subset = [];
        for(let j = 0;j < len;j++){
            if(((i >> j) & 1) == 1){
                subset.push(nums[j]);
            }
        }
        res.push(subset);
    }
    return res;
};
```