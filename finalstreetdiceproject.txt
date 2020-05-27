
import tkinter
from PIL import ImageTk
from PIL import Image

CANVAS_WIDTH = 1000
CANVAS_HEIGHT = 600

import random
MIN_RANDOM = 1
MAX_RANDOM = 6

"""
This program creates a game of street dice. The shooter first makes a bet as to whether they will
"pass" or "not pass". A "pass" on the first roll is rolling a 7 or 11, and a "not pass" is rolling 
a 2, 3, or 12. If the shooter does not roll any of these numbers, the number of the first roll becomes 
the "set point". The shooter continues to roll and must roll the set point again before rolling a 7.
If the shooter rolls a 7, they automatically lose.
This program also prints graphics for the die rolled.
The user must manually close the graphics in order to continue playing the game.
The shooter makes bets on whether or not he or she will pass after the first roll and for all subsequent
rolls, assuming the first roll was not a 2, 3, 7, 11, or 12.
"""

def main():
    print("This program simulates a game of Street Dice, played in the Broadway musical 'Guys and Dolls.'")
    # following text explains the rules of the game
    print("You will roll 2 die, each numbered from 1 to 6.")
    print("On your first roll, you pass if you roll a 7 or 11 and don't pass if you roll a 2, 3, or 12.")
    print("If you roll any other number between 4 and 10, that number becomes the new set point.")
    print("You must roll the set point before rolling another 7, or else you lose.")
    # user guesses whether they will pass or "crap out"
    rolled = 0
    canvas = make_canvas(CANVAS_WIDTH, CANVAS_HEIGHT, title='Rolled Die')
    canvas.create_text(100, 50, anchor="w", font="Courier 52", text="Do not close this")
    canvas.create_text(100, 200, anchor="w", font="Courier 52", text="image until you")
    canvas.create_text(100, 350, anchor="w", font="Courier 52", text="complete the game.")
    bet = input('Bet if you will "pass" or "not pass" on your first roll: ')
    if bet == 'pass' or bet == 'not pass':
        set_point = first_roll(rolled, canvas)
    else:
        bet = input('You must type "pass" or "not pass": ')
        first_roll(rolled, canvas)
        set_point = rolled
        print(set_point)

    if set_point == 4 or set_point == 5 or set_point == 6 or set_point == 8 or set_point == 9 or set_point == 10:
        # the number rolled on the first roll becomes the new set point
        # the shooter tries to roll this number again
        print(str(set_point) + " is now the set point.")
        set_point_roll(set_point, canvas)
    elif set_point == 7 or set_point == 11:
        # if a 7 or 11 is the first roll, the shooter automatically wins
        print('Congratulations! You have won the game.')
        if bet == 'pass':
            print("You bet correctly.")
        elif bet == 'not pass':
            print('You bet incorrectly.')
        print("Close the image to play again.")
        canvas.mainloop()
    else:
        # if the shooter rolls a 2, 3, or 12 on the first roll, the shooter automatically loses
        print('You have lost the game.')
        if bet == 'pass':
            print("You bet incorrectly.")
        elif bet == 'not pass':
            print('You bet correctly.')
        print("Close the image to play again.")
        canvas.mainloop()

def first_roll(rolled, canvas):
    roll = roll_die(rolled, canvas)
    return roll

def roll_die(rolled, canvas):
    # rolls 2 dice, each labeled 1-6, at random and adds the values (total is between 2 and 12)
    die_1 = random.randint(MIN_RANDOM, MAX_RANDOM)
    die_2 = random.randint(MIN_RANDOM, MAX_RANDOM)
    rolled = die_1 + die_2
    print("You rolled a " + str(rolled) + ".")
    make_graphic(die_1, die_2, canvas)

    return rolled

def set_point_roll(set_point, canvas):
    print("You must roll until you roll " + str(set_point) + " again. If you roll a 7, you lose.")
    bet = input('Bet if you will pass or not pass: ')
    if bet == 'pass' or bet == 'not pass':
        #introduces the rules for rolling to reach the set point
        new_roll(set_point, bet, canvas)
    else:
        bet = input('You must type "pass" or "not pass": ')
        new_roll(set_point, bet, canvas)

def new_roll(set_point, bet, canvas):
    # if the user inputs 'Roll', they will continue rolling die until they get either a 7 or the set point again
    next_roll = input("Enter 'Roll' to roll again: ")
    rolled = 0
    if next_roll == 'Roll':
        rolled = roll_die(rolled, canvas)
    else:
        print("You must type 'Roll' to roll again.")
        new_roll(set_point, bet, canvas)

    if rolled == 7:
        print("You have lost the game.")
        if bet == 'pass':
            print("You bet incorrectly.")
        elif bet == 'not pass':
            print("You bet correctly.")
        print("Close the image to play again.")
        canvas.mainloop()
    elif rolled == set_point:
        print("Congratulations! You have won the game!")
        if bet == 'pass':
            print("You bet correctly.")
        elif bet == 'not pass':
            print("You bet incorrectly.")
        print("Close the image to play again.")
        canvas.mainloop()
    elif rolled != 7 or set_point:
        print('You must roll again.')
        new_roll(set_point, bet, canvas)

