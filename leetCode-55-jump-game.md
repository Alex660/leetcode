# 跳跃游戏（中等）
# 题目描述
![截屏2019-10-31下午4.11.54.png](https://pic.leetcode-cn.com/19c3d17061ea8e8293f30da084f0b3acccf3f2a88800c90d21a7c1b721c24728-%E6%88%AA%E5%B1%8F2019-10-31%E4%B8%8B%E5%8D%884.11.54.png)
# 题目地址
<https://leetcode-cn.com/problems/jump-game/>
# 解法四优化版
![截屏2019-11-01上午10.03.12.png](https://pic.leetcode-cn.com/1e476d1602cda9f3efe4a350c6b6c1332785cea1d3e736cfb5c25d4061fb4224-%E6%88%AA%E5%B1%8F2019-11-01%E4%B8%8A%E5%8D%8810.03.12.png)
# 解法五优化版
![截屏2019-11-01下午2.12.41.png](https://pic.leetcode-cn.com/2ba72c50179002166016ef811bb1390d484434260b1bc29ad3402d70a52af7ca-%E6%88%AA%E5%B1%8F2019-11-01%E4%B8%8B%E5%8D%882.12.41.png)
### 动态规划思路之借花献佛
+ 利用递归回溯解决问题
+ 利用记忆表优化（自顶向下的动态规划）
+ 移除递归的部分（自底向上的动态规划）
+ 使用技巧减少时间和空间复杂度
#### 解法一：递归回溯
+ 时间复杂度：O(2^n)
  + 可想而知，运行将超时 
+ 空间复杂度：O(n)
+ 分析：
  + 遍历当前位置能向前跳步数的所有可能
    + 针对每一种可能递归调用下一步和下一步能跳步数的所有可能
  + 如果递归遍历过程中有一种跳到了最后一步，则说明此种跳法符合题意直接返回true
  + 如果当前跳法试过了当前可跳的所有跳法依旧不能到达最后一个位置，则说明当前跳法不可取 
    + 回溯上一步（有函数递归栈）
    + 直到栈空前，能够跳到最后一位说明可达，不能则返回false，没有可达的方案  
  + 解题技巧
    + 实际情况下，每次尽可能跳出当前自己所能跳的最大位置数，更容易接近终点位置，因此：
      > for(var nextPosition = position+1;nextPosition <= furthestPosition;nextPosition++){
    + 可优化为
      + for(var nextPosition = furthestPosition;nextPosition >position;nextPosition--){       
```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
    function canJumpFromWhere(position,nums){
        // 跳到终点了
        if(position == nums.length - 1){
            return true;
        }
        // 当前位置可跳的最远距离索引位置，取min是因为最远距离不能超过nums的长度对应的索引值
        var furthestPosition = Math.min(position+nums[position],nums.length - 1);
        for(var nextPosition = position+1;nextPosition <= furthestPosition;nextPosition++){
            if(canJumpFromWhere(nextPosition,nums)){
                return true;
            }
        }
        return false;
    }
    return canJumpFromWhere(0,nums);
};
```
#### 解法二：带备忘录的递归回溯
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)
+ 即动态规划思路的第二步
  + 也即自顶向下的动态规划 
    + 由解法一可知，当遇到一个位置点的其中一种跳法成功到达终点，就说明此种跳法可行,记录arr[position] = true
    + 若当前一种跳法不可行，却又并不影响下一种跳法,记录arr[position] = false
    + 因此对访问过的位置节点的各种跳法，作出记录，避免重复计算 
    + 动态规划初始条件
      + 若没有初始值，即没有一个已知的子问题的解的话，是不能动态推出结果的
      + 此题，最后一个位置节点时， arr[nums.length-1] = true，是一定的，已经在最后一个位置了，只需要跳0步就可以到达最终目标位置，符合题意
    + 自顶向下，“顶”体现在由最后的目标位置节点arr[nums.length-1]，一步一步推出第一步arr[0]的所有跳法集合，方向从左到右，且右边的节点位置跳法也要计算。
    + 解题技巧
      + javascript中，数组不需要初始初始值化，也可以随机访问，只不过是undefined,因此初始化备忘录数组时，
      + 只需要初始化数组最大容量即可，不需要填充初始值，
      + 这里用小技巧解决，可以不超出时间限制，亲测可行，效果喜人。
     ![截屏2019-11-01上午6.10.17.png](https://pic.leetcode-cn.com/d0a96f54a94acc973afe6678eaa72867992d1e6c3035cb3b84a78502287c6577-%E6%88%AA%E5%B1%8F2019-11-01%E4%B8%8A%E5%8D%886.10.17.png)
```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
    var arr = new Array(nums.length);
    arr[nums.length-1] = true;
    function canJumpFromWhere(position,nums){
        if(arr[position] != undefined){
            return arr[position];
        }
        var furthestPosition = Math.min(position+nums[position],nums.length - 1);
        for(var nextPosition = position+1;nextPosition<=furthestPosition;nextPosition++){
            if(canJumpFromWhere(nextPosition,nums)){
                arr[position] = true;
                return true;
            }
        }
        arr[position] = false;
        return false;
    }
    return canJumpFromWhere(0,nums);
};
```
#### 解法三：带备忘录的迭代
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)
+ 即动态规划的第三步
  + 自底向上的动态规划 
    + 消除递归回溯，改用迭代，省去了函数调用栈的空间
      + 通过从右向左遍历，这样动态推出左边直到第一个节点位置的所有跳法时判断右边位置的跳法就不需要再重新计算。
    + 以下代码实现可以不填充初始值，可以试试速度是否有优化 
```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
    var arr = new Array(nums.length).fill(false);
    arr[nums.length-1] = true;
    for(var right = nums.length-2;right>=0;right--){
        var furthestPosition = Math.min(right+nums[right],nums.length-1);
        for(var left = right + 1;left<=furthestPosition;left++){
            if(arr[left] == true){
                arr[right] = true;
                break;
            }
        }
    }
    return arr[0];
};
``` 
#### 解法四：贪心算法-PlanA
+ 时间复杂度：O(n)
  + 由此可知，方法没有差的，只有适不适合一说，此题，贪心算法时间要优于其它解法包括动态规划 
+ 空间复杂度：O(1)
+ 贪心算法定义
  + 是一种在每一步选择中都采取在当前状态下最好或最优(即最有利)的选择，
  + 从而希望导致结果是全局最好或最优的算法
+ 注意
  + 贪心算法并不能每次都能给出最优解
  + 例如最短路径问题，从顶点S开始，找到一条到顶点T的最短路径问题
+ 思路分析
  + 由解法三自底向上遍历的方式可知：
    >if(arr[left] == true){  
        arr[right] = true;  
        break;  
    }   
    + 当找到一个可以跳的跳法时，就跳出当前循环
      + 此时的left符合条件，且是有解的最左边的位置
    + 合并外层循环可知
      + 整个两层循环均是在找符合有解条件的最左边的 ***起跳位置*** 节点
        + 当且仅当起跳位置索引是0时，即为所求，并返回true
        + 否则返回false 
      + 本质在计算遍历符合题意最左边的节点 是否 为 ***起跳位置*** 0
      + 因此
        + 不需要知道其它位置节点的跳法是否为true
          + 即不需要维护一个数组存储 
        + 只需要维护一个最左边的变量位置
          + 对于每个节点，判断是否存在一步跳跃可以到达目标节点的位置
            + 如果可以，则此节点成为动态的最左边节点，跳出当前循环，进入下一轮外层循环寻找
            + 如果不可以，继续循环遍历 
          + 解题技巧
            + 当前节点i,最大起跳距离S == nums[i] + i 
              + 当且仅当S >= nums.length-1 时，可达
              + 否则继续当前循环寻找 
        + 遍历结束后判断是否等于起跳位置即是否为0？
          + 是，即为所求
          + 否，false 没得说！  
```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
    var leftPos =nums.length-1;
    for(var left = nums.length-1;left>=0;left--){
        if(nums[left]+left >= leftPos){
            leftPos = left;
        }
    }
    return leftPos == 0;
};
```
**优化技巧** 
+ 遍历每次都要去nums.length
  + 直接设置一个变量存储nums的长度
  + 就不需要每次去取数组的长度了
  + 优化速度喜人，看图：
    ![截屏2019-11-01上午10.03.20.png](https://pic.leetcode-cn.com/1669d3bf64954f3778f135233d3e7a12cc6b98f69a774fb770ab0d530625a112-%E6%88%AA%E5%B1%8F2019-11-01%E4%B8%8A%E5%8D%8810.03.20.png)
    + 直接提升了近40%的速度
```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
    var lenPoint = nums.length-1;
    var leftPos =lenPoint;
    for(var left = lenPoint;left>=0;left--){
        if(nums[left]+left >= leftPos){
            leftPos = left;
        }
    }
    return leftPos == 0;
};
```
![截屏2019-11-01上午10.18.18.png](https://pic.leetcode-cn.com/eb5d811a061071809cf65790d206b4ea59af909e611502fc91679f3ceef20fb4-%E6%88%AA%E5%B1%8F2019-11-01%E4%B8%8A%E5%8D%8810.18.18.png)
#### 解法五：贪心算法-PlanB
+ 分析
  + 解法四是不断更新左边节点来断逼近起跳点
    + 最后等于0即为所求
    + 否则返回false 
  + 此题解法相对解法四，可以反过来，着重点从跳跳节点位置转移到跳跳距离上来做文章
    + 栗子：[3,2,1,0,4] == nums
    + 依次遍历(每个节点对应可跳最大距离 == i+nums[i])数组
      + 存储第i个节点可以跳的最远的距离
        + 遍历到第(i+1)个节点时，判断当前节点索引值是否小于上一个节点的最远距离
         + 小于，则说明，上一个节点跳不到当前节点，更别说最后一个目标节点了，直接返回false，不可达
         + 大于，则上一个节点至少可以跳到当前节点但不一定能跳到最后一个节点
           + 更新上一节点最远距离
           + i++
        + 循环到n后，依旧没有碰到false，说明跳到最后了，起跳点至少可以跳到末尾，甚至大于它的距离 
```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
    var canJumpMax = 0;
    var len = nums.length;
    for(var i = 0;i<len;i++){
        if(i > canJumpMax){
         return false;   
        }
        canJumpMax = Math.max(canJumpMax,i+nums[i]);
    }
    return true;
};
```
+ 优化+1
  + 由以上解法可知
    + 只要遍历过程中碰到一种最远跳跳距离大于等于末尾位置索引值的情况，就可以退出循环了，直接返回true
    + 此种情况不必像上述一样，去遍历整个循环，直到n时，才返回true，没遍历到末尾说明false
    + 也不必像解法四一样，非要逼近最左边的节点，即起跳点，没到起跳点说明false
      + 循环过程中，找到可以一跳终点的情况就返回，即鲤鱼跳龙门！ 
```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canJump = function(nums) {
    var canJumpMax = 0;
    var len = nums.length;
    for(var i = 0;i<len;i++){
        if(i > canJumpMax){
         return false;
        }
        canJumpMax = Math.max(canJumpMax,i+nums[i]);
        if(canJumpMax >= len-1){
            return true;
        }
    }
};
```