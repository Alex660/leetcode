#  Z 字形变换(中等)
# 题目描述
![截屏2019-10-20上午9.30.18.png](https://pic.leetcode-cn.com/bb5a2986f3ee8ecad497f8a1ba17f24dbb5be125717e95c84981797ef3cd711b-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.30.18.png)
![截屏2019-10-20上午9.30.28.png](https://pic.leetcode-cn.com/a8e8ae5ae7922300a8e574409adb0540f11b797a4bbadfd8a09ac36ee54235b5-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.30.28.png)
# 题目地址
<https://leetcode-cn.com/problems/zigzag-conversion/solution/>
```javascript
//Input => s = L E E T C O D E I S H I R I N G 
      //=> j = 3 ,len_s = 16
  i 0 1 2 3 4 5 6 7
j
0   L   C   I   R
1   E T O E S I I G
2   E   D   H   N
//Output =>    L C I R E T O E S I I G E D H N 
```
```javascript
//Input => s = L E E T C O D E I S H I R I N G 
	  //=> j = 4 ,len_s = 16
 i 0 1 2 3 4 5 6
j
0  L     D     R
1  E   O E   I I
2  E C   I H   N
3  T     S     G
//Output =>    L D R E O E I I E C I H N T S G 
```
```javascript
//Input => s = A B K D E I J P A K B C D E J O
	  //=> numRows = 5 ,len_s = 16
 i 0 1 2 3 4 5 6 7
j
0  A       A
1  B     P K     O
2  K   J   B   J
3  D I     C E
4  E       D
//Output =>    A A B P K O K J B J D I C E E D
```
```javascript
//Intput =>    A B C D
       //=> numRows = 2

  i 0 1 2
j   
0   A C
1   B D
//Output =>   A C B D
```
#### 解法:
+ 时间复杂度O（n）
+ 输入原str
+ 从上往下 && 从左往右 改造 原str
+ 从左往右 && 从上往下 输出 新str
```javascript
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */
var convert = function(s, numRows) {
    if(numRows ==1){
        return s;
    }
	var result = Array(numRows);
	var resultStr = '';
    var len = s.length;
	for(var i=0;i<numRows;i++){
		result[i]=[];
	}
	var j = 0;
	var needDown = true;
	for(var i = 0;i<len;i++){
		result[j].push(s[i])
		if(needDown){
			j++;
		}else{
			j--;
		}
		if(j>=numRows){
			j-=2;
			j==0 ? needDown=true : needDown = false;
		}else if(j==0){
			needDown = true;
		}
	}
	// console.log(result)
	for(var i=0;i<numRows;i++){
		resultStr+=result[i].join('')
	}
	return resultStr;
};
```