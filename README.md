# Path-visualizer

## A* Algorithm 

A* algorithm is used in many real life sitiuations to find the shortest path where there could be many hindrances.Unlike many other algorithms it smarter which distinquishes it from other conventional path finding algorithms

## How the algorithm works?

First we need to understand the parameters used:
1. **g** represents the movement cost to move from the starting node to the current node.
2. **h** represents the estimated movement to move from the current node to the final node.This is not the actual distance however but an approximate distance calculated. We usually use either manhattan distance or Eucledian distance.
3. **f** is the sum of both g and h

Consider this grid with A as the start node and B as the end node
![image](https://user-images.githubusercontent.com/47849576/131640414-bcf421b5-339f-4e10-8dce-548332e1a14e.png)

We need to find the shortest path possible.As we start exploring the neighbors we can the g cost increasing while the h cost decreasing.On every exploration we pick the smallest f value

![image](https://user-images.githubusercontent.com/47849576/131640600-bec1ff11-5ac2-4590-aa71-8995daba9712.png)

Repeat the process until we find the shortest path

![image](https://user-images.githubusercontent.com/47849576/131641081-99f807c6-73d8-43cc-8e8e-cec4584d0e48.png)








