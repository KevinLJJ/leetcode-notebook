# 数组
## 26. 删除排序数组中的重复项
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例 1:
```
给定数组 nums = [1,1,2], 
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
你不需要考虑数组中超出新长度后面的元素。
```
示例 2:
```
给定 nums = [0,0,1,1,1,2,2,3,3,4],
函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
你不需要考虑数组中超出新长度后面的元素。
```
题解：
```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        j,n=0,len(nums)
        for i in range(n):
            if(nums[i]!=nums[j]):
                j+=1
                nums[j]=nums[i]
        return j+1
```
## 122. 买卖股票的最佳时机 II
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:
```
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```
示例 2:
```
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```
示例 3:
```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```
**思路：**贪心算法，只关心当前的结果，不关心全局，则只关系最近两天的盈利，如果盈利则计入总盈利，若不盈利则不计算。
**题解：**
```go
func maxProfit(prices []int) int {
    pro:=0 //总收益
    for i:=1;i<len(prices);i++{
        dayPro:=prices[i]-prices[i-1]//总是第一天买进第二天卖出，计算收益
        if dayPro>0{
            pro+=dayPro//如果盈利就卖，并计算收益
        }
    }
    return pro
}
```

## 38. 报数
**给定两个数组，编写一个函数来计算它们的交集。**

[题目地址](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii)

示例 1:


```
输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2,2]
```
示例 2:
```
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [4,9]
```
说明：

输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。
我们可以不考虑输出结果的顺序。

思路：
使用Map，遍历第一个数组且作为Key，将对应Value+1，遍历第二个数组，将值作为Key，如果Value大于0则说明是在第一个数组中存在的，加入结果集返回。
```go
func intersect(nums1 []int, nums2 []int) []int {
    var result[]int
    temp := make(map[int]int)
    for i :=0;i<len(nums1);i++{
        temp[nums1[i]]++
    }
    for j :=0;j<len(nums2);j++{
        if(temp[nums2[j]]>0){
        result = append(result,nums2[j])
        temp[nums2[j]]--
      }
    }
return result
}
```
## 66. 加一

[题目地址](https://leetcode-cn.com/problems/plus-one)

**给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。
你可以假设除了整数 0 之外，这个整数不会以零开头。**

示例 1:
```
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。
```

示例 2:
```
输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。
```
**思路**：除了直接给最后一位加1的情况外，主要考虑什么时候该进位，进到最后的时候应该考虑追加位数（如999+1=1000），用个循环判断最后一位为9时的次数。
```go
func plusOne(digits []int) []int {
    for i:=len(digits)-1;i>=0;i--{
        //从后往前检查是否需要进位
        if digits[i]==9{
            digits[i]=0
        }else{
            //不用进位加一即可
            digits[i]+=1
            return digits
        }
         //如果进位到首位，则首元素加一位
        if i==0{
            digits=append(digits,0)
            digits[0]+=1
        }
    }
    return digits
}
```
## 136. 只出现一次的数字
[题目地址](https://leetcode-cn.com/problems/single-number)

**给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。**

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

```
输入: [2,2,1]
输出: 1
```
示例 2:
```
>输入: [4,1,2,1,2]
输出: 4
```

**思路**：
开始想了一种比较笨的方法，就是先排序然后再每对每对比较，若有不等，则其一为单一元素。后面看到了异或的解法，甚是巧妙。
![](https://img-blog.csdnimg.cn/20190725223513564.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTY5Nzgx,size_16,color_FFFFFF,t_70)

```go 
func singleNumber(nums []int) int {
    result:=0
    for _,num:=range nums{
        //逐个异或，相同为0，相异为1
        result^=num
    }
    return result
}
```

## 1. 两数之和

[题目地址](https://leetcode-cn.com/problems/two-sum)

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:
```
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

思路：暴力穷举，如果两数之和等于输入`target`，则返回所在下标。
```go
func twoSum(nums []int, target int) []int {
    for i:=0;i<len(nums);i++{
        for j:=0;j<len(nums);j++{
            if i!=j{
                if nums[i]+nums[j]==target{
                    result:=[] int {i,j}
                    return result
                }
            }
        }
    }
    res:=[] int {}
    return res
}
```
## 283. 移动零

**给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。**

示例:
```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```
说明:

必须在原数组上操作，不能拷贝额外的数组。
尽量减少操作次数。

**思路**：
设置快慢指针，快指针遍历数组，如果遇到非零元素，则将慢指针+1，然后将快指针的值给慢指针。当快指针到数组尾的时候，慢指针后面的元素都可以给0。

```go
func moveZeroes(nums []int)  {
    slow:=0
    for fast:=0;fast<len(nums);fast++{
        if nums[fast]!=0{
            nums[slow]=nums[fast]
            slow+=1
        }
    }
    for i:=slow;i<len(nums);i++{
        nums[i]=0
    }
}
```

## 189. 旋转数组

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

示例 1:
```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

示例 2:
```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```
说明:

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
要求使用空间复杂度为 O(1) 的 原地 算法。

思路：
- 先逆序后k个元素，然后逆序前n-k个元素，然后对整体n个元素进行逆序。即为所求。

```go
func rotate(nums []int, k int)  {
    k=k%len(nums)//解决当K大于数组长度的情况
    reverse(nums[len(nums)-k:len(nums)])//将后k个元素逆序
    reverse(nums[:len(nums)-k])//将前n-k个元素逆序 
    reverse(nums[:len(nums)])//将整个数组逆序
}
//逆序传入的数组
func reverse(nums []int){
    left,right:=0,len(nums)-1//双指针，分别指向首尾元素
    for left<right{//当两个指针没有重合的时候
        nums[left],nums[right]=nums[right],nums[left]//替换两个元素所指向的位置
        //更新两个指针
        left++
        right--
    }
}
```

## 48. 旋转图像

[题目地址](https://leetcode-cn.com/problems/rotate-image)

**给定一个 n × n 的二维矩阵表示一个图像。将图像顺时针旋转 90 度。**

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

示例 1:
```
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```
示例 2:
```
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```
题解：
```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        matrix[:] = map(list,zip(*matrix[::-1]))
```