# Implementation of the Paddles
We can think of the paddles as rectangles that exist on the edge of the screen.
So we create a player class that is an object representation of the paddle and then we implement the movement method and the show method.

## The Init Method
The first step is the init method where we initialize some variables that the player would need.
```python
def __init__(self, x: int) -> None:
	self.x = x
```
The value that is going to be passed into the init function is then going to be stored in `self.x`

The next step is to then calculate the height of the paddles. We want the height to be 40% of the screen height. So that is 
```python
height = screen_height * (40/100)
```

but since we know how to do math, we can just say 
``` python
height  = screen_height * 0.4
```

So therefore, we just say 
```python
	self.h = screen_height * 0.4
```

Then the next step is to then calculate the y position of the player since we want the paddles initial position to be in the center of the screen we set the value of the property of the class to be 
```python
	self.y = (screen_height - self.h) / 2
```
 We minus the `screen_height` by the height of the paddle and divide by 2 to center the paddle on the screen.

Then we set the width of the player to 20 pixels.
```python
	self.w = 20
```

Then we initialize a rectangle that will represent the rectangle of the paddle that would be drawn on the screen. We use the variables that we defined before to create the rectangle object.
```python
	self.rect = pygame.rect.Rect(self.x, self.y, self.w, self.h)
```

Then we define how fast the paddles should move by storing the value in a property called `move_speed`.
```python
	self.move_speed = 15
```

Then the next step exists to fix an issue that our game would have had.

When we press a key, we are meant to be moving the player until we let go of the key.
Due to the fact that pygame only has a `KEYDOWN` event and not a `KEYHOLD` event, the event loop only executes what we put in the `KEYDOWN` section once. 
To re-execute what is in the `KEYDOWN` section we would have to release the key that we are using to move the player and then repress the key again in order to move the player. 
So to fix that, we could exploit the fact that pygame has a `KEYDOWN` event and a `KEYUP` event to make our player move as long as we hold a key down. 
So we define a property called `moving` that is a boolean that stores whether we should be moving or not.
```python
	self.moving = False
```

We also have a property called `moving_up` that specifies if we are moving up or not.
```python
	self.moving_up = False
```

And that wraps up the `__init__` function for our player class.

So the final player class `__init__` method should look like
```python
    def __init__(self, x: int) -> None:
        # ? We set the x and y position of the player. The value of the y position is at the center of the screen by default
        self.x = x
        self.h = screen_height*0.4
        self.y = (screen_height - self.h) / 2
        self.w = 20

        # ? This is the rectangle object of the player class
        self.rect = pygame.rect.Rect(self.x, self.y, self.w, self.h)

        # ? This is how fast the paddles move every frame
        self.move_speed = 15

        # ? This is a variable to basically tell if the paddle is moving
        # ? We do this because pygame doesn't have a built in key held down event type
        # ? Further explanation in the event loop
        self.moving = False
        self.moving_up = False

```


## The Update Method
This is the method that is called every frame and specifies how the paddles move up and down.
So we first check if the paddle is moving at all, if so we then check if it is moving up. So if we are moving up, we want to minus the `self.rect.y` (i.e the y position's of the paddle) by the `self.move_speed`. If we are moving down, we just add `self.rect.y` and  `self.move_speed`.
```python
if self.moving:
	if self.moving_up:
		self.rect.y -= self.move_speed
	else:
		self.rect.y += self.move_speed
```

But the major problem with this is that we need to check if we are moving we reach the top edge of the screen or the bottom edge of the screen and then we should stop moving the paddle. 
We can do this for the top edge by checking if the `self.rect.y < 0` . If `self.rect.y` is negative it should just set the value of the paddle's y location to be 0 which will just make it stay at the top position of the screen.
We do something similar for the bottom edge, we just check if the bottom part of the paddle(i.e `self.rect.y + self.rect.h`) has reached the bottom edge. We just check if `self.rect.y + self.rect.h > screen_height`, if so, we set the value of `self.rect.y` to be `screen_height-self.rect.h` which will just keep the player at the bottom of the screen.
So the update function with all the checks should something like this

```python
    def update(self):
        # ? We check if we are moving first and then change the y position of the paddle's rectangle accordingly
        if self.moving:
            if self.moving_up:
                # ? We check if we have reached the top of the screen and then stop changing the y postion of the paddle's rectangle
                if self.rect.y > 0:
                    self.rect.y -= self.move_speed
                else:
                    self.rect.y = 0
            else:
                # ? We check if we have reached the bottom of the screen and then stop changing the y postion of the paddle's rectangle
                if self.rect.y < screen_height - self.h:
                    self.rect.y += self.move_speed
                else:
                    self.rect.y = screen_height - self.h
```

And we are done with the update function.


## The Reset Function
This is just a function that resets the position of the paddles when it is called.

We just change the `self.rect.y` to be at the center of the screen and then change the `self.moving` to be false.

The function look like this.
```python
    # ? This just resets the paddle's location
    def reset(self):
        self.rect.y = (screen_height - self.h)/2
        self.moving = False
```

And we are done with the reset function.


##### The show Method
This method draws the `self.rect`  of the player

The code for the function looks like this.
```python
    # ? This shows the draws the paddle's rectangle object to the screen
    def show(self):
        pygame.draw.rect(display, foreground_color, self.rect)
```

And we are done with show function.

## Finalizing everything
We are done with the entire player class so the final code looks like

```python
# ? This is the Player Object
class Player:
    def __init__(self, x: int) -> None:
        # ? We set the x and y position of the player. The value of the y position is at the center of the screen by default
        self.x = x
        self.h = screen_height*0.4
        self.y = (screen_height - self.h) / 2
        self.w = 20

        # ? This is the rectangle object of the player class
        self.rect = pygame.rect.Rect(self.x, self.y, self.w, self.h)

        # ? This is how fast the paddles move every frame
        self.move_speed = 15

        # ? This is a variable to basically tell if the paddle is moving
        # ? We do this because pygame doesn't have a built in key held down event type
        # ? Further explanation in the event loop
        self.moving = False
        self.moving_up = False


    # ? This is the update function that gets called every frame
    def update(self):
        # ? We check if we are moving first and then change the y position of the paddle's rectangle accordingly
        if self.moving:
            if self.moving_up:
                # ? We check if we have reached the top of the screen and then stop changing the y postion of the paddle's rectangle
                if self.rect.y > 0:
                    self.rect.y -= self.move_speed
                else:
                    self.rect.y = 0
            else:
                # ? We check if we have reached the bottom of the screen and then stop changing the y postion of the paddle's rectangle
                if self.rect.y < screen_height - self.h:
                    self.rect.y += self.move_speed
                else:
                    self.rect.y = screen_height - self.h

    # ? This just resets the paddle's location def reset(self):
        self.rect.y = (screen_height - self.h)/2
        self.moving = False

    # ? This shows the draws the paddle's rectangle object to the screen
    def show(self):
        pygame.draw.rect(display, foreground_color, self.rect)
```

Now that we have implemented the player class we can move on to the madness that is the `Ball` class

