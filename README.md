Ex:2

Bfs:
class Graph:
    def __init__(self):
        self.graph = {}
    def add_edge(self, u, v):
        if u in self.graph:
            self.graph[u].append(v)
        else:
            self.graph[u] = [v]
            
    def bfs(self, start):
        visited = set()
        queue = [start]
        while queue:
            vertex = queue.pop(0)
            if vertex not in visited:
                print(vertex, end=' ')
                visited.add(vertex)
                queue.extend([neighbor for neighbor in self.graph.get(vertex, []) if neighbor not in visited])

g = Graph()
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
g.add_edge(2, 0)
g.add_edge(2, 3)
g.add_edge(3, 3)
print("BFS starting from vertex 2:")
g.bfs(2)


Dfs:
from collections import defaultdict

class Graph:
    def __init__(self):
        self.graph = defaultdict(list)
        
    def add_edge(self, u, v):
        self.graph[u].append(v)
        
    def dfs_util(self, v, visited):
        visited.add(v)
        print(v, end=' ')
        for neighbor in self.graph[v]:
            if neighbor not in visited:
                self.dfs_util(neighbor, visited)
                
    def dfs(self, start):
        visited = set()
        self.dfs_util(start, visited)


g = Graph()
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 2)
g.add_edge(2, 0)
g.add_edge(2, 3)
g.add_edge(3, 3)
print("DFS starting from vertex 2:")
g.dfs(2)


Ex:3

from collections import deque

def water_jug_problem(jug1_capacity, jug2_capacity, target):
    visited = set()
    queue = deque([(0, 0)])  # Initial state: (jug1, jug2)
    visited.add((0, 0))

    while queue:
        jug1, jug2 = queue.popleft()

        if jug1 == target or jug2 == target:
            return jug1, jug2

        # Possible actions: fill, empty, or pour
        actions = [
            (jug1_capacity, jug2),  # Fill jug1
            (jug1, jug2_capacity),  # Fill jug2
            (0, jug2),              # Empty jug1
            (jug1, 0),              # Empty jug2
            (min(jug1 + jug2, jug1_capacity), max(0, jug1 + jug2 - jug1_capacity)),  # Pour from jug2 to jug1
            (max(0, jug1 + jug2 - jug2_capacity), min(jug1 + jug2, jug2_capacity))   # Pour from jug1 to jug2
        ]

        for action in actions:
            if action not in visited:
                queue.append(action)
                visited.add(action)

    return None

# Example usage
jug1_capacity = 4
jug2_capacity = 3
target = 2

solution = water_jug_problem(jug1_capacity, jug2_capacity, target)
if solution:
    print(f"Solution found: ({solution[0]}, {solution[1]})")
else:
    print("No solution found.")

Ex:4

import math
import heapq

# Define the Cell class
class Cell:
  def __init__(self):
    self.parent_i = 0 # Parent cell's row index
    self.parent_j = 0 # Parent cell's column index
    self.f = float('inf') # Total cost of the cell (g + h)
    self.g = float('inf') # Cost from start to this cell
    self.h = 0 # Heuristic cost from this cell to destination

# Define the size of the grid
ROW = 9
COL = 10

# Check if a cell is valid (within the grid)
def is_valid(row, col):
  return (row >= 0) and (row < ROW) and (col >= 0) and (col < COL)

# Check if a cell is unblocked
def is_unblocked(grid, row, col):
  return grid[row][col] == 1

# Check if a cell is the destination
def is_destination(row, col, dest):
  return row == dest[0] and col == dest[1]

# Calculate the heuristic value of a cell (Euclidean distance to destination)
def calculate_h_value(row, col, dest):
  return ((row - dest[0]) ** 2 + (col - dest[1]) ** 2) ** 0.5

