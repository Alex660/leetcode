# 最后一个单词的长度（简单）
# 题目描述
![截屏2019-12-05上午9.16.58.png](https://pic.leetcode-cn.com/ff3b72f5d4799c324f8ed63c05f3c5b4e1f27fc549998e7138daeb5d9302d66e-%E6%88%AA%E5%B1%8F2019-12-05%E4%B8%8A%E5%8D%889.16.58.png)
# 题目地址
<https://leetcode-cn.com/problems/length-of-last-word/>
#### 解法一：库函数 A
+ 首尾去空
+ 长度为0，则不存在
+ 找到最后一个空字符的位置，slice之后的即为最后一个单词
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLastWord = function(s) {
    let str = s.trim();
    if(str.length == 0){
        return 0;
    }
    return str.slice(str.lastIndexOf(' ')+1).length;
};
```
#### 解法二：库函数 B
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLastWord = function(s) {
    let str = s.trim().split(' ');
    return str.length > 0 ? str[str.length-1].length : 0;
};
```
#### 解法三：库函数 C
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLastWord = function(s) {
    return s.trim().split(' ').pop().length;
};
```
#### 解法四：遍历
+ 从右往左最后一个字符位置开始，求最后一个单词的开头和结尾字符索引位置，相减即为所求
  + 结尾字符记录索引为end，初始化为字符串结尾字符索引
    + 遇到空字符，end--
    + 否则
      + end < 0，越界，字符串为空或不存在，返回0
      + end > 0
        + 继续end--判断字符串是否为空
          + 不为空，则说明找到了最后一个单词的结尾字符索引位置
  + 开头字符初始化为 start = end，依旧向前遍历
    + 不为空，则 end--
    + 为空，则说明找到了最后一个单词的首字符位置 start
  + 结果
    + end - start
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLastWord = function(s) {
    let end = s.length - 1;
    while(end >= 0 && s[end] == ' '){
        end--;
    }
    if(end < 0){
        return 0;
    }
    let start = end;
    while(start >=0 && s[start] != ' '){
        start--;
    }
    return end - start;
};
```