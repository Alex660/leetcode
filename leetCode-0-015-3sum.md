# 三数之和（中等）
# 题目描述
![截屏2019-11-09下午3.59.33.png](https://pic.leetcode-cn.com/9a3f7e8aff65eb1a0becde51d6095b2782de79c58e74eadac939e0f5f22900c4-%E6%88%AA%E5%B1%8F2019-11-09%E4%B8%8B%E5%8D%883.59.33.png)
# 题目地址
<https://leetcode-cn.com/problems/3sum/>
#### 解法一：暴力法
+ 类似题型
  [1. 两数之和](https://leetcode-cn.com/problems/two-sum/solution/1-liang-shu-zhi-he-by-alexer-660/) 
  + 区别就是判重
    + 此处我依旧用hash判断
+ 执行时间超时
  + 时间复杂度：O(n^3)  
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
    var result = [];
    var repeatSet = new Set();
    var len = nums.length;
    for(var i = 0;i < len-2;i++){
        for(var j = i+1;j < len-1;j++){
            for(var k = j+1;k < len;k++){
                if(nums[i]+nums[j]+nums[k] == 0){
                    var tmpResult = [nums[i],nums[j],nums[k]];
                    var tmpSortStr = String(tmpResult.sort());
                    if(!repeatSet.has(tmpSortStr)){
                        result.push(tmpResult);
                        repeatSet.add(tmpSortStr)
                    }
                }
            }
        }
    }
    return result;
};
```
#### 解法二：排序避重
+ 时间复杂度：O(N^2)
+ 空间复杂度：O(1)
+ 思路
  + 本题求解不难，由[1. 两数之和](https://leetcode-cn.com/problems/two-sum/solution/1-liang-shu-zhi-he-by-alexer-660/) 这道题可以看出不难
    + 但是如何去重求出符合题意的解很难，两个数避免重复，用一个hashSet即可如hash[target-nums[x]]!=nums[y]，因为只有两个数，是一一对应的，不是-1就是1
    + 而三数重复难以判断的原因是不能一一对应，如1，-1，0和-1，1，0；这种情况出现的原因是
      + nums里有两个以上1或两个以上-1，或两个以上0
    + 因此想要求解避免重复？
      + 那只能去掉多余的1或-1或0，只能他们一个，这怎么实现呢？
        + 一种方法是全局去重即直接数组去重，这似乎可以办到，避免求出来的解有重复
          + 但是可能会漏掉合理的解，例如：-1，-1，2是合理的因为数组去重，就不能再得到这组解了
        + 另一种是局部去重，怎么理解呢？
          + 就是在求的一组符合题意的解后，立刻判断之前的解是否有相同的解，诚如开头所说，三数之后的判重组合可能性较多，不如两数之后可能性只有两种来的简单
            + 通过直接跳过可能出现重复解的位置
              + 例如：...，-1，-1，-1，...
                + 当遍历到-1时，我们假设找到了一组数字形如x+(-1)+y == 0的符合题意的组合
                + 当遍历到第二个-1时，我们直接i++，跳过，因为上面一步已经求出了和-1组合的所有符合题意的一组数字，再用第二个-1重新循环组合时必然出现同一个和上面相同的解
                  + 那当遍历第一个-1求解需要第二个-1时呢？如-1,-1,2
                    + 是一样的，因为如果不跳过第二个-1，并开始组合的话依旧会出现-1，2，-1的可能解，而此解重复了
              + 而当找到一组组合时，要找到下一个和当前位置数字相同的数字，并跳过，需要遍历整个数组
                + 因此直接数组排序即可，重复的值一定在自己左右，直接++或--即可  
            + 因此当遍历到一组符合题意的解时，直接跳过下一个和当前遍历位置相同数字的是必要的。 
              + 下下一个如果还相同，则继续跳过 
            + 而跳过当前位置，可以通过移动指针的方法
              + 这里有三个数，所以至少需要两个指针，即双指针法，第三个指针借用数组的遍历  
                + 双指针法经常采用两端夹逼的方式来求解
  + 上面三数之和求解文字思路具象化
    + 跳过重复解寻找重复数字并跳过
      + nums从小到大排序
    + 一固定指针双移动指针 == 3个数字的索引位置
      + 第一个数跟着数组遍历而变化，从0开始，为i
      + 第二个数每一轮遍历初始化时，紧跟第一个数之后，为i+1
      + 第三个数和第二个数为双指针的右指针，所以初始化为len-1，最右边
        + 第一个数从0到len-1遍历，另外两个指针夹逼
          + 当nums[i] > 0时，跳出for循环，break，代码最后返回结果集
            + 也是循环终止条件，返回最终结果集
          + 当nums[i] <=0 时
            + 当i>0时
              + 当nums[i] == nums[i-1]时，直接进入下一轮循环继续寻找以免重复解
            + 确定双指针位置
              + L = i+1
              + R = len-1
              + 夹逼求另外两个数的可能组合（第一个数为当前遍历i位置的值，相对固定）
                + 当 L < R 时
                  + 当 sum = nums[i]+nums[L]+nums[R] = 0时
                    + 首先，加入结果集 
                    + 其次，不断更新双指针的位置直到当前遍历节点没有重复解的可能 
                      + 跳过左边重复的nums[L]
                        + nums[L] == nums[L+1]
                          + L++
                        + 直到nums[L] != nums[L+1]   
                      + 跳过右边重复的nums[R]
                        + nums[R] == nums[R-1]
                          + R--
                        + 直到nums[R] != nums[R-1] 
                  + 当 sum > 0 时
                    + R--
                  + 当 sum < 0时
                    + L--    
                + 当 L >= R 时
                  + 相遇，说明没有其它解了
        + 当循环完后，返回最终结果集
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
    var result = [];
    var len = nums.length;
    if(nums == null || len < 3){
        return result;
    }
    // 从小到大排序
    nums.sort((a,b) => a-b);
    // 第一个数从0开始遍历，作为三数第一个数的位置
    for(var i = 0;i < len;i++){
        // 题意是三数之后为0，大于0则当前位置肯定不存在解
        if(nums[i] > 0){
            break;
        }
        // 第一个数后面中有两个数如果相同且有解时，则此解一定会重复，因此事先跳过直接进入下一个i位置的遍历
        if(i > 0 && nums[i] == nums[i-1]){
            continue;
        }
        // 另外两个数的指针位置
        var L = i + 1;
        var R = len - 1;
        // 更新移动指针位置，夹逼求解
        while(L < R){
            var sum = nums[i] + nums[L] + nums[R];
            // 有解
            if(sum == 0){
                result.push([nums[i],nums[L],nums[R]]);
                // 更新指针位置
                // 同上面的continue状态解题，区别是要不断更新直到符合时
                while(nums[L] == nums[L+1]){
                    L++;
                }
                while(nums[R] == nums[R-1]){
                    R--;
                }
                // 夹逼
                L++;
                R--;
            }else if(sum < 0){
                L++;
            }else{
                R--;
            }
        }
    }
    return result;
};
```