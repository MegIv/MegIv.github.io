--- 
title: Khan's Algorithm
date: 2025-06-11 14:30:00 +0800
categories: [Blogging]
tags: [Algorithm Problems]
---

# 🔍 Kahn's Algorithm: Efficient Topological Sorting for DAGs

## What is Kahn's Algorithm? 🤔

**Kahn's Algorithm** is a method used for topological sorting in Directed Acyclic Graphs (DAGs). This algorithm is crucial for determining the order of tasks that have dependencies, ensuring that each task is performed in a valid sequence. The basic principle of Kahn's Algorithm is that it processes nodes in the graph by examining their incoming edges and ensuring that a node is processed only after its dependencies are completed.

### Key Concepts:
- **Topological Sort**: A linear ordering of vertices in a directed graph where for every directed edge (u,v), vertex u comes before v in the ordering
- **In-degree**: The number of incoming edges to a vertex
- **DAG**: Directed Acyclic Graph - a directed graph with no cycles

## How Kahn's Algorithm Works 🧠

### Step-by-Step Process:

1. **Calculate In-degrees**: Count the number of incoming edges for each vertex
2. **Initialize Queue**: Add all vertices with in-degree 0 to a queue (these have no dependencies)
3. **Process Queue**: 
   - Remove a vertex from the queue
   - Add it to the topological order
   - For each neighbor of this vertex, decrease its in-degree by 1
   - If any neighbor's in-degree becomes 0, add it to the queue
4. **Repeat**: Continue until the queue is empty
5. **Check for Cycles**: If all vertices are processed, the graph is acyclic; otherwise, it contains a cycle

### Visual Example:

```
Graph: A → D → C
       ↑
       F → B → D

Step 1: In-degrees: A=1, B=1, C=1, D=2, F=0
Step 2: Queue = [F]
Step 3: Process F → Queue = [A, B], Order = [F]
Step 4: Process A → Queue = [B], Order = [F, A] 
Step 5: Process B → Queue = [D], Order = [F, A, B]
Step 6: Process D → Queue = [C], Order = [F, A, B, D]
Step 7: Process C → Queue = [], Order = [F, A, B, D, C]
```

## When is Kahn's Algorithm Needed? 🗓️

### Primary Use Cases:

1. **Task Scheduling**: When tasks depend on one another (e.g., course prerequisites)
2. **Cycle Detection**: To detect cycles in a directed graph, ensuring no circular dependencies
3. **Dependency Management**: Managing tasks or resources with specific ordering constraints
4. **Build Systems**: Determining compilation order in software projects
5. **Project Management**: Organizing project tasks with dependencies

## Example Case: Course Prerequisites 📚

Consider a university course system with the following prerequisites:

- **Calculus I** → **Calculus II** → **Differential Equations**
- **Programming I** → **Data Structures** → **Algorithms**
- **Linear Algebra** → **Machine Learning**
- **Data Structures** → **Machine Learning**

Valid topological orders could include:
1. `Calculus I → Programming I → Linear Algebra → Calculus II → Data Structures → Differential Equations → Algorithms → Machine Learning`
2. `Programming I → Calculus I → Linear Algebra → Data Structures → Calculus II → Algorithms → Differential Equations → Machine Learning`

## Implementation Examples 💻

### Standard Kahn's Algorithm (C++)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>

using namespace std;

vector<int> kahnsAlgorithm(int numVertices, vector<vector<int>>& graph) {
    vector<int> inDegree(numVertices, 0);
    vector<int> topologicalOrder;
    queue<int> q;
    
    // Calculate in-degrees
    for (int i = 0; i < numVertices; i++) {
        for (int neighbor : graph[i]) {
            inDegree[neighbor]++;
        }
    }
    
    // Add vertices with 0 in-degree to queue
    for (int i = 0; i < numVertices; i++) {
        if (inDegree[i] == 0) {
            q.push(i);
        }
    }
    
    // Process vertices
    while (!q.empty()) {
        int current = q.front();
        q.pop();
        topologicalOrder.push_back(current);
        
        // Reduce in-degree of neighbors
        for (int neighbor : graph[current]) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }
    
    // Check for cycles
    if (topologicalOrder.size() != numVertices) {
        cout << "Graph contains a cycle!" << endl;
        return {};
    }
    
    return topologicalOrder;
}

