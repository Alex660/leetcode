# 最长回文子串（中等）
# 题目描述
![截屏2019-10-20上午9.31.37.png](https://pic.leetcode-cn.com/c7197f29f856fe74c25a1cc1536097994ef981a0f8329b487256b55225c1a7c8-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.31.37.png)
# 题目地址
<https://leetcode-cn.com/problems/longest-palindromic-substring/solution/>
# 解法三
![截屏2019-10-14下午4.58.36.png](https://pic.leetcode-cn.com/ee655184c35fab8aed6aaba85ae015d1eff262cc72f3f050f29cd0a3c5c2ef16-%E6%88%AA%E5%B1%8F2019-10-14%E4%B8%8B%E5%8D%884.58.36.png)
>参考文章：  
<https://zhuanlan.zhihu.com/p/67559846>
	
#### 解法一：暴力枚举法 
+ 时间复杂度O(n的立方) 空间复杂度O(1)【没有额外地址存储&&常数个变量】
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
#### 解法二：动态规划
+ 求一个字符串的最长回文字符串 
+  => 字符串和自身反转后的字符串求最长公共子串 && 去掉非回文公共子串【只保留符合的字符串】 
+  => 字符串和反转字符串 分别置于横、纵坐标 通过网格（二维数组）一一比较字符串【每一个字串就是一个子问题】
+  => 当前网格值即当前公共字串的长度等于上一个公共字串的长度+1【横坐标 == 纵坐标】
+  => 通过最长字串的长度len 和 结束位置end
+  => [end+1-len,end] 左右闭合空间 => s.substring(i-len+1,i+1) == 公共最长字串
+  => 得出最长公共字串
+  => S='aacdecaa' 和 S='aacedcaa'
+  => 最长公共字串为aac 不是回文串 <= 'caa'为'acc'的反向副本 <= 反向字符串的原始索引与字符串的正常索引不一致 && 只需检查首或尾的索引是否相同
+  => 公共字符串反向字符串末尾字符的原始索引 == arr.length-i-1 && 正向公共字符串首字符的索引推导 == j-公共子串长度+1
+  => 两者相等 ? 回文字符串 : 反向副本

+  1、求最长公共字串 
+  2、1中插入回文串判断

+  空间复杂度：【两层遍历】O（n^2）
+  时间复杂度：【二维数组】O（n^2）
```javascript
//    -----arr[i][j]-----

//      i 0 1 2 3 4 5 6 7
//    j   a a c d e c a a 
//    0 a 1 1 0 0 0 0 1 1
//    1 a 1 2 0 0 0 0 1 2 
//    2 c 0 0 3 0 0 1 0 0
//    3 e 0 0 0 0 1 0 0 0
//    4 d 0 0 0 1 0 0 0 0
//    5 c 0 0 1 0 0 1 0 0
//    6 a 1 1 0 0 0 0 2 1
//    7 a 1 2 0 0 0 0 1 3

// 	 i 0 1 2 3 4 
//    j   b a b a d 
//    0 d 0 0 0 0 1
//    1 a 0 1 0 1 0 
//    2 b 1 0 2 0 0
//    3 a 0 2 0 3 0
//    4 b 1 0 3 0 0

//      i 0 1 2 3 4 5 6 7 8 9
//    j   a a a c d e c a a a
//    0 a 1 1 1 0 0 0 0 1 1 1
//    1 a 1 2 2 0 0 0 0 1 2 2
//    2 a 1 2 3 0 0 0 0 1 2 3 
//    3 c 0 0 0 4 0 0 1 0 0 0
//    4 e 0 0 0 0 0 1 0 0 0 0
//    5 d 0 0 0 0 1 0 0 0 0 0
//    6 c 0 0 0 1 0 0 1 0 0 0
//    7 a 1 1 1 0 0 0 0 2 1 1
//    8 a 1 2 2 0 0 0 0 1 3 2
//    9 a 1 2 3 0 0 0 0 1 2 4
```
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
	if(!s || s.length == 0){
		return s;
	}else if(s.length == 1){
        return s[0];
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
			console.log('tmpLen',tmpLen)
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

#### 解法三：Manacher算法
+ 时间复杂度 O（n） 线性

+ str: babcbabcba len = 10 
    + =>T:   #b#a#b#c#b#a#b#c#b#a#  len# = 21

    + => 存储字符中对应字符的 对称半径（不含自身） => 设原字符串等长的数组p
```javascript
  i	  0  1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21  22  
T[i]  #  b  #  a  #  b  #  c  #  b  #   a   #   b   #   c   #   b   #   a   #   !=b  ?
p[i]  0  1  0  3  0  1  0  7  0  1  0   9
```
+ 假设遍历到 i==11 时，计算得出p[11] = 9 即对称半径为 p[i] = 9;

+ 则设 p[11]为对称中心点 即 c = 11 ,右边边界点横坐标/数组下标为 c+p[11] = 11 + 9 = 20 = R = mx

+ 遍历到 i==12 ，已知p[11] = 9 && 为中心点C ，对称半径为 9 && 由中心对称特性 可知 =>

+ p[12] == p[10] => 归纳对称点坐标公式为 => j == 2*c - i && j-p[j] > 2*c-mx => p[j] == p[i] => 从而求的动态p[i]值【动态半径】

+ 由图可知 右边揭下标减去最新的i 要想p[i]的关于c的对称点p[j]相等 && 由对称性和p[j]代表半径的意思可知 mx-i的长度必须大于p[j]的对称半径 

+ 否则 p[i]过界就不能直接取当前对称点对称半径范围内对称点的半径值 而必须用中心扩展法去一一计数算得p[i]的回文半径/对称半径


+ mx-i > p[j] =>  mx + j  - 2*c > p[j] => j - p[j] > 2*c - mx

+ <=> i<R

+ <=> p[i] = Math.min(mx-i,p[j]) => p[i] = Math.min(mx-i,p(2*c-i))

+ 临界点：
	i>R
		此时p[i]>mx-i  此时中心扩展一步一步求解
	i=R【右边界】
		同上
	i=0【左边界】
		同上

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
	+ 即为所求 
```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    if(!s || s.length == 0){
		return s;
	}else if(s.length == 1){
		return s[0];
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