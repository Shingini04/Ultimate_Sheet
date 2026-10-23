# First Occurrence in Window (Interview Revision Notes)

---

### Problem Statement
Given an array `arr` of size `N` and a window size `K`, find the first negative number in every contiguous subarray of size `K` (return `0` if none exists).

---

### Approach & Complexity Comparison

| Approach | Data Structure | Time Complexity | Space Complexity | Reason |
| :--- | :--- | :--- | :--- | :--- |
| **Brute Force** | None | O(N × K) | O(1) extra | Re-scans all K elements for every window. |
| **Set** | `std::set` / Ordered Map | O(N log K) | O(K) | Insertion/deletion per slide costs O(log K). |
| **Optimal** | `std::queue` (FIFO) | **O(N)** | **O(K)** | Oldest elements exit first; each element enters and leaves queue at most once. |

---

### Core Intuition
- Window elements follow **First-In, First-Out (FIFO)** order.
- Maintain a **Queue** storing only indices of negative numbers.
- The element at the queue front is always the **first negative number** of the current window.

---

Pattern Extensions
Use this queue-based sliding window pattern for any fixed-window question asking for the "first element satisfying property P":

First prime number in window

First odd/even number in window

First element greater than X or multiple of X

---

### Pseudocode (O(N) Time, O(K) Space)

```text
function findFirstNegative(arr, N, K):
    q = empty queue
    result = empty list

    // Step 1: Process initial window [0 ... K-1]
    for i from 0 to K - 1:
        if arr[i] < 0:
            q.push(i)
    
    // Record result for first window
    if q is not empty:
        result.append(arr[q.front()])
    else:
        result.append(0)

    // Step 2: Slide window from K to N - 1
    for i from K to N - 1:
        // Add new incoming element
        if arr[i] < 0:
            q.push(i)

        // Remove outgoing element if it was negative
        if arr[i - K] < 0:
            q.pop()

        // Record answer for current window
        if q is not empty:
            result.append(arr[q.front()])
        else:
            result.append(0)

    return result

