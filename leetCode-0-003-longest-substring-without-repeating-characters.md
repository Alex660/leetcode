# 无重复字符的最长子串（中等）
# 题目描述
![截屏2020-01-16下午11.14.47.png](https://pic.leetcode-cn.com/e5f22055c24b7a2fad529e43020227ee9113e14f01a3da9a74e6808601f57997-%E6%88%AA%E5%B1%8F2020-01-16%E4%B8%8B%E5%8D%8811.14.47.png)
# 题目地址
<https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/>
## 滑动窗口思想
+ 讲解
  + [滑动窗口11道](https://github.com/Alex660/Algorithms-and-data-structures/blob/master/demos/%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A311%E9%81%93.md)
+ 类似题型
  + 1、[3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
  + 2、[30. 串联所有单词的子串](https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/)
  + 3、[76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)
  + 4、[159. 至多包含两个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-two-distinct-characters/)
  + 5、[209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)
  + 6、[239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
  + 7、[340. 至多包含 K 个不同字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-most-k-distinct-characters/)
  + 8、[438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)
  + 9、[567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)
  + 10、[632. 最小区间](https://leetcode-cn.com/problems/smallest-range-covering-elements-from-k-lists/)
  + 11、[727. 最小窗口子序列](https://leetcode-cn.com/problems/minimum-window-subsequence/)
+ 戳看👇
  + [leetCode所有题解](https://github.com/Alex660/leetcode)
#### 解法：滑动窗口
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    if(s.length == 0) return 0;
    let hash = {};
    let max = 0;
    let left = 0;
    for(let right = 0;right < s.length;right++){
        let moveLeft = hash[s[right]];
        if(moveLeft){
            left = Math.max(left,moveLeft);
        }
        hash[s[right]] = right + 1;
        max = Math.max(max,right - left + 1);
    }
    return max;
};
```
+ 亦可这样写
```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let windows = {};
    let res = 0;
    let left = 0,right = 0;
    while(right < s.length){
        let c1 = s[right];
        windows[c1] != undefined ? windows[c1]++ : windows[c1] = 1;
        right++;
        while(windows[c1] > 1){
            let c2 = s[left];
            windows[c2]--;
            left++;
        }
        res = Math.max(res,right - left);
    }
    return res;
};
```