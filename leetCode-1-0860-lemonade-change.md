# 柠檬水找零（简单）
# 题目描述
![截屏2019-10-31上午6.07.22.png](https://pic.leetcode-cn.com/766512384014bdbce1361a5e9ad4dc6179d23c9cf16f41732becb66887fc91a5-%E6%88%AA%E5%B1%8F2019-10-31%E4%B8%8A%E5%8D%886.07.22.png)
![截屏2019-10-31上午6.07.42.png](https://pic.leetcode-cn.com/94baba9b6812fa95c0400b536de17eafc1c3d5a0f9ba6b85a42a9499ff70eb72-%E6%88%AA%E5%B1%8F2019-10-31%E4%B8%8A%E5%8D%886.07.42.png)
# 题目地址
<https://leetcode-cn.com/problems/lemonade-change/>
#### 解法一：贪心选择
+ 题意每张账单只能是5、10、20
+ 柠檬水均是5元一份
+ 店家自己没有零钱
+ 解法与思路
  + 当收到20时
    + 优先匹配店家手里的一张10和一张5，如有返回true
      + 店家手里的10-- 
      + 店家手里的5-- 
    + 如没有重新匹配3张15，如有返回true
      + 店家手里的5-=3 
    + 如都没有返回false
  + 当收到10时
    + 优先匹配一张5如有返回true
      + 店家手里的5--，10++ 
    + 如没有返回false
  + 当收到5时
    + 店家手里的5++  
```javascript
/**
 * @param {number[]} bills
 * @return {boolean}
 */
var lemonadeChange = function(bills) {
    var five = 0;
    var ten = 0;
    var len = bills.length;
    for(var i = 0;i<len;i++){
        if(bills[i] == 5){
            five++;
        }else if(bills[i] == 10){
            if(five == 0){
                return false
            };
            five--;
            ten++;
        }else if(bills[i] == 20){
            if(ten > 0 && five > 0){
                ten--;
                five--;
            }else if(five >= 3){
                five -= 3;
            }else{
                return false;
            }
        }
    }
    return true;
};
```