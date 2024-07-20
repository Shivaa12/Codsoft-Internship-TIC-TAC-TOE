# Codsoft-Internship-TIC-TAC-TOE
import math

def print_board(board):
    for row in [board[i*3:(i+1)*3] for i in range(3)]:
        print('| ' + ' | '.join([f'\033[91m{cell}\033[0m' if cell == 'X' else f'\033[94m{cell}\033[0m' if cell == 'O' else cell for cell in row]) + ' |')

def check_winner(board, player):
    win_combinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],  
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  
        [0, 4, 8], [2, 4, 6]  
    ]
    for combination in win_combinations:
        if all(board[i] == player for i in combination):
            return True
    return False

def is_board_full(board):
    return ' ' not in board

def get_available_moves(board):
    return [i for i, spot in enumerate(board) if spot == ' ']

def minimax(board, depth, is_maximizing):
    if check_winner(board, 'O'):
        return 1
    elif check_winner(board, 'X'):
        return -1
    elif is_board_full(board):
        return 0

    if is_maximizing:
        best_score = -math.inf
        for move in get_available_moves(board):
            board[move] = 'O'
            score = minimax(board, depth + 1, False)
            board[move] = ' '
            best_score = max(score, best_score)
        return best_score
    else:
        best_score = math.inf
        for move in get_available_moves(board):
            board[move] = 'X'
            score = minimax(board, depth + 1, True)
            board[move] = ' '
            best_score = min(score, best_score)
        return best_score

def ai_move(board):
    best_score = -math.inf
    best_move = None
    for move in get_available_moves(board):
        board[move] = 'O'
        score = minimax(board, 0, False)
        board[move] = ' '
        if score > best_score:
            best_score = score
            best_move = move
    board[best_move] = 'O'

def human_move(board):
    move = -1
    while move not in get_available_moves(board):
        move = int(input("Enter your move (1-9): ")) - 1
    board[move] = 'X'

def play_game():
    board = [' ' for _ in range(9)] 
    while True:
        print_board(board)
        if check_winner(board, 'O'):
            print("AI wins!")
            break
        elif check_winner(board, 'X'):
            print("Human wins!")
            break
        elif is_board_full(board):
            print("It's a tie!")
            break

        human_move(board)
        if check_winner(board, 'X') or is_board_full(board):
            continue
        ai_move(board)

    play_again = input("Do you want to play again? (yes/no): ")
    if play_again.lower() == "yes":
        play_game()
    else:
        print("Goodbye!")

play_game()
