# 全排列（中等）
# 题目描述
![截屏2019-12-22下午9.33.11.png](https://pic.leetcode-cn.com/6089dc997aef399b9fe359820de6dfff8fb29f5b5ed41db5cb8fd3340992e063-%E6%88%AA%E5%B1%8F2019-12-22%E4%B8%8B%E5%8D%889.33.11.png)
# 题目地址
<https://leetcode-cn.com/problems/permutations/>
#### 解法：递归回溯
+ 代码写法
  + [参看各类算法模板 - 递归一节 - Python&Java版](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/theoreticalKnowledge/AlgorithmTemplate%E7%AE%97%E6%B3%95%E6%A8%A1%E6%9D%BF.md)
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function(nums) {
    let n = nums.length;
    let res = [];
    let tmpPath = [];
    let backtrack = (tmpPath) => {
        if(tmpPath.length == n){
            res.push(tmpPath);
            return;
        }
        for(let i = 0;i < n;i++){
            if(!tmpPath.includes(nums[i])){
                tmpPath.push(nums[i]);
                backtrack(tmpPath.slice());
                tmpPath.pop();
            }
        }
    }
    backtrack(tmpPath);
    return res;
};
```