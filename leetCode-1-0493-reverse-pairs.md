# 翻转对（困难）
# 题目描述
![截屏2019-11-28上午6.54.10.png](https://pic.leetcode-cn.com/05468a5585bad738c1f85e42eaac0097bfd62ab7d1f143777f612098b2eba7dd-%E6%88%AA%E5%B1%8F2019-11-28%E4%B8%8A%E5%8D%886.54.10.png)
# 题目地址
<https://leetcode-cn.com/problems/reverse-pairs/>
#### 解法一：归并排序 A
+ 思路
  + 数组中 arr[0~i~n] 重要翻转对
    + i < i+1 && nums[i] > 2*nums[i+1]
    + nums[i]和nums[i+1]为逆序对
    + 逆序对
      + i < i+1 && nums[i] > nums[i+1]
      + nums[i]和nums[i+1]为逆序对
  + 示例解说
    + [1,3,2,3,1] => 2
      + 3 > 1
      + 3 > 1
    + [12,10,1,3,5] => 5
      + 12 > 1
      + 12 > 1
      + 12 > 5
      + 10 > 1
      + 10 > 3
    + 直观暴力法
      + 双层遍历
        + 从左到右，每次遍历一个元素
          + 判断当前元素的1/2是否大于后面每个元素
            + 有，++
            + 无，继续第二次遍历
      + 优化
        + 将数组一分为二
          + 如果我们左边区间的当前元素**大于**右边区间的其中一个元素的两倍
            + **且左边区间递增，右边区间递增**
            + 那么**不用像暴力法那样再继续遍历右边区间的下下个元素**
              + 因为是单调递增的，因此左边区间**当前元素到结尾元素**都可以和**右边区间的那个元素组**成重要翻转对
              + 意即从暴力法的(左)一对多(右)，
              + **变成当前的(左)多对一(右)，且这种计算组合方式的个数直接用减法就可以求出 == 左边区间的长度 - 当前左边区间的索引值**
        + 栗子
          + [ 10, 12 ] 、[ 1, 3, 5 ]
          + 因为10 > 1 * 2  且 12 >10
          + 所以10，12和1都可以和1组成一个重要翻转对，就不需要
        + 而适合上述优化方法的算法有
          + 归并排序
            + 栗子：***[ 12 , 10 , 1 , 3 , 5 ]***
            + ![截屏2019-11-28下午5.38.49.png](https://pic.leetcode-cn.com/0f63bc5313f572f57f691385c3b4a8e72d60d98007869b95b9cbd05f3d1fb9bd-%E6%88%AA%E5%B1%8F2019-11-28%E4%B8%8B%E5%8D%885.38.49.png)
  + 归并排序 - (Merge Sort) ~ 分治
    + **把长度为n的输入序列分成两个长度为n/2的子序列**
    + **对这两个子序列分别采用归并排序**
    + **将两个排序好的子序列合并成一个最终的排序序列**
    + ![](https://static001.geekbang.org/resource/image/db/2b/db7f892d3355ef74da9cd64aa926dc2b.jpg)
    
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var reversePairs = function(nums) {
    let count = 0;
    let mergeArr = (left,right) => {
        let result = [];
        let left_i = 0;
        let right_j = 0;
        let tmpI = 0,tmpJ = 0;
        while(tmpI < left.length && tmpJ < right.length){
            if(left[tmpI]/2 > right[tmpJ]){
                count += left.length-tmpI;
                tmpJ++;
            }else{
                tmpI++;
            }
        }
        while(left_i < left.length && right_j < right.length){
            if(left[left_i] < right[right_j]){
                result.push(left[left_i]);
                left_i++;
            }else{
                result.push(right[right_j]);
                right_j++;
            }
        }
        return [...result,...left.slice(left_i),...right.slice(right_j)];
    }
    let mergeSort = (arr) => {
        if(arr.length <= 1){
            return arr;
        }
        let mid = arr.length >> 1;
        let left = arr.slice(0,mid);
        let right= arr.slice(mid);
        return mergeArr(mergeSort(left),mergeSort(right));
    }
    mergeSort(nums);
    return count;
};
```
+ 简写版
  + 因为只需要归并过程中的比对求解
  + 因此归并排序本身不用排，可以直接调用库函数
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var reversePairs = function(nums) {
    let count = 0;
    let mergeArr = (left,right) => {
        let i = 0,j = 0;
        while(i < left.length && j < right.length){
            if(left[i]/2 > right[j]){
                count += left.length-i;
                j++;
            }else{
                i++;
            }
        }
        return [...left,...right].sort((a,b) => a-b);
    }
    let mergeSort = (arr) => {
        if(arr.length <= 1){
            return arr;
        }
        let mid = arr.length >> 1;
        let left = arr.slice(0,mid);
        let right= arr.slice(mid);
        return mergeArr(mergeSort(left),mergeSort(right));
    }
    mergeSort(nums);
    return count;
};
``` 
+ 但这样一来，效率没有自己写的归并排序好，有舍有得
#### 解法二：归并排序 B
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var reversePairs = function(nums) {
    let count = 0;
    let mergeArr = (arr,left,mid,right) => {
        let temp = [];
        let i = left,j = mid + 1,sortedIndex = 0,p = j;
        let tmpI = i,tmpJ = j;
        while(tmpI <= mid){
            while(tmpJ <= right && arr[tmpI]/2 > arr[tmpJ]){
                tmpJ++;
            }
            count += tmpJ - (mid + 1);
            tmpI++;
        }
        while(i <= mid && j <= right){
            if(arr[i] <= arr[j]){
                temp[sortedIndex++] = arr[i++];
            }else{
                temp[sortedIndex++] = arr[j++];
            }
        }
        while(i <= mid){
            temp[sortedIndex++] = arr[i++];
        }
        while(j <= right){
            temp[sortedIndex++] = arr[j++];
        }
        for(let r = 0;r < temp.length;r++){
            arr[left + r] = temp[r];
        }
    }
    let mergeSort = (arr,left,right) => {
        if(left >= right){
            return;
        }
        let mid = (left + right) >> 1;
        mergeSort(arr,left,mid);
        mergeSort(arr,mid+1,right);
        mergeArr(arr,left,mid,right);
    }
    mergeSort(nums,0,nums.length-1);
    return count;
};
```
+ 或者这样写
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var reversePairs = function(nums) {
    let count = 0;
    let mergeArr = (arr,left,mid,right) => {
        let temp = [];
        let i = left,j = mid + 1,sortedIndex = 0,p = j;
        let tmpI = i,tmpJ = j;
        while(tmpI <= mid){
            while(tmpJ <= right && arr[tmpI]/2 > arr[tmpJ]){
                tmpJ++;
            }
            count += tmpJ - (mid + 1);
            tmpI++;
        }
         let sorted = [...arr.slice(left,right+1)].sort((a,b) =>  a - b);
        for(let r = 0;r < sorted.length;r++){
            arr[left + r] = sorted[r];
        }
    }
    let mergeSort = (arr,left,right) => {
        if(left >= right){
            return;
        }
        let mid = (left + right) >> 1;
        mergeSort(arr,left,mid);
        mergeSort(arr,mid+1,right);
        mergeArr(arr,left,mid,right);
    }
    mergeSort(nums,0,nums.length-1);
    return count;
};
```
#### 解法三：树状数组
```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var reversePairs = function(nums) {
    let n = nums.length;
    let c = new Array(n + 1).fill(0);
    let count = 0;
    let lowbit = (x) => {
        return x&(-x);
    }
    let getSum = (x) => {
        let sum = 0;
        while(x <= n){
            sum += c[x];
            x += lowbit(x);
        }
        return sum;
    }
    let update = (x) => {
        while(x){
            c[x] += 1;
            x -= lowbit(x);
        }
    }
    let count_i = (val) => {
        let l = 0,r = sortArr.length - 1,m = 0;
        while(l <= r){
            m = l + ((r - l) >> 1);
            if(sortArr[m] >= val){
                r = m - 1;
            }else{
                l = m + 1;
            }
        }
        return l + 1;
    }
    let sortArr = nums.slice().sort((a,b) => a - b);
    for(let i = 0;i < nums.length;i++){
        count += getSum(count_i(2 * nums[i] + 1));
        update(count_i(nums[i]));
    }
    return count;
};
```