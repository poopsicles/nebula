# Introduction to Pygame
Congratulations on making it this far! ✨

If you've successfully completed all of the exercises in the previous modules (and practised a bit) then you should have a decent grasp of the basics of Python. So it's about time you got introduced to the tool we're going to use to create some awesome little games - Pygame.

Pygame is a multimedia library for Python for making games and multimedia applications.
That is to say, it wraps around the SDL (Simple DirectMedia Layer) library - making it easier for people like you and me to use.

Before we get around to using it however, we have to install it.

## Installing Pygame
To get started, open the terminal (Command Prompt on Windows, Terminal.app on macOS) and confirm Python is installed correctly using the following command:

```Bash
$ python --version

Python 3.11.0b1
```

The actual minor version doesn't matter...as long as you get something newer than `Python 3.4`, you should be good to go.

Then run the following command to install Pygame using the `pip` tool:

```Bash
$ python -m pip install -U pygame --user

Collecting pygame
Downloading pygame-2.1.2.tar.gz (10.1 MB)
Preparing metadata (setup.py)
...
```

If it fails at any way, you can check out the [official instructions](https://www.pygame.org/wiki/GettingStarted) in order to get help installing it.

You can also use Replit, which is an online service for running code over at their [website](https://replit.com/languages/pygame).


## Getting started with Pygame
To get started, open a new Python file and import the `pygame` module:

```Python
import pygame
```

This allows us to use any Pygame-related functions, which we obviously will be making a lot of use out of in any game we're trying to make.

Then initiate it by with the `pygame.init()` command, 

```Python
pygame.init()
```

We'll also have to quit it at the end of the program by using `pygame.quit()`.

Everything we're doing is going to be in our own *window*, our little section of the screen where all the action will be happening - and we need to tell Pygame the size of the window we'll be using. We'll do this by using the `display.set_mode(width, height)` function, like so:

```Python
screen = pygame.display.set_mode((640, 240))
```

If you were to run the program now, you would see that it just opens a window and closes it really quickly - this means that everything is working fine so far.

We can then set the title of our window - that's the little text that's shown at the top of the screen:

```Python
pygame.display.set_caption('A super simple window')
```

You can run your Pygame `.py` file the same way you would run any other Python file, by clicking the Play button in your editor, or using the appropriate terminal command:

```
~ ❯ python main.py

pygame 2.1.2 (SDL 2.0.16, Python 3.8.12)
Hello from the pygame community. https://www.pygame.org/contribute.html
```


## Familiarising yourself with the interface
Now that we have a functioning window, we want to actually put something in it. But lets walk through how a Pygame program functions to begin with.

For starters, everything in our program runs in a loop - it's just a lot of code reacting to events which the user uses to interact with the program - things like the mouse being moved or a key being pressed on the keyboard.

```Python
running = True

while running:
    for event in pygame.event.get():
		print(event)
		
        if event.type == pygame.QUIT:
            running = False

pygame.quit() 
```

Here, the main game loop is the `while running: ` which runs all the code within it as long as the `running` variable is true, and this code consists of printing out every event that occurs to the console. 

```
Example output:

<Event(4-MouseMotion {'pos': (173, 192), 'rel': (173, 192), 'buttons': (0, 0, 0), 'window': None})>
<Event(2-KeyDown {'unicode': 'a', 'key': 97, 'mod': 0, 'scancode': 0, 'window': None})>
<Event(3-KeyUp {'key': 97, 'mod': 0, 'scancode': 0, 'window': None})>
<Event(12-Quit {})>
```

If the event just happens to be the user clicking the Close button of the window, the `if` statement within the `while` block ends the program by jumping to the `pygame.quit()` line.

Next, we'll discuss the concept of a clock - which is simply used to measure time passage in games.

```Python
clock = pygame.time.Clock()
```

The clock speed can be increased to speed up the game, or reduced to slow down the game. It's also necessary for co-ordinating various aspects of the window, and making sure they all move in tandem with one another - we wouldn't want one thing to move unfairly fast now, would we?

```Python
clock.tick(60) # Runs at the end of every loop
```

We can also update the display, which is necessary for any kind of moving graphics as you'd want them to move at the appropriate speed. This, of course, is helped by the use of the clock which co-ordinates timing:

```Python
pygame.display.update()
```

This then redraws any elements that have moved since the last clock tick.

At the end of everything, we `quit()` the Python application to finally close the window.

```Python
quit()
```

If you got everything right, the final program should look like this:

```Python
import pygame

pygame.init()

screen = pygame.display.set_mode((640, 240))
pygame.display.set_caption('A super simple window')
clock = pygame.time.Clock()

running = True

while running:
    for event in pygame.event.get():
        print(event)
        		
        if event.type == pygame.QUIT:
            running = False
                    
    pygame.display.update()
    clock.tick(60) 
    
pygame.quit() 

quit()
```