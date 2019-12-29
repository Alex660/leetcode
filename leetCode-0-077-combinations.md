# 组合（中等）
# 题目描述
![截屏2019-10-27上午7.23.21.png](https://pic.leetcode-cn.com/2ea572742904dfe9e0d7d7c2f0808afb6e4e3c66d31da62f94f166e5beba6683-%E6%88%AA%E5%B1%8F2019-10-27%E4%B8%8A%E5%8D%887.23.21.png)
# 题目地址
<https://leetcode-cn.com/problems/combinations/>
#### 回溯算法系列
+ [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/solution/39-zu-he-zong-he-by-alexer-660/)
+ [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/solution/40-zu-he-zong-he-ii-by-alexer-660/)
+ [46. 全排列](https://leetcode-cn.com/problems/permutations/solution/46-quan-pai-lie-by-alexer-660/)
+ [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/solution/47-quan-pai-lie-ii-by-alexer-660/)
+ [77. 组合](https://leetcode-cn.com/problems/combinations/solution/77-zu-he-by-alexer-660/)
+ [78. 子集](https://leetcode-cn.com/problems/subsets/solution/78-zi-ji-by-alexer-660/)
+ [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/solution/90-zi-ji-ii-by-alexer-660/)
#### 解法一：递归回溯
+ 从{1,2,3,...,n}中选择k个数，输出所有组合
  + 先选择一个数字作为临时组合，
    + 然后再进入递归和上一个组合进行组合，直到终止条件则为子集，收集起来
    + 进入递归下一个数字组合前，要重置当前组合状态，什么意思呢？
      + 例如n = 6、k = 3时
        + [1],k=1
           + 从头递归下一轮
             + [1]+[2],k=2,不符合子集
               + 从头递归下一轮,i++
                 + [1]+[2]+[3],k=3,符合子集，收入,退出当前递归
               + 重置,pop并继续当前循环  
                 + [1]+[3],k=2,不符合子集
                   + 从头递归下一轮,i++
                     + [1]+[3]+[4],k=3,符合子集，收入，退出当前递归
                   + 重置,pop并继续当前循环
                     + [1]+[4],k=2,不符合子集
                       + 递归....
                         + 第i层递归直到i==k时，退出第i层递归
                       + 重置...
                         + 直到循环到达数组终点时停止遍历 
           + 重置，pop,继续当前循环
             + [2],k=1,不符合子集
               + 从头递归下一轮,i++
                + ...
               + 重置,pop并继续当前循环
                 + [3]，,k=1,不符合子集
                   + 递归....
                      + 第i层递归直到i==k时，退出第i层递归 
                   + 重置...
                      + 直到循环到达数组终点时停止遍历 

                        + ....
                        + ....   

```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function(n, k) {
    var result = [];
    var subresult = [];
    function combineSub(start,subresult){
        //terminator
        if(subresult.length == k){
            result.push(subresult.slice(0));
            return;
        }
        for(var i= start;i<=n;i++){
            subresult.push(i);
            combineSub(i+1,subresult);
            subresult.pop();            
        }   
    }
    combineSub(1,subresult);
    return result;
};
```
+ 优化版【参考大佬的】
  + for 循环 i 从 start 到 n 时
    + 当n=5,k=4时
      + 当为第n层第一轮组合时，即 subresult.length == 1 时
        + 还需要 4-1=3个数字
        + i == 4时，最多能把4和5加入到组合中去 而此时 subresult.length == 1+2 == 3 即最多有3个，达不到条件n=4个   
        + i == 3时，最多能把3,4,5加入到组合中去，刚好能符合n==4个
        + ....
        + 即 n-(k-len) + 1 == 3 = i ，i只需要循环到 i = 3即可
    + k - 当前组合的个数 = 还需要的数字个数 
```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function(n, k) {
    var result = [];
    var subresult = [];
    function combineSub(start,subresult){
        //terminator
        if(subresult.length == k){
            result.push(subresult.slice(0));
            return;
        }
        var len = subresult.length;
        for(var i= start;i<=n-(k-len)+1;i++){
            subresult.push(i);
            combineSub(i+1,subresult);
            subresult.pop();            
        }   
    }
    combineSub(1,subresult);
    return result;
};
```
#### 解法二：迭代回溯
+ [参考了其它语言大佬们的解法](https://leetcode.com/problems/combinations/discuss/26992/Short-Iterative-C%2B%2B-Answer-8ms)
  + 迭代是将上面回溯的思想改成符合迭代规范的代码格式，换汤不换药
  + 回溯几个主要的操作
    + 根据函数调用栈的执行顺序和for循环及外层递归调用自己的特点可知：
      + 回溯就是最后一轮i = n+1 的循环加上内层k次组合 
      + 总循环结束后 就回到上一轮i = n，
      + 加上内层K次组合 ，总循环 直到 i =0,subreuslt = []时；
    + 由上一条可知 当subresult的length == k 时，即达到了所需数字组合的数字 
      + 当前subresult的值数组即为结果中的一个子集
    + 每个for循环里面，进入递归，
      + 即是添加下一个数字作为基准组合重新进行一轮组合
  + 以上三条回溯特点即是写入迭代法中需要注意的必须模仿条件 因而才能以迭代模仿递归 
```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function(n, k) {
    var result = [];
    var subresult = [];
    // 初始化k个数字的临时组合各个元素为0
    // 便于++对应 n的1，2，3，4..n
    for(var r = 0;r<k;r++){
        subresult[r] = 0;
    }
    // n的总个数达不到k组合的个数 不可能实现
    if(n < k){
        return result;
    }
    // 迭代索引
    var i = 0;
    while(i >= 0){
        // 当前数字加一 对于回溯中 subresult push 一个新的i 即为上一个i+1
        subresult[i]++;
        // 当i循环到n时，此时 i=n+1 
        // i-- 对应回溯的函数调用执行栈的逆序出栈 即回到上一层的组合状态操作
        if(subresult[i] > n){
            i--;
        }
        // 索引从0开始 i == k-1 相等于 回溯i从1开始后i==k 的情况
        else if(i == k -1){
            result.push(subresult.slice(0));
        }
        // 
        else{
            // 对于回溯外层for循环的下一层i操作 即start
            ++i;
            // 相当于回溯的pop操作 去掉新加的值 退回上一层的值重新递归 此处为迭代
            subresult[i] = subresult[i-1];
        }
    }
    return result;
};
```
#### 解法三：递归合并
+ 从n个数字中选第k个，由此可知，我们把结果分为两种
  + 数字n被选择为结果集子集中的一个元素
    + 那么只需要从n-1个数字中选出k-1个
  + 数字n没有被选中
    + 剩下的就只能从n-1个数字中选出k个  
+ 抽象成公式为
  + C(n,k)=C(n-1,k-1)+C(n-1,k)
+ 因此题解就是把上面两种结果合并起来，即为所求 
+ 简单图示
  ![66.gif](https://pic.leetcode-cn.com/83d3cceec827ef12166d4fff1bae6f2d703e1f69bc70eee3d0cc682dd817e65b-66.gif)  
```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function(n, k) {
    var result = [];
    var subresult = [];
    // n==k/k==0 对于两种情况边界
	if( n==k || k == 0){
        var tmp = [];
		for(var i = 1;i<=k;i++){
			subresult.push(i);
        }
        // 构造如[[x]]的返回形式
        tmp.push(subresult);
		return tmp;
	}
	// n-1 里选 k-1 个
    var result = combine(n-1,k-1);
    // 组合[[x,y]]：第n个数被选择=>[[x,y,n]]
	result.map((arr) => arr.push(n));
	// n-1 里选 k 个
    var tmp = combine(n-1,k);
    // 第二种情况的结果打散装入返回数组里去
    // result:[[x,y]]、tmp[[x1,y1]]
    // ==> result:[[x,y],[x1,y1]]
    tmp.map((arr) => result.push(arr));
    // 返回每次成功组装的结果集，最后递归终点则返回所以可能组合
	return result;
};
```
#### 解法四：动态规划
+ 看懂了解法三，基本就是改写代码结构，改递归压栈为迭代自底向上的动态规划
```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function(n, k) {
    var dp = new Array(n+1);
    for(var ii = 0;ii<=n;ii++){
        dp[ii] = new Array(k+1);
        dp[ii][0] = [[]];
    }
    console.log(dp);
    // i：1~n
    for(var i = 1;i <= n;i++){
        //j：1～i/k
        for(var j = 1;j <= i && j <= k ;j++){
            dp[i][j] = [];
            // 从 i-1 个里选 j 个
            // 即从上题解法的：从 n-1 个里选 k个
            if(i > j){
                var tmpA = dp[i-1][j];
                for(var t = 0;t<tmpA.length;t++){
                    dp[i][j].push(tmpA[t]);
                }
            }
            // 从 i-1 个里选 j-1个
            var tmpB = dp[i-1][j-1];
            for(var z = 0;z < tmpB.length;z++){
                // 这里注意不能修改dp[i-1][j-1]的原数组元素，需要深拷贝
                var tmpC = [].concat(tmpB[z]);
                tmpC.push(i);
                dp[i][j].push(tmpC);
            }
        }
    }
    return dp[n][k];
};
```
#### 解法五：迭代组合
+ 分析
  + 在n个数里寻找k个数的组合，等价于
    + 设n=5(1,2,3,4,5),k=3 
    + 寻找1个数的所有可能组合，[1],[2],[3]
      + 遍历1个数的所有组合
        + 每次增加一个数组合成
          + [1] => [1,2],[1,3],[1,4]
          + [2] => [2,3],[2,4]
          + [3] => [3,4]
        + k = 2 ,result = [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
        + k < 3 ,不符合题意,继续组合一个数字
          + 每次增加一个数组合成
            + [1,2] => [1,2,3],[1,2,4],[1,2,5]
            + [1,3] => [1,3,4],[1,3,5]
            + [1,4] => [1,4,5]
            + [2,3] => [2,3,4],[2,3,5]
            + [2,4] => [2,4,5]
            + [3,4] => [3,4,5]
          + k = 3 ,result = [[1,2,3],[1,2,4],[1,2,5],[1,3,4],[1,3,5],[1,4,5],[2,3,4],[2,3,5],[2,4,5],[3,4,5]] 
          + k = 3,符合题意，返回result
    + 注意
      + 整体是升序组合数字，直到组合个数符合题意，归并返回
      + [4],[5] 不可能，由上一句和解法一优化版分析可知：4至少再往后加两个数字且只能加5，6才有可能符合题意，但 n <= 5 && k <= 3;
      + [1,5],[2,5],[3,5] 不可能，原因同上
      + k - 当前组合的个数 = 还需要的数字个数  
  + 图解
    ![迭代组合.png](https://pic.leetcode-cn.com/8160d47babe3ace4f8a5028ec8b81bb193bdcf02e418f7b976d25db6bec884de-%E8%BF%AD%E4%BB%A3%E7%BB%84%E5%90%88.png) 
```javascript
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
var combine = function(n, k) {
    if(n == 0 || k == 0 || k > n) return [];
    var res = [];
    // 个数为1的所以可能
    for(var z = 1;z <= n+1-k;z++){
        res.push([z]);
    }
    // 第一层循环，从 2 到 k，直到k符合题意
    for(var i = 2;i <= k;i++){
         var tmp = [];
        // 第二层循环，遍历之前的所有结果
        for(var r = 0;r <res.length;r++){
            // 第三层循环，继续扩展组合集合，每次增加一个数
            // 此处s <= n-(k-(i-1))+1类似解法一的优化版不必循环到n
            // k - 当前组合的个数 = 还需要的数字个数 
            // (k-(i-1)) == 还需要的数字个数，i-1因为第一个只有一个数字的所有组合已经确定
            // n - 还需要的数字个数 + 1  == 遍历的末尾索引值，+1是因为第n个数也可以取(如n=5：1，2，3，4，5)
            for(var s = res[r][res[r].length-1]+1;s <= n-(k-(i-1))+1;s++){
                var tmpB = [].concat(res[r]);
                tmpB.push(s);
                tmp.push(tmpB);
            }
        }
        res = tmp;
    }
    return res;
}
```