[![](https://img.shields.io/github/stars/LeetCode-in-Swift/LeetCode-in-Swift?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift)
[![](https://img.shields.io/github/forks/LeetCode-in-Swift/LeetCode-in-Swift?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift/fork)

## 295\. Find Median from Data Stream

Hard

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

*   For example, for `arr = [2,3,4]`, the median is `3`.
*   For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

*   `MedianFinder()` initializes the `MedianFinder` object.
*   `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
*   `double findMedian()` returns the median of all elements so far. Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.

**Example 1:**

**Input**

    ["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
    [[], [1], [2], [], [3], []]

**Output:** [null, null, null, 1.5, null, 2.0]

**Explanation:**

    MedianFinder medianFinder = new MedianFinder();
    medianFinder.addNum(1); // arr = [1]
    medianFinder.addNum(2); // arr = [1, 2]
    medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
    medianFinder.addNum(3); // arr[1, 2, 3]
    medianFinder.findMedian(); // return 2.0 

**Constraints:**

*   <code>-10<sup>5</sup> <= num <= 10<sup>5</sup></code>
*   There will be at least one element in the data structure before calling `findMedian`.
*   At most <code>5 * 10<sup>4</sup></code> calls will be made to `addNum` and `findMedian`.

**Follow up:**

*   If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
*   If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

## Solution

```swift
import HeapModule

class MedianFinder {
    var leftHeap: Heap<Int>
    var rightHeap: Heap<Int>

    init() {
        leftHeap = []
        rightHeap = []
    }
    
    func addNum(_ num: Int) {
        leftHeap.insert(num)
        while let rightMin = rightHeap.min, rightMin < leftHeap.max! {
            leftHeap.insert(rightHeap.removeMin())
            rightHeap.insert(leftHeap.removeMax())
        }
        let mid = (leftHeap.count + rightHeap.count) / 2
        while rightHeap.count < mid {
            rightHeap.insert(leftHeap.removeMax())
        }
    }
    
    func findMedian() -> Double {
        let count = leftHeap.count + rightHeap.count
        if count % 2 == 0 {
            return Double(leftHeap.max! + rightHeap.min!) / 2
        } else {
            return Double(leftHeap.max!)
        }
    }
}

/*
 * Your MedianFinder object will be instantiated and called as such:
 * let obj = MedianFinder()
 * obj.addNum(num)
 * let ret_2: Double = obj.findMedian()
 */
```