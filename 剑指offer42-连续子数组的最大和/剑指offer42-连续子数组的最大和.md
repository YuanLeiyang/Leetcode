### 剑指offer42-连续子数组的最大和
输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

```

**基本思路**

使用动态规划，状态转移方程为 dp[i] = Math.max(dp[i-1] + num[i], num[i])。

即子数组必须是下标连续的，要么将当前的努力累加，否则只能放弃之前的努力，从今天（最新的）开始。（蕴含着人生的哲理）

**思路优化**

对于状态转移方程，原式应为：
if （dp[i-1] + num[i] < num[i]） {
    dp[i] = num[i];
} else {
    dp[i] = dp[i-1] + num[i];
}

观察判断条件，不等式两边可以同时消去num[i]，从而简化为dp[i-1] < 0。这该怎么理解呢？

【插播：在大海上漂泊时，千万不要喝海水，因为海水是咸的，会越喝越渴。】

当dp[i-1] < 0 时，它就变成了"海水"。因为 负数 + 任意数 < 任意数，所以当发现dp[i-1]为负数时可以果断放弃它，从而选择最新的num[i]。

**空间优化**

对于状态转移方程，发现dp[i]只对dp[i-1]有依赖，则可以使用一个变量temp保存dp[i]。动态规划中的数组退化为有限个变量，空间复杂度由O(n)优化为O(1)。


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