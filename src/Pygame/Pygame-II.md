# Advanced Pygame
Here, we're doing to try using Pygame to do something a little more complicated - drawing a rectangle and moving it around. We're going to be making use of colours and event listeners in order to create the application.

## Working with colours
Colours can be represented as various combinations of red, green, and blue values. The complete absence of all of them results in black, while the complete presence is white.

```Python
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
```

The three primary colours are defined as:

```Python
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
```

These can then be mixed to produce various secondary colours:

```Python
YELLOW = (255, 255, 0)
CYAN = (0, 255, 255)
MAGENTA = (255, 0, 255)
```

So, to change our screen background colour, we can use the `screen.fill()` function with the colour that we want and then `pygame.display.update()` in order to make sure the change shows on screen.

```Python
screen.fill(CYAN)
pygame.display.update()
```


## Attaching changes to events
Let's start with our typical event loop skeleton, shall we?

```Python
import pygame

pygame.init()

screen = pygame.display.set_mode((640, 240))
pygame.display.set_caption('Another super simple window')

running = True

while running:
    for event in pygame.event.get():		
        if event.type == pygame.QUIT:
            running = False
            
	pygame.display.update()

pygame.quit() 

quit()
```

Add some nice colours:

```Python
pygame.display.set_caption('Another super simple window')

WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
YELLOW = (255, 255, 0)
CYAN = (0, 255, 255)
MAGENTA = (255, 0, 255)
```

And let's change the background colour in response to a key being pressed, in this case `B` for black and `W` for white:

```Python
running = True
background = WHITE # at the beginning, before any keys have been pressed

while running:
    for event in pygame.event.get():		
        if event.type == pygame.QUIT:
            running = False
            
        if event.type == pygame.KEYDOWN:
		    if event.key == pygame.K_b:
		        background = BLACK
		    elif event.key == pygame.K_w:
		        background = WHITE
		        
	screen.fill(background)
	pygame.display.update()
```

And now you can see that when any of those two buttons is clicked...the background changes colour.


## Working with rectangles
To draw a simple rectangle, use the `pygame.rect.Rect(x, y, width, height)` to assign it to an object:

```Python
box = pygame.rect.Rect(50, 60, 100, 50)
```

The `x` and `y` attributes handle the position of the top-left corner of the rectangle, thereby positioning it, while the `width` and `height` are pretty self-explanatory.

You can use the `move()` function to move the rectangle in a certain x-y direction:

```Python
speed = [2, 2]
box = box.move(speed)

if box.left < 0 or box.right > width:
    speed[0] = -speed[0]
if box.top < 0 or box.bottom > height:
    speed[1] = -speed[1]
```

What the second part of the code does, is that it ensures that the rectangle doesn't pass the boundaries of the window...reversing the direction of the movement.

Now we have to actually *draw* all the changes to the screen:

```Python
pygame.draw.rect(screen, RED, box, 1) # add the rectangle to the screen
pygame.display.update() # update the display
```

Putting it all together:
```Python
import pygame

pygame.init()

screen = pygame.display.set_mode((640, 240))
pygame.display.set_caption('Another super simple window')

WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
YELLOW = (255, 255, 0)
CYAN = (0, 255, 255)
MAGENTA = (255, 0, 255)

running = True
background = WHITE

box = pygame.rect.Rect(50, 60, 100, 50)
speed = [2, 2]

while running:
    for event in pygame.event.get():		
        if event.type == pygame.QUIT:
            running = False
                        
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_b:
                background = BLACK
            elif event.key == pygame.K_w:
                background = WHITE
        
    if box.left < 0 or box.right > 640:
        speed[0] = -speed[0]
    if box.top < 0 or box.bottom > 240:
        speed[1] = -speed[1]
        
    screen.fill(background)
    box = box.move(speed)
    
    pygame.draw.rect(screen, RED, box, 1) 
    pygame.display.update() 
        
pygame.quit() 

quit()
```

If you run it at this point, you'll realise that it's incredibly fast...this is because it's actually executing as fast as possible.

We don't want that, so let's add a clock to slow things down to normal speed:

```Python
pygame.display.set_caption('Another super simple window')
clock = pygame.time.Clock()

# ~~snip~~

	pygame.display.update()
	clock.tick(60)
```

There, much better 😃.


## Conclusion
By now, you should be ready to foray into more advanced Pygame tutorials, and in the next few modules we'll be putting all of these and more functions to create some fully-functional games that you can play with your friends or by yourself even.

You can go online to find more about Pygame [here](https://pygame.readthedocs.io/en/latest/1_intro/intro.html), if you need any more information about various functions and their uses.

Don't forget to have fun!