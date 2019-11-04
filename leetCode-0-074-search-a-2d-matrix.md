# 搜索二维矩阵（中等）
# 题目描述
![截屏2019-11-04上午5.38.00.png](https://pic.leetcode-cn.com/f998516244509fd3334b2f476e444e1db00c68bbc720d88db47fc80dcf3f80df-%E6%88%AA%E5%B1%8F2019-11-04%E4%B8%8A%E5%8D%885.38.00.png)
# 题目地址
<https://leetcode-cn.com/problems/search-a-2d-matrix/>
# 解法：二分查找法
+ 时间复杂度：O(log(mn))
+ 空间复杂度：O(1)
+ 由题意可得，在一个有序的二维数组中查找一个值
+ 类似在一维数组中查找一个值
  + 这类题基本可以用二分查找法
  + [戳看此法用到的题](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/33-sou-suo-xuan-zhuan-pai-xu-shu-zu-by-alexer-660/)
+ 本题关键在于两点
  + 想到用二分查找法
    + 模板
      ```java
        public int bsearch(int[] a, int n, int value) {
            int low = 0;
            int high = n - 1;

            while (low <= high) {
                int mid = (low + high) / 2;
                if (a[mid] == value) {
                    return mid;
                } else if (a[mid] < value) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }

            return -1;
        }
      ```  
  + 怎么求二维数组的中间值
    + 试想二维数组被压缩成一维数组后依旧是有序
      + 那么mid = (0+matrix.length-1)/2 = (low + high)/2
      + 中间值即为matrix[mid]
    + 而此处是二维，
      + 设m= matrix.length、n = matrix[0].length; 
      + 所以low = 0,high = matrix.length * matrix[0].length - 1 = m * n -1
      + 中间值索引为 (low+high)/2
      + 中间值为 matrix[mid/n][mid%n]
  + 解题技巧
    + n除以2^k可以换成位运算，提升代码性能
      + n>>k
```javascript
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var searchMatrix = function(matrix, target) {
    var m = matrix.length;
    if(m == 0)return false;
    var n = matrix[0].length;
    var low = 0;
    var high = m*n - 1;
    while(low<=high){
        var mid = (low+high)>>1;
        var row = parseInt(mid/n);
        var col = mid%n;
        var matrixMid = matrix[row][col];
        if(matrixMid < target){
            low = mid + 1;
        }else if(matrixMid > target){
            high = mid -1;
        }else if(matrixMid == target){
            return true;
        }
    }
    return false;
};
```