# Trace the path from source to destination
def trace_path(cell_details, dest):
  print("The Path is ")
  path = []
  row = dest[0]
  col = dest[1]

  # Trace the path from destination to source using parent cells
  while not (cell_details[row][col].parent_i == row and cell_details[row][col].parent_j == col):
    path.append((row, col))
    temp_row = cell_details[row][col].parent_i
    temp_col = cell_details[row][col].parent_j
    row = temp_row
    col = temp_col

  # Add the source cell to the path
  path.append((row, col))
  # Reverse the path to get the path from source to destination
  path.reverse()

  # Print the path
  for i in path:
    print("->", i, end=" ")
  print()

# Implement the A* search algorithm
def a_star_search(grid, src, dest):
  # Check if the source and destination are valid
  if not is_valid(src[0], src[1]) or not is_valid(dest[0], dest[1]):
    print("Source or destination is invalid")
    return

  # Check if the source and destination are unblocked
  if not is_unblocked(grid, src[0], src[1]) or not is_unblocked(grid, dest[0], dest[1]):
    print("Source or the destination is blocked")
    return

  # Check if we are already at the destination
  if is_destination(src[0], src[1], dest):
    print("We are already at the destination")
    return

  # Initialize the closed list (visited cells)
  closed_list = [[False for _ in range(COL)] for _ in range(ROW)]
  # Initialize the details of each cell
  cell_details = [[Cell() for _ in range(COL)] for _ in range(ROW)]

  # Initialize the start cell details
  i = src[0]
  j = src[1]
  cell_details[i][j].f = 0
  cell_details[i][j].g = 0
  cell_details[i][j].h = 0
  cell_details[i][j].parent_i = i
  cell_details[i][j].parent_j = j

  # Initialize the open list (cells to be visited) with the start cell
  open_list = []
  heapq.heappush(open_list, (0.0, i, j))

  # Initialize the flag for whether destination is found
  found_dest = False

  # Main loop of A* search algorithm
  while len(open_list) > 0:
    # Pop the cell with the smallest f value from the open list
    p = heapq.heappop(open_list)

    # Mark the cell as visited
    i = p[1]
    j = p[2]
    closed_list[i][j] = True

    # For each direction, check the successors
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0), (1, 1), (1, -1), (-1, 1), (-1, -1)]
    for dir in directions:
      new_i = i + dir[0]
      new_j = j + dir[1]

      # If the successor is valid, unblocked, and not visited
      if is_valid(new_i, new_j) and is_unblocked(grid, new_i, new_j) and not closed_list[new_i][new_j]:
        # If the successor is the destination
        if is_destination(new_i, new_j, dest):
          # Set the parent of the destination cell
          cell_details[new_i][new_j].parent_i = i
          cell_details[new_i][new_j].parent_j = j
          print("The destination cell is found")
          # Trace and print the path from source to destination
          trace_path(cell_details, dest)
          found_dest = True
          return
        else:
          # Calculate the new f, g, and h values
          g_new = cell_details[i][j].g + 1.0
          h_new = calculate_h_value(new_i, new_j, dest)
          f_new = g_new + h_new

          # If the cell is not in the open list or the new f value is smaller
          if cell_details[new_i][new_j].f == float('inf') or cell_details[new_i][new_j].f > f_new:
            # Add the cell to the open list
            heapq.heappush(open_list, (f_new, new_i, new_j))
            # Update the cell details
            cell_details[new_i][new_j].f = f_new
            cell_details[new_i][new_j].g = g_new
            cell_details[new_i][new_j].h = h_new
            cell_details[new_i][new_j].parent_i = i
            cell_details[new_i][new_j].parent_j = j

  # If the destination is not found after visiting all cells
  if not found_dest:
    print("Failed to find the destination cell")

