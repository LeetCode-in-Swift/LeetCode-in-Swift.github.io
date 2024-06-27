[![](https://img.shields.io/github/stars/LeetCode-in-Swift/LeetCode-in-Swift?label=Stars&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift)
[![](https://img.shields.io/github/forks/LeetCode-in-Swift/LeetCode-in-Swift?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/LeetCode-in-Swift/LeetCode-in-Swift/fork)

## 146\. LRU Cache

Medium

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

*   `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
*   `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
*   `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

**Input** ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"] [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

**Output:** [null, null, null, 1, null, -1, null, -1, 3, 4]

**Explanation:**

    LRUCache lRUCache = new LRUCache(2);
    lRUCache.put(1, 1); // cache is {1=1}
    lRUCache.put(2, 2); // cache is {1=1, 2=2}
    lRUCache.get(1); // return 1
    lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
    lRUCache.get(2); // returns -1 (not found)
    lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
    lRUCache.get(1); // return -1 (not found)
    lRUCache.get(3); // return 3
    lRUCache.get(4); // return 4 

**Constraints:**

*   `1 <= capacity <= 3000`
*   <code>0 <= key <= 10<sup>4</sup></code>
*   <code>0 <= value <= 10<sup>5</sup></code>
*   At most 2<code> * 10<sup>5</sup></code> calls will be made to `get` and `put`.

## Solution

```swift
class LRUCache {
    private class LruCacheNode {
        var key: Int
        var value: Int
        var prev: LruCacheNode?
        var next: LruCacheNode?
        
        init(_ k: Int, _ v: Int) {
            key = k
            value = v
        }
    }
    
    private var capacity: Int
    private var cacheMap: [Int: LruCacheNode] = [:]
    private var head: LruCacheNode?
    private var tail: LruCacheNode?
    
    init(_ cap: Int) {
        capacity = cap
    }
    
    func get(_ key: Int) -> Int {
        if let node = cacheMap[key] {
            moveToHead(node)
            return node.value
        } else {
            return -1
        }
    }
    
    func put(_ key: Int, _ value: Int) {
        if let node = cacheMap[key] {
            node.value = value
            moveToHead(node)
        } else {
            if cacheMap.count < capacity {
                let newNode = LruCacheNode(key, value)
                cacheMap[key] = newNode
                if cacheMap.count == 1 {
                    head = newNode
                    tail = newNode
                } else {
                    newNode.next = head
                    head?.prev = newNode
                    head = newNode
                }
            } else {
                if let last = tail {
                    cacheMap.removeValue(forKey: last.key)
                    tail = last.prev
                    tail?.next = nil
                    if tail == nil {
                        head = nil
                    }
                    put(key, value)
                }
            }
        }
    }
    
    private func moveToHead(_ node: LruCacheNode) {
        if node === head {
            return
        }
        if node === tail {
            tail = node.prev
        }
        node.prev?.next = node.next
        node.next?.prev = node.prev
        node.prev = nil
        node.next = head
        head?.prev = node
        head = node
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * let obj = LRUCache(capacity)
 * let ret_1: Int = obj.get(key)
 * obj.put(key, value)
 */
```