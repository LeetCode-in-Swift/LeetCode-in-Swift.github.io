[![](https://img.shields.io/github/stars/LeetCode-in-Swift/LeetCode-in-Swift?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift)
[![](https://img.shields.io/github/forks/LeetCode-in-Swift/LeetCode-in-Swift?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift/fork)

## 139\. Word Break

Medium

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "leetcode", wordDict = ["leet","code"]

**Output:** true

**Explanation:** Return true because "leetcode" can be segmented as "leet code". 

**Example 2:**

**Input:** s = "applepenapple", wordDict = ["apple","pen"]

**Output:** true

**Explanation:** Return true because "applepenapple" can be segmented as "apple pen apple". Note that you are allowed to reuse a dictionary word. 

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]

**Output:** false 

**Constraints:**

*   `1 <= s.length <= 300`
*   `1 <= wordDict.length <= 1000`
*   `1 <= wordDict[i].length <= 20`
*   `s` and `wordDict[i]` consist of only lowercase English letters.
*   All the strings of `wordDict` are **unique**.

## Solution

```swift
class Solution {
    func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
        var words = [[Character]]()
        for word in wordDict {
            words.append(Array(word))
        }
        let sSlice = Array(s)[...]
        var visited = Array(repeating: false, count: sSlice.count)
        return canBeBroken(sSlice, words, &visited)
    }

    func canBeBroken(
        _ chars: ArraySlice<Character>, 
        _ words: [[Character]],
        _ visited: inout [Bool]
    ) -> Bool {
        let startIndex = chars.startIndex
        guard startIndex != chars.endIndex else { return true }
        guard !visited[startIndex] else { return false }
        for word in words {
            guard 
                chars.starts(with: word),
                canBeBroken(chars[startIndex.advanced(by: word.count)...], words, &visited)
            else { 
                continue 
            }
            return true
        }
        visited[startIndex] = true
        return false
    }
}
```