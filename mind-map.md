```text
Java Map Decision Guide (Java 8–25)

Need Speed                  → HashMap
                             • Hash Table
                             • Avg O(1)

Need Order                  → LinkedHashMap
                             • Hash Table + Doubly Linked List

Need Sorting                → TreeMap
                             • Red-Black Tree
                             • O(log n)

Need Concurrency            → ConcurrentHashMap
                             • Thread Safe
                             • Avg O(1)

Need Enum Keys              → EnumMap
                             • Array-based (Fastest for Enums)

Need Identity (==)          → IdentityHashMap
                             • Reference Equality

Need Auto Cleanup           → WeakHashMap
                             • Weak References + GC

Need Sorted + Thread Safe   → ConcurrentSkipListMap
                             • Skip List
                             • O(log n)
```

### Java Evolution

```text
Java 8
├── HashMap
│   └── Linked List → Red-Black Tree (Heavy Collisions)
│
└── ConcurrentHashMap
    └── Redesigned (CAS + Fine-grained Locking)

Java 9+
├── Map.of()
├── Map.ofEntries()
└── Map.copyOf()
    └── Immutable Maps
```

