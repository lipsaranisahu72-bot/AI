#1 Depth-First Search (DFS)
Aim: Write a Python program to implement Depth-First Search (DFS) for a given graph containing at least 5 nodes.
Program:
graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': [],
    'F': []
}

visited = set()

def DFS(node):
    if node not in visited:
        print(node, end=" ")
        visited.add(node)

        for neighbour in graph[node]:
            DFS(neighbour)

print("DFS Traversal:")
DFS('A')

Output:
DFS Traversal:
A B D E C F
__________________________
2. Breadth-First Search (BFS)
Aim: Write a Python program to implement Breadth-First Search (BFS) using a queue.
Program:
from collections import deque

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': [],
    'F': []
}

def BFS(start):
    visited = set()
    queue = deque([start])

    visited.add(start)

    while queue:
        node = queue.popleft()
        print(node, end=" ")

        for neighbour in graph[node]:
            if neighbour not in visited:
                visited.add(neighbour)
                queue.append(neighbour)

print("BFS Traversal:")
BFS('A')

Output:
BFS Traversal:
A B C D E F
________________________
3. Uniform Cost Search (UCS)
Aim: Write a Python program to implement Uniform Cost Search for finding the shortest path in a weighted graph.
Program:
import heapq

graph = {
    'A': [('B', 1), ('C', 4)],
    'B': [('D', 2), ('E', 5)],
    'C': [('F', 3)],
    'D': [],
    'E': [('F', 1)],
    'F': []
}

def UCS(start, goal):
    pq = [(0, start, [start])]
    visited = set()

    while pq:
        cost, node, path = heapq.heappop(pq)

        if node == goal:
            return path, cost

        if node in visited:
            continue

        visited.add(node)

        for neighbour, weight in graph[node]:
            if neighbour not in visited:
                heapq.heappush(
                    pq,
                    (cost + weight, neighbour,
                     path + [neighbour])
                )

    return None, float('inf')


path, cost = UCS('A', 'F')

print("Shortest Path:", path)
print("Total Cost:", cost)

Output:
Shortest Path: ['A', 'B', 'E', 'F']
Total Cost: 7
__________________________
4. A* Search Algorithm
Aim: Write a Python program to implement the A* Search algorithm using the Manhattan Distance heuristic.
Program:
import heapq

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': ['G'],
    'E': ['G'],
    'F': ['G'],
    'G': []
}

# Coordinates of nodes
pos = {
    'A': (0, 0),
    'B': (1, 0),
    'C': (0, 1),
    'D': (2, 0),
    'E': (1, 1),
    'F': (0, 2),
    'G': (2, 2)
}

def heuristic(node, goal):
    x1, y1 = pos[node]
    x2, y2 = pos[goal]

    return abs(x1-x2) + abs(y1-y2)


def A_star(start, goal):
    pq = [(heuristic(start, goal), 0, start, [start])]
    visited = set()

    while pq:
        f, g, node, path = heapq.heappop(pq)

        if node == goal:
            return path, g

        if node in visited:
            continue

        visited.add(node)

        for neighbour in graph[node]:
            if neighbour not in visited:
                new_g = g + 1
                new_f = new_g + heuristic(neighbour, goal)

                heapq.heappush(
                    pq,
                    (new_f, new_g, neighbour,
                     path + [neighbour])
                )

    return None, float('inf')


path, cost = A_star('A', 'G')

print("A* Search Path:", path)
print("Total Cost:", cost)

Output:
A* Search Path: ['A', 'B', 'E', 'G']
Total Cost: 3
________________________
5. Greedy Best-First Search
Aim: Write a Python program to implement Greedy Best-First Search using a heuristic function.
Program:
import heapq

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [],
    'E': ['G'],
    'F': ['G'],
    'G': []
}

heuristic = {
    'A': 6,
    'B': 4,
    'C': 5,
    'D': 3,
    'E': 2,
    'F': 1,
    'G': 0
}

def greedy_search(start, goal):
    pq = [(heuristic[start], start)]
    visited = set()

    while pq:
        h, node = heapq.heappop(pq)

        if node in visited:
            continue

        print(node, end=" ")

        if node == goal:
            return

        visited.add(node)

        for neighbour in graph[node]:
            if neighbour not in visited:
                heapq.heappush(
                    pq,
                    (heuristic[neighbour], neighbour)
                )


print("Greedy Best First Search:")
greedy_search('A', 'G')

Output:
Greedy Best First Search:
A B E G
__________________________
6. Maze Solving Using A* Search
Aim: Write a Python program to solve a maze using the A* Search algorithm. Here, 0 represents an open cell and 1 represents a wall.
Program:
import heapq

maze = [
    [0, 0, 0, 1],
    [1, 0, 0, 1],
    [0, 0, 0, 0],
    [1, 1, 0, 0]
]

start = (0, 0)
goal = (3, 3)

def heuristic(a, b):
    return abs(a[0]-b[0]) + abs(a[1]-b[1])


def A_star(maze, start, goal):
    rows = len(maze)
    cols = len(maze[0])

    pq = [(0, start, [start])]
    visited = set()

    directions = [
        (-1, 0),
        (1, 0),
        (0, -1),
        (0, 1)
    ]

    while pq:
        cost, current, path = heapq.heappop(pq)

        if current == goal:
            return path

        if current in visited:
            continue

        visited.add(current)

        for dr, dc in directions:
            r = current[0] + dr
            c = current[1] + dc

            if (0 <= r < rows and
                0 <= c < cols and
                maze[r][c] == 0):

                neighbour = (r, c)

                if neighbour not in visited:
                    g = len(path)
                    h = heuristic(neighbour, goal)

                    heapq.heappush(
                        pq,
                        (g + h, neighbour,
                         path + [neighbour])
                    )

    return None


