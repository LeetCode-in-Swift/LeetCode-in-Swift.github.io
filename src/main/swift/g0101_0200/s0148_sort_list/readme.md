[![](https://img.shields.io/github/stars/LeetCode-in-Swift/LeetCode-in-Swift?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift)
[![](https://img.shields.io/github/forks/LeetCode-in-Swift/LeetCode-in-Swift?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift/fork)

## 148\. Sort List

Medium

Given the `head` of a linked list, return _the list after sorting it in **ascending order**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

**Input:** head = [4,2,1,3]

**Output:** [1,2,3,4] 

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

**Input:** head = [-1,5,3,4,0]

**Output:** [-1,0,3,4,5] 

**Example 3:**

**Input:** head = []

**Output:** [] 

**Constraints:**

*   The number of nodes in the list is in the range <code>[0, 5 * 10<sup>4</sup>]</code>.
*   <code>-10<sup>5</sup> <= Node.val <= 10<sup>5</sup></code>

**Follow up:** Can you sort the linked list in `O(n logn)` time and `O(1)` memory (i.e. constant space)?

## Solution

```swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init() { self.val = 0; self.next = nil; }
 *     public init(_ val: Int) { self.val = val; self.next = nil; }
 *     public init(_ val: Int, _ next: ListNode?) { self.val = val; self.next = next; }
 * }
 */
class Solution {
    func sortList(_ head: ListNode?) -> ListNode? {
        // simple check for empty & 1 value heads
        if head?.next == nil { return head }
        // start of solution
        var sortedArr = [Int]()
        var newHead = head
        // add all of the values in a simple array
        while newHead != nil {
            if let val = newHead?.val {
                sortedArr.append(val)
            }
            newHead = newHead?.next
        }
        // sort descending to get the array reversed
        sortedArr = sortedArr.sorted { $0 > $1 }
        // reuse the old ListNode var to add a new Object inside it
        // which is the first one in the array
        newHead = ListNode(sortedArr.removeFirst() ?? 0)
        // append the rest of the array to the ListNode
        for item in sortedArr {
            let new = ListNode(item)
            new.next = newHead
            newHead = new
        }
        return newHead
    }
}
```