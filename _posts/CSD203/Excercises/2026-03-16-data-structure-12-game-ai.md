---
title: "TIC-TAC-TOE Game using Heuristic Alpha-Beta Tree Search Algorithm"
description: >-
  TIC-TAC-TOE Game using Heuristic Alpha-Beta Tree Search Algorithm
author: [shandy]
date: 2026-03-16
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 411
# pin: true
# media_subpath: '/posts/01'
---

## Introduction

![](/assets/img/2026-03-16-23-00-49.png)

This project aims to implement the Heuristic Alpha-Beta Tree Search Algorithm in Tic-Tac-Toe. The algorithm combines heuristic evaluation and alpha-beta pruning to create an intelligent AI opponent for the game. The goal is to showcase the algorithm’s effectiveness, analyze its performance, and evaluate its strategic decision-making capabilities.

Algorithm Description: The Heuristic Alpha-Beta Tree Search Algorithm combines minimax search and pruning techniques. It evaluates game states using heuristics and optimizes the search process by pruning unnecessary branches.

Program Design: The program simulates a Tic-Tac-Toe game environment with functions for player moves, AI moves, game state evaluation, and winner determination. It is designed for code reusability, readability, and maintainability.

Code Explanations: Key functions include handling moves, evaluating game states, and implementing the algorithm through minimax with alpha-beta pruning.

Experimental Results: Experimental results assess the AI opponent’s performance based on win rate, average moves to win, and game completion time. The algorithm can be tested against different opponents or difficulty levels.

Analysis: The analysis interprets results, discusses strengths and weaknesses, compares with other algorithms, and evaluates computational complexity.

Recommendations and Future Development: Recommendations include fine-tuning heuristics, exploring enhancements to alpha-beta pruning, and extending the algorithm to other games or environments.

Overall, this project demonstrates the potential of the Heuristic Alpha-Beta Tree Search Algorithm in Tic-Tac-Toe and contributes to the understanding of heuristic algorithms in game theory and AI.

## Algorithm Description (Minimax Algorithm - Alpha-Beta Pruning)

This project implements the Heuristic Alpha-Beta Tree Search Algorithm for Tic-Tac-Toe. The algorithm combines heuristic evaluation and alpha-beta pruning to create an intelligent AI opponent. It utilizes minimax search and pruning techniques to evaluate game states efficiently. The program includes functions for player and AI moves, game state evaluation, and determining the winner. Experimental results evaluate AI performance, and the analysis discusses strengths, weaknesses, and potential improvements. The project contributes to game theory and AI understanding.

Alpha-Beta pruning is not actually a new algorithm, but rather an optimization technique for the minimax algorithm. It reduces the computation time by a huge factor. This allows us to search much faster and even go into deeper levels in the game tree. It cuts off branches in the game tree which need not be searched because there already exists a better move available. It is called Alpha-Beta pruning because it passes 2 extra parameters in the minimax function, namely alpha and beta.

Let's define the parameters alpha and beta. 

Alpha is the best value that the maximizer currently can guarantee at that level or above. 
Beta is the best value that the minimizer currently can guarantee at that level or below.

![](/assets/img/2026-03-16-23-08-56.png)

1. The initial call starts from A. The value of alpha here is -INFINITY and the value of beta is +INFINITY. These values are passed down to subsequent nodes in the tree. At A the maximizer must choose max of B and C, so A calls B first
2. At B it the minimizer must choose min of D and E and hence calls D first.
3. At D, it looks at its left child which is a leaf node. This node returns a value of 3. Now the value of alpha at D is max( -INF, 3) which is 3.
4. To decide whether its worth looking at its right node or not, it checks the condition beta<=alpha. This is false since beta = +INF and alpha = 3. So it continues the search.
5. D now looks at its right child which returns a value of 5.At D, alpha = max(3, 5) which is 5. Now the value of node D is 5
6. D returns a value of 5 to B. At B, beta = min( +INF, 5) which is 5. The minimizer is now guaranteed a value of 5 or lesser. B now calls E to see if he can get a lower value than 5.
7. At E the values of alpha and beta is not -INF and +INF but instead -INF and 5 respectively, because the value of beta was changed at B and that is what B passed down to E
8. Now E looks at its left child which is 6. At E, alpha = max(-INF, 6) which is 6. Here the condition becomes true. beta is 5 and alpha is 6. So beta<=alpha is true. Hence it breaks and E returns 6 to B
9. Note how it did not matter what the value of E's right child is. It could have been +INF or -INF, it still wouldn't matter, We never even had to look at it because the minimizer was guaranteed a value of 5 or lesser. So as soon as the maximizer saw the 6 he knew the minimizer would never come this way because he can get a 5 on the left side of B. This way we didn't have to look at that 9 and hence saved computation time.
10. E returns a value of 6 to B. At B, beta = min( 5, 6) which is 5.The value of node B is also 5

