---
title: "Caro Game using Heuristic Alpha-Beta Tree Search Algorithm"
description: >-
  TIC-TAC-TOE Game using Heuristic Alpha-Beta Tree Search Algorithm
author: [shandy]
date: 2026-03-17
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 411
# pin: true
# media_subpath: '/posts/01'
---

## Code samples:

```python
import math
import random

BOARD_SIZE = 10
WIN_COUNT = 5

EMPTY = "."
AI = "X"
HUMAN = "O"

board = [[EMPTY for _ in range(BOARD_SIZE)] for _ in range(BOARD_SIZE)]

def print_board():
    # in header cột
    print("   ", end="")
    for col in range(len(board)):
        print(col, end=" ")
    print()

    for i, row in enumerate(board):
        print(f"{i:2} ", end="")  # in row index
        print(" ".join(row))
    print()

def is_valid(x, y):
    return 0 <= x < BOARD_SIZE and 0 <= y < BOARD_SIZE

def check_win(player):
    directions = [(1,0),(0,1),(1,1),(1,-1)]
    for x in range(BOARD_SIZE):
        for y in range(BOARD_SIZE):
            if board[x][y] != player:
                continue
            for dx,dy in directions:
                count = 0
                nx,ny = x,y
                while is_valid(nx,ny) and board[nx][ny] == player:
                    count += 1
                    nx += dx
                    ny += dy
                if count >= WIN_COUNT:
                    return True
    return False

def get_neighbors():
    moves = set()
    for x in range(BOARD_SIZE):
        for y in range(BOARD_SIZE):
            if board[x][y] != EMPTY:
                for dx in [-1,0,1]:
                    for dy in [-1,0,1]:
                        nx = x+dx
                        ny = y+dy
                        if is_valid(nx,ny) and board[nx][ny] == EMPTY:
                            moves.add((nx,ny))

    if not moves:
        return [(BOARD_SIZE//2, BOARD_SIZE//2)]
    return list(moves)

def evaluate_line(count, open_ends):
    if count >= 5:
        return 100000
    if count == 4 and open_ends == 2:
        return 10000
    if count == 3 and open_ends == 2:
        return 1000
    if count == 2 and open_ends == 2:
        return 100
    return 0

def evaluate_player(player):
    score = 0
    directions = [(1,0),(0,1),(1,1),(1,-1)]
    for x in range(BOARD_SIZE):
        for y in range(BOARD_SIZE):
            if board[x][y] != player:
                continue
            for dx,dy in directions:
                count = 0
                open_ends = 0
                i = 0
                while True:
                    nx = x + dx*i
                    ny = y + dy*i
                    if not is_valid(nx,ny):
                        break
                    if board[nx][ny] == player:
                        count += 1
                        i += 1
                    else:
                        if board[nx][ny] == EMPTY:
                            open_ends += 1
                        break
                i = -1
                while True:
                    nx = x + dx*i
                    ny = y + dy*i
                    if not is_valid(nx,ny):
                        break
                    if board[nx][ny] == player:
                        count += 1
                        i -= 1
                    else:
                        if board[nx][ny] == EMPTY:
                            open_ends += 1
                        break
                score += evaluate_line(count, open_ends)
    return score

def evaluate():
    return evaluate_player(AI) - evaluate_player(HUMAN)

def minimax(depth, alpha, beta, maximizing):
    if check_win(AI):
        return 100000
    if check_win(HUMAN):
        return -100000
    if depth == 0:
        return evaluate()
    moves = get_neighbors()
    if maximizing:
        best = -math.inf
        for x,y in moves:
            board[x][y] = AI
            val = minimax(depth-1, alpha, beta, False)
            board[x][y] = EMPTY
            best = max(best,val)
            alpha = max(alpha,val)
            if beta <= alpha:
                break
        return best
    else:
        best = math.inf
        for x,y in moves:
            board[x][y] = HUMAN
            val = minimax(depth-1, alpha, beta, True)
            board[x][y] = EMPTY
            best = min(best,val)
            beta = min(beta,val)
            if beta <= alpha:
                break
        return best

def ai_move():
    best_score = -math.inf
    move = None
    for x,y in get_neighbors():
        board[x][y] = AI
        score = minimax(2, -math.inf, math.inf, False)
        board[x][y] = EMPTY
        if score > best_score:
            best_score = score
            move = (x,y)
    return move

def play():
    while True:
        print_board()
        x = int(input("Row: "))
        y = int(input("Col: "))
        if board[x][y] != EMPTY:
            continue
        board[x][y] = HUMAN
        if check_win(HUMAN):
            print_board()
            print("You win!")
            break
        move = ai_move()
        board[move[0]][move[1]] = AI

        if check_win(AI):
            print_board()
            print("AI wins!")
            break
play()
```