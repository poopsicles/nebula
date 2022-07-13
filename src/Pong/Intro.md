# Introduction to Pong
Pong is a simple game that has two **paddles** and a ball. The entire concept of the game is to bounce the ball in the direction of your opponent using a paddle.

![The game running](PONG-Image.png)

## Things that we need to implement
1.  We define some variables that our little game is going to use like the background color, the foreground color, the font, the screen width and height and some other stuffs
2. We need to create the paddles and write code for the movement of the paddles
3. We need to create the ball and write the physics for the ball bouncing up and down or left and write
4. We need to keep track of scoring and display it to the screen


## Defining the Pygame stuffs
In this step, We define the variables that we would be using throughout the game and initialize some pygame specific stuffs.

The first thing that we need to do is import some modules
```python
from math import *
import pygame
import sys
import random
```

>We assume that these are at the top of your file throughout the entire course

Then we need to define how large our screen is.
```python
screen_size = (screen_width, screen_height) = (1000, 400)
```

In the line above, we create a variable `screen_size` that holds a tuple with the `screen_width` and `screen_height`. The value of 1000 and 400 is given to the screen's width and screen's height respectively. This means that the screen is 1000 pixels wide and 400 pixels high.

Then we create a pygame display surface with the screen size and store it in the variable name `display`.
```python
display = pygame.display.set_mode(screen_size)`
```

The next step is to then define some colors that the game would use for the game. We use a hexadecimal value for the colors. You can use any color that you want for the background and the foreground of the game.

```python
background_color = pygame.Color("#1b1b1e")
white_color = pygame.Color("#F2F3D9")
```

Then we need to define some variables that will hold the score. The value of the scores by default will be `0`.
```python
left_score = 0
right_score = 0
```

Then we need to define a clock object and framerate that would help to keep timing in our game. 
```python
clock = pygame.time.Clock()
FRAMERATE = 60
```

Then we set the title of the screen to be *Pong*
```python
pygame.display.set_caption("Pong")
```

Then we initialize pygame 
```python
pygame.init()
```

Now we have all the basic variables and have initialized pygame we can then move on to creating the class that would represent the ball and the player