So far this is how our game tree looks. The 9 is crossed out because it was never computed. 

![](/assets/img/2026-03-16-23-10-43.png)

11. B returns 5 to A. At A, alpha = max( -INF, 5) which is 5. Now the maximizer is guaranteed a value of 5 or greater. A now calls C to see if it can get a higher value than 5.
12. At C, alpha = 5 and beta = +INF. C calls F
13. At F, alpha = 5 and beta = +INF. F looks at its left child which is a 1. alpha = max( 5, 1) which is still 5.
14. F looks at its right child which is a 2. Hence the best value of this node is 2. Alpha still remains 5
15. F returns a value of 2 to C. At C, beta = min( +INF, 2). The condition beta <= alpha becomes true as beta = 2 and alpha = 5. So it breaks and it does not even have to compute the entire sub-tree of G.
16. The intuition behind this break-off is that, at C the minimizer was guaranteed a value of 2 or lesser. But the maximizer was already guaranteed a value of 5 if he choose B. So why would the maximizer ever choose C and get a value less than 2 ? Again you can see that it did not matter what those last 2 values were. We also saved a lot of computation by skipping a whole sub-tree.
17. C now returns a value of 2 to A. Therefore the best value at A is max( 5, 2) which is a 5.
18. Hence the optimal value that the maximizer can get is 5

This is how our final game tree looks like. As you can see G has been crossed out as it was never computed. 

![](/assets/img/2026-03-16-23-11-32.png)

```python
# Initial values of Alpha and Beta 
MAX, MIN = 1000, -1000 

# Returns optimal value for current player 
#(Initially called for root and maximizer) 
def minimax(depth, nodeIndex, maximizingPlayer, 
            values, alpha, beta): 
 
    # Terminating condition. i.e 
    # leaf node is reached 
    if depth == 3: 
        return values[nodeIndex] 

    if maximizingPlayer: 
     
        best = MIN 

        # Recur for left and right children 
        for i in range(0, 2): 
            
            val = minimax(depth + 1, nodeIndex * 2 + i, 
                          False, values, alpha, beta) 
            best = max(best, val) 
            alpha = max(alpha, best) 

            # Alpha Beta Pruning 
            if beta <= alpha: 
                break 
         
        return best 
     
    else:
        best = MAX 

        # Recur for left and 
        # right children 
        for i in range(0, 2): 
         
            val = minimax(depth + 1, nodeIndex * 2 + i, 
                            True, values, alpha, beta) 
            best = min(best, val) 
            beta = min(beta, best) 

            # Alpha Beta Pruning 
            if beta <= alpha: 
                break 
         
        return best 
     
if __name__ == "__main__": 
 
    values = [3, 5, 6, 9, 1, 2, 0, -1]  
    print("The optimal value is :", minimax(0, 0, True, values, MIN, MAX)) 
```

**Output**

```
The optimal value is : 5
```

## Program Design:

- The program follows a modular design with separate functions for different tasks.
- It implements the Heuristic Alpha-Beta Tree Search algorithm.
- Object-oriented programming principles are utilized.
- The choice of programming language and libraries is not specified.
- The program may include a graphical user interface.
- The design emphasizes modularity, efficiency, and user-friendliness.

## Function of each piece of code:

![](/assets/img/2026-03-16-23-15-57.png)

### 1. Import libraries:

```python
from random import choice
from math import inf
```

The first piece of code imports the “choice” function from the “random” library and the “inf” constant from the “math” library.

### 2. The board:

```python
# The Tic-Tac-Toe board
board = [[0, 0, 0],
         [0, 0, 0],
         [0, 0, 0]]
```

The second piece of code defines the initial state of the Tic-Tac-Toe board. It is represented as a 3x3 nested list where each element represents a cell on the board. The value 0 indicates an empty cell.

### 3. display and clear the game board:

