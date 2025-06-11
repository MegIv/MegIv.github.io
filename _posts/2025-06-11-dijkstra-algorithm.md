--- 
title: Dijkstra's Algorithm
date: 2025-06-11 14:39:00 +0800
categories: [Blogging]
tags: [Algorithm Problems]
---

# 🛣️ Dijkstra's Algorithm: Finding the Shortest Path in Graphs

## What is Dijkstra's Algorithm? 🤔

Dijkstra's Algorithm is a method used to find the shortest path between two points in a graph. The graph consists of nodes (vertices) and edges (connections between nodes) with specific weights, representing the distance or cost between nodes. Dijkstra's Algorithm helps determine the shortest path from a starting point to a target point by considering all possible paths and their respective distances.

## How Dijkstra's Algorithm Works 🧠

1. **Initialize the table**: Set up a table where each node has a distance value, with the initial node having a distance of zero, and all other nodes having an infinite distance.

2. **Select the initial node**: Pick the starting node and calculate the shortest distance to all other nodes directly connected to it.

3. **Update the distances**: For each neighboring node, update the distance if a shorter path is found through the current node.

4. **Repeat the process**: Continue visiting nodes, updating the distances, and selecting the node with the smallest tentative distance until the destination node is reached.

5. **Output the shortest path**: Once the destination node is reached, the path that was followed gives the shortest route.

## Example Problem: Shortest Path from Node A to Node F 🚗

Consider the following graph where you need to find the shortest path from node A to node F:

```
A —1— B —2— D —4— F
|      |      |
5      3      6
|      |      |
C —7— E
```

**Shortest Path**: A → B → D → F with a total weight of 4.

## Dijkstra's Algorithm Code Example 💻

Here's an example of Dijkstra's Algorithm in C++:

```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;

void Dijkstra(int n, vector<vector<pair<int, int>>>& graph, int start) {
    vector<int> dist(n, INT_MAX); // Initialize distances as infinity
    dist[start] = 0; // Distance to start node is 0

    for (int i = 0; i < n - 1; ++i) {
        // Find the node with the smallest distance
        int u = -1;
        for (int j = 0; j < n; ++j) {
            if (dist[j] != INT_MAX && (u == -1 || dist[j] < dist[u])) {
                u = j;
            }
        }

        // Update distances for neighboring nodes
        for (auto& neighbor : graph[u]) {
            int v = neighbor.first;
            int weight = neighbor.second;
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
            }
        }
    }

    // Output the shortest distance to each node
    for (int i = 0; i < n; ++i) {
        cout << "Distance from " << start << " to " << i << ": " << dist[i] << endl;
    }
}

int main() {
    int n = 6; // Number of nodes
    vector<vector<pair<int, int>>> graph(n);

    // Add edges (node, weight)
    graph[0].push_back({1, 1}); // A -> B
    graph[1].push_back({2, 2}); // B -> D
    graph[2].push_back({3, 4}); // D -> F
    graph[0].push_back({3, 5}); // A -> C
    graph[3].push_back({4, 3}); // C -> E
    graph[4].push_back({5, 6}); // E -> F

    Dijkstra(n, graph, 0); // Start from node A
    return 0;
}
```

## Advantages of Dijkstra's Algorithm ✅

- **Guaranteed Shortest Path**: Dijkstra's algorithm ensures that the shortest path is always found, provided all edge weights are non-negative.

- **Efficient for Dense Graphs**: It's very effective for graphs that are densely connected, such as GPS routing or network traffic management.

- **Works with Many Applications**: Dijkstra's algorithm is used in a variety of real-world applications, such as network routing, navigation, and logistics.

## Disadvantages of Dijkstra's Algorithm ❌

- **Does Not Handle Negative Weights**: Dijkstra's algorithm doesn't work correctly if there are negative edge weights, as it assumes that the shortest path can only be found by adding positive weights.

- **Slow for Sparse Graphs**: In very large graphs that are sparse (not many edges), Dijkstra can become slow unless optimized with a priority queue or heap.

- **Computational Overhead**: If you're only interested in the shortest path between two specific nodes, Dijkstra's algorithm calculates all paths to all nodes, which can be inefficient.

## Real-World Applications of Dijkstra's Algorithm 🌍

- **Navigation Systems**: Dijkstra's Algorithm is widely used in GPS systems to find the shortest route between locations, considering real-time traffic data and road conditions.

- **Network Routing**: It's used in routing protocols like OSPF and IS-IS to determine the best path for data packets across a network.

- **Logistics**: Companies use Dijkstra's Algorithm to optimize delivery routes, ensuring the most efficient path for transporting goods.

- **Flight Booking**: Dijkstra's algorithm helps airlines find the shortest or least expensive flight paths between cities.

## Conclusion 📝

Dijkstra's Algorithm is a highly effective method for finding the shortest path in graphs with non-negative edge weights. It is widely used in various applications, from GPS navigation to network routing, making it an essential tool for solving optimization problems. While it has its limitations with negative weights and sparse graphs, its reliability in finding the optimal path makes it a go-to algorithm in many real-world scenarios.