# 爬楼梯（简单）
# 题目描述
![截屏2019-10-20上午9.14.32.png](https://pic.leetcode-cn.com/86caff1d642f51aaf570fb0d7bba556b060304539eadd68764ba6e1dc5694add-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.14.32.png)
# 题目地址
<https://leetcode-cn.com/problems/climbing-stairs/solution/>
# 解法一：
![1571265941(1).png](https://pic.leetcode-cn.com/e2df510c0bb6a7635d94241de9fb24ee64dcd0c9ddcac9a8d59eda31e9ff0848-1571265941\(1\).png)

#### 解法一：双指针法
> 分析：输入[0,1,0,3,12]  输出[1,3,12,0,0]
+ 1、维护一个总是指向0的动态指针 i
+ 2、每次遇到非0位置的数将其位置的数与0位置指针索引上的数进行交换值并更新1的指针i++
+ 2处交换 一处总为0 所以直接赋值为0 不用存储临时变量 但如此就需要判断 i 是否等于 j 去掉为自己的情况
+ 代码实现
```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    var i = 0;
	for(var r = 0;r<nums.length;r++){
		if(nums[r]!=0){
			if(i != r){
				nums[i] = nums[r];
				nums[r] = 0;
			}
			i++;
		}
	}
};
```
#### 解法二：遍历取非0值 && push 0
```javascript
var moveZeroes = function(nums) {
	var insertZero = 0;
	var n = nums.length;
	for(var i =0 ;i<n;i++){
		if(nums[i] != 0){
			nums[insertZero++] = nums[i]
		}
	}
	while(insertZero < n){
		nums[insertZero++] = 0;
	}
}
```