```python
# Function to display the game board
def Gameboard(board):
    chars = {1: 'X', -1: 'O', 0: ' '}
    for x in board:
        for y in x:
            ch = chars[y]
            print(f'| {ch} ', end='')
        print('|\n' + '_____________')
    print("")
    print('===============')
    print("")

# Function to clear the game board
def Clearboard(board):
    for x, row in enumerate(board):
        for y, col in enumerate(row):
            board[x][y] = 0
```

The third piece of code includes two functions related to the game board.

**ِA. Gameboard(board):**

- This function takes the current state of the board as input.
- It maps the numeric values on the board to their corresponding symbols (‘X’, ‘O’, or empty space).
- It then displays the board on the console, showing the current positions of the players.

**B. Clearboard(board):**

- This function clears the game board by setting all the cells to 0 (empty).
- It iterates through each cell of the board and assigns the value 0 to it.
- This function is used to reset the board for a new game or when needed.

### 4. Check The won:

```python
# Function to check if a player has won
def winningPlayer(board, player):
    conditions = [[board[0][0], board[0][1], board[0][2]],
                  [board[1][0], board[1][1], board[1][2]],
                  [board[2][0], board[2][1], board[2][2]],
                  [board[0][0], board[1][0], board[2][0]],
                  [board[0][1], board[1][1], board[2][1]],
                  [board[0][2], board[1][2], board[2][2]],
                  [board[0][0], board[1][1], board[2][2]],
                  [board[0][2], board[1][1], board[2][0]]]

    if [player, player, player] in conditions:
        return True

    return False

# Function to check if the game has been won
def gameWon(board):
    return winningPlayer(board, 1) or winningPlayer(board, -1)


# Function to print the game result
def printResult(board):
    if winningPlayer(board, 1):
        print('Player has won! ' + '\n')
    elif winningPlayer(board, -1):
        print('AI has won! ' + '\n')
    else:
        print('The game is over with the result of Draw' + '\n')
```

The fourth piece of code includes functions related to checking the game’s winning condition and printing the game result.

**A. winningPlayer(board, player):**

- This function takes the game board and a player (-1 for AI, 1 for the player) as input.
- It checks all possible winning conditions on the board to determine if the specified player has won.
- The function returns True if the player has won and False otherwise.

**B. gameWon(board):**

- This function takes the game board as input.
- It uses the winningPlayer() function to check if either the player or the AI has won the game.
- The function returns True if either player has won and False otherwise.

**C. printResult(board):**

This function takes the game board as input.
It uses the winningPlayer() function to determine the game’s result.
If the player has won, it prints “Player has won!”
If the AI has won, it prints “AI has won!”
If the game is a draw, it prints “The game is over with the result of Draw.”
These functions are responsible for checking the game’s outcome and printing the corresponding result on the console.

### 5. Check The board:

```
# Function to get the list of blank positions on the board
def blanks(board):
    blank = []
    for x, row in enumerate(board):
        for y, col in enumerate(row):
            if board[x][y] == 0:
                blank.append([x, y])
    return blank


# Function to check if the board is full
def boardFull(board):
    if len(blanks(board)) == 0:
        return True
    return False
```

The fifth piece of code includes functions related to checking the board’s status and available moves.

**A. blanks(board):**

- This function takes the game board as input.
- It iterates through each cell of the board and checks if it is empty (marked with 0).
- The function collects the coordinates of empty cells and returns them as a list.

**B. boardFull(board):**

- This function takes the game board as input.
- It uses the blanks() function to obtain the list of empty positions.
- If the length of the list is zero, it means the board is full.
- The function returns True if the board is full and False otherwise.
- These functions help determine the availability of moves on the board. The blanks() function provides a list of empty positions, and the boardFull() function checks if the board is completely filled with moves.

### 6. The Move:

```python
# Function to set a move on the board
def setMove(board, x, y, player):
    board[x][y] = player


# Function to evaluate the game state
def evaluate(board):
    if winningPlayer(board, 1):
        return 1
    elif winningPlayer(board, -1):
        return -1
    else:
        return 0

# Heuristic function for the AI
def heuristic(board):
    return evaluate(board)
```

## Full code:

