---
title: "1167 Tree Diameter"
date: 2025-06-18
layout: post
categories: [Baekjoon, Graph]
tags: [DFS, Tree, Algorithm]
---

Diameter of Tree: the longest distance between the nodes in tree
How did I solve?
1. Pick any node
2. Do dijkstra
3. Do dijkstra with the node that is furthest from the node I picked at first
4. The answer is the furthest distance from the node I did dijkstra at second

How could I improve?
1. It was better to use dfs instead of dijkstra. Since it is tree which does not have any cycle, there is only one path between any two nodes.
2. The link below shows another way to solve this problem. Let's say it is a binary tree. You can pick up any node from the tree, and the diameter is the deepest depth from left child + right child

https://www.acmicpc.net/board/view/159856
