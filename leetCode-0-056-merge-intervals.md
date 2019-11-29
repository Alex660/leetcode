# 合并区间（中等）
# 题目描述
![截屏2019-11-27下午10.38.33.png](https://pic.leetcode-cn.com/199081cb5869575f5feb38ce02cfe158b792c5b5f4d404568ef54571bccae838-%E6%88%AA%E5%B1%8F2019-11-27%E4%B8%8B%E5%8D%8810.38.33.png)
# 题目地址
<https://leetcode-cn.com/problems/merge-intervals/>
#### 解法：排序
+ 时间复杂度：O( nlogn )
+ 空间复杂度：O(n)
+ 思路
  + 可以重叠的空间
    + a = [1,4]、b = [2,3] => [1,4]
    + a = [1,3]、b = [2,6] => [1,6]
    + a = [1,2]、b = [2,5] => [1,5]
  + 不可重叠
    + a = [1,4]、b = [5,7]
  + 归纳
    + 当a[1] >= b[0]时，两个区间一定有重叠
    + 重叠后的区间
      + 左边 == a[0]
      + 右边 == Max(a[1],b[1])
  + 解题技巧
    + 通过子数组首元素排序，确保左边相对确定下来
    + 下一个子数组的第一个元素小于等于当前子数组的第二个元素时，会有重叠
      + 需要循环穷举
```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function(intervals) {
    let result = [];
    let len = intervals.length;
    if(len == 0){
        return [];
    }
    intervals.sort( (a,b) => a[0] - b[0]);
    let i = 0;
    while( i < len){
        let currLeft = intervals[i][0];
        let currRight = intervals[i][1];
        while(i < len - 1 && intervals[i+1][0] <= currRight){
            i++;
            currRight = Math.max(intervals[i][1],currRight);
        }
        result.push([currLeft,currRight]);
        i++;
    }
    return result;
};
```