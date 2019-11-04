# 分发饼干（简单）
# 题目描述
![截屏2019-11-04下午2.45.57.png](https://pic.leetcode-cn.com/9a1932a49d20d2fb597400efc72c91ce27ced8a51aeb588f0a9ce39c1f791839-%E6%88%AA%E5%B1%8F2019-11-04%E4%B8%8B%E5%8D%882.45.57.png)
![截屏2019-11-04下午2.46.09.png](https://pic.leetcode-cn.com/3132fdcab883ed26ff9df055c769994fe9c80cd6843279c66c16d0dbbdd37888-%E6%88%AA%E5%B1%8F2019-11-04%E4%B8%8B%E5%8D%882.46.09.png)
# 题目地址
<https://leetcode-cn.com/problems/assign-cookies/description/>
#### 解法：贪心算法
+ 类似题型
  + [860. 柠檬水找零](https://leetcode-cn.com/problems/lemonade-change/solution/860-ning-meng-shui-zhao-ling-by-alexer-660/) 
+ 解题关键
  + 优先满足胃口小的小朋友的需求，即贪心问题。
    + 设最大可满足的孩子数量为maxNum = 0
    + 胃口小的拿小的，胃口大的拿大的
      + 两边升序，然后一一对比
        + 当饼干j >= 胃口i 时
          + i++、j++
          + maxNum++
        + 当饼干j <  胃口i时，说明饼干不够吃，换更大的
          + j++      
      + 到边界后停止
+ 小问题
  + js可以调用sort()函数默认是返回从小到大排序的
  + 但在leetCode里不知道为啥不行 
    + 只能加一个函数参数了 
```javascript
/**
 * @param {number[]} g
 * @param {number[]} s
 * @return {number}
 */
var findContentChildren = function(g, s) {
    g = g.sort((a,b) => a-b);
    s = s.sort((a,b) => a-b);
    var gLen = g.length;
    var sLen = s.length;
    var i = 0;
    var j = 0;
    var maxNum = 0;
    while(i < gLen && j < sLen){
        if(s[j] >= g[i]){
            i++;
            j++;
            maxNum++;
        }else{
            j++;
        }
    }
    return maxNum;
};
```