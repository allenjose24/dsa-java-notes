---
name: dsa-algorithm-docs
description: Generate compact, copy-paste-ready Markdown documentation for a Data Structures & Algorithms (DSA) topic — sorting algorithms, searching algorithms, array/string techniques, tree/graph traversals, recursion patterns, etc. Use this skill whenever the user asks to "document", "explain", or "write documentation/notes" for a DSA algorithm as part of their learning journey, or asks for a "cheat sheet" / "reference doc" for an algorithm. Always follow the exact structure, formatting, and conventions below (Java-only code, LeetCode-style function-only snippets, Big-O/Ω/Θ symbols, horizontal tables, hyperlinked LeetCode problems) — do not deviate unless the user explicitly asks for a change.
---

# DSA Algorithm Documentation Generator

Produces a single, compact Markdown file documenting one DSA algorithm/topic, matching a fixed template the user has approved. Output goes to a `.md` file (via `create_file`) and is presented with `present_files` — never just printed inline, since these docs are meant to be saved into the user's personal DSA documentation collection.

## Non-Negotiable Conventions

These rules were explicitly set by the user through iteration. Do not silently revert to defaults (e.g. Python, vertical tables, plain "O(n)" everywhere, `main` methods) — always follow these:

1. **Language: Java only.** No other language, ever, unless explicitly requested for that one instance.
2. **Code style: LeetCode-style, function-only.** Never include a `public class`, `main` method, or example driver/print code. Just the method itself (plus a short Javadoc comment above it). Assume it would be pasted directly into a LeetCode function stub.
3. **Complexity notation: use the actual symbols**, not the word "Big O":
   - `Ω` (Omega) for **Best Case**
   - `Θ` (Theta) for **Average Case**
   - `O` (Big O) for **Worst Case**
   - Space complexity still uses `O`.
4. **No repeated information.** State each complexity/fact exactly once. Don't have a table AND a separate "quick reference" table repeating the same values — one consolidated table + one bullet explanation list is enough.
5. **Tables are horizontal**, not vertical two-column (property/value) tables. Each characteristic (best/avg/worst/space/stable/in-place/adaptive) is its own column header, with values in a single row underneath.
6. **Related LeetCode problems section**, formatted as a Markdown hyperlinked list, exactly like:
   ```
   - [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
   ```
   Number + title inside the link text, direct leetcode.com URL as the href. Pick 3-6 problems genuinely relevant to the algorithm/pattern being documented (don't force a fit if the algorithm has no natural LeetCode analogue — say so briefly instead).
7. **Syntax-highlighted code blocks.** Always fence Java code with ```` ```java ```` (never a bare ```` ``` ````) so keywords/variables render with color in the rendered Markdown.
8. **Compact overall.** No filler paragraphs, no restating the same idea twice across sections. Prefer bullets over prose paragraphs.

## Exact Document Structure

Follow this section order and heading style precisely. Use `---` horizontal rules between major sections as shown.

```markdown
# <Algorithm Name>

## What It Is
<2-4 sentence plain-language explanation of the core idea/mechanism>

---

## Visual Walkthrough
Sample array/input:
```
<sample input, e.g. [5, 1, 4, 2, 8]>
```

### Pass 1 / Step 1
<Show the actual comparisons/operations happening, arrow-notation style:>
```
[5, 1, 4, 2, 8]  → compare (5,1) → swap  → [1, 5, 4, 2, 8]
...
```
<one-line takeaway after each pass/step>

### Pass 2 / Step 2
...(continue until fully resolved)...

### Final Result
```
<final sorted/found/computed output>
```

**Key intuition:** <one or two sentences on the invariant that makes the algorithm work>

---

## Time & Space Complexity

| Ω (Best Case) | Θ (Average Case) | O (Worst Case) | Space | Stable | In-Place | Adaptive |
|---|---|---|---|---|---|---|
| Ω(...) | Θ(...) | O(...) | O(...) | Yes/No | Yes/No | Yes/No |

- **Ω(...) — Best Case:** <why>
- **Θ(...) — Average Case:** <why>
- **O(...) — Worst Case:** <why>
- **Space — O(...):** <why>
- **Stable/Not Stable:** <one line>

(Omit Stable/In-Place/Adaptive columns if genuinely inapplicable to the algorithm type, e.g. searching algorithms — use judgment, but keep it a single horizontal table either way.)

---

## Optimized Implementation (No Built-in Sort/Search Functions)

```java
/**
 * <one-line description>
 * Time Complexity: O(...) worst/average, Ω(...) best case
 * Space Complexity: O(...)
 */
public <returnType> <methodName>(<params>) {
    // full working implementation
    // manual logic only — no Arrays.sort(), Collections.sort(),
    // Arrays.binarySearch(), etc. unless the algorithm IS a wrapper
    // around a lower-level primitive the user is expected to know
}
```

### Why This Version Is Optimized
1. **<optimization technique>** — <what it does and which complexity case it improves>
2. **<optimization technique>** — <explanation>
3. **No built-in functions used** — <confirm manual implementation>

---

## Related LeetCode Problems
- [<number>. <Title>](https://leetcode.com/problems/<slug>/)
- [<number>. <Title>](https://leetcode.com/problems/<slug>/)
- ...

---

## When to Use <Algorithm Name>
- <scenario where it's a good fit>
- <scenario where it's a good fit>
- **Not recommended** for <scenario>, because <reason> — better alternatives: <name them>

---
```

## Workflow

1. **Confirm the algorithm/topic** if ambiguous (e.g. "sorting" alone is too broad — ask which one, or if the user lists several, produce one file per algorithm unless they ask for a combined doc).
2. **Verify LeetCode problem numbers/titles/slugs** before including them — if unsure of the exact number or slug, search rather than guessing, since a wrong number or dead link defeats the purpose of the section.
3. **Write the file** using `create_file` to `/mnt/user-data/outputs/<algorithm_name>.md` (snake_case filename).
4. **Present it** with `present_files`.
5. Keep the chat response after presenting short — a one-line summary of what's included, no restating the whole doc in the chat.

## Notes on Reuse Across Topics

This template generalizes beyond sorting:
- **Searching algorithms** (binary search, linear search): "Visual Walkthrough" becomes step-by-step narrowing of a search range instead of passes/swaps.
- **Recursion/DP patterns**: "Visual Walkthrough" can show a call tree or table-filling sequence instead of array swaps; complexity table may drop Stable/In-Place/Adaptive columns since they don't apply — use judgment per the note above.
- **Graph/tree traversals**: sample "array" becomes a sample tree/graph (can represent as adjacency list or simple ASCII diagram in the code block).
- Always keep Java-only, function-only, symbol-based complexity, and the LeetCode links section regardless of topic.
