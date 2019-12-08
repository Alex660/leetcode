# 最长回文子串（中等）
# 题目描述
![截屏2019-10-20上午9.31.37.png](https://pic.leetcode-cn.com/c7197f29f856fe74c25a1cc1536097994ef981a0f8329b487256b55225c1a7c8-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.31.37.png)
# 题目地址
<https://leetcode-cn.com/problems/longest-palindromic-substring/solution/>
#### 解法一：暴力枚举法 
+ 时间复杂度O(n^3) 
+ 空间复杂度O(1)
```javascript
function isPalindrome(str) {
    var len  = str.length
    var middle = parseInt(len/2)
    for(var i = 0;i<middle;i++){
        if(str[i]!=str[len-i-1]){
            return false
        }
    }
    return true
}
var ans = '';
var max = 0;
var len = s.length
for(var i = 0;i<len;i++){
    for(var r = i+1;r<=len;r++){
        var tmpStr = s.substring(i,r)
        if(isPalindrome(tmpStr) && tmpStr.length > max){
            ans = s.substring(i,r)
            max = tmpStr.length;
        }
    }
}
return ans;
```
#### 解法二：动态规划 - A
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    if(!s || s.length < 2){
        return s;
    }
    var s_f = s.split('').reverse().join('');
    var resultStr = s[0];
    var maxLen = 1;
    var tmpLen = 1;
    var maxStrIndex = 0;
    var len = s.length;
    //判断字符串是否回文
    function isPalinerome(i,r){
        if(len - i - 1 == r -tmpLen + 1){
            return true
        }
        return false;
    }
    //初始化二维数组
    var len = s.length;
    var arr = new Array(len);
    for(var i = 0;i<len;i++){
        arr[i] = [];
        for(var r = 0;r<len;r++){
            arr[i][r] = 0
        }
    }
    for(var i = 0;i<len;i++){
        for(var r=0;r<len;r++){
            if(s[i] == s_f[r]){
                if(i==0 || r==0){
                    arr[i][r] = 1
                }else{
                    arr[i][r] = arr[i-1][r-1] + 1
                    tmpLen = arr[i][r]
                }
                if(tmpLen > maxLen && isPalinerome(i,r)){
                    maxStrIndex = r;
                    maxLen = tmpLen;
                    resultStr =  s.substring(i-tmpLen+1,i+1);
                }
            }
        }
    }
    return resultStr;
};
```
#### 解法三：动态规划 - B
+ 状态定义
  + dp[i,j]：字符串s从索引i到j的子串是否是回文串
    + true： s[i,j] 是回文串
    + false：s[i,j] 不是回文串
+ 转移方程
  + **dp[i][j] = dp[i+1][j-1] && s[i] == s[j]**
    + s[i] == s[j]：说明当前中心可以继续扩张，进而有可能扩大回文串的长度
    + dp[i+1][j-1]：true
      + 说明s[i,j]的**子串s[i+1][j-1]**也是回文串
      + 说明，i是从最大值开始遍历的，j是从最小值开始遍历的
    + 特殊情况
      + j - i < 2：意即子串是一个长度为0或1的回文串
  + 总结
    + **dp[i][j] = s[i] == s[j] && ( dp[i+1][j-1] || j - i < 2)**
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    let n = s.length;
    let res = '';
    let dp = Array.from(new Array(n),() => new Array(n).fill(0));
    for(let i = n-1;i >= 0;i--){
        for(let j = i;j < n;j++){
            dp[i][j] = s[i] == s[j] && (j - i < 2 || dp[i+1][j-1]);
            if(dp[i][j] && j - i +1 > res.length){
                res = s.substring(i,j+1);
            }
        }
    }
    return res;
};
```
#### 解法四：Manacher算法
+ 时间复杂度 O（n)
+ 举个栗子
	```javascript
	i	 0  1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21  22  
	T[i]  #  b  #  a  #  b  #  c  #  b  #   a   #   b   #   c   #   b   #   a   #   !=b  ?
	p[i]  0  1  0  3  0  1  0  7  0  1  0   9
	```
