# Bubble Sort

## What It Is
Bubble Sort is a comparison-based sorting algorithm. It repeatedly steps through the array, compares adjacent elements, and swaps them if they are in the wrong order. With each full pass, the largest unsorted element "bubbles up" to its correct position at the end of the array — hence the name.

---

## Visual Walkthrough

Sample array:
```
[5, 1, 4, 2, 8]
```

### Pass 1
Compare each pair of adjacent elements, swap if left > right.

```
[5, 1, 4, 2, 8]  → compare (5,1) → swap  → [1, 5, 4, 2, 8]
[1, 5, 4, 2, 8]  → compare (5,4) → swap  → [1, 4, 5, 2, 8]
[1, 4, 5, 2, 8]  → compare (5,2) → swap  → [1, 4, 2, 5, 8]
[1, 4, 2, 5, 8]  → compare (5,8) → no swap → [1, 4, 2, 5, 8]
```
End of Pass 1: `[1, 4, 2, 5, 8]` — largest element (8) is now in its final position.

### Pass 2
```
[1, 4, 2, 5, 8]  → compare (1,4) → no swap
[1, 4, 2, 5, 8]  → compare (4,2) → swap  → [1, 2, 4, 5, 8]
[1, 2, 4, 5, 8]  → compare (4,5) → no swap
```
(Last element already sorted, so we skip it.)
End of Pass 2: `[1, 2, 4, 5, 8]`

### Pass 3
```
[1, 2, 4, 5, 8]  → compare (1,2) → no swap
[1, 2, 4, 5, 8]  → compare (2,4) → no swap
```
No swaps occurred → **array is already sorted**. We can stop early here.

### Final Sorted Array
```
[1, 2, 4, 5, 8]
```

**Key intuition:** After pass `i`, the last `i` elements are guaranteed to be in their correct, final position. So each subsequent pass needs to check fewer elements.

---

## Time & Space Complexity

| Ω (Best Case) | Θ (Average Case) | O (Worst Case) | Space | Stable | In-Place | Adaptive |
|---------------|-------------------|-----------------|-------|--------|----------|----------|
| Ω(n) | Θ(n²) | O(n²) | O(1) | Yes | Yes | Yes |

- **Ω(n) — Best Case:** Array is already sorted; one pass with no swaps confirms it (only possible with the optimized/early-exit version).
- **Θ(n²) — Average Case:** Roughly n²/2 comparisons and swaps on random data.
- **O(n²) — Worst Case:** Array is in reverse order; every element must be compared and swapped every pass.
- **Space — O(1):** Sorting is done in-place, using only a few extra variables (no auxiliary array needed).
- **Stable:** Equal elements retain their relative order.

---

## Optimized Implementation (No Built-in Sort Functions)

```java
/**
 * Sorts an array in ascending order using an optimized Bubble Sort.
 * Time Complexity: O(n^2) worst/average, Ω(n) best case
 * Space Complexity: O(1)
 */
public void bubbleSort(int[] arr) {
    int n = arr.length;

    for (int i = 0; i < n - 1; i++) {
        boolean swapped = false; // Optimization flag

        // Last i elements are already sorted, so skip them
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                // Manual swap without built-in swap functions
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }

        // If no swaps occurred, array is already sorted — stop early
        if (!swapped) {
            break;
        }
    }
}
```

### Why This Version Is Optimized
1. **`swapped` flag** — if a full pass completes with zero swaps, the array is already sorted, so the algorithm exits early instead of running all `n-1` passes unnecessarily. This is what gives the **Ω(n) best case**.
2. **Shrinking inner loop (`n - 1 - i`)** — each pass ignores the last `i` elements, since they're already guaranteed sorted from previous passes. This avoids redundant comparisons.
3. **No built-in functions used** — swapping is done manually with a temp variable; no `sort()`, `Arrays.sort()`, or library calls involved.

---

## Related LeetCode Problems
- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
- [75. Sort Colors](https://leetcode.com/problems/sort-colors/)
- [148. Sort List](https://leetcode.com/problems/sort-list/)
- [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
- [179. Largest Number](https://leetcode.com/problems/largest-number/)

---

## When to Use Bubble Sort
- Educational purposes — great for understanding comparison-based sorting and algorithmic optimization.
- Small datasets or nearly-sorted data where the early-exit optimization shines (near Ω(n)).
- **Not recommended** for large datasets — O(n²) average/worst case makes it inefficient compared to O(n log n) algorithms like Merge Sort or Quick Sort.

---