int main() {
    int numVertices = 6;
    vector<vector<int>> graph(numVertices);
    
    // Building the graph: A=0, B=1, C=2, D=3, E=4, F=5
    graph[5].push_back(0); // F → A
    graph[5].push_back(1); // F → B
    graph[0].push_back(3); // A → D
    graph[1].push_back(3); // B → D
    graph[3].push_back(2); // D → C
    
    vector<int> result = kahnsAlgorithm(numVertices, graph);
    
    if (!result.empty()) {
        cout << "Topological Order: ";
        for (int vertex : result) {
            cout << char('A' + vertex) << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

### Python Implementation

```python
from collections import deque, defaultdict

def kahns_algorithm(graph, num_vertices):
    # Calculate in-degrees
    in_degree = [0] * num_vertices
    for vertex in graph:
        for neighbor in graph[vertex]:
            in_degree[neighbor] += 1
    
    # Initialize queue with vertices having 0 in-degree
    queue = deque()
    for i in range(num_vertices):
        if in_degree[i] == 0:
            queue.append(i)
    
    topological_order = []
    
    while queue:
        current = queue.popleft()
        topological_order.append(current)
        
        # Reduce in-degree of neighbors
        for neighbor in graph[current]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    # Check for cycles
    if len(topological_order) != num_vertices:
        return None  # Graph has a cycle
    
    return topological_order

# Example usage
def main():
    graph = defaultdict(list)
    graph[5] = [0, 1]  # F → A, B
    graph[0] = [3]     # A → D
    graph[1] = [3]     # B → D
    graph[3] = [2]     # D → C
    
    result = kahns_algorithm(graph, 6)
    
    if result:
        print("Topological Order:", [chr(ord('A') + v) for v in result])
    else:
        print("Graph contains a cycle!")

if __name__ == "__main__":
    main()
```

### Course Scheduling Example

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <string>
#include <unordered_map>

class CourseScheduler {
private:
    unordered_map<string, vector<string>> graph;
    unordered_map<string, int> inDegree;
    vector<string> courses;

public:
    void addCourse(const string& course) {
        if (graph.find(course) == graph.end()) {
            graph[course] = vector<string>();
            inDegree[course] = 0;
            courses.push_back(course);
        }
    }
    
    void addPrerequisite(const string& prerequisite, const string& course) {
        addCourse(prerequisite);
        addCourse(course);
        graph[prerequisite].push_back(course);
        inDegree[course]++;
    }
    
    vector<string> getSchedule() {
        queue<string> q;
        vector<string> schedule;
        
        // Add courses with no prerequisites
        for (const string& course : courses) {
            if (inDegree[course] == 0) {
                q.push(course);
            }
        }
        
        while (!q.empty()) {
            string current = q.front();
            q.pop();
            schedule.push_back(current);
            
            for (const string& nextCourse : graph[current]) {
                inDegree[nextCourse]--;
                if (inDegree[nextCourse] == 0) {
                    q.push(nextCourse);
                }
            }
        }
        
        if (schedule.size() != courses.size()) {
            cout << "Circular dependency detected!" << endl;
            return {};
        }
        
        return schedule;
    }
};

int main() {
    CourseScheduler scheduler;
    
    // Add prerequisites
    scheduler.addPrerequisite("Calculus I", "Calculus II");
    scheduler.addPrerequisite("Calculus II", "Differential Equations");
    scheduler.addPrerequisite("Programming I", "Data Structures");
    scheduler.addPrerequisite("Data Structures", "Algorithms");
    scheduler.addPrerequisite("Linear Algebra", "Machine Learning");
    scheduler.addPrerequisite("Data Structures", "Machine Learning");
    
    vector<string> schedule = scheduler.getSchedule();
    
    if (!schedule.empty()) {
        cout << "Recommended course schedule:" << endl;
        for (int i = 0; i < schedule.size(); i++) {
            cout << (i + 1) << ". " << schedule[i] << endl;
        }
    }
    
    return 0;
}
```

## Time and Space Complexity 📊

- **Time Complexity**: O(V + E)
  - V = number of vertices
  - E = number of edges
  - Each vertex and edge is processed exactly once

- **Space Complexity**: O(V)
  - For storing in-degrees, queue, and result array

## Advantages of Kahn's Algorithm ✅

- **Efficient**: Linear time complexity O(V + E)
- **Cycle Detection**: Can detect cycles in directed graphs
- **Intuitive**: Easy to understand and implement
- **Practical**: Directly applicable to real-world dependency problems
- **Stable**: Maintains relative order when possible

## Disadvantages of Kahn's Algorithm ❌

- **DAG Only**: Cannot work with cyclic graphs
- **Multiple Solutions**: May produce different valid orderings
- **Extra Space**: Requires additional data structures (in-degree array, queue)
- **No Preference**: Cannot prioritize between equally valid choices

## Advanced Variations 🚀

### 1. All Topological Sorts

```cpp
void findAllTopologicalSorts(vector<vector<int>>& graph, vector<int>& inDegree, 
                           vector<int>& path, vector<vector<int>>& result) {
    bool hasZeroInDegree = false;
    
    for (int i = 0; i < graph.size(); i++) {
        if (inDegree[i] == 0) {
            hasZeroInDegree = true;
            
            // Choose this vertex
            path.push_back(i);
            
            // Reduce in-degree of neighbors
            for (int neighbor : graph[i]) {
                inDegree[neighbor]--;
            }
            
            // Recursively find remaining sorts
            findAllTopologicalSorts(graph, inDegree, path, result);
            
            // Backtrack
            path.pop_back();
            for (int neighbor : graph[i]) {
                inDegree[neighbor]++;
            }
        }
    }
    
    if (!hasZeroInDegree && path.size() == graph.size()) {
        result.push_back(path);
    }
}
```

### 2. Lexicographically Smallest Topological Sort

```cpp
vector<int> lexicographicalTopologicalSort(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> inDegree(n, 0);
    
    for (int i = 0; i < n; i++) {
        for (int neighbor : graph[i]) {
            inDegree[neighbor]++;
        }
    }
    
    priority_queue<int, vector<int>, greater<int>> pq; // Min-heap
    
    for (int i = 0; i < n; i++) {
        if (inDegree[i] == 0) {
            pq.push(i);
        }
    }
    
    vector<int> result;
    
    while (!pq.empty()) {
        int current = pq.top();
        pq.pop();
        result.push_back(current);
        
        for (int neighbor : graph[current]) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] == 0) {
                pq.push(neighbor);
            }
        }
    }
    
    return result;
}
```

## Real-World Applications 🌍

### 1. Build Systems (Make/Gradle)
```bash
# Makefile dependencies
main.o: main.cpp utils.h
utils.o: utils.cpp utils.h
program: main.o utils.o
```

### 2. Package Management
```json
{
  "dependencies": {
    "express": "^4.18.0",
    "mongoose": "^6.0.0"
  },
  "devDependencies": {
    "jest": "^28.0.0",
    "nodemon": "^2.0.0"
  }
}
```

### 3. Task Scheduling in Operating Systems
- Process scheduling with dependencies
- Resource allocation ordering
- System initialization sequences

### 4. Database Operations
- Transaction ordering
- Schema migration dependencies
- Constraint satisfaction

### 5. Game Development
- Skill tree progression
- Quest dependencies
- Resource gathering orders

## Comparison with DFS-based Topological Sort 🔄

| Aspect | Kahn's Algorithm | DFS-based |
|--------|------------------|-----------|
| Approach | BFS-like (queue) | DFS with stack |
| Cycle Detection | Natural | Requires extra logic |
| Implementation | Iterative | Recursive/Iterative |
| Memory Usage | O(V) for queue | O(V) for recursion stack |
| Multiple Solutions | Easy to modify | More complex |
| Intuition | Processing dependencies | Finishing time ordering |

## Common Pitfalls and Solutions ⚠️

### 1. Forgetting Cycle Detection
```cpp
// Always check if all vertices are processed
if (result.size() != numVertices) {
    throw runtime_error("Graph contains a cycle");
}
```

### 2. Incorrect In-degree Calculation
```cpp
// Make sure to initialize all vertices
for (int i = 0; i < numVertices; i++) {
    inDegree[i] = 0; // Initialize before counting
}
```

### 3. Modifying Original Graph
```cpp
// Use a copy of in-degrees for the algorithm
vector<int> workingInDegree = inDegree; // Don't modify original
```

## Practice Problems 🎯

1. **Course Schedule** - LeetCode 207
2. **Course Schedule II** - LeetCode 210
3. **Alien Dictionary** - LeetCode 269 (Premium)
4. **Minimum Height Trees** - LeetCode 310
5. **Parallel Courses** - LeetCode 1136 (Premium)
6. **Sort Items by Groups Respecting Dependencies** - LeetCode 1203
7. **Build Order** - Cracking the Coding Interview

## Step-by-Step Example 📝

**Problem**: Schedule courses A, B, C, D, E with prerequisites:
- B requires A
- C requires B
- D requires A
- E requires C and D

```
Step 1: Build graph
A → [B, D]
B → [C]
C → [E]
D → [E]
E → []