def main():
  # Define the grid (1 for unblocked, 0 for blocked)
  grid = [
    [1, 0, 1, 1, 1, 1, 0, 1, 1, 1],
    [1, 1, 1, 0, 1, 1, 1, 0, 1, 1],
    [1, 1, 1, 0, 1, 1, 0, 1, 0, 1],
    [0, 0, 1, 0, 1, 0, 0, 0, 0, 1],
    [1, 1, 1, 0, 1, 1, 1, 0, 1, 0],
    [1, 0, 1, 1, 1, 1, 0, 1, 0, 0],
    [1, 0, 0, 0, 0, 1, 0, 0, 0, 1],
    [1, 0, 1, 1, 1, 1, 0, 1, 1, 1],
    [1, 1, 1, 0, 0, 0, 1, 0, 0, 1]
  ]

  # Define the source and destination
  src = [8, 0]
  dest = [0, 0]

  # Run the A* search algorithm
  a_star_search(grid, src, dest)

if __name__ == "__main__":
  main()


Ex:5

def TowerOfHanoi(n , source, destination, auxiliary):
  if n==1:
    print ("Move disk 1 from source",source,"to destination",destination)
    return
  TowerOfHanoi(n-1, source, auxiliary, destination)
  print ("Move disk",n,"from source",source,"to destination",destination)
  TowerOfHanoi(n-1, auxiliary, destination, source)
   
# Driver code
n = 4
TowerOfHanoi(n,'A','B','C')


Ex:6

import random
import numpy as np

# Coordinate of the points/cities
coordinate = np.array([[1,2], [30,21], [56,23], [8,18], [20,50], [3,4], [11,6], [6,7], [15,20], [10,9], [12,12]])

# Adjacency matrix for a weighted graph based on the given coordinates
def generate_matrix(coordinate):
    matrix = []
    for i in range(len(coordinate)):
        for j in range(len(coordinate)):      
            p = np.linalg.norm(coordinate[i] - coordinate[j])
            matrix.append(p)
    matrix = np.reshape(matrix, (len(coordinate),len(coordinate)))
    return matrix

# Finds a random solution
def solution(matrix):
    points = list(range(0, len(matrix)))
    solution = []
    for i in range(0, len(matrix)):
        random_point = points[random.randint(0, len(points) - 1)]
        solution.append(random_point)
        points.remove(random_point)
    return solution

# Calculate the path based on the random solution
def path_length(matrix, solution):
    cycle_length = 0
    for i in range(0, len(solution)):
        cycle_length += matrix[solution[i]][solution[i - 1]]
    return cycle_length

# Generate neighbors of the random solution by swapping cities and returns the best neighbor
def neighbors(matrix, solution):
    neighbors = []
    for i in range(len(solution)):
        for j in range(i + 1, len(solution)):
            neighbor = solution.copy()
            neighbor[i] = solution[j]
            neighbor[j] = solution[i]
            neighbors.append(neighbor)
             
    # Assume that the first neighbor in the list is the best neighbor     
    best_neighbor = neighbors[0]
    best_path = path_length(matrix, best_neighbor)
     
    # Check if there is a better neighbor
    for neighbor in neighbors:
        current_path = path_length(matrix, neighbor)
        if current_path < best_path:
            best_path = current_path
            best_neighbor = neighbor
    return best_neighbor, best_path

def hill_climbing(coordinate):
    matrix = generate_matrix(coordinate)
     
    current_solution = solution(matrix)
    current_path = path_length(matrix, current_solution)
    neighbor = neighbors(matrix, current_solution)[0]
    best_neighbor, best_neighbor_path = neighbors(matrix, neighbor)
 
    while best_neighbor_path < current_path:
        current_solution = best_neighbor
        current_path = best_neighbor_path
        neighbor = neighbors(matrix, current_solution)[0]
        best_neighbor, best_neighbor_path = neighbors(matrix, neighbor)
 
    return current_path, current_solution

final_solution = hill_climbing(coordinate)
print("The solution is \n", final_solution[1])


Ex:7

from queue import PriorityQueue

