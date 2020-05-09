# 移除元素（简单）
# 题目描述
![截屏2020-05-09 下午2.27.09.png](https://pic.leetcode-cn.com/dfa851054161caf79ff4dbb9169434011da3880cdfbaa42add26814b56d7c28e-%E6%88%AA%E5%B1%8F2020-05-09%20%E4%B8%8B%E5%8D%882.27.09.png)
![截屏2020-05-09 下午2.27.18.png](https://pic.leetcode-cn.com/7d2ca4a7cfc546075d5a64b65e6aa39990a32553a0f363cda8c03d168e5a43fa-%E6%88%AA%E5%B1%8F2020-05-09%20%E4%B8%8B%E5%8D%882.27.18.png)
# 题目地址
<https://leetcode-cn.com/problems/remove-element/>
#### 解法：双指针
```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let i = 0,j = 0
    while(j < nums.length) {
        if(nums[j] !== val) {
            nums[i++] = nums[j]
        }
        j++
    }
    return i
};
```
+ 优化
  + 当原数组等于给定值的元素较少时，上面的方法仍旧会遍历复制每个元素到前排
  + 只有当遇到有给定值的元素时做复制操作
    + 将遇到要删除的元素(首指针++)丢到数组最后，并同时缩小数组长度(用指针模拟实现即：尾指针--)
```javascript
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let i = 0,j = nums.length
    while(i < j) {
        if(nums[i] !== val) {
            i++
        }else {
            nums[i] = nums[j-1]
            j--
        }
    }
    return j
};
```