+ 思路简析
	+ 假设遍历到 i==11 时，计算得出p[11] = 9 即对称半径为 p[i] = 9;
	+ 则设 p[11]为对称中心点 即 c = 11 ,
	+ 右边边界点横坐标/数组下标为 
		+ c+p[11] = 11 + 9 = 20 = R = mx
	+ 遍历到 i==12 ，已知p[11] = 9 && 为中心点C ，
	+ 对称半径为 9 && 由中心对称特性 可知
		+ p[12] == p[10] 
		+ 归纳对称点坐标公式为 
		+ j == 2*c - i && j-p[j] > 2*c-mx => p[j] == p[i] 
		+ 从而求的动态p[i]值【动态半径】
	+ 由示例 
	+ 右边下标减去最新的i 
	+ 要想p[i]的关于c的对称点p[j]相等
	+ 且由对称性和p[j]代表半径的意思可知 
		+ mx-i的长度必须大于p[j]的对称半径 
	+ 否则 p[i]过界就不能直接取当前对称点对称半径范围内对称点的半径值 
		+ 而必须用中心扩展法去一一计数算得p[i]的回文半径/对称半径
	+ mx-i > p[j] =>  mx + j  - 2*c > p[j] => j - p[j] > 2*c - mx
		+ i<R
		+ p[i] = Math.min(mx-i,p[j]) = p[i] = Math.min(mx-i,p(2*c-i))
	+ 临界点：
    	+ i>R
    		+ 此时p[i]>mx-i  此时中心扩展一步一步求解
    	+ i=R【右边界】
    		+ 同上
    	+ i=0【左边界】
    		+ 同上
	+ 中心扩展法：
		+ 【用来访问R右边的数用来扩展，上去访问过的节点不会再进入while 因此时间复杂度还是O(n)】
		```javascript
		var left = i-p[i]-1
		var right = i+p[i]+1
		while(left<=0 && right<=T.length && p[left] == p[right]){
			p[i]++
		}
		p[] == p[i+p[i]+1]  => p[i]++
		```
	+ 当p[i]+i>R 时，更新 R 和 c => c = i && R = i + p[i]
		+ 求得所有回文串半径长度的数组p，遍历得出最大的一个 获得下标 i
		+ (i - p[i])/2 == 原字符串中回文串开始下标 = start
		+ s.substring(start,start+p[i])
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    if(!s || s.length < 2){
        return s;
    }
    var s_f = '#'+s.split('').join('#')+'#';
    let c = 0,R = 0;
    var len = s.length;
    var t_len = s_f.length;
    var maxLen = 0;
    var maxIndex = 0;
    var originIndex = 0;
    var p = new Array(t_len);
    p[0] = 0;
    for(var i = 1;i<t_len-1;i++){
        var j = 2*c-i;
        if(i<R){
            p[i] = Math.min(p[j],R-i)
        }else{
            p[i] = 0;
        }
        var left = i-p[i]-1;
        var right = i+p[i]+1;
        while(left>=0 && right<t_len && s_f[left]==s_f[right]){
            left--;
            right++;
            p[i]++;
        }
        if(i+p[i]>R){
            c = i;
            R = i+p[i];
        }
        if(p[i]>maxLen){
            maxLen = p[i];
            maxIndex = i;
            originIndex = parseInt((i-p[i])/2)
        }
    } 
    return s.substring(originIndex,originIndex + maxLen);
};
```
#### 解法五：中心扩展法
+ 思路
  + 回文串一定是对称的
    + 每次选择一个中心，进行中心向两边扩展比较左右字符是否相等
    + 中心点的选取有两种
      + aba，中心点是b
      + aa，中心点是两个a之间
  + 所以共有两种组合可能
    + left：i，right：i
    + left：i，right：i+1
  + 图解
    + ![截屏2019-12-06上午7.54.28.png](https://pic.leetcode-cn.com/533a8538f42e6a6495b87d0c054224fdaa2e6da1cd9158f3e9042894137961fc-%E6%88%AA%E5%B1%8F2019-12-06%E4%B8%8A%E5%8D%887.54.28.png)
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    if(!s || s.length < 2){
        return s;
    }
    let start = 0,end = 0;
    let n = s.length;
    // 中心扩展法
    let centerExpend = (left,right) => {
        while(left >= 0 && right < n && s[left] == s[right]){
            left--;
            right++;
        }
        return right - left - 1;
    }
    for(let i = 0;i < n;i++){
        let len1 = centerExpend(i,i);
        let len2 = centerExpend(i,i+1);
        // 两种组合取最大回文串的长度
        let maxLen = Math.max(len1,len2);
        if(maxLen > end - start){
            // 更新最大回文串的首尾字符索引
            start = i - ((maxLen - 1) >> 1);
            end = i + (maxLen >> 1);
        }
    }
    return s.substring(start,end+1);
};
```