class PuzzleState:
    goal_state = (1, 2, 3, 4, 5, 6, 7, 8, 0)

    def __init__(self, state, parent=None, depth=0):
        self.state = state
        self.parent = parent
        self.depth = depth

    def __lt__(self, other):
        return self.depth < other.depth

    def get_neighbors(self):
        neighbors = []
        i = self.state.index(0)
        row, col = i // 3, i % 3

        for drow, dcol in ((1, 0), (0, 1), (-1, 0), (0, -1)):
            if 0 <= row + drow < 3 and 0 <= col + dcol < 3:
                neighbor_state = list(self.state)
                neighbor_row, neighbor_col = row + drow, col + dcol
                neighbor_i = 3 * neighbor_row + neighbor_col
                neighbor_state[i], neighbor_state[neighbor_i] = neighbor_state[neighbor_i], neighbor_state[i]
                neighbors.append(PuzzleState(tuple(neighbor_state), parent=self, depth=self.depth + 1))
        return neighbors

    def manhattan_distance(self):
        return sum(abs(self.state[i] // 3 - i // 3) + abs(self.state[i] % 3 - i % 3) for i in range(1, 9))

    def is_goal(self):
        return self.state == self.goal_state

def astar_search(initial_state):
    open_list = PriorityQueue()
    open_list.put(initial_state)
    visited = set()

    while not open_list.empty():
        current_state = open_list.get()

        if current_state.is_goal():
            return current_state

        visited.add(current_state)

        for neighbor in current_state.get_neighbors():
            if neighbor not in visited:
                open_list.put(neighbor)

    return None

if __name__ == "__main__":
    initial_state = PuzzleState((1, 2, 3, 4, 5, 6, 7, 0, 8))
    solution_state = astar_search(initial_state)
    if solution_state:
        path = []
        while solution_state:
            path.insert(0, solution_state.state)
            solution_state = solution_state.parent
        for state in path:
            for i in range(3):
                print(" ".join(map(str, state[i * 3:i * 3 + 3])))
            print()
    else:
        print("No solution found.")


Ex:8

N = 4
 

ld = [0] * 30
 

rd = [0] * 30
 

cl = [0] * 30
 

 
 
def printSolution(board):
    for i in range(N):
        for j in range(N):
            print(" Q " if board[i][j] == 1 else " . ", end="")
        print()
 

 
 
def solveNQUtil(board, col):
    
    if col >= N:
        return True
 
   
    for i in range(N):
      
        if (ld[i - col + N - 1] != 1 and rd[i + col] != 1) and cl[i] != 1:
           
            board[i][col] = 1
            ld[i - col + N - 1] = rd[i + col] = cl[i] = 1
 
         
            if solveNQUtil(board, col + 1):
                return True
 
          
            board[i][col] = 0 
            ld[i - col + N - 1] = rd[i + col] = cl[i] = 0
 
   
    return False
 

 
 
def solveNQ():
    board = [[0 for _ in range(N)] for _ in range(N)]
 
    if not solveNQUtil(board, 0):
        print("Solution does not exist")
        return False
 
    printSolution(board)
    return True
 
 

if __name__ == "__main__":
    solveNQ()


Ex:9

# Python3 program to implement traveling salesman
# problem using naive approach.
from sys import maxsize
from itertools import permutations
V = 4
 
# implementation of traveling Salesman Problem
def travellingSalesmanProblem(graph, s):
 
    # store all vertex apart from source vertex
    vertex = []
    for i in range(V):
        if i != s:
            vertex.append(i)
 
    # store minimum weight Hamiltonian Cycle
    min_path = maxsize
    next_permutation=permutations(vertex)
    for i in next_permutation:
 
        # store current Path weight(cost)
        current_pathweight = 0
 
        # compute current path weight
        k = s
        for j in i:
            current_pathweight += graph[k][j]
            k = j
        current_pathweight += graph[k][s]
 
        # update minimum
        min_path = min(min_path, current_pathweight)
         
    return min_path
 
 
# Driver Code
if __name__ == "__main__":
 
    # matrix representation of graph
    graph = [[0, 10, 15, 20], [10, 0, 35, 25],
            [15, 35, 0, 30], [20, 25, 30, 0]]
    s = 0
    print(travellingSalesmanProblem(graph, s))
