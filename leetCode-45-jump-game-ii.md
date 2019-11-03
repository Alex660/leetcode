# 跳跃游戏 II（困难）
# 题目描述
![截屏2019-11-01下午2.47.32.png](https://pic.leetcode-cn.com/0e9cfa2842c5917d1379858846c60a7e844eb28096954a64e0d6c7c8e9d383da-%E6%88%AA%E5%B1%8F2019-11-01%E4%B8%8B%E5%8D%882.47.32.png)
# 题目地址
<https://leetcode-cn.com/problems/jump-game-ii/>
#### 解法一：递归回溯
+ 时间复杂度：O(2^n)
+ 空间复杂度：O(n)
+ [戳看leetCode第55题-跳跃的游戏-解法一](https://leetcode-cn.com/problems/jump-game/solution/55-tiao-yue-you-xi-by-alexer-660/)
+ 分析
  + 换汤不换药
    + 这一题不过是求众多可能组合中，所用步数最小的解
    + 直接维护一个最小变量即可
    + 遍历中，增加一个step步数，每次++
  + 一如既往，时间超时   
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function(nums) {
    var minStep = Number.MAX_SAFE_INTEGER;
    function canJumpFromWhere(position,nums,step){
        // 跳到终点了
        if(position == nums.length - 1){
            minStep = Math.min(minStep,step);
        }
        // 当前位置可跳的最远距离索引位置，取min是因为最远距离不能超过nums的长度对应的索引值
        var furthestPosition = Math.min(position+nums[position],nums.length - 1);
        for(var nextPosition = position+1;nextPosition <= furthestPosition;nextPosition++){
            canJumpFromWhere(nextPosition,nums,step+1)         
        }
    }
    canJumpFromWhere(0,nums,0);
    return minStep;
};
```
#### 解法二：贪心算法
+ 时间复杂度：O(n)
+ 空间复杂度：O(1)
+ [戳看leetCode第55题-跳跃的游戏-解法五-贪心算法-PlanB](https://leetcode-cn.com/problems/jump-game/solution/55-tiao-yue-you-xi-by-alexer-660/)
+ 两题区别
  + 此题
    >假设你总是可以到达数组的最后一个位置。  
    使用最少的跳跃次数到达数组的最后一个位置。
  + 第55题
    >判断你是否能够到达最后一个位置。
  + 因此
    + 此题说明，无论怎么跳都会到达终点，所以测试用例中，不会出现诸如[3,2,1,0,4]、[1,0,4,2,3]等的终点不可达的情况
      + 意思是你可以尽情跳，跳少跳多都不用担心跳不到
        + 结合题目要求，使用最少的跳跃次数到达数组的最后一个位置
          + 那还说啥，每次跳当前位置最远距离不就好啦，步数自然是最少的，根本不用判断是否可达，因为无论你是站着跳，还是躺着跳，都会跳到人生巅峰！
    + 根据上述分析，写代码的时候，每次更新上一步所能跳的最远距离，遍历时，当且仅当上一步(可能隔好几步，这里是泛指)的最远距离 == 当前位置时，说明当前节点是你跳跳过程中每个阶段跳对应的可以跳的最远的距离，步数+1即可
  + 代码
    + 相比第55题而言，就直接去掉了不可达的情况,因为此题不存在不可达的情况 
      ```javascript
       if(i > canJumpMax){
         return false;   
       }
      ``` 
      + 第55题需要判断是否可达，哪怕到了最后倒数第二位依旧可能不可达
        ```javasctipt
          if(i > canJumpMax){
              return false;   
          }
          canJumpMax = Math.max(canJumpMax,i+nums[i]);
        ```
      + 解题技巧  
        + 此题也有可能出现第i个节点位置跳的距离大于末尾节点的，如果我们还是通过上述思路分析的那样，每次更新第i个节点的最远的跳的距离为下一个起跳点的话
          + 假设共有n个节点符合题意，为最少跳的位置节点 
          + 前n-k个符合要求的起跳点可能一下跳不到末尾
          + 但是第k+1之后的节点,其中的一个节点可能可以跳到，这个节点可能是倒数第一个，也可能是倒数第二个或第三个
            + 意思是最后一个符合条件的节点位置出现的情况有两种
              + 第一种是恰好在最后一个末尾位置
              + 第二种是不在最后一个末尾位置，而是在倒数第k个位置
                + 此种情况时说明已经达到来最优解，不用遍历到最后一位了，直接退出循环就好
        + 因此生成了下面第二种优化方法，优化速度喜人(同第55题基本一样) 
          ![截屏2019-11-02上午6.59.18.png](https://pic.leetcode-cn.com/02b7559e2630216de6d95bd45a0a88fcb67c4015560b51fc8fd87160184d80ab-%E6%88%AA%E5%B1%8F2019-11-02%E4%B8%8A%E5%8D%886.59.18.png) 
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function(nums) {
    var steps = 0;
    var canJumpMax = 0;
    var last_canJumpMax = 0;
    var len = nums.length;
    for(var i = 0;i<len-1;i++){
        canJumpMax = Math.max(canJumpMax,i+nums[i]);
        if(last_canJumpMax == i){
            last_canJumpMax= canJumpMax;
            steps++;
        }
    }
    return steps;
};
```
+ 优化版如下
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var jump = function(nums) {
    var steps = 0;
    var canJumpMax = 0;
    var last_canJumpMax = 0;
    var len = nums.length;
    for(var i = 0;i<len-1;i++){
        canJumpMax = Math.max(canJumpMax,i+nums[i]);
        if(last_canJumpMax == i){
            last_canJumpMax= canJumpMax;
            steps++;
        }
        if(last_canJumpMax >= len-1){
            break;
        }
    }
    return steps;
};
```