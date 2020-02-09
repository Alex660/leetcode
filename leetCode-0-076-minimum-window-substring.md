# 最小覆盖子串（困难）
# 题目描述
![截屏2019-12-28下午7.39.20.png](https://pic.leetcode-cn.com/a6417e24bbbc5359db33809de7329230e76b0ac5491e3d86fe968830f1cf2d6d-%E6%88%AA%E5%B1%8F2019-12-28%E4%B8%8B%E5%8D%887.39.20.png)
# 题目地址
<https://leetcode-cn.com/problems/minimum-window-substring/>
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
#### 解法一：双指针
```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {string}
 */
var minWindow = function(s, t) {
    let hash = {};
    let n = t.length;
    let m = s.length;
    for(let i = 0;i < n;i++){
        if(hash[t[i]]){
            hash[t[i]]++
        }else{
            hash[t[i]] = 1
        }
    }
    let left = 0;
    let right = 0;
    let minLeft = 0;
    let minRight = -1;
    let minLen = Number.MAX_SAFE_INTEGER;
    let hasAllT = (hash) => {
        for(let key in hash){
            if(hash[key] > 0){
                return false;
            }
        }
        return true;
    }
    while(right < m){
        let tmpRight = s[right];
        if(hash[tmpRight] != undefined){
            hash[tmpRight]--;
            while(hasAllT(hash)){
                let tmpLen = right - left + 1;
                if(tmpLen < minLen){
                    minLeft = left;
                    minRight = right;
                    minLen = tmpLen;
                }
                let tmpLeft = s[left];
                if(hash[tmpLeft] != undefined){
                    hash[tmpLeft]++;
                }
                left++;
            }
        }
        right++;
    }
    return s.substring(minLeft,minRight+1);
};
```
#### 解法二：滑动窗口
```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {string}
 */
var minWindow = function(s, t) {
    let res = '';
    let left = 0,right = 0;
    let needs = {};
    let windows = {};
    let match = 0,start = 0,minLen = Number.MAX_SAFE_INTEGER;
    for(let i = 0;i < t.length;i++){
        needs[t[i]] ? needs[t[i]]++ : needs[t[i]] = 1;
    }
    let needsLen = Object.keys(needs).length;
    while(right < s.length){
        let c1 = s[right];
        if(needs[c1]){
            windows[c1] ?  windows[c1]++ : windows[c1] = 1;
            if(windows[c1] === needs[c1]){
                match++;
            }
        }
        right++;
        while(match == needsLen){
            if(right - left < minLen){
               start = left;
               minLen = right - left; 
            }
            let c2 = s[left];
            if(needs[c2]){
                windows[c2]--;
                if(windows[c2] < needs[c2]){
                    match--;
                }
            }
            left++;
        }
    }
    return minLen === Number.MAX_SAFE_INTEGER ? '' : s.substr(start,minLen);
};
```