def make_graphic(die_1, die_2, canvas):
    # makes graphics depicting the die rolled
    canvas.delete('all')
    if die_1 == 1 and die_2 == 1:
        #if two 1s are rolled, the graphic shows the user rolled a "snake eyes"
        canvas.create_text(275, 530, anchor="w", font='Courier 52', text="Snake Eyes!", fill="blue")
    dice_1 = make_dice_1(canvas, die_1)
    dice_2 = make_dice_2(canvas, die_2)
    canvas.update()


def make_dice_1(canvas, die_1):
    canvas.create_rectangle(520, 20, 980, 480)
    canvas.create_text(225, 530, anchor="w", font='Courier 52', text=str(die_1))
    if die_1 == 1:
        canvas.create_oval(180, 180, 320, 320, fill="black")
    elif die_1 == 2:
        canvas.create_oval(80, 80, 200, 200, fill="black")
        canvas.create_oval(280, 280, 400, 400, fill="black")
    elif die_1 == 3:
        canvas.create_oval(50, 50, 150, 150, fill="black")
        canvas.create_oval(180, 180, 280, 280, fill="black")
        canvas.create_oval(310, 310, 410, 410, fill="black")
    elif die_1 == 4:
        canvas.create_oval(100, 100, 220, 220, fill="black")
        canvas.create_oval(100, 270, 220, 390, fill="black")
        canvas.create_oval(280, 100, 400, 220, fill="black")
        canvas.create_oval(280, 270, 400, 390, fill="black")
    elif die_1 == 5:
        canvas.create_oval(60, 50, 160, 150, fill="black")
        canvas.create_oval(60, 320, 160, 420, fill="black")
        canvas.create_oval(190, 180, 290, 280, fill="black")
        canvas.create_oval(330, 50, 430, 150, fill="black")
        canvas.create_oval(330, 320, 430, 420, fill="black")
    elif die_1 == 6:
        canvas.create_oval(100, 50, 200, 150, fill="black")
        canvas.create_oval(100, 180, 200, 280, fill="black")
        canvas.create_oval(100, 310, 200, 410, fill="black")
        canvas.create_oval(300, 50, 400, 150, fill="black")
        canvas.create_oval(300, 180, 400, 280, fill="black")
        canvas.create_oval(300, 310, 400, 410, fill="black")
    # creates a graphic for the first die roll, based on the number rolled

def make_dice_2(canvas, die_2):
    canvas.create_rectangle(20, 20, 480, 480)
    canvas.create_text(725, 530, anchor="w", font='Courier 52', text=str(die_2))
    # creates a graphic for the second die roll, based on the number rolled
    # on the same canvas as the first die
    if die_2 == 1:
        canvas.create_oval(680, 180, 820, 320, fill="black")
    elif die_2 == 2:
        canvas.create_oval(580, 80, 700, 200, fill="black")
        canvas.create_oval(780, 280, 900, 400, fill="black")
    elif die_2 == 3:
        canvas.create_oval(550, 50, 650, 150, fill="black")
        canvas.create_oval(680, 180, 780, 280, fill="black")
        canvas.create_oval(810, 310, 910, 410, fill="black")
    elif die_2 == 4:
        canvas.create_oval(600, 100, 720, 220, fill="black")
        canvas.create_oval(600, 270, 720, 390, fill="black")
        canvas.create_oval(780, 100, 900, 220, fill="black")
        canvas.create_oval(780, 270, 900, 390, fill="black")
    elif die_2 == 5:
        canvas.create_oval(560, 50, 660, 150, fill="black")
        canvas.create_oval(560, 320, 660, 420, fill="black")
        canvas.create_oval(690, 180, 790, 280, fill="black")
        canvas.create_oval(830, 50, 930, 150, fill="black")
        canvas.create_oval(830, 320, 930, 420, fill="black")
    else:
        canvas.create_oval(600, 50, 700, 150, fill="black")
        canvas.create_oval(600, 180, 700, 280, fill="black")
        canvas.create_oval(600, 310, 700, 410, fill="black")
        canvas.create_oval(800, 50, 900, 150, fill="black")
        canvas.create_oval(800, 180, 900, 280, fill="black")
        canvas.create_oval(800, 310, 900, 410, fill="black")


def make_canvas(width, height, title=None):

    """

    DO NOT MODIFY

    Creates and returns a drawing canvas

    ready for drawing.

    """

    objects = {}

    top = tkinter.Tk()

    top.minsize(width=width, height=height)

    if title:

        top.title(title)

    canvas = tkinter.Canvas(top, width=width + 1, height=height + 1)

    canvas.pack()

    return canvas

if __name__ == '__main__':
    main()