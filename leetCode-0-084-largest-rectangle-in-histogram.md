# 柱状图中最大的矩形（困难）
# 题目描述
![截屏2019-11-09上午9.51.18.png](https://pic.leetcode-cn.com/cff071b6441610f4de44d1bfc91e2c593c0a9b8d29abf02a18915d57d51c7b2a-%E6%88%AA%E5%B1%8F2019-11-09%E4%B8%8A%E5%8D%889.51.18.png)
![截屏2019-11-09上午9.51.26.png](https://pic.leetcode-cn.com/02b9117c63abd2f1f5ec28fbab2cf1021cba42f3ab6631250f55082c9cc9cbc3-%E6%88%AA%E5%B1%8F2019-11-09%E4%B8%8A%E5%8D%889.51.26.png)
# 题目地址
<https://leetcode-cn.com/problems/largest-rectangle-in-histogram/>
#### 解法一：暴力枚举
+ 时间复杂度：O(n^3)
+ 空间复杂度：O(1)
+ 相邻两柱子间可构成的最大矩形面积为
  + 两柱间的宽度 * 其中一根矮柱子的高度
+ 求多个柱子间可构成的最大矩形面积？
  + 分解成两两柱子对求面积
  + 例如[1,4,3]
  + maxArea = Math.max(Area(1,4),Area(1,3),Area(4,3))
+ 执行时间超时
```javascript
/**
 * @param {number[]} heights
 * @return {number}
 */
var largestRectangleArea = function(heights) {
    var maxArea = 0;
    for(var i = 0;i < heights.length;i++){
        for(var j = i;j < heights.length;j++){
            var minHeight = Number.MAX_SAFE_INTEGER;
            for(var k = i;k <= j;k++){
                minHeight = Math.min(minHeight,heights[k]);
            }
            maxArea = Math.max(maxArea ,minHeight* (j-i+1));
        }
    }
    return maxArea;
};
```
#### 解法二：暴力优化版
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(1)
+ 相对于解法一
  + 我们优化了第三层循环处求一组柱子中最矮柱子高度的逻辑
  + 其实一轮遍历的过程中
    + 0～i~n时，第i+1的最小柱子高度为前一轮的最小柱子高度
    + 例如
      + [3,2,6,7,4] 
      + i: 3
      + k: [2,6,7,4]
        + minheight:2
      + i:3
      + k:[6,7,4]
        + minheight:2
        + 即直接沿用了上一个k遍历位置时的最小高度
        + 所以无需遍历i~j层的最小高度
        + 0~i层的最小高度如果在i~j~n中最小，是可以通用的
        + 因为解法一k层循环内部的比较中的maxArea也是存储了0~i层的最小高度而已
    + 每前进一位，就比较更新最小高度
+ 执行时间不超时  
```javascript
/**
 * @param {number[]} heights
 * @return {number}
 */
var largestRectangleArea = function(heights) {
    var maxArea = 0;
    for(var i = 0;i < heights.length;i++){
        var minHeight = Number.MAX_SAFE_INTEGER;
        for(var j = i;j < heights.length;j++){
            minHeight = Math.min(minHeight,heights[j]);
            maxArea = Math.max(maxArea ,minHeight* (j-i+1));
        }
    }
    return maxArea;
};
```
#### 解法三：分治法
+ 时间复杂度：O(nlogn),最坏情况下数组有序为O(n^2)
+ 空间复杂度：O(n) 
+ 分析
  + 由题意和以上解法可知
    + 欲求最大矩形面积
      + 必先找到最矮的柱子，然后宽度向两边延伸极限
      + 宽度 * 此最矮柱子高度 = 即为所求
  + 既是分治，意即求最大问题分解后的子问题，合并或比较子问题的解，符合题意的即为所求
    1. 找到最矮的柱子，并尽可能向两边延伸，求出面积
    2. 对当前最矮柱子左边，重复第一步
    3. 对当前最矮柱子右边，重复第一步
    4. 合并前三步的面积，返回其中最大的面积值，即为所求 
```javascript
/**
 * @param {number[]} heights
 * @return {number}
 */
var largestRectangleArea = function(heights) {
    function calcArea(start,end){
        if(start > end){
            return 0;
        }
        var minIndex = start;
        for(var i = start;i <= end;i++){
            if(heights[minIndex] > heights[i]){
                minIndex = i;
            }
        }
        return Math.max(heights[minIndex] * (end - start + 1),Math.max(calcArea(start,minIndex-1),calcArea(minIndex+1,end)));
    }
    return calcArea(0,heights.length-1);
};
```
#### 解法四：化繁为简
+ 时间复杂度：O(n^2)
+ 空间复杂度：O(1)
+ 试想，当高度确定时，怎么最大的矩形面积？当然是宽度必须尽可能的大！
  + 结合柱状图的特征，当前高度确定柱子所在处，尽量向两边延伸，直到到达
    + 左边高度小于它的柱子，记录索引为：left_i
    + 右边高度小于它的柱子，记录索引为：right_i
  + 则最大宽度为：height(n) * (right_i - left_i - 1)
+ 由题意可知，最大面积矩形的高度只能出自n个柱子中的其中一个
  + 那直接将每个柱子的高度所能构成的最大面积全部求出来，取其中的最大值即为所求。
+ 当前柱子为高，其所能构成的最大面积是
  + maxArea =  heights[i] * (right_i - left_i - 1)
+ 那么问题来了
  + 怎么在程序中找left_i 和 right_i？
    + 挨个儿搜 = 暴力枚举 
+ 结果
  + 虽然未超时，但是用时将近900多毫秒  
```javascript
/**
 * @param {number[]} heights
 * @return {number}
 */
var largestRectangleArea = function(heights) {
    var maxArea = 0;
    var len = heights.length;
    for(var i = 0;i < len;i++){
        var left_i = i;
        var right_i = i;
        while(left_i >= 0 && heights[left_i] >= heights[i]){
            left_i--;
        }
        while(right_i < len && heights[right_i] >= heights[i]){
            right_i++;
        }
        maxArea = Math.max(maxArea,(right_i - left_i - 1) * heights[i])
    }
    return maxArea;
};
``` 
#### 解法五：解法四优化版
+ 此解法优化性能颇高相对于解法四的900多毫秒直接提升了将近16倍
  ![截屏2019-11-11下午6.29.16.png](https://pic.leetcode-cn.com/567e95f8ca4eb0c42bb13d14f3807a285616227de021f80817596918e4d1961c-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8B%E5%8D%886.29.16.png)
+ 解法四时间复杂度居高不下的原因在于每次都要寻找当前柱子的左右单调性变化位置索引
  + 而寻找的过程是O(n^2)
  + 所以此解法的目的亦是弥补此缺陷 
+ 其实寻找当前柱子的left_i和right_i没必要向两边遍历整个数组
+ 想下解法二优化版的核心思想是什么？
  + 同一轮遍历中后面柱状图的最小高度取决于前面柱状图的高度，所以解法四舍弃了寻找遍历当前柱子周围的最小柱子高度操作
  + 此题是一样的道理
    + 高度值对应索引，如果找到了最小高度，就是找到了最小高度对应的索引值，也就找到了left_i和right_i
    + 此处的最小高度值相当于小于当前遍历柱子两边第一个柱子的索引对应值
  + 当我们找当前柱子height[i]左边第一个小于它的值对应的索引i时，
    + 如果heights[i-1] >= heights[i]
      + 其实就是在找左边第一个小于heights[i-1]一样，重复上述过程
      + dp[n-1] = dp[n-2]
      + 有点类似动态规划的味道在里面
    + 如果heights[i-1] < heights[i]
      + 说明找到了left_i = i-1
+ 因此，直接定义两个当前柱子的left_i[i]和right_i[i]的数组 
  + 而当heights[i-1] >= heights[i]时
    + left_i[i] = left_i[i-1]
      + 而当heights[i-2] >= heights[i-1]时  
        + left_i[i-1] = left_i[i-2] 
          + 而当heights[i-2] >= heights[i-1]时 
            + left_i[i-2] = left_i[i-3]
              + ...
    + ...
  + right_i[i] 同理
  + 当heights[i+1] >= heights[i]时
    + right_i[i] = right_i[i+1]
      + 当heights[i+2] >= heights[i+1]时
        + right_i[i+1] = right_i[i+2]
          + 当heights[i+2] >= heights[i+3]时
            + right_i[i+2] = right_i[i+3]
              + ...
  + 简化为
    + 遍历到i
      + i--
        + left_i
        + var tmp = i-1
        + 当heights[tmp] >= heights[i] && ...时
          + tmp = left_i[tmp]，并重复比较赋值动作
      + i++
        + right_i
        + var tmp = i+1
        + 当heights[tmp] >= heights[i] && ...时
          + tmp = right_i[tmp]，并重复比较赋值动作
+ 解题技巧
  + left_i首部增加0元素
  + right_i尾部增加heights.length元素
  + 缘由
    + 当数组遍历到左右边界仍没有找到第一个小于它的两个值时，面积计算操作将不会执行
    + 因此如果不另外处理可能会漏掉最优解
    + 左边增加一个最小值-1，因为柱状图高度最小也是大于0的正数
    + 右边增加heights数组的长度，即使遍历到数组末尾，i的值也会自动增加到heights.length，这样求宽度时，就不会漏掉最后一根柱子
```javascript
/**
 * @param {number[]} heights
 * @return {number}
 */
var largestRectangleArea = function(heights) {
    var maxArea = 0;
    var len = heights.length;
    var left_i = new Array(len);
    left_i[0] = -1;
    var right_i = new Array(len);
    right_i[len-1] = len;
    for(var i = 1;i < len;i++){
        var tmp = i-1;
        while(tmp >= 0 && heights[tmp] >= heights[i]){
            tmp = left_i[tmp];
        }
        left_i[i] = tmp;
    }
    for(var i = len - 2;i >= 0;i--){
        var tmp = i+1;
        while(tmp < len && heights[tmp] >= heights[i]){
            tmp = right_i[tmp];
        }
        right_i[i] = tmp;
    }
    for(var i = 0;i < len;i++){
        maxArea = Math.max(maxArea,(right_i[i] - left_i[i] - 1) * heights[i])
    }
    return maxArea;
};
```
#### 解法六：栈解
+ 解法四的升华版
  + 解法四时间复复杂度高的原因在于循环寻找left_i 和 right_i的过程中
  + 那么此解就在于优化此项缺陷 
+ 时间复杂度：O(n)
  + n个数字各出入栈一次 
+ 空间复杂度：O(n)
+ 核心思想
  + 同解法四
    + 宽度变 && 高度相对不变 => 求最大面积
      + 高度为动态更新的单调递增栈不断出栈的值
    + 高度相对不变即为栈顶元素时，在n个且是单调递增的柱状图中，当宽度是更新中栈的长度时，当前n个柱状图能构成的矩形面积才能最大
  + 区别
    + 维护单调递增栈可以很容易的求出left_i和right_i的距离即宽度是多少
      + 即直接相减再减1即可  
+ 总结
  + 在相邻两根单调递增的柱状图中，求所能构成矩形的最大面积时，
     + **宽度**为：2，
     + **高度**高度为：左边最矮的那棵 
  + 在一段不止两根的单调递增的柱状图中，求所能构成矩形的最大面积时，循环出栈求n个递增柱状图的最大面积
     + **宽度**为：单调递增栈的区间长度
     + **高度**相对固定为：不断更新的单调递增的栈顶元素的左边元素高度值
  + 直到heights[j] < heights[i]时，停止出栈
  + 求以上各面积的最大值
  + 也可以每次维护一个最大值
    + maxArea = Math.max(maxArea,maxArea(n))  
+ 思路
   + ***栈、单调递增**
   + 动态维护一个单调递增序列，用栈
     + 遍历heights数组，i从0到heights.length-1
       + 不断将柱子的索引值压栈，直到下一个序号对应的柱子高度 < 当前遍历序号的柱子高度值为止；此时，heights[i-1] > heights[i]
       + 然后
        1. 弹处位于栈顶的heights[i-1]元素对应的序号值i-1，此处设为j = i-1；栈设为stack，存储一段单调递增的序号数组
           1. 将heights[j] 即栈顶元素作为高
           2. 宽 == 当前柱子所在序号 - 作为高的柱子的左边柱子的序号 - 1
              1. 每次更新最大面积 maxarea = Max.max(maxarea,宽*高) ，即 
                 1. maxarea = Math.max(area,heights[stack[stack.lengths-1]] * (i - stack[stack.length-2]) - 1)
                 2. 或者maxarea = Math.max(area,heights[stack[stack.pop()]] * (i - stack[stack.length-1]) - 1)
           3. 当且仅当heights[j] > heights[i]时，循环出栈动作
        2. 随着栈中单调递增元素的减少
           1. 直到heights[j] <= heights[i]时，停止出栈动作；并继续遍历数组，继续入栈
           2. 直到数组遍历完
              1. 栈中没有剩余数据，直接返回maxarea
              2. 还有数据，继续按照上述核心逻辑求最大面积
     + 核心逻辑理解
        + 求最大面积
        ![截屏2019-11-11下午2.30.17.png](https://pic.leetcode-cn.com/06f2bc108723205b547f3bcf32fd6612c525f6de6a805b008de3c5799c1693db-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8B%E5%8D%882.30.17.png)
          + 如图所示
          + 遍历 Heights [...,...,2,4,5,6,4,....]
          + 维护一个单调递增栈，存储元素对应索引：Stack [4,5,6,7]
            + 此时i = 8、j = i-1=7，当且仅当heights[j] > heights[i] 时
              + **从右向左**循环出栈，开始求每个阶段能组合成的面积，并维护一个最大值maxArea
                + 1、索引6到8之间矩形的最大面积，出栈索引为7高度为6的柱子是固定高度
                  + 只有一根柱子：索引7
                  + maxArea1 = (8-6-1) * 6 = 6
                  + 直观上：一根柱子所能构成的最大矩形面积是自己
                + 2、索引5到8之间，出栈索引为6高度为5的柱子是固定高度
                  + maxArea2 = (8-5-1) * 5 = 10
                  + 直观上：索引6和7两跟柱子所能构成的最大矩形面积是多少呢？
                    + 1、确定了两根柱子，那么宽度就确定了是 2
                    + 2、当高度确定时，一段递增区域的的柱状图所能构成的矩形最大面积的宽度必然为区间长度
                + 3、索引4到8之间，出栈索引为5高度为4的柱子是固定高度
                  + maxArea3 = (8-4-1) * 4 = 12 
                + 4、此时栈顶元素为2，小于heghts[8],所以退出循环
                  + 重新进入遍历Height数组，捕捉单调元素入栈的过程中
                    + 碰到单调递减边界节点时，则重新出栈寻找最大面积
                    + 上三步的最大矩形面积是Math.max(maxArea1,maxArea2,maxArea3)
            + 边界处理
              ![截屏2019-11-11下午2.48.34.png](https://pic.leetcode-cn.com/9398d83ace9b842d9b9bdf91c4ef81248a66b456416aca9d6165a51950e52d77-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8B%E5%8D%882.48.34.png)  
              + 如图所示
                + 当遍历到i = 3时
                  + 当从右到左出栈循环到i=0时，即求索引（0-1 = -1）的柱子到索引为3的柱子之间的最大面积？
                    + 最大面积为 （3-（0-1）-1）* 2 = 6
                    + 因为索引下标最小为0，此处设0左边的索引为-1
                      + 直观上可知，高度2确定，索引0，1，2柱子所能构成的最大矩形面积即为2*3 = 6，算法正确！所以设为-1符合正解
                      + 因此栈初始化为[-1]
              ![截屏2019-11-11下午2.58.34.png](https://pic.leetcode-cn.com/e195e992b5caa18759ffc1b09e91e76fbb1373765705936f6862bc162b6b2a45-%E6%88%AA%E5%B1%8F2019-11-11%E4%B8%8B%E5%8D%882.58.34.png)
              + 如图所示
                + 当遍历结束，且栈还有数据时 
                  + 由以上可知，当前遍历**i**只是充当了一个**减法求宽**的角色，此时可以直接设置i = stack.length-1即可   
                  + 复用以上maxArea求解逻辑，算法正确！
            + 减法求宽
              + 宽 == 当前柱子所在序号 - 作为高的柱子的左边柱子的序号 - 1  
            + 遍历结束后
              + 若栈空，则直接返回maxArea
              + 若栈有剩余数据，则剩下的一定是单调递增序列
                + 复用上述求最大面积逻辑即可 
```javascript
/**
 * @param {number[]} heights
 * @return {number}
 */
var largestRectangleArea = function(heights) {
    var maxarea = 0;
    var stack = [-1];
    for(var i = 0;i < heights.length;i++){
        while(stack.length > 1 && heights[stack[stack.length-1]] >= heights[i]){
            maxarea = Math.max(maxarea,heights[stack.pop()] * (i - stack[stack.length-1] - 1));
        }
        stack.push(i);
    }
    while(stack.length > 1){
        maxarea = Math.max(maxarea,heights[stack.pop()] * (heights.length - stack[stack.length-1] - 1));
    }
    return maxarea;
};
```
+ 优化版
  + 同栈的初始值为-1可知，我们考虑了左边界的情况，但右边界在遍历中没有考虑
  + 而是在判断栈空时考虑，栈不为空，说明当栈有数据时，遇到了直到遍历到最后一个i时，也没有遇到小于栈顶元素顶值，即没有遇到破坏单调递增性质顶位置节点
  + 难点
    + 上述解法，首部加-1和再遍历一次剩余的栈着实不好理解
      + 首部加-1和是为了当栈的长度为2时
        >while(stack.length > 1 && heights[stack[stack.length-1]] >= heights[i]){  
            maxarea = Math.max(maxarea,heights[stack.pop()] * (i - stack[stack.length-1] - 1));  
            }
        + 上述代码，循环里面经过先出栈，栈就只剩下一个元素了，那唯一栈顶的左侧索引没有
            + 例如：i=1,j=0
            + i-1时就已经是0了，其实此时宽度为1，所以迎合之后的减法这里再减个-1，宽度就增加回首元素的宽度了 
            + 再例如：i=2,j=0
            + i-1-(0-1) = 2,同样宽度也不会漏掉了首柱子的宽度值
      + 再遍历一次剩余的栈是为了当遍历到数组末尾i栈依旧单调递增时：
        + 会出现heights[stack[stack.length-1]] == undefined 的情况，即right_i取不到的情况就无法继续算栈里剩余柱子的面积组合，所以就需要遍历剩下的栈
        > while(stack.length > 1){  
            maxarea = Math.max(maxarea,heights[stack.pop()] * (heights.length - stack[stack.length-1] - 1));  
          }
    + 解决方案
      + 索性就不考虑边界了，直接数组首尾各增加一个比所有柱状图高度都小的高度值
        + 如0，这样左边边界和右边界一定只能到0和height.length
        + 这样夹在中间的单调递增栈不可能会因边界left_i或者right_i不存在而使得面积无法继续计算从而栈剩余数据的情况出现
          + 这样栈一定会被清空
      + 和**解法五**中，索引数组首或尾注入边界值有异曲同工之妙。
  + 话不多说，就是干！  
```javascript
/**
 * @param {number[]} heights
 * @return {number}
 */
var largestRectangleArea = function(heights) {
    var maxarea = 0;
    var stack = [];
    heights.push(0);
    heights.unshift(0);
    for(var i = 0;i < heights.length;i++){
        while(stack.length > 0 && heights[stack[stack.length-1]] > heights[i]){
            maxarea = Math.max(maxarea,heights[stack.pop()] * (i - stack[stack.length-1] - 1));
        }
        stack.push(i);
    }
    return maxarea;
};
```   
#### 解法七：分治+线段树
[参考文献](https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/28941/segment-tree-solution-just-another-idea-onlogn-solution)