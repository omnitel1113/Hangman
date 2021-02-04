# Hangman
Code for Hangman game in Python using turtle library graphic functions
## Summary
To make my project # 2 from the "Python is easy" course of {PIRPLE.com} I studied the turtle graphic library for Python and used it in my Hangman game code. I also used the 3000 most common English words dictionary by Education First (EF) organization - https://www.ef.com/wwen/english-resources/english-vocabulary/top-3000-words/.

I tried to explain how I used different functions from turtle library with comments within in my code. 

## The Hangman code
```

import turtle as t
from random import randint
import time
#initialize turtle window and draw gallows
t.setup(700, 550)
t.title("HANGMAN")
t.penup()
t.setpos(100,-100)
t.pendown()
t.setx(250)
t.setx(175)
t.sety(200)
t.setx(50)
t.sety(125)
t.hideturtle()
x = -250
y = -200
h = 50
v = 90
hgn = 0
AC = []
def main():
    t.setundobuffer(10)
    # use number input method of turtle
    ch = t.numinput("Number of players", "How many players will play Hangman - one or two. Please enter 1 or 2: ", 1, 1, 2)
    print()
    if ch == 1:
        # use write method of turtle
        t.penup()
        t.setpos(x, y+60)
        t.write("OK. Here are the blanks for the letters of the word that the computer chose from dictionary.", True, align = "left", font=("Arial", 10, "normal"))
        words = open("3000words.csv", "r")
        L = words.read()
        WL = list(L.split(","))
        words.close()
        onePlayer(WL)
    elif ch == 2:
        # use text input method of turtle
        Word = t.textinput("Word for the game", "Player1 please enter the word for the game")
        twoPlayers(Word)
    else:
        # use write method of turtle
        t.penup()
        t.setpos(x, y+20)
        t.pencolor("red") # changing pen colors of turtle
        t.hideturtle()
        t.write("Invalid entry.", True, align = "left", font=("Arial", 10, "normal"))
        time.sleep(1)
        t.undo() # turtle function for undoing last action
        t.pencolor("black") # changing pen colors of turtle
        choice()
def onePlayer(WL):
    i = randint(0, len(WL))
    Word = WL[i]
    if len(Word) > 4:
        # print(Word)
        # turtle functions to draw blanks on the places of the letters from the word
        t.penup()
        t.setpos(x, y)
        for j in range(len(Word)):
            t.pendown()
            t.forward(25)
            t.penup()
            t.forward(10)
        t.hideturtle() # hide the turtle pointer
        LC = list(Word)
        Letter(LC, Word)
    else:
        onePlayer(WL)
def twoPlayers(Word):
    # print(Word)
    # turtle functions to draw blanks on the places of the letters from the word
    t.penup()
    t.setpos(x, y)
    t.hideturtle()
    for j in range(len(Word)):
        t.pendown()
        t.forward(25)
        t.penup()
        t.forward(10)
    t.hideturtle() # hide the turtle pointer
    LC = list(Word)
    Letter(LC, Word)
    
def Letter(LC, Word):
    global hgn
    # use text input method of turtle
    Let = t.textinput("Choose a letter", "Player please choose a letter to find out if it exists in the word")
    print()
    if len(Let) != 1:
        # use text input method of turtle
        t.penup()
        t.setpos(x, y+40)
        t.pencolor("red")
        t.write("Invalid entry.", True, align = "left", font=("Arial", 10, "normal"))
        time.sleep(1)
        t.undo()
        t.pencolor("black")
        Letter(LC, Word)
    elif Let == "^":
        # use text input method of turtle
        t.penup()
        t.setpos(x, y+40)
        t.pencolor("red")
        t.write("Invalid entry.", True, align = "left", font=("Arial", 10, "normal"))
        time.sleep(1)
        t.undo()
        t.pencolor("black")
        Letter(LC, Word)
    else:
        Lt = Let.lower()
        if AC.count(Lt) == 0:
            AC.append(Lt)
            c = LC.count(Lt)
            if c != 0:
                LetType(LC, Lt, c, Word) # type a letter by turtle
            else:
                # draw hangman step by step via functions arranged in a list. call the corresponding function by its index
                DrH = [Head, Body, LtHand, RtHand, LtLeg, RtLeg]
                DrH[hgn]()
                hgn +=1
                if hgn == 6:
                    # use write method of turtle
                    t.penup()
                    t.setpos(x-60, y+420)
                    t.pencolor("blue")
                    t.hideturtle()
                    t.write("Sorry! You were HANGED. The word was: ", True, align = "left", font=("Arial", 14, "normal"))
                    t.pencolor("green")
                    t.write(Word, True, align = "left", font=("Arial", 14, "normal"))
                    t.pencolor("black")
                    return
                else:
                    Letter(LC, Word)
        else:
            # use write method of turtle
            t.penup()
            t.setpos(x, y+40)
            t.pencolor("red")
            t.hideturtle()
            t.write("You have already entered this letter. Try again.", True, align = "left", font=("Arial", 10, "normal"))
            time.sleep(1)
            t.undo()
            t.pencolor("black")
            Letter(LC, Word)
            
def LetType(LC, Lt, c, Word):
    for j in range(c):
        ix = LC.index(Lt)
        t.penup()
        t.setpos(x+35*ix+12.5, y)
        t.write(Lt, True, align = "center", font=("Arial", 24, "normal"))
        LC.pop(ix)
        LC.insert(ix, "^")
    if LC.count("^") == len(LC):
        # use write method of turtle
        t.penup()
        t.setpos(x-70, y+420)
        t.pencolor("blue")
        t.hideturtle()
        t.write("CONGRATULATIONS! YOU WIN THE GAME WITHOUT BEING HANGED.", True, align = "left", font=("Arial", 14, "normal"))
        t.pencolor("black")
        return
    else:
        Letter(LC, Word)
    
# turtle functions to draw a part of the hangman
def Head():
    t.penup()
    t.setpos(h, v)
    t.pendown()
    t.circle(20)
    t.hideturtle()
def Body():
    t.penup()
    t.setpos(h, v)
    t.pendown()
    t.setheading(270)
    t.forward(80)
    t.hideturtle()
def LtHand():
    t.penup()
    t.setpos(h, v-30)
    t.pendown()
    t.setheading(140)
    t.forward(50)
    t.hideturtle()
def RtHand():
    t.penup()
    t.setpos(h, v-30)
    t.pendown()
    t.setheading(40)
    t.forward(50)
    t.hideturtle()
def LtLeg():
    t.penup()
    t.setpos(h, v-80)
    t.pendown()
    t.setheading(230)
    t.forward(70)
    t.hideturtle()
def RtLeg():
    t.penup()
    t.setpos(h, v-80)
    t.pendown()
    t.setheading(320)
    t.forward(70)
    t.hideturtle()
    
main()
    
    ```
               
