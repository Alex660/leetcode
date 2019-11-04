# 整数反转(简单)
# 题目描述
![截屏2019-10-20上午9.29.20.png](https://pic.leetcode-cn.com/3680ad7e18447f982033da6cc280e19e39d3be0a82c9b6e4078e6b71ba274dc0-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.29.20.png)
# 题目地址
<https://leetcode-cn.com/problems/reverse-integer/solution/>

> 反转一个整数s 要去新整数范围在 [-2^31,2^31-1]   
> 例如：123 => 321 -124 => 421

+ 假设要求范围在 [19,63]

+ 反转整数等于y = y*10 +pop

+ 如果 y>63 || y<19 就代表如题 溢出 =>
	+ y>63
		+ y*10 > 63/10 && pop 存在 则一定溢出 <=  y*10 > maxValue/10
		+ y*10 == maxValue【除去个位】 && pop > maxValue%10 【个位】

	+ y<19
		+ y*10 < 19/10 && pop 存在 则更小了就溢出了 <= y*10 < minValue/10
		+ y*10 == minValue【除去个位】 && pop < minValue%10 【个位】

	+ y = y/10 每次自动去掉最后一位 => 更新 拼接的 pop = y%10


+ 分析：
	+ 求倒数位置开始数字的每一位 => x%10 => pop = x%10
	+ 从倒数位置开始去掉最后一位 => x/10

+ 从原整数最后一位数字开始取每一位pop，从上次看是*10并动态拼接起来位数成反转的部分或整个整数s_f && 
+ 每一步判断是否超出范围 ？ 溢出false : 返回拼接后的反转字符串
#### 解法一
```javascript
function reverse(x){
	var ans = 0;
	var pop = 0;
	var maxValue = Math.pow(2,31)-1;
	var minValue = -Math.pow(2,31);
	var maxValuePre = parseInt(maxValue/10);
	var maxValueEnd = maxValue%10;
	var minValuePre = parseInt(minValue/10);
	var minValueEnd = minValue%10;
	while(x!=0){
		pop = x %10
		if(ans > maxValuePre &&  pop){
			return false;
		}else if(ans == maxValuePre && pop > maxValueEnd){
			return false;
		}else if(ans < minValuePre && pop){
			return false;
		}else if(ans == minValuePre && pop < minValueEnd){
			return false;
		}else{
			x = parseInt(x/10)
			ans = ans * 10 + pop
		}
	}
	return ans;
}
```