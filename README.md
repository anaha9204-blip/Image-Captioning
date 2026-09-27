def print_board(board):
    print("\n")
    for i in range(0, 9, 3):
        print(" | ".join(board[i:i+3]))
        if i < 6:
            print("--+---+--")


def check_winner(board, player):
    wins = [
        (0,1,2), (3,4,5), (6,7,8),
        (0,3,6), (1,4,7), (2,5,8),
        (0,4,8), (2,4,6)
    ]

    for a, b, c in wins:
        if board[a] == board[b] == board[c] == player:
            return True

    return False


def minimax(board, is_maximizing):

    if check_winner(board, "O"):
        return 1

    if check_winner(board, "X"):
        return -1

    if " " not in board:
        return 0

    if is_maximizing:
        best_score = -100

        for i in range(9):
            if board[i] == " ":
                board[i] = "O"
                score = minimax(board, False)
                board[i] = " "
                best_score = max(best_score, score)

        return best_score

    else:
        best_score = 100

        for i in range(9):
            if board[i] == " ":
                board[i] = "X"
                score = minimax(board, True)
                board[i] = " "
                best_score = min(best_score, score)

        return best_score


def best_move(board):

    best_score = -100
    move = -1

    for i in range(9):

        if board[i] == " ":

            board[i] = "O"

            score = minimax(board, False)

            board[i] = " "

            if score > best_score:
                best_score = score
                move = i

    return move


board = [" "] * 9

print("TIC-TAC-TOE AI")
print("You = X")
print("AI  = O")

while True:

    print_board(board)

    try:
        position = int(input("\nEnter position (1-9): ")) - 1
    except ValueError:
        print("Enter a number from 1 to 9.")
        continue

    if position < 0 or position > 8 or board[position] != " ":
        print("Invalid position!")
        continue

    board[position] = "X"

    if check_winner(board, "X"):
        print_board(board)
        print("\nYou Win!")
        break

    if " " not in board:
        print_board(board)
        print("\nDraw!")
        break

    move = best_move(board)
    board[move] = "O"

    print("\nAI played position:", move + 1)

    if check_winner(board, "O"):
        print_board(board)
        print("\nAI Wins!")
        break
