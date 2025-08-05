---
title: "Topology Sort"
date: 2025-06-29
layout: post
categories: [Algorithm, Concepts]
tags: [DFS, Tree, Graph]
---

Topology Sort
=============

What is it?
-------------
Topological sorting is a way to arrange tasks in order.
It helps you figure out what needs to be done first, and what should come after.
In other words, it sorts things step by step — based on which tasks depend on others.
It can be also used to detect cycles in a directed graph.

Conditions for Using Topological Sort
-------------
1. The graph must be a directed graph
2. The graph must be acyclic

2 Ways to Implement
-------------
There are two common ways to implement topological sort: BFS-based and DFS-based approaches. 
Both methods solve the same problem, but they work in different ways and are useful in different situations.

BFS-based
-------------
This is the most popular method, especially in coding interviews and practical problems.

The idea is to keep track of how many incoming edges each node has. This is called the in-degree.
You start by adding all nodes with in-degree 0 into a queue — these are nodes with no prerequisites.
Then, one by one, you take a node from the queue, add it to the result, and reduce the in-degree of its neighbors.
If any neighbor’s in-degree becomes 0, you add it to the queue.

This method is easy to implement, detects cycles naturally (if not all nodes are processed), and guarantees that all dependencies are respected.

Code: BFS-Based
-------------
```cpp
int n, m;
vector<vector<int>> graph;
vector<int> indegree;

void topologicalSort() 
{
    queue<int> q;
    vector<int> result;

    for (int i = 1; i <= n; i++) 
    {
        if (indegree[i] == 0) q.push(i);
    }

    while (!q.empty()) 
    {
        int curr = q.front(); 
        q.pop();
        result.push_back(curr);

        for (int next : graph[curr]) 
        {
            indegree[next]--;
            if (indegree[next] == 0)
                q.push(next);
        }
    }

    if (result.size() < n) 
    {
        cout << "Cycle detected!" << '\n';
    } 
    else 
    {
        cout << "Topological Order: ";
        for (int x : result) cout << x << " ";
        cout << '\n';
    }
}
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);

    cin >> n >> m;
    graph.resize(n + 1);
    indegree.resize(n + 1, 0);

    for (int i = 0; i < m; i++) 
    {
        int u, v; // u → v
        cin >> u >> v;
        graph[u].push_back(v);
        indegree[v]++;
    }

    topologicalSort();
    return 0;
}
```

DFS-based
-------------
This method uses depth-first search and works by visiting all neighbors before finishing the current node.
You run DFS on all unvisited nodes.
When you finish visiting a node and all its dependencies, you add it to a stack or list.
Finally, you reverse the stack to get the topological order.
This method is useful when:
You are already using DFS for other purposes (like finding longest paths in a DAG),
Or you need more control over the traversal process.

Code: DFS-Based
-------------
```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

int n, m;
vector<vector<int>> graph;
vector<bool> visited;
stack<int> result;

vector<int> state;
bool hasCycle = false;

void dfs(int node) {
    state[node] = 1;
    visited[node] = true;

    for (int next : graph[node]) {
        if (state[next] == 1) {
            hasCycle = true;
            return;
        }
        if (!visited[next]) {
            dfs(next);
            if (hasCycle) return;
        }
    }

    state[node] = 2;
    result.push(node);
}

int main() {
    cin >> n >> m;
    graph.resize(n + 1);
    visited.resize(n + 1, false);
    state.resize(n + 1, 0); // 0: unvisited, 1: visiting, 2: visited

    for (int i = 0; i < m; i++) {
        int u, v; // u → v
        cin >> u >> v;
        graph[u].push_back(v);
    }

    for (int i = 1; i <= n; i++) {
        if (!visited[i]) dfs(i);
        if (hasCycle) break;
    }

    if (hasCycle) {
        cout << "Cycle detected!" << '\n';
    } else {
        cout << "Topological Order: ";
        while (!result.empty()) {
            cout << result.top() << " ";
            result.pop();
        }
        cout << endl;
    }

    return 0;
}
```

