# 至多包含两个不同字符的最长子串（中等）
# 题目描述
![截屏2020-01-31下午5.19.18.png](https://pic.leetcode-cn.com/d26f1a657b71133f45f3b5993b33fed57d30e9c8082df7b20f7a42f63126a5de-%E6%88%AA%E5%B1%8F2020-01-31%E4%B8%8B%E5%8D%885.19.18.png)
# 题目地址
<https://leetcode-cn.com/problems/longest-substring-with-at-most-two-distinct-characters/>
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
var lengthOfLongestSubstringTwoDistinct = function(s) {
    let n = s.length;
    if(n < 3) return n;
    let left = 0,right = 0;
    let windows = {};
    let match = 0;
    let maxLen = Number.MIN_SAFE_INTEGER;
    while(right < n){
        let c1 = s[right];
        windows[c1] ?  windows[c1]++ : (windows[c1] = 1) && match++;
        right++;
        while(match > 2){
            let c2 = s[left];
            if(windows[c2] === 1){
                match--;
            }
            windows[c2]--;
            left++;
        }
        maxLen = Math.max(maxLen,right - left);
    }
    return maxLen;
};
```