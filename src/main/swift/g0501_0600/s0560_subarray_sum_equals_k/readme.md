[![](https://img.shields.io/github/stars/LeetCode-in-Swift/LeetCode-in-Swift?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift)
[![](https://img.shields.io/github/forks/LeetCode-in-Swift/LeetCode-in-Swift?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift/fork)

## 560\. Subarray Sum Equals K

Medium

Given an array of integers `nums` and an integer `k`, return _the total number of continuous subarrays whose sum equals to `k`_.

**Example 1:**

**Input:** nums = [1,1,1], k = 2

**Output:** 2 

**Example 2:**

**Input:** nums = [1,2,3], k = 3

**Output:** 2 

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   `-1000 <= nums[i] <= 1000`
*   <code>-10<sup>7</sup> <= k <= 10<sup>7</sup></code>

## Solution

```swift
class Solution {
    func subarraySum(_ nums: [Int], _ k: Int) -> Int {
        var result = 0
        var dict = [Int: Int]()
        dict[0] = 1
    
        var sum = 0
        for num in nums {
            sum += num
            if let val = dict[sum - k] {
                result += val
            }
            dict[sum, default: 0] += 1
        }
        return result 
    }
}
```