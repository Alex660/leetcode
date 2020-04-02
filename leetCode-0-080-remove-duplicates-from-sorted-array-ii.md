# 删除排序数组中的重复项 II （中等）
# 题目描述
![截屏2020-03-31上午10.14.56.png](https://pic.leetcode-cn.com/03f4647d3d5d75fba4c5437852d7efc262e03646cd97dd08a42d40e3b9036ec9-%E6%88%AA%E5%B1%8F2020-03-31%E4%B8%8A%E5%8D%8810.14.56.png)
![截屏2020-03-31上午10.15.05.png](https://pic.leetcode-cn.com/f427f122de36db95513c80f7db130f0aa6e5efd800523a67794ec519853716ca-%E6%88%AA%E5%B1%8F2020-03-31%E4%B8%8A%E5%8D%8810.15.05.png)
# 题目地址
<https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/>
#### 解法：双指针
+ [解法同 - 26. 删除排序数组中的重复项 - 解法二](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/solution/26-shan-chu-pai-xu-shu-zu-zhong-de-zhong-fu-xian-6/)
+ 唯一区别是从第三、一个元素起开始比对
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    let count = 0
    let n = nums.length
    if(n < 3) return n
    let j = 1
    for(let i = 2;i < n;i++) {
        if(nums[i] != nums[j-1]) {
            j++
            nums[j] = nums[i]
        }
    }
    return j + 1
};
```