path = A_star(maze, start, goal)

if path:
    print("Path Found:")
    print(path)
else:
    print("No Path Found")

Output:
Path Found:
[(0, 0), (0, 1), (1, 1), (1, 2),
 (2, 2), (2, 3), (3, 3)]
____________________________
7. Minimax Algorithm with Alpha-Beta Pruning
Aim: Write a Python program to implement the Minimax algorithm with Alpha-Beta Pruning for the Tic-Tac-Toe game.
Program:
board = [' '] * 9

def display():
    for i in range(0, 9, 3):
        print(board[i], '|', board[i+1],
              '|', board[i+2])


def winner(b):
    wins = [
        (0,1,2), (3,4,5), (6,7,8),
        (0,3,6), (1,4,7), (2,5,8),
        (0,4,8), (2,4,6)
    ]

    for a, b1, c in wins:
        if board[a] == board[b1] == board[c] != ' ':
            return board[a]

    return None


def minimax(alpha, beta, maximizing):
    result = winner(board)

    if result == 'X':
        return 1

    if result == 'O':
        return -1

    if ' ' not in board:
        return 0

    if maximizing:
        best = -float('inf')

        for i in range(9):
            if board[i] == ' ':
                board[i] = 'X'
                score = minimax(alpha, beta, False)
                board[i] = ' '

                best = max(best, score)
                alpha = max(alpha, best)

                if beta <= alpha:
                    break

        return best

    else:
        best = float('inf')

        for i in range(9):
            if board[i] == ' ':
                board[i] = 'O'
                score = minimax(alpha, beta, True)
                board[i] = ' '

                best = min(best, score)
                beta = min(beta, best)

                if beta <= alpha:
                    break

        return best


def best_move():
    best_score = -float('inf')
    move = -1

    for i in range(9):
        if board[i] == ' ':
            board[i] = 'X'

            score = minimax(
                -float('inf'),
                float('inf'),
                False
            )

            board[i] = ' '

            if score > best_score:
                best_score = score
                move = i

    return move


# Play a sample game
board[0] = 'O'
board[4] = 'X'

move = best_move()

if move != -1:
    board[move] = 'X'

print("Tic Tac Toe Board:")
display()

Output:
Tic Tac Toe Board:
O |   | X
  | X |  
  |   |
________________________
8. Hill Climbing Algorithm with Random Restarts
Aim: Write a Python program to implement the Hill Climbing algorithm with random restarts.
Program:
import random

def function(x):
    return -(x - 5) ** 2 + 25


def hill_climbing():
    x = random.randint(0, 10)

    while True:
        current = function(x)

        neighbours = []

        if x > 0:
            neighbours.append(x - 1)

        if x < 10:
            neighbours.append(x + 1)

        best = max(neighbours, key=function)

        if function(best) <= current:
            return x, function(x)

        x = best


best_solution = None
best_value = float('-inf')

# Random restarts
for i in range(5):
    solution, value = hill_climbing()

    if value > best_value:
        best_solution = solution
        best_value = value

print("Best Solution:", best_solution)
print("Maximum Value:", best_value)

Output:
Best Solution: 5
Maximum Value: 25
________________________
9. Representation of the 8-Puzzle Problem
Aim: Write a Python program to represent the state of the 8-Puzzle using a 2D list and display the puzzle state.
Program:
# 8 Puzzle Representation

puzzle = [
    [1, 2, 3],
    [4, 0, 5],
    [7, 8, 6]
]

def display_puzzle(puzzle):
    print("8 Puzzle State:")

    for row in puzzle:
        print(row[0], row[1], row[2])


display_puzzle(puzzle)

Output:
8 Puzzle State:
1 2 3
4 0 5
7 8 6
Here, 0 represents the empty space.
____________________________
10. Generating All Possible Moves in the 8-Puzzle
Aim: Write a Python program to generate all possible moves (up, down, left and right) from a given state in the 8-Puzzle.
Program:
def possible_moves(puzzle):

    moves = []

    # Find empty space
    for i in range(3):
        for j in range(3):
            if puzzle[i][j] == 0:
                row, col = i, j

    directions = {
        "Up": (-1, 0),
        "Down": (1, 0),
        "Left": (0, -1),
        "Right": (0, 1)
    }

    for name, (dr, dc) in directions.items():

        r = row + dr
        c = col + dc

        if 0 <= r < 3 and 0 <= c < 3:

            new_puzzle = [x[:] for x in puzzle]

            # Swap empty space
            new_puzzle[row][col], new_puzzle[r][c] = (
                new_puzzle[r][c],
                new_puzzle[row][col]
            )

            moves.append((name, new_puzzle))

    return moves


puzzle = [
    [1, 2, 3],
    [4, 0, 5],
    [7, 8, 6]
]

print("Possible Moves:")

for direction, state in possible_moves(puzzle):
    print("\n", direction)

    for row in state:
        print(row)
Output:
Possible Moves:

Up
[1, 0, 3]
[4, 2, 5]
[7, 8, 6]

Down
[1, 2, 3]
[4, 8, 5]
[7, 0, 6]

Left
[1, 2, 3]
[0, 4, 5]
[7, 8, 6]

Right
[1, 2, 3]
[4, 5, 0]
[7, 8, 6]
