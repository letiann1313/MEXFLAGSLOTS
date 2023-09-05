'SlotMachine'
import random
print('''Welcome to the Slot Machine Simulator
You'll start with $500. You'll be asked if you want to play.
Answer with yes/no. you can also use y/n
No case sensitivity in your answer.
For example you can answer with YEs, yEs, Y, nO, N.
To win you must get one of the following combinations:
13\t13\t13\t\tpays\t$5000
MM\tMM\tMM/13\tpays\t$1000
RED\tRED\tRED/13\tpays\t$500
GREEN\tGREEN\tGREEN/13\tpays\t$100
MEXFLAG\tMEXFLAG\tMEXFLAG\t\tpays\t$50
MEXFLAG\tMEXFLAG\t -\t\tpays\t$10
MEXFLAG\t -\t -\t\tpays\t$5
''')
#Constants:
INIT_STAKE = 500
ITEMS = ["MEXFLAG", "WHITE", "GREEN", "RED", "MM", "13"]

firstWheel = None
secondWheel = None
thirdWheel = None
stake = INIT_STAKE

def play():
    global stake, firstWheel, secondWheel, thirdWheel
    playQuestion = askPlayer()
    while(stake != 0 and playQuestion == True):
        firstWheel = spinWheel()
        secondWheel = spinWheel()
        thirdWheel = spinWheel()
        printScore()
        playQuestion = askPlayer()

def askPlayer():
    '''
    Asks the player if he wants to play again.
    expecting from the user to answer with yes, y, no or n
    No case sensitivity in the answer. yes, YeS, y, y, nO . . . all works
    '''
    global stake
    while(True):
        answer = input("You have $" + str(stake) + ". Would you like to play? ")
        answer = answer.lower()
        if(answer == "yes" or answer == "y"):
            return True
        elif(answer == "no" or answer == "n"):
            print("You ended the game with $" + str(stake) + " in your hand.")
            return False
        else:
            print("wrong input!")

def spinWheel():
    '''
    returns a random item from the wheel
    '''
    randomNumber = random.randint(0, 5)
    return ITEMS[randomNumber]

def printScore():
    '''
    prints the current score
    '''
    global stake, firstWheel, secondWheel, thirdWheel
    if((firstWheel == "MEXFLAG") and (secondWheel != "MEXFLAG")):
        win = 10
    elif((firstWheel == "MEXFLAG") and (secondWheel == "MEXFLAG") and (thirdWheel != "MEXFLAG")):
        win = 20
    elif((firstWheel == "MEXFLAG") and (secondWheel == "MEXFLAG") and (thirdWheel == "MEXFLAG")):
        win = 50
    elif((firstWheel == "GREEN") and (secondWheel == "GREEN") and ((thirdWheel == "GREEN") or (thirdWheel == "13"))):
        win = 100
    elif((firstWheel == "RED") and (secondWheel == "RED") and ((thirdWheel == "RED") or (thirdWheel == "13"))):
        win = 500
    elif((firstWheel == "MM") and (secondWheel == "MM") and ((thirdWheel == "MM") or (thirdWheel == "13"))):
        win = 1000
    elif((firstWheel == "13") and (secondWheel == "13") and (thirdWheel == "13")):
        win = 5000
    else:
        win = -13

    stake += win
    if(win > 0):
        print(firstWheel + '\t' + secondWheel + '\t' + thirdWheel + ' -- You win $' + str(win))
    else:
        print(firstWheel + '\t' + secondWheel + '\t' + thirdWheel + ' -- You lose')

play()