Step 2: Calculate in-degrees
A: 0, B: 1, C: 1, D: 1, E: 2

Step 3: Initialize queue with zero in-degree
Queue: [A]

Step 4: Process A
Queue: [B, D], Result: [A]
Update in-degrees: B: 0, D: 0

Step 5: Process B
Queue: [D, C], Result: [A, B]
Update in-degrees: C: 0

Step 6: Process D
Queue: [C], Result: [A, B, D]
Update in-degrees: E: 1

Step 7: Process C
Queue: [E], Result: [A, B, D, C]
Update in-degrees: E: 0

Step 8: Process E
Queue: [], Result: [A, B, D, C, E]

Final order: A → B → D → C → E
```

## Conclusion 📝

**Kahn's Algorithm** is a fundamental and efficient method for topological sorting in Directed Acyclic Graphs. Its intuitive approach of processing nodes with no dependencies first makes it ideal for solving real-world problems involving task scheduling, course prerequisites, and dependency management.

The algorithm's ability to detect cycles naturally, combined with its linear time complexity, makes it a preferred choice for many applications. Understanding both the basic implementation and advanced variations opens up possibilities for solving complex dependency-related problems in software engineering, project management, and system design.

Whether you're building a course registration system, managing software dependencies, or organizing project tasks, Kahn's Algorithm provides a robust and efficient solution for maintaining proper ordering constraints.

---

## Additional Resources 📚

- [Introduction to Algorithms (CLRS) - Chapter 22](https://mitpress.mit.edu/books/introduction-algorithms-third-edition)
- [GeeksforGeeks - Kahn's Algorithm](https://www.geeksforgeeks.org/topological-sorting-indegree-based-solution/)
- [LeetCode - Topological Sort Problems](https://leetcode.com/tag/topological-sort/)
- [Visualgo - Graph Traversal](https://visualgo.net/en/dfsbfs)