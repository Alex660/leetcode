# Nim 游戏（简单）
# 题目描述
![截屏2020-05-05 上午10.03.51.png](https://pic.leetcode-cn.com/f0697b418b982e1929ecb0a674b04e064b558152298999fadd2eafd7fcac424f-%E6%88%AA%E5%B1%8F2020-05-05%20%E4%B8%8A%E5%8D%8810.03.51.png)
# 题目地址
<https://leetcode-cn.com/problems/nim-game/>
#### 解法：
+ 博弈论
```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var canWinNim = function(n) {
    // 或 return (n & 3) !== 0
    return n % 4 !== 0
};
```