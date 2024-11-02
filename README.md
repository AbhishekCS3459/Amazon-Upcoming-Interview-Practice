# Amazon-Upcoming-Interview-Practice

## Amazon SDE I | India | Online Assessment

This repository contains solutions to coding problems for Amazon's Software Development Engineer (SDE) I role assessment. Here, I’ve solved one of the code challenges independently, demonstrating my problem-solving approach for the "Amazon Books" problem.

---

## Problem Statement: Amazon Books

Amazon Books is a retail store currently launching a new novel series titled **"The Story of Amazon"**. The series consists of `n` volumes, numbered sequentially from `1` to `n`, which are all out of stock initially.

Starting today, Amazon will release exactly one volume per day over the next `n` days, eventually bringing all volumes back in stock. Each day, you want to buy as many volumes as possible under the following conditions:
1. You cannot buy a volume unless you already have all its prequels (i.e., volumes numbered less than it).
2. You cannot repurchase any volume you have already bought.

Your task is to determine the list of volumes you buy each day. Return an array of `n` arrays:
- The `i-th` array should contain the list of volumes you bought on day `i`, sorted in increasing order.
- If no volumes were bought on a particular day, include `-1` for that day.

---

### Examples

**Example 1**  
Input: `volumes = [2, 1, 4, 3]`  
Output: `[[−1], [1, 2], [−1], [3, 4]]`  
Explanation:
- **Day 1:** Volume `2` is available, but you don’t own volume `1`. So, you can’t buy anything on this day.
- **Day 2:** Volume `1` becomes available, allowing you to buy both volumes `1` and `2` as all prequels are now owned.
- **Day 3:** Volume `4` becomes available, but you’re missing volume `3`.
- **Day 4:** Volume `3` becomes available, allowing you to buy volumes `3` and `4`.

**Example 2**  
Input: `volumes = [1, 4, 3, 2, 5]`  
Output: `[[1], [−1], [−1], [2, 3, 4], [5]]`

---

### Constraints
- \(1 \leq n \leq 10^5\)
- \(1 \leq \text{volumes}[i] \leq n\)
- All elements in `volumes` are distinct.

---

## Solution solved by my own

Each day, a specific volume becomes available according to the order given in the array `volumes`. For each day, we check the highest possible volumes we can buy by ensuring all prior volumes are already in possession. If you have all prequels for a newly available volume, you can add it to your collection for that day. If no volumes are buyable on a given day, `-1` is recorded for that day.

## Code Solution

Here’s my C++ solution for this problem:

```cpp
#include <vector>
using namespace std;

vector<vector<int>> solve(vector<int> volumes, int n) {
    vector<int> avail(n + 1, 0);
    vector<vector<int>> ans(n);
    
    // Initializing with 0 as available since for vol 1, previous vol must be made available
    avail[0] = 1;
    for (int idx = 0; idx < n; ++idx) {
        int curr_vol = volumes[idx];
        
        // Mark this volume as available
        avail[curr_vol] = 1;
        
        // Check if the previous volume is available
        if (avail[curr_vol - 1]) {
            vector<int> temp;
            temp.push_back(curr_vol);
            
            // While the next volume is available and within bounds, add to list
            while (curr_vol + 1 <= n && avail[curr_vol + 1]) {
                temp.push_back(curr_vol + 1);
                curr_vol++;
            }
            ans[idx] = temp;
        } else {
            // Push -1 if no volumes can be bought on this day
            ans[idx].push_back(-1);
        }
    }
    return ans;
}
```

### Explanation of the Code
- We use an array `avail` to track availability of each volume.
- For each day (index `idx` in `volumes`), we mark the current volume as available.
- If the previous volume is available, we gather all volumes from the current up to the highest sequentially available volume.
- If no volumes can be bought on a given day, `-1` is added to that day’s result.

---

## Complexity Analysis
- **Time Complexity**: \(O(n)\), as we iterate over the list of volumes and perform constant-time operations for each volume.
- **Space Complexity**: \(O(n)\), as we store the availability and the result array.
