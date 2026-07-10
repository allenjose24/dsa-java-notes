# Merge Sort

## What It Is
Merge Sort is a **divide-and-conquer** algorithm. It recursively splits the array into halves until each piece has a single element (trivially sorted), then repeatedly merges pairs of sorted pieces back together into larger sorted sequences until the whole array is sorted.

---

## Visual Walkthrough

Sample array:
```
[5, 1, 4, 2, 8]
```

### Divide Phase
Split recursively into halves until single elements remain.
```
[5, 1, 4, 2, 8]
   ├── [5, 1]        → [5]     [1]
   └── [4, 2, 8]
          ├── [4]
          └── [2, 8]  → [2]    [8]
```

### Merge Phase
Merge sorted pieces back together, smallest-first, two elements at a time.
```
merge([5], [1])       → [1, 5]
merge([2], [8])       → [2, 8]
merge([4], [2, 8])    → [2, 4, 8]
merge([1, 5], [2, 4, 8]) → [1, 2, 4, 5, 8]
```

### Final Sorted Array
```
[1, 2, 4, 5, 8]
```

**Key intuition:** Merging two already-sorted lists takes just one linear pass — compare the fronts of both lists, take the smaller, and repeat. The recursion depth is `log n` (halving each time), and each level does `n` total work merging, giving `O(n log n)`.

---

## Time & Space Complexity

| Ω (Best Case) | Θ (Average Case) | O (Worst Case) | Space | Stable | In-Place | Adaptive |
|---|---|---|---|---|---|---|
| Ω(n log n) | Θ(n log n) | O(n log n) | O(n) | Yes | No | No |

- **Ω(n log n) — Best Case:** Even a sorted array must fully split and merge — no shortcut exists to skip recursion levels.
- **Θ(n log n) — Average Case:** `log n` levels of splitting, each doing `O(n)` merge work.
- **O(n log n) — Worst Case:** Same as best/average — merge sort's performance doesn't depend on input order.
- **Space — O(n):** Requires auxiliary arrays to hold elements during merging; not in-place.
- **Stable:** Equal elements retain their relative order, since the merge step takes from the left list first on ties.

---

## Optimized Implementation (No Built-in Sort Functions)

```java
/**
 * Sorts an array in ascending order using Merge Sort.
 * Time Complexity: O(n log n) in all cases (best, average, worst)
 * Space Complexity: O(n)
 */
public void mergeSort(int[] arr, int left, int right) {
    if (left >= right) {
        return; // Base case: single element is already sorted
    }

    int mid = left + (right - left) / 2;

    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}

private void merge(int[] arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    int[] leftArr = new int[n1];
    int[] rightArr = new int[n2];

    for (int i = 0; i < n1; i++) {
        leftArr[i] = arr[left + i];
    }
    for (int j = 0; j < n2; j++) {
        rightArr[j] = arr[mid + 1 + j];
    }

    int i = 0, j = 0, k = left;

    // Merge the two temp arrays back into arr, in sorted order
    while (i < n1 && j < n2) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k] = leftArr[i];
            i++;
        } else {
            arr[k] = rightArr[j];
            j++;
        }
        k++;
    }

    // Copy any remaining elements
    while (i < n1) {
        arr[k] = leftArr[i];
        i++;
        k++;
    }
    while (j < n2) {
        arr[k] = rightArr[j];
        j++;
        k++;
    }
}
```

### Why This Version Is Optimized
1. **`mid = left + (right - left) / 2`** — avoids integer overflow that `(left + right) / 2` could cause on large arrays.
2. **Tie-break (`leftArr[i] <= rightArr[j]`)** — taking from the left array on equal values preserves original relative order, keeping the sort stable.
3. **No built-in functions used** — merging is done manually with temp arrays and index tracking; no `Arrays.sort()`, `Collections.sort()`, or library calls involved.

---

## Related LeetCode Problems
- [148. Sort List](https://leetcode.com/problems/sort-list/)
- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
- [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
- [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)
- [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

---

## When to Use Merge Sort
- Large datasets where **guaranteed O(n log n)** performance matters, regardless of input order.
- Sorting **linked lists** — merge sort doesn't need random access, unlike Quick Sort's partitioning.
- Situations requiring a **stable sort** (e.g. sorting records by one key while preserving order from a prior sort on another key).
- **Not recommended** when memory is tight — the O(n) auxiliary space can be a drawback compared to in-place O(n log n) alternatives like Quick Sort or Heap Sort.
