# Selection Sort

## What It Is
Selection Sort divides the array into a sorted and an unsorted part. On each pass, it finds the **minimum element** in the unsorted part and swaps it into the correct position at the front. Unlike Bubble Sort, it minimizes the number of swaps — at most `n-1` swaps total, regardless of input.

---

## Visual Walkthrough

Sample array:
```
[5, 1, 4, 2, 8]
```

### Pass 1
Find the minimum in the unsorted range `[0..4]`, swap it to index 0.
```
[5, 1, 4, 2, 8]  → min = 1 (index 1) → swap with index 0 → [1, 5, 4, 2, 8]
```
End of Pass 1: `[1, 5, 4, 2, 8]` — index 0 is now finalized.

### Pass 2
Find the minimum in `[1..4]`, swap it to index 1.
```
[1, 5, 4, 2, 8]  → min = 2 (index 3) → swap with index 1 → [1, 2, 4, 5, 8]
```
End of Pass 2: `[1, 2, 4, 5, 8]` — index 1 is now finalized.

### Pass 3
Find the minimum in `[2..4]`, swap it to index 2.
```
[1, 2, 4, 5, 8]  → min = 4 (index 2) → already in place → [1, 2, 4, 5, 8]
```

### Pass 4
Find the minimum in `[3..4]`, swap it to index 3.
```
[1, 2, 4, 5, 8]  → min = 5 (index 3) → already in place → [1, 2, 4, 5, 8]
```

### Final Sorted Array
```
[1, 2, 4, 5, 8]
```

**Key intuition:** After pass `i`, the first `i+1` elements are the smallest `i+1` elements of the array, placed in sorted order. The algorithm always scans the full remaining range on every pass — it can't detect an already-sorted array early, unlike Bubble Sort.

---

## Time & Space Complexity

| Ω (Best Case) | Θ (Average Case) | O (Worst Case) | Space | Stable | In-Place | Adaptive |
|---|---|---|---|---|---|---|
| Ω(n²) | Θ(n²) | O(n²) | O(1) | No | Yes | No |

- **Ω(n²) — Best Case:** Even a sorted array requires a full scan each pass to confirm the minimum — no early exit is possible.
- **Θ(n²) — Average Case:** Roughly n²/2 comparisons regardless of initial order.
- **O(n²) — Worst Case:** Same n²/2 comparisons; input order doesn't change comparison count.
- **Space — O(1):** In-place, uses only a few index/value variables.
- **Not Stable:** A standard swap-based implementation can change the relative order of equal elements.
- **Advantage:** Only up to `n-1` swaps total — useful when swap/write cost is expensive (e.g. flash memory).

---

## Optimized Implementation (No Built-in Sort Functions)

```java
/**
 * Sorts an array in ascending order using Selection Sort.
 * Time Complexity: O(n^2) in all cases (best, average, worst)
 * Space Complexity: O(1)
 */
public void selectionSort(int[] arr) {
    int n = arr.length;

    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;

        // Find the index of the minimum element in the unsorted range
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }

        // Swap only if a smaller element was found (minimizes writes)
        if (minIndex != i) {
            int temp = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = temp;
        }
    }
}
```

### Why This Version Is Optimized
1. **`minIndex` tracking** — only tracks the index of the smallest element found so far instead of swapping on every comparison, avoiding unnecessary writes.
2. **Conditional swap (`minIndex != i`)** — skips the swap entirely when the current element is already the minimum, keeping total swaps at or below `n-1`.
3. **No built-in functions used** — minimum-finding and swapping are done manually; no `Arrays.sort()`, `Collections.min()`, or library calls involved.

---

## Related LeetCode Problems
- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
- [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
- [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
- [414. Third Maximum Number](https://leetcode.com/problems/third-maximum-number/)
- [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

---

## When to Use Selection Sort
- Educational purposes — clean illustration of the "find extremum, place it" pattern used in Top-K/Kth-element problems.
- Situations where **minimizing writes/swaps** matters more than minimizing comparisons (e.g. writing to flash memory or EEPROM).
- **Not recommended** for large or nearly-sorted datasets — no early-exit optimization exists, so it always runs full O(n²) comparisons, unlike Bubble Sort or Insertion Sort which adapt to partially sorted input.
