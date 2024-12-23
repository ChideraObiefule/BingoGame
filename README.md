# BingoGame
Python based game, Bingo (multiplayer)
# By submitting this assignment, I agree to the following:
#   "Aggies do not lie, cheat, or steal, or tolerate those who do."
#   "I have not given or received any unauthorized aid on this assignment."
#
# Name:         Chidera Obiefule
# Section:      559
# Assignment:   Lab Topic 11 Fun Game
# Date:         11/20/2022
#
import random
import numpy as np
import turtle


t = turtle.Turtle()
s = turtle.Screen()
s.setup(width=750, height=750)
s.bgcolor("white")
t.speed(2000)
t.penup()
t.goto(-350, -350)
t.color("maroon")
t.pendown()
# Create a 5 x 5 array of 0s - middle space is given
# This "ghost board" is used to track the board positions that get marked off as the numbers are called
# The player doesnt see this
check_board = np.array([[0, 0, 0, 0, 0],
                        [0, 0, 0, 0, 0],
                        [0, 0, 1, 0, 0],
                        [0, 0, 0, 0, 0],
                        [0, 0, 0, 0, 0]])

instructions = ""
try:
    with open("bingo_game_instructions.txt", "r") as inst_file:   # Open the file
        txt_lines = inst_file.readlines()
        instructions = f'{txt_lines[0]}\n{txt_lines[1]}\n{txt_lines[2]}\n{txt_lines[3]}'
except:
    print('Error occured reading text file.\nPlease ensure the text file is named "bingo_game_instructions"')

def end_game():
    print("Thanks for playing!")
def draw_instructions(file):
    '''This function is responsible for drawing the instructions on the title screen'''
    t.penup()
    t.goto(-300,0)
    t.write(file, font=("Verdana", 15, "normal"))


def win_check(cboard):
    '''This function checks to see if a complete set of 5 is found on the check_board'''
    # Check to see if any win conditions have been met
    prod_column1 = int(cboard[0, 0]) * int(cboard[0, 1]) * int(cboard[0, 2]) * int(cboard[0, 3]) * int(cboard[0, 4])
    prod_column2 = int(cboard[1, 0]) * int(cboard[1, 1]) * int(cboard[1, 2]) * int(cboard[1, 3]) * int(cboard[1, 4])
    prod_column3 = int(cboard[2, 0]) * int(cboard[2, 1]) * int(cboard[2, 2]) * int(cboard[2, 3]) * int(cboard[2, 4])
    prod_column4 = int(cboard[3, 0]) * int(cboard[3, 1]) * int(cboard[3, 2]) * int(cboard[3, 3]) * int(cboard[3, 4])
    prod_column5 = int(cboard[4, 0]) * int(cboard[4, 1]) * int(cboard[4, 2]) * int(cboard[4, 3]) * int(cboard[4, 4])

    prod_row1 = int(cboard[0, 0]) * int(cboard[1, 0]) * int(cboard[2, 0]) * int(cboard[3, 0]) * int(cboard[4, 0])
    prod_row2 = int(cboard[0, 1]) * int(cboard[1, 1]) * int(cboard[2, 1]) * int(cboard[3, 1]) * int(cboard[4, 1])
    prod_row3 = int(cboard[0, 2]) * int(cboard[1, 2]) * int(cboard[2, 2]) * int(cboard[3, 2]) * int(cboard[4, 2])
    prod_row4 = int(cboard[0, 3]) * int(cboard[1, 3]) * int(cboard[2, 3]) * int(cboard[3, 3]) * int(cboard[4, 3])
    prod_row5 = int(cboard[0, 4]) * int(cboard[1, 4]) * int(cboard[2, 4]) * int(cboard[3, 4]) * int(cboard[4, 4])

    prod_diag1 = int(cboard[0, 0]) * int(cboard[1, 1]) * int(cboard[2, 2]) * int(cboard[3, 3]) * int(cboard[4, 4])
    prod_diag2 = int(cboard[0, 4]) * int(cboard[1, 3]) * int(cboard[2, 2]) * int(cboard[3, 1]) * int(cboard[0, 1])

    if prod_column1 == 1:  # If any complete sets of 5 are found, change the run state to 0
        return 0
    elif prod_column2 == 1:
        return 0
    elif prod_column3 == 1:
        return 0
    elif prod_column4 == 1:
        return 0
    elif prod_column5 == 1:
        return 0
    elif prod_row1 == 1:
        return 0
    elif prod_row2 == 1:
        return 0
    elif prod_row3 == 1:
        return 0
    elif prod_row4 == 1:
        return 0
    elif prod_row5 == 1:
        return 0
    elif prod_diag1 == 1:
        return 0
    elif prod_diag2 == 1:
        return 0
    else:  # If no complete sets are found, set the run state to 1
        return 1


def populate_board(board):
    '''This function populates the graphical board with characters from the board created in the function build_board'''
    coord_dict = {}

    for a in range(5):
        for b in range(5):
            t.speed(10)
            t.goto(-335 + 100 * a, 75 - 100 * b)
            coord_dict[board[b, a]] = (-300 + 100 * a, 100 - 100 * b)
            t.write(board[b, a], font=("Verdana", 30, "normal"))

    return coord_dict


