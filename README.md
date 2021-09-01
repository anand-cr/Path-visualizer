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


## Project walk through


### Setting up the grid

```
import pygame
import math
from queue import PriorityQueue

WIDTH = 800
WIN = pygame.display.set_mode((WIDTH, WIDTH))
pygame.display.set_caption("A* Pathfinding")

RED = (255,0,0)
GREEN = (0,255,0)
YELLOW = (255,255,0)
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
PURPLE = (128, 0, 128)
ORANGE = (255, 165, 0)
GREY = (128, 128, 128)
TURQUOISE = (64, 224, 208)

```

### Setting up colors

In this section we configure the color of each block. Each block can be a start block end block barrier or a free node to move to

To make the rectangle lines appear


```
def draw(self, win):
        pygame.draw.rect(win, self.colour, (self.x, self.y, self.width, self.width))
```



Now we need to check which of the blocks are free to move to. ie, check if there are barriers on the way.
If the space is free then we can add them to a list of neighbours.We will calculate the heurestic function of these nodes later.




```
def update_neighbours(self, grid):
        self.neighbours = []
        if self.row < self.total_rows - 1 and not grid[self.row +1][self.col].is_barrier():  #DOWN
            self.neighbours.append(grid[self.row +1][self.col])

        if self.row > 0 and not grid[self.row - 1][self.col].is_barrier():   # UP
            self.neighbours.append(grid[self.row -1][self.col])

        if self.col < self.total_rows - 1 and not grid[self.row][self.col+ 1].is_barrier():  #RIGHT
            self.neighbours.append(grid[self.row ][self.col+1])

        if self.col > 0 and not grid[self.row][self.col - 1].is_barrier():  #LEFT
            self.neighbours.append(grid[self.row][self.col - 1]
```

Once we have the path we need to outline the path:

```
def reconstruct_path(came_from, current, draw):
    while current in came_from:
        current = came_from[current]
        current.make_path()
        draw()
```

Now the actual bulk of algorithm comes in:

We define our heurestic function. Here we are using the manhattan function. We here take the end node and co-ordinates of the neighbors of the current node and decide which is the best option to take.

```
def h(p1, p2):
    x1, y1 = p1
    x2, y2 = p2
    return abs(x1-x2) + abs(y1-y2)

```

Here the main algorithm takes a function draw ,start and end nodes with the grid structure

Step 1: add the start score with its f score into the open_set
        Count will help  us break ties in case there are same f scores we can choose which was added first.
        Came from will keep track of where we came from (A -> B -> c ...)
        
        

```

def algorithm(draw, grid, start, end):
    count = 0                                    # count to show which node we are in
    open_set = PriorityQueue()                   # returns the smallest value in the list
    open_set.put((0, count, start))              #put the first node count and the start node itself
    came_from = {}                               #Keep a dictionary to keep track of the node it we came from
    
    # to calculate the g score, initialise it with inf
    g_score = {spot: float("inf") for row in grid for spot in row} 
    g_score[start] = 0
    f_score = {spot: float("inf") for row in grid for spot in row}
    f_score[start] = h(start.get_pos(), end.get_pos())
    
  ```
 
        Open_set_hash will have keep track of what is in the priority queue
        case: Open set is empty 
                -T his means we have considered all possible nodes and if we didnt find a path, then the path doesnt                  exist
        Get the current node,and sync with the open set hash
        if your cuurent == end node then we need to constuct the path
        
        
        
        
        
 
 ```
    
    open_set_hash = {start}
    #Quit condition
    while not open_set.empty():
        for event in pygame.event.get():
            if event.type ==pygame.QUIT:
                pygame.quit()
                
                
        current = open_set.get()[2]      # returns the best value
        open_set_hash.remove(current)    
        
        if current == end:
            reconstruct_path(came_from, end, draw)
            end.make_end()
            start.make_start()
            return True
            
            
```
        Consider the neighbors of the current node and the gscore of the neighbor will be 1 more than the current           node 
        if that gscore is better than gscore of neighbor
                -update the came from , gscore, and find the new f score
                -Now since it is has a better path, we put it into the open set if is not there already
                -sync with open set hash and make it open
                - if current != start then we close it since we already considered it 
                
        
        
```

        for neighbour in current.neighbours:
            temp_g_score = g_score[current]+1

            if temp_g_score < g_score[neighbour]:
                came_from[neighbour] = current
                g_score[neighbour] = temp_g_score
                f_score[neighbour] = temp_g_score + h(neighbour.get_pos(), end.get_pos())
                if neighbour not in open_set_hash:
                    count += 1
                    open_set.put((f_score[neighbour], count, neighbour))
                    open_set_hash.add(neighbour)
                    neighbour.make_open()
        draw()
        
        
        if current != start:
            current.make_closed()

    return False
 
 ```






