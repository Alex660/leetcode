# 插入区间（困难）
# 题目描述
![截屏2020-01-08上午11.02.51.png](https://pic.leetcode-cn.com/e4f1148b8605ec012b5ef88ccd740043605f3b333e3cd62e1e68e4edb1c4d1a3-%E6%88%AA%E5%B1%8F2020-01-08%E4%B8%8A%E5%8D%8811.02.51.png)
# 题目地址
<https://leetcode-cn.com/problems/insert-interval/>
#### 解法一：合并区间
+ [思路完全同 - 56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/solution/56-he-bing-qu-jian-by-alexer-660/)
  + 无非是将两个区间合到一起去，换汤不换药。
```javascript
/**
 * @param {number[][]} intervals
 * @param {number[]} newInterval
 * @return {number[][]}
 */
var insert = function(intervals, newInterval) {
    let res = [];
    intervals.push(newInterval);
    let len = intervals.length;
    if(len == 0) return [];
    intervals.sort((a,b) => a[0] - b[0]);
    let i = 0;
    while(i < len){
        let currLeft = intervals[i][0];
        let currRight = intervals[i][1];
        while(i < len - 1 && intervals[i+1][0] <= currRight){
            i++;
            currRight = Math.max(intervals[i][1],currRight);
        }
        res.push([currLeft,currRight]);
        i++;
    }
    return res;
};
```
#### 解法二：见缝插针
+ 思路
  + 在有序区间列表里插入一个区间，取得新区间的左边界newStart，右边界newEnd
  + 遍历原区间
    + 当newStart 大于 当前区间的右边界时
      + 说明两个区间没有交集，不用合并，又因为原区间列表有序
        + 所以直接将当前区间排入新的区间列表中
    + 否则当newEnd 大于 当前区间的左边界时
      + 两个区间有重合
        + 所以将两个区间的最小左边界和最大右边界重新组合为新的区间，即为合并
        + 将其排入新区间列表中
    + 否则当区间列表没有遍历完时
      + 将剩下的排入新区间列表中去
```javascript
/**
 * @param {number[][]} intervals
 * @param {number[]} newInterval
 * @return {number[][]}
 */
var insert = function(intervals, newInterval) {
    let res = [];
    let i = 0;
    let n = intervals.length;
    let newStart = newInterval[0];
    let newEnd = newInterval[1];
    while(i < n && newStart > intervals[i][1]){
        res.push(intervals[i]);
        i++;
    }
    while(i < n && newEnd >= intervals[i][0]){
        newStart = Math.min(newStart,intervals[i][0]);
        newEnd = Math.max(newEnd,intervals[i][1]);
        i++;
    }
    res.push([newStart,newEnd]);
    while(i < n){
        res.push(intervals[i]);
        i++;
    }
    return res;
};
```