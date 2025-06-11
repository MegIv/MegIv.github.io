--- 
title: Depth-First Search (DFS)
date: 2025-06-11 14:27:00 +0800
categories: [Blogging]
tags: [Algorithm Problems]
---

# 🕹️ Depth-First Search (DFS): A Deep Dive into Graph Exploration

## What is Depth-First Search (DFS)? 🤔

**Depth-First Search (DFS)** is an algorithm used for traversing or searching tree or graph data structures. It works by exploring as far as possible along a branch before backtracking. In simple terms, DFS will explore one branch fully before moving to the next.

### Example:
Imagine you're exploring a cave system with multiple tunnels. You'd follow one tunnel as deep as possible until you reach a dead end, then backtrack and try another tunnel. This is how DFS works — it explores one path fully before going to another branch.

## Key Concepts of DFS 🧠

1. **Stack-Based**: DFS uses a stack (Last In, First Out - LIFO) to store the nodes.
2. **Start at the Root (Node)**: Begin from the initial node, visit all its neighbors, and continue deeper into each branch.
3. **Backtrack**: If a node doesn't lead to the goal, DFS will backtrack and explore another branch.
4. **Recursive Nature**: DFS can be implemented both iteratively (with a stack) and recursively.

## Algorithm Steps for DFS 🔄

### Iterative Approach:
1. **Set the Start Node**: Select the starting node.
2. **Push the Start Node** into the stack.
3. **Visit the Top Node**: Pop the node from the top of the stack, mark it as visited, and check if it's the goal.
4. **Explore Neighbors**: For every unvisited neighbor, push it onto the stack and continue the process until the goal is found or the stack is empty.

### Recursive Approach:
1. **Mark Current Node as Visited**: Start with the initial node.
2. **Process Current Node**: Perform the required operation.
3. **Recursively Visit Neighbors**: For each unvisited neighbor, call DFS recursively.
4. **Backtrack**: Return when all neighbors are explored.

## Example Case 🧭

Consider the graph:

```
    1
   / \
  2   3
 / \   \
4   5   6
```

**DFS Exploration Path (starting from node 1):**
- **Iterative**: 1 → 3 → 6 → 2 → 5 → 4 (depends on order neighbors are added)
- **Recursive**: 1 → 2 → 4 → 5 → 3 → 6 (left-to-right traversal)

## DFS Implementation Examples 💻

### C++ Iterative Implementation

```cpp
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

void DFS_Iterative(int start, vector<vector<int>>& graph, vector<bool>& visited) {
    stack<int> s;
    s.push(start);
    
    while (!s.empty()) {
        int node = s.top();
        s.pop();
        
        if (!visited[node]) {
            visited[node] = true;
            cout << node << " ";
            
            // Add neighbors to stack (in reverse order for left-to-right traversal)
            for (int i = graph[node].size() - 1; i >= 0; i--) {
                if (!visited[graph[node][i]]) {
                    s.push(graph[node][i]);
                }
            }
        }
    }
}

void DFS_Recursive(int node, vector<vector<int>>& graph, vector<bool>& visited) {
    visited[node] = true;
    cout << node << " ";
    
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            DFS_Recursive(neighbor, graph, visited);
        }
    }
}

int main() {
    int n = 7; // nodes 0-6
    vector<vector<int>> graph(n);
    
    // Building the example graph
    graph[1] = {2, 3};
    graph[2] = {1, 4, 5};
    graph[3] = {1, 6};
    graph[4] = {2};
    graph[5] = {2};
    graph[6] = {3};
    
    vector<bool> visited(n, false);
    
    cout << "DFS Iterative traversal starting from node 1: ";
    DFS_Iterative(1, graph, visited);
    cout << endl;
    
    // Reset visited array
    fill(visited.begin(), visited.end(), false);
    
    cout << "DFS Recursive traversal starting from node 1: ";
    DFS_Recursive(1, graph, visited);
    cout << endl;
    
    return 0;
}
```

### Python Implementation

