### 剑指offer42-连续子数组的最大和
输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

```

可以考虑使用动态规划进行解决，状态转移方程为：
dp[i] = max(item[i], dp[i-1] + item[i])。 ------- (1)
求出dp[i]后需要重新遍历数组dp，取出最大值。也可以使用变量result在求出dp[i]时进行比较判断，最后result即为最大值。

另外观察方程(1)可知，状态转移方程的关键点在于dp[i-1]是否大于0，如果该值小于0，那么其实可以直接放弃dp[i-1]，转而查看item[i]的值。使用数学思想可以理解为“一个小于0的数 + 未知数 < 未知数”。又有result在不断比较值的大小，所以即使是递减的数列，result也可以纪录下最大值。

dp[i-1]是一个数字，又使用了result记录了最终结果，所以无需开辟额外数组空间。时间复杂度O(n)， 空间复杂度O(1)。


```
//Go
func maxSubArray(nums []int) int {
	if nums == nil {
		return 0
	}
	result := ^int(^uint(0) >> 1)
	temp := result

	for _, value := range nums {
		if temp < 0 {
			temp = value
		} else {
			temp += value
		}
		if temp > result {
			result = temp
		}
	}
	return result
}

```

```
//Python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        result = nums[0]
        temp = 0
        length = len(nums)
        for i in range(length):
            if temp >0:
                temp += nums[i]
            else:
                temp = nums[i]
            if temp > result:
                result = temp
        return result
```**