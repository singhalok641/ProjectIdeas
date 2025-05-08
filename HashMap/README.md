## Problem — Design a Generic HashMap

Implement a generic data structure `MyHashMap<K, V>` that stores key-value pairs with average-case **O(1)** time complexity for the following operations:

| Method                       | Description                                                                      |
| ---------------------------- | -------------------------------------------------------------------------------- |
| `void put(K key, V value)`   | Insert a key with its value. If the key already exists, overwrite the old value. |
| `V get(K key)`               | Return the value associated with `key`, or `null` if the key is not present.     |
| `boolean containsKey(K key)` | Return `true` if the map contains `key`; otherwise `false`.                      |
| `int size()`                 | Return the current number of key-value pairs.                                    |
| `void remove(K key)`         | Delete the mapping for `key` if it exists.                                       |

### Requirements

1. **Generics** – Your class must be declared `public class MyHashMap<K, V>`.
2. **Null Handling** – Support one `null` key and any number of `null` values (match Java’s `HashMap` behavior).
3. **Capacity & Load Factor** – Begin with an initial bucket array of length `16` and resize (double capacity) when `size > loadFactor × capacity`, where `loadFactor = 0.75`.
4. **Collision Strategy** – Use separate chaining with singly-linked lists.
   *You do **not** need to implement treeification or advanced optimizations.*
5. **Complexity** – Assume a perfect hash distribution.
   *Average-case* `put`, `get`, `remove`, `containsKey` → **O(1)**.
   `size` → **O(1)**.
6. **API Only** – You do **not** need to implement `keySet()`, `entrySet()`, serialization, or fail-fast iterators.

### Constraints

* Up to `10⁵` calls across all test cases.
* Keys’ `hashCode()` may be negative.
* Keys and values can be any reference type.

### Example

```java
MyHashMap<String, Integer> map = new MyHashMap<>();
map.put("apple", 5);
map.put("banana", 3);
map.put("apple", 7);        // update
System.out.println(map.get("apple"));      // 7
System.out.println(map.containsKey("pear"));// false
System.out.println(map.size());            // 2
map.remove("banana");
System.out.println(map.size());            // 1
```

### Notes

* Implement all methods as **public** and do not use `java.util.HashMap` internally.
* Your submission will be tested with multiple `MyHashMap` instances and interleaved method calls.
