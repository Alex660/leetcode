# 串联所有单词的子串（困难）
# 题目描述
![截屏2020-01-17上午7.36.21.png](https://pic.leetcode-cn.com/bff14dd05493231a803417cc74f59bb9c938dba6a012a6935f968647b5322de2-%E6%88%AA%E5%B1%8F2020-01-17%E4%B8%8A%E5%8D%887.36.21.png)
# 题目地址
<https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/>
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
 * @param {string[]} words
 * @return {number[]}
 */
var findSubstring = function(s, words) {
    let left = 0,right = 0,wordsLen = words.length;
    if(wordsLen == 0) return [];
    let res = [];
    let gapLen = words[0].length;
    let needs = {};
    let windows = {};
    for(let i = 0;i < wordsLen;i++){
        needs[words[i]] ? needs[words[i]]++ : needs[words[i]] = 1;
    }
    let needsLen = Object.keys(needs).length;
    let match = 0;
    for(let i = 0;i < gapLen;i++){
        right = left = i;
        match = 0;
        while(right <= s.length - gapLen){
            let c1 = s.substring(right,right + gapLen);
            right += gapLen;
            windows[c1] ? windows[c1]++ : windows[c1] = 1;
            if(windows[c1] === needs[c1]){
                ++match;
            }
            while(left < right && match == needsLen){
                if(Math.floor((right - left) / gapLen) == wordsLen){
                    res.push(left);
                }
                let c2 = s.substring(left,left + gapLen);
                left += gapLen;
                windows[c2]-- ;
                if(needs[c2] && windows[c2] < needs[c2]){
                    match--;
                }
            }
        }
        windows = {};
    }
    return res;
};
```