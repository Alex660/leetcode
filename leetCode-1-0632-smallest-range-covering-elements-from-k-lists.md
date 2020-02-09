# 最小区间（困难）
# 题目描述
![截屏2020-02-09下午4.47.09.png](https://pic.leetcode-cn.com/030f6bcb241118a80639e80ffbe1c68fcbad1a9b22ff15f3cd8164f8fc857813-%E6%88%AA%E5%B1%8F2020-02-09%E4%B8%8B%E5%8D%884.47.09.png)
# 题目地址
<https://leetcode-cn.com/problems/smallest-range-covering-elements-from-k-lists/>
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
+ [参考这里](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/discuss/104920/Java-8-Sliding-window)
+ ![截屏2020-02-09下午4.46.48.png](https://pic.leetcode-cn.com/4c60708eb664983913331db16cfc9f9770b37b1e81ea528baf83f8f1b1ab281c-%E6%88%AA%E5%B1%8F2020-02-09%E4%B8%8B%E5%8D%884.46.48.png)
```javascript
/**
 * @param {number[][]} nums
 * @return {number[]}
 */
var smallestRange = function(nums) {
    let points = [];
    for(let i = 0;i < nums.length;i++){
        for(let j = 0;j < nums[i].length;j++){
            points.push([nums[i][j],i]);
        }
    }
    points.sort((a,b) => a[0] - b[0]);
    let counts = new Array(nums.length).fill(0);
    let countUnique = 0,minStart = -1,minLen = Number.MAX_SAFE_INTEGER;
    for(let i = 0,j = 0;j < points.length;j++){
        if(counts[points[j][1]]++ === 0) countUnique++;
        while(countUnique === counts.length){
            if(points[j][0] - points[i][0] + 1 < minLen){
                minStart = points[i][0];
                minLen = points[j][0] - points[i][0] + 1;
            }
            let prev = points[i][0];
            while(i <= j && prev === points[i][0]){
                if(--counts[points[i++][1]] === 0) countUnique--;
            }
        }
    }
    return [minStart,minStart + minLen - 1];
};
```