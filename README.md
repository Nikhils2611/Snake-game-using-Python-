# Snake-game-using-Python-
#Classic game of snake mania created  using python

import random   #Returns a random number between the given range of choice()
import turtle   #Turtle” is a Python feature like a drawing board, which lets us command a turtle to draw all over it
import time     # Python time module allows to work with time in Python

delay = 0.1 
score = 0 
highest_score = 0 

bodies=[]    #We will keep track of the length of the body of the snake 

#Creation of the screen of the Game
s=turtle.Screen
s.title("Snake Game")
s.bgcolor("grey")
s.setup(width=600, height=600)

#Creation of the snake head
head=turtle.Turtle()
head.speed(0)  # if you set the speed to 0, it has a special meaning — turn off animation and go as fast as possible. 
head.shape("square")
head.color("white")
head.fillcolor("blue")
head.penup()    #if you use penup() it will stop drawing when you move the turtle(no lines left behind)
head.goto(0, 0) #turtle starts in centre
head.direction="stop"   #to change the direction that the turtle is facing

#Snake food 
food= turtle.Turtle()
food.speed(0)
food.shape("square")
food.color("yellow")
food.fillcolor("green")
food.penup()
food.ht()   #hideturtle or ht means hide the turtle, so you can admire your drawing
food.goto(0, 200)
food.st() #showturtle or st is used to show the turtle 

#Creating Score board
sb=turtle.Turtle()
sb.shape("square")
sb.fillcolor("black")
sb.penup()
sb.ht()
sb.goto(-250, -250)
sb.write("Score = 0  |  Highest Score = 0")

def moveup():
    if head.direction != "down":
        head.direction= "up"
def movedown():
    if head.direction != "up":
        head.direction= "down"
def moveleft():
    if head.direction != "right":
        head.direction= "left"
def moveright():
    if head.direction != "left":
        head.direction= "right"

#All these functions(upwards) are defined so that the snake do not crosses itself

def movestop():
    head.direction= "stop"
def move():
    if head.direction=="up":
        y=head.ycor()
        head.sety(y+20)
    if head.direction=="down":
        y=head.ycor()
        head.sety(y-20)
    if head.direction=="left":
        x=head.xcor()
        head.setx(x-20)
    if head.direction=="right":
        x=head.xcor()
        head.setx(x+20)
    
#Event Handling (Keyboard functions)
s.listen()
s.onkey(moveup , "Up")
s.onkey(movedown , "Down")
s.onkey(moveright , "Right")
s.onkey(moveleft , "Left")
s.onkey(movestop , "Space")

#Main loop that will run contiuosly 
while(True):
    s.update()  #This is to check the collision with screen and to check collisions with borders
    if head.xcor() > 290:
        head.setx(-290)
    if head.xcor() <-290:
        head.setx(290)
    if head.ycor() >290:
        head.sety(-290)
    if head.ycor() <-290:
        head.sety(290)

#Check collision with food 
if head.distance(food)<20:
    #move the food to a new random place 
    x=random.randint(-290 , 290)
    y=random.randint(-290 , 290)
    food.goto(x,y)
    
    #Increasing the length of the snake 
    body=turtle.Turtle()
    body.speed(0)
    body.shape("square")
    body.penup()
    body.color("red")
    body.fillcolor("black")
    bodies.appen(body)  #Appending food to body
    score=score+10   #increasing the score
    delay=delay-0.001   #Changing delay , to increase speed
    #Updating highest score--
    if score > highest_score:
        highest_score = score
    sb.clear()
    sb.write("Score: ()  | Highest Score: () ".formt(score,highest_score))

    #Moving the snake body
    for index in range(len(bodies)-1,0,-1):
        x=bodies[index-1].xcor()
        y=bodies[index-1].ycor()
        bodies[index].goto(x,y)
        
        if len(bodies)>0:
            x=head.xcor()
            y=head.ycor()
            bodies[0].goto(x,y)
        move()
        
    #Checking collision with snake body
    for body in bodies:
        if body.distance(head)<20:
            time.sleep(1)
            head.goto(0,0)
            head.directioc="stop"
            
        #hide bodies
        for body in bodies:
            body.ht()
            bodies.clear()
            score=0
            delay=0.1
            
        #Update score
        sb.clear()
        sb.write("Score: ()  | Highest Score: () ".formt(score,highest_score))
    time.sleep(delay)
s.mainloop()
#This is the end of our game.
