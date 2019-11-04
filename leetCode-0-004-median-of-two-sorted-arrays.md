# 寻找两个有序数组的中位数（困难）
# 题目描述
![截屏2019-10-20上午9.32.41.png](https://pic.leetcode-cn.com/65c5838a814a6cdbdb987c28c72e200bc289e52dff1a84ad7071cb7173309a61-%E6%88%AA%E5%B1%8F2019-10-20%E4%B8%8A%E5%8D%889.32.41.png)
# 题目地址
<https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/>
#### 解法：
```javascript
var A = [4,9,19,13];
var B = [7,19,3,10];
var n = 4; var m = 4;
m = >  可分成几份 => m+1   0,1,2,3,4...i-1,---i---,i+1,i+2...m-1,m
n = >  可分成几份 => n+1   0,1,2,3,4...j-1,---j---,j+1,j+2...n-1,n
& A,B 数组有序
& 0<=i<=m
```
+ left_part: A[0,i-1]+B[0,j-1]
+ right_part:	A[i+1,m]+B[j+1,n]

+ <1>:m+n 为偶数时   
    + left_part.length == right_part.length
    + => i+j = m-i + n-j => j = (m+n) / 2 -i 
	+ Max(left_part) <= Min(right_part)
+ 中位数 == ( Max(left_part) + Min(right_part) ) / 2
+ <2>:m+n 为奇数时  
    + left_part.length - right_part.length = 1 
    + => 
    + i+j = m-i + n-j+1 => j = (m+n+1) / 2 - i
	+ Max(left_part) <= Min(right_part)
+ 中位数 == Max(A[i-1],B[j-1]) (比右半部分多出的那个数)


+ <1> j = (m+n)/2 - i <2> j =(m+n+1) / 2 -i  
+ 奇数/2 是向下去整的【10/2 = 5 == parseInt(11/2)】 
+ 所以 j = (m+n+1) / 2 - i  [ 同样兼容偶数 => ((m+n)/2) == parseInt((m+n+1)/2)]

+ 推导 =>  因为: 0 <= i <= m &&  j = (m_n+1) / 2 - i (m+n为奇偶数) && j>0
```javascript
    j>0  
    => 
        j= (m+n+1)/2 - i>0
    <=
    当且仅当 n>=m 时 ,j一定 >= 0 
    => 
        j= (m+n+1)/2 - i >= (m+m+1)/2 -i = m-i + 1/2 > 0
    所以: n>=m	
    因为: n>=m && i<=m   =>  j<n
        <=  
        j = (m+n+1) / 2 - i  <= (2n + 1) /2 <= n+1/2 <= n
```
+ 为了保证 Max(left_part) <= Min(right_part) 
     + A,B数组有序 => A[i-1]<=A[i] 
     + B[j-1]<=B[j]
+   B[j-1] <= A[i] && A[i-1] <= B[j]
```javascript
function findMedianSortedArrays(A,B){
	var m = A.length,n=B.length;
	m==0 && n==0 return
	m==0 => n==1 ? B[0] : var middle = parseInt(n/2)  && return (n%2==0 ? (B[middle-1]+B[middle])/2 : B[middle]
	m>n  => findMeidanSortedArrays(B,A)

    二分法开启 => i = parseInt(m/2)
    while(i>=0){
        j = parseInt((m+n+1)/2) - i

        1.B[j-1]>A[i] 临界点：j!=0 i!=m
            当此时，i++ ,j =(m+n+1)/2 - i
        2.A[i-1]>B[j] 临界点：i!=0 j!=n
            当此时，i-- ,j =(m+n+1)/2 - i
        3.临界点
            当j = 0 时，Max(left_part) =  A[i-1]
            当i = 0 时，Max(left_part) =  B[j-1]

            (m+n)%2 == 1 return 中位数==maxLeft

            当i = m 时，Min(right_part) = B[j]
            当j = n 时，Min(right_part) = A[i]

            return 中位数==(maxLeft+minRight)/2
    }
}
```
### 解法
```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var findMedianSortedArrays = function(nums1, nums2) {
    var m = nums1.length;
    var n = nums2.length;
    if(m ==0 && n==0){
		return
	}else if(m == 0){
		if(n==1){
			return nums2[0]
		}
		var middle = parseInt(n/2)
		if(n%2==0){
			return (nums2[middle-1]+nums2[middle])/2
		}else{
			return nums2[middle]
		}
	}else if(m>n){
        return findMedianSortedArrays(nums2,nums1)
    }
    //二分法开启
    var i = parseInt(m/2);
    while(i>=0){
        var j = parseInt((m+n+1)/2) - i;
        if(i!=0 && i!=n &&nums1[i-1]>nums2[j]){
            i--;
        }else if(j!=0 && i!=m && nums2[j-1]>nums1[i]){
            i++;
        }else{
            var maxLeft = 0;
            if(j == 0){
                maxLeft = nums1[i-1]
            }else if(i == 0){
                maxLeft = nums2[j-1]
            }else{
                maxLeft = Math.max(nums1[i-1],nums2[j-1])
            }
            //总和为奇数maxLeft即为所求中位数
            if((m+n)%2== 1){
                return maxLeft;
            }
            
            var minRight = 0;
            if(j == n){
                minRight = nums1[i]
            }else if(i == m){
                minRight = nums2[j]
            }else{
                minRight = Math.min(nums1[i],nums2[j])
            }
            return (maxLeft+minRight)/2;
        }
    }
};
```