# 整数转罗马数字（中等）
# 题目描述
  
![截屏2019-10-20上午9.22.36.png](https://pic.leetcode-cn.com/c7ea3e9207c77c0f7c33a364111e69556c30ce04480e4f8e6743b6bcc82756cd-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.22.36.png)
# 题目地址
<https://leetcode-cn.com/problems/integer-to-roman/solution/>

> 给定一个整数，将其转为罗马数字，输入数字在1到3999范围内。  
> 已知：罗马数字包含以下七种字符：I,V,X,L,C,D,M
```javascript
//转换表为：
    字符     数值     进位
     I       1       个位值为1
     V       5       个位值为5
     X       10      十位值为1
     L       50      十位值为5
     C       100     百位值为1
     D       500     百位值为5
     M       1000    千位值为1
```
#### 由罗马数字表示规则可知
+ 一般情况下 数字组成：大的数字位+小的数字位 ---- <1>
    + 例如：6：VI 7:VII 8:VIII
    + 可知：数值大小 == 大的数字位+小的数字位
    + 再如：1:I 2:II 3: III 
+ 但当罗马数字进位值（个、十、百、千）== 9 或 == 4 时，数字组成变为：小的数字+大的数字  ---- <2>
```javascript
// 例如：
      4:IV   9:IX 
     40:XL  90:XC  
    400:CD 900:CM
```
+ 可知：数值大小 == 大的数字位-小的数字位
    + 由上述可以推出罗马数字每个进位值1-9的表示法：          
    + 个位【1-9】：I、II、III、IV、V、VI、VII、VIII、IX   （对应阿拉伯数字 X%10）          
    + 十位【1-9】：X、XX、XXX、XL、L、LX、LXX、LXXX、XC   （对应阿拉伯数字 (X%100)/10）    
    + 百位【1-9】：C、CC、CCC、CD、D、DC、DCC、DCCC、CM   （对应阿拉伯数字 (X%1000)/100）  
    + 千位【1-9】：M、MM、MMM                           （对应阿拉伯数字 X/1000）        

+ 由以上可知一个阿拉伯数字 => 罗马数字
+ num => num/1000 + (num%1000)/100 +(num%100)/10 + num%10

#### 代码
```javascript
/**
 * @param {number} num
 * @return {string}
 */
var intToRoman = function(num) {
    var Q = ["", "M", "MM", "MMM"];
    var B = ["", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"];
    var S = ["", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"];
    var G = ["", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"];
    return Q[Math.floor(num/1000)] + B[Math.floor((num%1000)/100)] + S[Math.floor((num%100)/10)] + G[num%10];
};
```