```python
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    result = []
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            result.append(node)
            
            # Add neighbors to stack (reverse order for consistent traversal)
            for neighbor in reversed(graph[node]):
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return result

def dfs_recursive(graph, node, visited=None, result=None):
    if visited is None:
        visited = set()
    if result is None:
        result = []
    
    visited.add(node)
    result.append(node)
    
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited, result)
    
    return result

# Example usage
graph = {
    1: [2, 3],
    2: [1, 4, 5],
    3: [1, 6],
    4: [2],
    5: [2],
    6: [3]
}

print("DFS Iterative:", dfs_iterative(graph, 1))
print("DFS Recursive:", dfs_recursive(graph, 1))
```

## Time and Space Complexity 📊

- **Time Complexity**: O(V + E)
  - V = number of vertices
  - E = number of edges
  
- **Space Complexity**: 
  - **Iterative**: O(V) for the stack
  - **Recursive**: O(h) where h is the height of the recursion tree (worst case O(V))

## Types of DFS 🔍

### 1. Pre-order DFS
- Process node before visiting children
- Most common form of DFS

### 2. Post-order DFS
- Process node after visiting all children
- Useful for topological sorting

### 3. In-order DFS (Binary Trees)
- Process left subtree, then node, then right subtree
- Used for binary search trees

## Real-World Applications of DFS 🌍

### 1. Pathfinding and Maze Solving
DFS can find a path through a maze, though not necessarily the shortest one.

```cpp
bool findPath(vector<vector<int>>& maze, int x, int y, int destX, int destY, 
              vector<vector<bool>>& visited) {
    if (x == destX && y == destY) return true;
    
    visited[x][y] = true;
    
    // Explore all 4 directions
    int dx[] = {-1, 1, 0, 0};
    int dy[] = {0, 0, -1, 1};
    
    for (int i = 0; i < 4; i++) {
        int nx = x + dx[i];
        int ny = y + dy[i];
        
        if (isValid(nx, ny, maze) && !visited[nx][ny] && maze[nx][ny] == 0) {
            if (findPath(maze, nx, ny, destX, destY, visited)) {
                return true;
            }
        }
    }
    
    return false;
}
```

### 2. Cycle Detection
Detect cycles in directed and undirected graphs.

### 3. Topological Sorting
Order vertices in a directed acyclic graph.

### 4. Connected Components
Find all connected components in an undirected graph.

### 5. Web Crawling
Explore web pages by following links deeply.

### 6. Game AI and Decision Trees
Explore game states and possible moves.

## Advantages of DFS ✅

- **Memory Efficient**: Uses less memory than BFS for wide graphs
- **Easy to Implement**: Straightforward implementation, especially recursive version
- **Finds Solution in Deep Graphs**: Excellent for exploring deep paths
- **Natural Recursion**: Maps well to recursive problems
- **Path Reconstruction**: Easy to reconstruct the path taken

## Disadvantages of DFS ❌

- **No Shortest Path Guarantee**: Does not guarantee the shortest path in unweighted graphs
- **Can Get Stuck**: May go very deep in infinite or very deep graphs
- **Stack Overflow Risk**: Recursive implementation may cause stack overflow
- **Not Optimal**: May find a solution but not the best one
- **Poor for Shortest Distance**: Inefficient for finding shortest distances

## DFS vs BFS Comparison 🔄

| Aspect | DFS | BFS |
|--------|-----|-----|
| Data Structure | Stack (LIFO) | Queue (FIFO) |
| Memory Usage | Lower | Higher |
| Shortest Path | Not guaranteed | Guaranteed (unweighted) |
| Implementation | Easier (recursive) | Slightly more complex |
| Space Complexity | O(h) | O(V) |
| Best Use Case | Deep searches, path existence | Shortest path, level exploration |
| Completeness | Yes (finite graphs) | Yes |
| Optimality | No | Yes (unweighted) |

## Step-by-Step DFS Execution Example 📝

Given graph: 1-2-4, 1-3-6, 2-5

