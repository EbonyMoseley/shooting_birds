# shooting_birds

# HW7: shooting stars game using graphics
# Author: Ebony Moseley
# Apr. 4, 2017

from graphics import *
import random


def make_bird(x,y):
    """Makes a bird that will be used many times for the game
    at a given point. x and y are formal parameters.
    Returns the bird as a graphics polygon."""
    
    bird = Polygon(Point(x-20,y),Point(x+15,y),Point(x,y+15),Point(x-10,y-10),Point(x+10,y-10))
    bird.setFill("dark gray")
    bird.setOutline("dark gray")
    
    return bird


def main():
    #window dimensions
    width = 800
    height = 600
    
    #makes the window
    win = GraphWin("Catch the Bird", width, height)
    win.setBackground("light blue")
    
    #sun added for appeal
    sun = Circle(Point(800,0),100)
    sun.draw(win)
    sun.setFill("yellow")
    sun.setOutline("yellow")
    #grass added for appeal
    grass = Rectangle(Point(0,550),Point(800,600))
    grass.draw(win)
    grass.setFill("dark green")
    grass.setOutline("dark green")
    

    #makes 30 birds appear one at a time
    for i in range(30):
        
        #makes the bird appear in random location each time
        x = random.randint(0,800)
        y = random.randint(0,600)
        
        #calls the helper function to make the bird
        #x and y are the helper function's actual parameters
        bird = make_bird(x,y)
        bird.draw(win)

        #makes each bird move in a different direction
        dx = random.uniform(-2,2)
        dy = random.uniform(-2,2)

        #while the bird is within the boundaries of the window (minus a little)
        while x > 10 and x < width - 10 and y > 10 and y < height - 10:
            #moves bird in the randomly chosen direction
            bird.move(dx,dy)
            #gets the x and y values of the first point of the polygon
            center = bird.getPoints()[0]
            #centralizes x by adding 20 back
            x = center.getX() + 20
            y = center.getY()

            #finds the mouse's click
            click = win.checkMouse()

            #if the mouse has been clicked
            if click != None:
                #gets x and y coordinates of where the mouse was clicked
                click.x = click.getX()
                click.y = click.getY()
                
                #if where the mouse was clicked is near the x and y coordinate of the bird's point
                if click.x <= x + 15 and click.x >= x - 15 and click.y <= y + 15 and click.y >= y - 15:
                    #"kills" bird and goes to the next bird
                    #this is a bit gory
                    bird.undraw()
                    blood = Oval(Point(x,y),Point(x+40,y-5))
                    blood.setFill("dark red")
                    blood.setOutline("dark red")
                    blood.draw(win)
                    blood.move(0,height-y)

                    
    #at the end of the game (when all 30 birds have gone), writes "THE END"
    text = Text(Point(400,300), "THE END")
    text.draw(win)
    text.setSize(30)


main()
