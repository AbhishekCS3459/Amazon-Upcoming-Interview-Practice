
### Notes on Solving the Word Search Problem Using DFS

**Problem Summary**:
In this problem, I need to determine if a specific word can be found within a grid of characters by moving sequentially through adjacent cells (up, down, left, or right), without revisiting any cell in the same path.

**Issues I Faced and Solutions**:

1. **Incorrect Memoization (`dp` array)**:
   - Initially, I tried to use a `dp` array to store results for each cell position. However, this caused incorrect results since the DFS paths depend on both the visited cells (`vis`) and the current position in the word. Memoizing based only on the cell (`idx`, `jdx`) doesn’t work in this context.
   - **Solution**: I removed the `dp` array from the code, as memoization in DFS is unnecessary when path-specific states (like `vis`) are involved.

2. **Passing `vis` by Value**:
   - In my initial code, I was passing the `vis` matrix (which tracks visited cells) by value in the recursive DFS calls. This means that each call got a copy of `vis`, so any updates to `vis` within a recursive path didn’t carry over to the other recursive calls.
   - **Solution**: I fixed this by passing `vis` by reference. This way, all recursive calls within the same search path share the same `vis`, allowing backtracking to work correctly.

3. **Resetting `vis` for Each Starting Cell**:
   - I initially created the `vis` matrix only once in the `exist` function, before checking each starting cell. This caused unintended visits to carry over from previous searches.
   - **Solution**: I moved the initialization of `vis` inside the loop that iterates over possible starting cells. Now, each new starting cell search has a fresh `vis` matrix.


**Incorrect Code Done by me Initially** :
```cpp
class Solution {
public:
    int dx[4] = {1, -1, 0, 0};
    int dy[4] = {0, 0, -1, 1};
    int dp[10][10];
    bool dfs(int idx, int jdx, vector<vector<char>>& board,
             vector<vector<int>> vis, string& word, int curr) {
        if (curr == word.length() - 1) {
            return 1;
        }
        if (dp[idx][jdx] != -1)
            return dp[idx][jdx];
        vis[idx][jdx] = 1;
        int n = board.size(), m = board[0].size();

        for (int kdx = 0; kdx < 4; ++kdx) {
            int new_idx = idx + dx[kdx];
            int new_jdx = jdx + dy[kdx];

            if (new_idx >= 0 && new_idx < n && new_jdx >= 0 && new_jdx < m &&
                board[new_idx][new_jdx] == word[curr + 1] &&
                vis[new_idx][new_jdx] == 0) {

                if (dfs(new_idx, new_jdx, board, vis, word, curr + 1))
                    return dp[idx][jdx] = true;
            }
        }

        return dp[idx][jdx] = false;
    }
    bool exist(vector<vector<char>>& board, string word) {
        int n = board.size(), m = board[0].size();

        vector<vector<int>> vis(n, vector<int>(m, 0));
        for (int idx = 0; idx < n; ++idx) {
            for (int jdx = 0; jdx < m; ++jdx) {
                if (board[idx][jdx] == word[0]) {
                    memset(dp, -1, sizeof(dp));
                    if (dfs(idx, jdx, board, vis, word, 0))
                        return true;
                }
            }
        }

        return false;
    }
};
```

**Correct Code**:
```cpp
class Solution {
public:
    int dx[4] = {1, -1, 0, 0};
    int dy[4] = {0, 0, -1, 1};

    bool dfs(int idx, int jdx, vector<vector<char>>& board,
             vector<vector<int>>& vis, string& word, int curr) {
        if (curr == word.length() - 1) {
            return true;
        }
        
        vis[idx][jdx] = 1;  // Mark cell as visited
        int n = board.size(), m = board[0].size();

        for (int kdx = 0; kdx < 4; ++kdx) {
            int new_idx = idx + dx[kdx];
            int new_jdx = jdx + dy[kdx];

            if (new_idx >= 0 && new_idx < n && new_jdx >= 0 && new_jdx < m &&
                board[new_idx][new_jdx] == word[curr + 1] &&
                vis[new_idx][new_jdx] == 0) {

                if (dfs(new_idx, new_jdx, board, vis, word, curr + 1))
                    return true;
            }
        }
        
        vis[idx][jdx] = 0;  // Unmark cell to allow other paths
        return false;
    }
    
    bool exist(vector<vector<char>>& board, string word) {
        int n = board.size(), m = board[0].size();

        for (int idx = 0; idx < n; ++idx) {
            for (int jdx = 0; jdx < m; ++jdx) {
                if (board[idx][jdx] == word[0]) {
                    vector<vector<int>> vis(n, vector<int>(m, 0)); // Reset vis per start
                    if (dfs(idx, jdx, board, vis, word, 0))
                        return true;
                }
            }
        }
        return false;
    }
};
```

**Original Incorrect Code Example** (for reference):

```cpp
class Solution {
public:
    int dx[4] = {1, -1, 0, 0};
    int dy[4] = {0, 0, -1, 1};
    int dp[10][10]; // Memoization table, not suitable here

    bool dfs(int idx, int jdx, vector<vector<char>>& board,
             vector<vector<int>> vis, string& word, int curr) {
        if (curr == word.length() - 1) {
            return 1;
        }
        if (dp[idx][jdx] != -1)
            return dp[idx][jdx];
        
        vis[idx][jdx] = 1; // Mark cell as visited
        int n = board.size(), m = board[0].size();

        for (int kdx = 0; kdx < 4; ++kdx) {
            int new_idx = idx + dx[kdx];
            int new_jdx = jdx + dy[kdx];

            if (new_idx >= 0 && new_idx < n && new_jdx >= 0 && new_jdx < m &&
                board[new_idx][new_jdx] == word[curr + 1] &&
                vis[new_idx][new_jdx] == 0) {

                if (dfs(new_idx, new_jdx, board, vis, word, curr + 1))
                    return dp[idx][jdx] = true;
            }
        }
        
        return dp[idx][jdx] = false;
    }

    bool exist(vector<vector<char>>& board, string word) {
        int n = board.size(), m = board[0].size();

        vector<vector<int>> vis(n, vector<int>(m, 0)); // Create vis array
        for (int idx = 0; idx < n; ++idx) {
            for (int jdx = 0; jdx < m; ++jdx) {
                if (board[idx][jdx] == word[0]) {
                    memset(dp, -1, sizeof(dp)); // Reset dp
                    if (dfs(idx, jdx, board, vis, word, 0))
                        return true;
                }
            }
        }
        return false;
    }
};
```

--- 

By addressing these issues, I’ve improved the accuracy and efficiency of my DFS-based solution.