```
Initial: Stack = [1], Visited = {}
Step 1:  Pop 1, mark visited → Stack = [2,3], Visited = {1}
Step 2:  Pop 3, mark visited → Stack = [2,6], Visited = {1,3}
Step 3:  Pop 6, mark visited → Stack = [2], Visited = {1,3,6}
Step 4:  Pop 2, mark visited → Stack = [4,5], Visited = {1,3,6,2}
Step 5:  Pop 5, mark visited → Stack = [4], Visited = {1,3,6,2,5}
Step 6:  Pop 4, mark visited → Stack = [], Visited = {1,3,6,2,5,4}
```

## Advanced DFS Techniques 🚀

### 1. DFS with Memoization
```cpp
unordered_map<int, bool> memo;

bool dfsWithMemo(int node, vector<vector<int>>& graph, int target) {
    if (memo.find(node) != memo.end()) {
        return memo[node];
    }
    
    if (node == target) {
        return memo[node] = true;
    }
    
    for (int neighbor : graph[node]) {
        if (dfsWithMemo(neighbor, graph, target)) {
            return memo[node] = true;
        }
    }
    
    return memo[node] = false;
}
```

### 2. DFS for Cycle Detection
```cpp
bool hasCycleDFS(int node, vector<vector<int>>& graph, 
                 vector<int>& color) {
    color[node] = 1; // Gray (being processed)
    
    for (int neighbor : graph[node]) {
        if (color[neighbor] == 1) return true; // Back edge found
        if (color[neighbor] == 0 && hasCycleDFS(neighbor, graph, color)) {
            return true;
        }
    }
    
    color[node] = 2; // Black (completely processed)
    return false;
}
```

### 3. DFS for Strongly Connected Components (Kosaraju's Algorithm)
```cpp
void dfsFirstPass(int node, vector<vector<int>>& graph, 
                  vector<bool>& visited, stack<int>& finishOrder) {
    visited[node] = true;
    
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            dfsFirstPass(neighbor, graph, visited, finishOrder);
        }
    }
    
    finishOrder.push(node);
}
```

## Common Pitfalls and Solutions ⚠️

### 1. Stack Overflow in Recursive DFS
**Problem**: Deep recursion causes stack overflow
**Solution**: Use iterative DFS or increase stack size

### 2. Infinite Loops in Cyclic Graphs
**Problem**: DFS gets stuck in cycles
**Solution**: Always maintain a visited set/array

### 3. Wrong Traversal Order
**Problem**: Unexpected order of node visits
**Solution**: Be careful about the order of adding neighbors

## Practice Problems 🎯

1. **Path Existence**: Check if a path exists between two nodes
2. **All Paths**: Find all paths between two nodes
3. **Island Counting**: Count connected components in a 2D grid
4. **Cycle Detection**: Detect cycles in directed graphs
5. **Topological Sort**: Order courses with prerequisites
6. **Binary Tree Paths**: Find all root-to-leaf paths
7. **Word Ladder**: Transform one word to another
8. **Sudoku Solver**: Use DFS with backtracking

## Conclusion 📝

**Depth-First Search (DFS)** is a fundamental algorithm for graph and tree traversal that explores paths deeply before backtracking. While it doesn't guarantee the shortest path, DFS is invaluable for problems requiring deep exploration, cycle detection, and path finding. Its simplicity and memory efficiency make it a go-to choice for many algorithmic problems.

Understanding both iterative and recursive implementations, along with their trade-offs, is crucial for applying DFS effectively in various scenarios. Whether you're solving mazes, detecting cycles, or exploring game states, DFS provides a robust foundation for graph-based problem solving.

---

## Additional Resources 📚

- [Visualgo DFS Animation](https://visualgo.net/en/dfsbfs)
- [GeeksforGeeks DFS Tutorial](https://www.geeksforgeeks.org/depth-first-search-or-dfs-for-a-graph/)
- [LeetCode DFS Problems](https://leetcode.com/tag/depth-first-search/)
- [DFS Applications in Competitive Programming](https://cp-algorithms.com/graph/depth-first-search.html)