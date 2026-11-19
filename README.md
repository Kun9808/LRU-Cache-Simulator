# LRU Cache Simulator (Java)

A generic **LRU (Least Recently Used) Cache** built from scratch in core Java using a **HashMap + custom Doubly Linked List**, with a console menu and a request simulator that reports hit/miss statistics.

No frameworks and no `LinkedHashMap`. Every data structure is implemented by hand.

---

## Problem Statement

A cache stores a limited number of items for fast access. When it is full and a new item arrives, something must be removed. An **LRU cache** removes the item that has **not been used for the longest time**.

The challenge is doing `get` and `put` in **O(1)** time. This project solves it by combining:

- **HashMap**: finds any key in O(1)
- **Doubly Linked List**: moves or removes any node in O(1) and keeps the usage order

---

## How It Works

```
HashMap<K, Node>            key -> node reference (O(1) lookup)

head <-> [MRU] <-> [ ... ] <-> [LRU] <-> tail
 ^                                        ^
 dummy node                          dummy node
```

- `head` and `tail` are **dummy nodes**, which removes null-check edge cases.
- On every `get` or `put`, the node moves to the front (right after `head`) and becomes the **Most Recently Used**.
- When the cache is full, the node at `tail.prev` (the **Least Recently Used**) is removed from the list, and its key is deleted from the HashMap. This is why each node stores its key.

---

## Features

- Generic implementation: `LRUCache<K, V>`
- `put(key, value)`: insert or update
- `get(key)`: fetch and mark as most recently used
- `remove(key)`
- Automatic eviction of the least recently used item
- Display cache contents from MRU to LRU
- Statistics: hits, misses, evictions, hit ratio
- Console menu with input validation and exception handling
- Simulation mode with an 80/20 "hot data" request pattern

---

## Project Structure

```
LRUCacheSimulator/
├── src/
│   ├── Node.java
│   ├── LRUCache.java
│   ├── CacheStats.java
│   ├── Simulator.java
│   └── Main.java
└── README.md
```

---

## How to Compile and Run

**Requirements:** JDK 17 or newer.

**Command line**

```bash
javac src/*.java
java -cp src Main
```

**IDE (IntelliJ IDEA / Eclipse)**

1. Create a Java project and add the files from `src/`.
2. Run `Main.java`.

---

## Sample Output

**Manual usage (capacity = 3)**

```
Enter cache capacity: 3

1.Put  2.Get  3.Remove  4.Display  5.Stats  6.Simulate  7.Exit
Choice: 1
Key: A
Value: 1
MRU -> [A:1] <- LRU

... (put B:2 and C:3 the same way)

MRU -> [C:3] [B:2] [A:1] <- LRU

Choice: 2
Key: A
HIT -> 1

Choice: 1
Key: D
Value: 4
MRU -> [D:4] [A:1] [C:3] <- LRU        (B was evicted: least recently used)

Choice: 5
Hits: 1 | Misses: 0 | Evictions: 1 | Hit Ratio: 100.00%
```

**Simulation mode (example run)**

```
Cache capacity: 10
Number of requests: 1000
Key range (e.g. 20 means keys 1-20): 50
Hits: 812 | Misses: 188 | Evictions: 178 | Hit Ratio: 81.20%
```

*(Simulation numbers vary on each run because requests are random.)*

---

## Complexity

| Operation | Time Complexity | Notes |
|-----------|-----------------|-------|
| `get`     | O(1)            | HashMap lookup + move node to front |
| `put`     | O(1)            | Insert, update, or evict in constant time |
| `remove`  | O(1)            | HashMap removal + unlink node |
| `display` | O(n)            | Traverses the list |
| **Space** | O(capacity)     | One map entry and one node per item |

---

## Future Improvements

- JUnit test cases for eviction, update, and edge cases
- FIFO cache and an LRU vs FIFO hit ratio comparison
- LFU (Least Frequently Used) cache
- Thread safety using `ReentrantLock` or `ReadWriteLock`
- TTL (time-based expiry) for entries
- Save and load cache state to a file
- Swing / JavaFX user interface

---

**Kunal Tambe**
Pune, Maharashtra | tambe.k2004@gmail.com