```python

from random import choice
from math import inf

board = [[0, 0, 0],
         [0, 0, 0],
         [0, 0, 0]]

def Gameboard(board):
    chars = {1: 'X', -1: 'O', 0: ' '}
    for x in board:
        for y in x:
            ch = chars[y]
            print(f'| {ch} |', end='')
        print('\n' + '---------------')
    print('===============')

def Clearboard(board):
    for x, row in enumerate(board):
        for y, col in enumerate(row):
            board[x][y] = 0

def winningPlayer(board, player):
    conditions = [[board[0][0], board[0][1], board[0][2]],
                     [board[1][0], board[1][1], board[1][2]],
                     [board[2][0], board[2][1], board[2][2]],
                     [board[0][0], board[1][0], board[2][0]],
                     [board[0][1], board[1][1], board[2][1]],
                     [board[0][2], board[1][2], board[2][2]],
                     [board[0][0], board[1][1], board[2][2]],
                     [board[0][2], board[1][1], board[2][0]]]

    if [player, player, player] in conditions:
        return True

    return False

def gameWon(board):
    return winningPlayer(board, 1) or winningPlayer(board, -1)

def printResult(board):
    if winningPlayer(board, 1):
        print('X has won! ' + '\n')

    elif winningPlayer(board, -1):
        print('O\'s have won! ' + '\n')

    else:
        print('Draw' + '\n')

def blanks(board):
    blank = []
    for x, row in enumerate(board):
        for y, col in enumerate(row):
            if board[x][y] == 0:
                blank.append([x, y])

    return blank

def boardFull(board):
    if len(blanks(board)) == 0:
        return True
    return False

def setMove(board, x, y, player):
    board[x][y] = player

def playerMove(board):
    e = True
    moves = {1: [0, 0], 2: [0, 1], 3: [0, 2],
             4: [1, 0], 5: [1, 1], 6: [1, 2],
             7: [2, 0], 8: [2, 1], 9: [2, 2]}
    while e:
        try:
            move = int(input('Enter a number between 1-9: '))
            if move < 1 or move > 9:
                print('Invalid Move! Try again!')
            elif not (moves[move] in blanks(board)):
                print('Invalid Move! Try again!')
            else:
                setMove(board, moves[move][0], moves[move][1], 1)
                Gameboard(board)
                e = False
        except(KeyError, ValueError):
            print('Enter a number!')

def getScore(board):
    if winningPlayer(board, 1):
        return 10

    elif winningPlayer(board, -1):
        return -10

    else:
        return 0

def abminimax(board, depth, alpha, beta, player):
    row = -1
    col = -1
    if depth == 0 or gameWon(board):
        return [row, col, getScore(board)]

    else:
        for cell in blanks(board):
            setMove(board, cell[0], cell[1], player)
            score = abminimax(board, depth - 1, alpha, beta, -player)
            if player == 1:
                # X is always the max player
                if score[2] > alpha:
                    alpha = score[2]
                    row = cell[0]
                    col = cell[1]

            else:
                if score[2] < beta:
                    beta = score[2]
                    row = cell[0]
                    col = cell[1]

            setMove(board, cell[0], cell[1], 0)

            if alpha >= beta:
                break

        if player == 1:
            return [row, col, alpha]

        else:
            return [row, col, beta]

def o_comp(board):
    if len(blanks(board)) == 9:
        x = choice([0, 1, 2])
        y = choice([0, 1, 2])
        setMove(board, x, y, -1)
        Gameboard(board)

    else:
        result = abminimax(board, len(blanks(board)), -inf, inf, -1)
        setMove(board, result[0], result[1], -1)
        Gameboard(board)

def x_comp(board):
    if len(blanks(board)) == 9:
        x = choice([0, 1, 2])
        y = choice([0, 1, 2])
        setMove(board, x, y, 1)
        Gameboard(board)

    else:
        result = abminimax(board, len(blanks(board)), -inf, inf, 1)
        setMove(board, result[0], result[1], 1)
        Gameboard(board)

def makeMove(board, player, mode):
    if mode == 1:
        if player == 1:
            playerMove(board)

        else:
            o_comp(board)
    else:
        if player == 1:
            o_comp(board)
        else:
            x_comp(board)

def pvc():
    while True:
        try:
            order = int(input('Enter to play 1st or 2nd: '))
            if not (order == 1 or order == 2):
                print('Please pick 1 or 2')
            else:
                break
        except(KeyError, ValueError):
            print('Enter a number')

    Clearboard(board)
    if order == 2:
        currentPlayer = -1
    else:
        currentPlayer = 1

    while not (boardFull(board) or gameWon(board)):
        makeMove(board, currentPlayer, 1)
        currentPlayer *= -1

    printResult(board)

# Driver Code
print("=================================================")
print("TIC-TAC-TOE using MINIMAX with ALPHA-BETA Pruning")
print("=================================================")
pvc()
```