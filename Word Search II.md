### **Word Search I (Problem 79)**

- **Problem Statement**: Given a grid of characters and a string, determine if the string can be formed by sequentially adjacent cells (horizontally or vertically). The same letter cell may not be used more than once.

- **Solution Approach**: 
  - **DFS Algorithm**: The solution employs a straightforward DFS to explore each cell of the grid to find the characters of the given word.
  - **State Tracking**: A `vis` (visited) array is used to track which cells have already been used in the current search path to avoid reusing letters.
  - **Base Case**: The search terminates successfully when the last character of the word is matched.

- **Complexity**:
  - **Time Complexity**: O(m * n * 4^l), where l is the length of the word (the worst-case scenario involves exploring every direction for each character).
  - **Space Complexity**: O(m * n) for the visited array.
Here’s a concise summary that captures the essential points for understanding **Word Search II** and the decision to use a **Trie** structure. This will help you grasp why a Trie is beneficial over a direct search for this problem, especially in comparison to **Word Search I**.

### Summary Notes for Word Search II

#### Problem Overview
- **Objective**: Given a 2D grid of characters and a list of words, return all words that can be formed by sequentially adjacent cells (horizontally or vertically). The same cell cannot be used more than once for a single word.
  
#### Key Differences from Word Search I
- **Word Search I** focuses on finding a single word within the grid using Depth-First Search (DFS).
- **Word Search II** requires identifying multiple words from a list, necessitating a more efficient searching method due to potential complexity with many words.

#### Why Use a Trie?
  I have used trie so that while doing the dfs for each words in the board if any time
   we find that the current path is not leading to any word present in words then we will stop 
   or prune their like we start from o->a->t->h and this has endOfWord=true means that word is present in words
   hence without traversing again through the words I came to know that the word is present in words
   if any time like starting word =a is not present in trie so we wont start from a instead we will start from
   e hence e->a->t --> endOfWord=true and hence it is marked true.
        
1. **Efficiency in Searching**:
   - The Trie structure allows us to store words in a way that common prefixes are shared, which reduces the memory footprint and speeds up the search process. This is particularly useful when the grid contains characters that can lead to multiple potential words.
   
2. **Pruning Searches**:
   - As we perform DFS on the grid, we can check against the Trie to determine if the current path can form any word in the list. If at any point the current path does not lead to a valid prefix in the Trie, we can stop further exploration along that path (pruning), significantly reducing unnecessary computations.

3. **Managing Multiple Words**:
   - With many words to search for, traversing the list of words for each DFS call would lead to repeated work and inefficiency. Using a Trie, we can check for the existence of a word as we build it, making it possible to gather results dynamically as we traverse the board.

#### Implementation Overview
- **TrieNode Structure**:
  - Each `TrieNode` contains:
    - `word`: to store the complete word when reached.
    - `endOfWord`: a boolean flag to indicate if the node marks the end of a valid word.
    - `children`: an array to hold pointers to the next characters.

- **Inserting Words into the Trie**:
  - Each word is inserted character by character into the Trie. This process ensures that all prefixes of the word are properly represented in the Trie.

- **DFS with Trie**:
  - The DFS is initiated from every cell in the grid that corresponds to a letter in the Trie.
  - As we traverse, we check if the current character leads to a valid node in the Trie:
    - If it does, we continue exploring in adjacent cells.
    - If a node marks the end of a word, we record that word.
  - The grid cell is temporarily marked to prevent reuse during the current word formation.

#### Why Not Use DFS Alone?
- A plain DFS approach would involve checking each possible combination of cells to form words. This would lead to:
  - **High Time Complexity**: As the number of words increases, the number of checks grows exponentially.
  - **Redundant Searches**: Without a Trie, we'd be redundantly searching through the list of words for each path explored, making the solution inefficient.

### Conclusion
Using a Trie in **Word Search II** enhances efficiency by allowing:
- Quick prefix checks,
- Early termination of non-promising paths,
- Dynamic word discovery while traversing the board, thereby handling multiple words effectively compared to a brute-force DFS approach.

These notes should provide you with a strong grasp of the problem, the motivation for using a Trie, and how it enhances the solution's efficiency in **Word Search II**.
