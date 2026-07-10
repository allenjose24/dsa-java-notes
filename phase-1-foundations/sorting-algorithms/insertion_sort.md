# Insertion Sort

## What It Is
Insertion Sort builds the sorted array one element at a time. It takes each element from the unsorted part and inserts it into its correct position within the already-sorted part, shifting larger elements to the right to make room. It mimics how people sort playing cards in hand.

---

## Visual Walkthrough

Sample array:
```
[5, 1, 4, 2, 8]
```

### Pass 1 (key = 1)
Sorted portion is `[5]`. Compare `key` backward, shifting larger elements right.
```
[5, 1, 4, 2, 8]  → 1 < 5 → shift 5 right → insert 1 at index 0 → [1, 5, 4, 2, 8]
```

### Pass 2 (key = 4)
Sorted portion is `[1, 5]`.
```
[1, 5, 4, 2, 8]  → 4 < 5 → shift 5 right → 4 > 1 → stop, insert at index 1 → [1, 4, 5, 2, 8]
```

### Pass 3 (key = 2)
Sorted portion is `[1, 4, 5]`.
```
[1, 4, 5, 2, 8]  → 2 < 5 → shift → 2 < 4 → shift → 2 > 1 → stop, insert at index 1 → [1, 2, 4, 5, 8]
```

### Pass 4 (key = 8)
Sorted portion is `[1, 2, 4, 5]`.
```
[1, 2, 4, 5, 8]  → 8 > 5 → no shift needed → stays at index 4 → [1, 2, 4, 5, 8]
```

### Final Sorted Array
```
[1, 2, 4, 5, 8]
```

**Key intuition:** After pass `i`, the first `i+1` elements are sorted **relative to each other** (not necessarily in final position like Selection Sort — they can still shift right as smaller elements are inserted later).

---

## Time & Space Complexity

| Ω (Best Case) | Θ (Average Case) | O (Worst Case) | Space | Stable | In-Place | Adaptive |
|---|---|---|---|---|---|---|
| Ω(n) | Θ(n²) | O(n²) | O(1) | Yes | Yes | Yes |

- **Ω(n) — Best Case:** Array is already sorted; each element only needs one comparison against its left neighbor with no shifting.
- **Θ(n²) — Average Case:** Roughly n²/4 comparisons and shifts on random data.
- **O(n²) — Worst Case:** Array is in reverse order; every new element must shift all the way to index 0.
- **Space — O(1):** In-place, uses only a `key` variable and index tracking.
- **Stable:** Equal elements retain their relative order, since an element only shifts past strictly greater elements.

---

## Optimized Implementation (No Built-in Sort Functions)

```java
/**
 * Sorts an array in ascending order using Insertion Sort.
 * Time Complexity: O(n^2) worst/average, Ω(n) best case
 * Space Complexity: O(1)
 */
public void insertionSort(int[] arr) {
    int n = arr.length;

    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        // Shift elements greater than key one position to the right
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        // Place key in its correct position
        arr[j + 1] = key;
    }
}
```

### Why This Version Is Optimized
1. **Shifting instead of repeated swapping** — moves elements right in a single pass per key instead of doing pairwise swaps, halving the write operations compared to a swap-based approach.
2. **Early-exit inner loop (`arr[j] > key`)** — stops as soon as the correct position is found; on a sorted or nearly-sorted array this exits almost immediately, giving the **Ω(n) best case**.
3. **No built-in functions used** — shifting and placement are done manually with array indexing; no `Arrays.sort()`, `Collections.sort()`, or library calls involved.

---

## Related LeetCode Problems
- [147. Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/)
- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
- [75. Sort Colors](https://leetcode.com/problems/sort-colors/)
- [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

---

## When to Use Insertion Sort
- Small datasets, where its low overhead outperforms more complex O(n log n) algorithms.
- Nearly-sorted or streaming data — the adaptive early-exit makes it approach O(n) when input is close to sorted.
- **Not recommended** for large, randomly-ordered datasets — O(n²) average/worst case makes it inefficient compared to Merge Sort or Quick Sort at scale.