def build_board():
    '''build_board creates a 5x5 array using randomly generated numbers within a set range to simulate a player's card'''
    row1 = []  # These are empty rows
    row2 = []
    row3 = []
    row4 = []
    row5 = []

    # Here we build each row by generating random numbers between a range, and append them to the row
    min = 1
    max = 15
    for o in range(5):
        call_num = random.randint(min, max)  # Generate a random number between 1 and 15
        while call_num in row1:  # If a random number was already called once before
            call_num = random.randint(min, max)  # Generate a different number - repeat until a new number is found
        row1.append(call_num)  # Append the number to row1

    min = 16
    max = 30
    for o in range(5):
        call_num = random.randint(min, max)
        while call_num in row2:
            call_num = random.randint(min, max)
        row2.append(call_num)

    min = 31
    max = 45
    for o in range(5):
        call_num = random.randint(min, max)
        while call_num in row3:
            call_num = random.randint(min, max)
        row3.append(call_num)
    row3[2] = "Free"

    min = 46
    max = 60
    for o in range(5):
        call_num = random.randint(min, max)
        while call_num in row4:
            call_num = random.randint(min, max)
        row4.append(call_num)

    min = 61
    max = 75
    for o in range(5):
        call_num = random.randint(min, max)
        while call_num in row5:
            call_num = random.randint(min, max)
        row5.append(call_num)

    board = np.array([row1, row2, row3, row4, row5])  # Convert the rows to an array
    board = np.transpose(board)  # Rotate the board the right way up
    print(f'Creating:', board)
    return board


counter = 1
called_nums = []


def start_game(board, coord_dict, win_con):
    '''start_game is what starts the game, and continuously runs throughout the game.'''
    print(win_con)
    if win_con == 1:
        t.goto(275, -25)
        t.color("white")
        t.dot(size=125)  # Cover the last generated number
        t.color("maroon")
        global counter
        # Create random number generator between 1 and 75, not repeating
        t.speed("fastest")
        t.goto(-100, -100)
        t.dot(size=75)      # Free space dot
        t.goto(275, -100)
        t.pendown()
        t.width(2.5)
        t.circle(75)
        t.penup()

        counter += 1
        generated_num = random.randint(1, 75)           # Generate a random number between 1 and 75
        while generated_num in called_nums:             # If that number was already called once before...
            generated_num = random.randint(1, 75)       # Generate a different number - repeat until a new number is found
        t.goto(230,-70)
        t.write(generated_num, font=("Verdana", 60, "normal"))
        called_nums.append(generated_num)           # Append the random number to a list of numbers

        for i in range(5):                      # Iterates for 5 rows
            for k in range(5):                  # Iterates for 5 columns
                if (board[i,k]) == "Free":      # If a position found was "Free", ignore it
                    pass
                elif int(board[i,k]) == generated_num:  # If a position was the generated number...
                    check_board[i,k] = 1                # Mark it off on the ghost board

                    t.goto(coord_dict[str(generated_num)])
                    t.dot(size=75)
        win_con = win_check(check_board)
        if win_con == 0:
            print('WIN')
            draw_board(True, 0)


def draw_board(ingame, win_con):
    '''draw_board first draws the 5x5 grid in turtle, and '''
    s.listen()

    if ingame:
        if win_con == 1:
            t.goto(-350,-350)
            t.pendown()
            for i in range(4):
                t.forward(500)
                t.left(90)
            t.penup()
            t.goto(-250, -350)
            t.left(90)
            t.pendown()
            t.forward(500)
            t.right(90)
            t.forward(100)
            t.right(90)
            t.forward(500)
            t.left(90)
            t.forward(100)
            t.left(90)
            t.forward(500)
            t.right(90)
            t.forward(100)
            t.right(90)
            t.forward(500)
            t.penup()
            t.goto(-350, -250)
            t.left(90)
            t.pendown()
            t.forward(500)
            t.left(90)
            t.forward(100)
            t.left(90)
            t.forward(500)
            t.right(90)
            t.forward(100)
            t.right(90)
            t.forward(500)
            t.left(90)
            t.forward(100)
            t.left(90)
            t.forward(500)
            t.penup()
            t.right(180)
            t.goto(-325, 175)
            t.write("B", font=("Verdana", 60, "normal"))
            t.goto(-215, 175)
            t.write("I", font=("Verdana", 60, "normal"))
            t.goto(-125, 175)
            t.write("N", font=("Verdana", 60, "normal"))
            t.goto(-25, 175)
            t.write("G", font=("Verdana", 60, "normal"))
            t.goto(75, 175)
            t.write("O", font=("Verdana", 60, "normal"))
            t.penup()

        else:
            # add victory text
            print("ALREADY WON")
            t.goto(-325,0)
            t.color("yellow")
            t.write("BINGO", font=("Verdana", 80, "normal"))
            t.color(0, 0, 0, .1)
            end_game()

        built_board = build_board()
        board_dict = populate_board(built_board)
        start_game(built_board, board_dict, win_con)


        def next_turn():
            '''next_turn runs the next turn for each round'''
            print('draw: ', win_con)
            start_game(built_board, board_dict, win_con)
        s.onkey(next_turn, "space")

        def change_view():
            '''change_view allows the player to switch to the title screen and the game view'''
            t.clear()
            draw_board(False, 1)

        s.onkey(change_view, "Return")
        s.mainloop()
    else:
        draw_instructions(instructions)

        def change_view():
            '''change_view allows the player to switch to the title screen and the game view'''
            t.clear()
            draw_board(True, 1)

        s.onkey(change_view, "Return")
        s.mainloop()


draw_board(False, 1)
