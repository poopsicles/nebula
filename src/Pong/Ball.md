# Implementation of the ball
In this section, we are going to implement the ball class. This is the entire crux of our game. This class is the bulkiest and the most tricky because it defines basically all of our game. Enough talking let's just get started with it 

## Logic of the ball
Before we dive into the code we need to know what we are going to implement. We need to talk about *logic*.
The first thing that we need to talk about is how the ball will move around.
The ball is meant to move in an angle that will be gotten from a list of all possible angles

The angles that the game should support should be
```python
possible_angles = [15, 45, 60, 120, 150, 135]
```

To make the ball move in a direction we use a **Vector**.

A **Vector** can be thought of as an arrow that just points in a direction(In our case it points at an angle). 
The vector has an **angle** that is going to be pointing at and a **magnitude** that represents how long it is(The **magnitude** for us will just the speed of the ball). 
We can use multiple vectors to move an object in a specified direction.

A vector has two components, the **x component** and the **y component**. 
The **x component** just specifies which the speed which the vector is moving in the x direction and the **y component** specifies the speed at which it moves in the y direction.

We can calculate the x and y component of a vector using the following formula
```
x_component = (magnitude * cos(angle))
y_component = (magnitude * sin(angle))
```

For our game we will just need one vector to specify which angle the ball is going to be moving in.

So to get the ball moving in the direction of the vector we just calculate the x and y component of the vector and then add it to the ball's x and y location.

And that should be all for moving the ball.

But wait we are not yet done with the *logic* of the ball.

We still have one major hurdle to overcome which is...**Collisions**.

So we are going to talk about collisions right now.

The requirements that we would need for our game's collision is as follows
1. We need to know when we collide with any side of the screen and we also need to check if we collide with the paddles.
2. If we collide, we then need to reverse the direction of the vector and also check if we scored so that we increment the score counter for the other player and reset the game state.
3. And I think that's all??

So let's get right into it.

First we can easily check if two rectangle in pygame by using a method in the `pygame.rect.Rect` class called `colliderect`.
The `colliderect` function returns `True` or `False` depending on if two rectangles collide.
We will use that to check if we collide with a paddle.

So now checking collisions with the screen, this was discussed in one of your previous chapters but for sake of emphasis, let's just talk about them again.

All we just need to check if we,
1. Collide with the top or the bottom of the screen, so that we can reverse the direction of the ball so that it can bounce
2. Collide with the left or the right of the screen, so that we can then increase the score of the player that scored.

First, let's do collisions with the top and the bottom of the screen.

In order to check if the ball collides with the top of the screen, we just need to check the if the ball's y location is less than zero, we flip the y component of the ball so that the ball then start's pointing down but keeps moving in the original x direction.

We can reverse the **y component** by multiplying it by -1
```python
if(self.rect.y < 0):
	self.y_component *= -1
```

Since that's done, we then need to check if the ball collides with the bottom of the screen.

To check if the ball has collided with the bottom of the screen, we just check if the ball's y location + the ball's height is greater than the height of the screen. If it collides, we also flip the **y component** of the vector
```python
if (self.rect.y + self.rect.h > screen_height):
	self.y_component *= -1
```

So the final snippet for checking if the ball collides with either the top or the bottom of the screen would be

```python
if self.body.y <= 0 or self.body.y + self.body.h >= screen_height:
	self.y_component *= -1
```

Now on to the next step, we need to then check if we collide with either of the paddles.

The logic for this is quite similar to the logic for checking if the ball collides with the top or bottom part of the screen. 
It's just that all we have to do when we collide with a paddle is instead of flipping the **y component** we flip the **x component** of the vector.

We can check if the ball is colliding with a paddle by just calling the function `colliderect` on the ball's rectangle object and the paddle's rectangle object.

So the code for that would look like
```python
if self.rect.colliderect(left_paddle.rect) or self.rect.colliderect(right_paddle.rect):
	self.x_component *= -1
```

So we successfully have a ball that can bounce around.

All that remains is just one final step, which is to just to check if the ball collides with either the left side of the screen.

So all we need to do is just check if we collide with either the left side or the right side of the screen.

We can easily do this by just checking if the ball's x location is less than zero or if the ball's location + the ball's width is greater than the screen width. And if we collide with the sides, we increment the score for the player at the other side and call the reset function of the ball.

So for the left side it would be
```python
if self.x < 0:
	right_score += 1
	self.reset()
```

So for the right side, it would be
```python
if self.rect.x + self.rect.w > screen_width:
	left_score += 1
	self.reset()
```

And now we should be done with collisions right? Right?

Nah

With our current implementation of the collisions we would have errors, lemme explain

Let's take a look at where we are checking collision between two paddles and where we are checking if we collide with the left or right edge of the screen.
```python
if self.rect.colliderect(left_paddle.rect) or self.rect.colliderect(right_paddle.rect):
	self.x_component *= -1

if self.rect.x < 0:
	right_score += 1
	self.reset()

if self.rect.x + self.rect.w > screen_width:
	left_score += 1
	self.reset()
```

The major problem with this implementation is that the ball which collides with a paddle could collide with the paddle and also touch the edge of the screen which would then cause the following events to occur.
- The ball's x component is reversed
- The check for the score occurs which will return true and treat the collision as a goal scored which will then reset the game.

This is a problem because the second event is not meant to occur at all.

We can fix this by checking if the ball is colliding with a paddle and if so it shouldn't increment the score or reset the game.

The code for the changing the score will then look like this.
```python
if self.rect.x <= 0 and not self.rect.colliderect(left_paddle.rect):
	right_score += 1
	self.reset(left_paddle, right_paddle)


if self.rect.x + self.rect.w >= screen_width and not self.rect.colliderect(right_paddle.rect):
	left_score += 1
	self.reset(left_paddle, right_paddle)
```

That should fix the problem.

So we are done again right? Right?

I think you know the answer to this question

So the other major problem that we could have is kinda like a snake in the grass of errors. 
It something that has to do with the with flipping the ball's x component when we collide with a paddle.

Why don't you try to figure out the problem by looking at the code?

You done?

So after you skipped over the last two lines, lemme explain this other problem.

Let's look at the snippet in question

```python
if self.rect.colliderect(left_paddle.rect) or self.rect.colliderect(right_paddle.rect):
	self.x_component *= -1
```

We reverse the x component when we collide with a paddle.
The ball is inside the paddle and then when the x direction is reversed the ball could still be inside the paddle causing the ball x direction to then reverse again and again till it get's out of the paddle.

We can fix this by checking if the ball is inside the rectangle and then if that is true we shouldn't be able to reverse the x component until it we stop colliding with a paddle.

```python
if self.rect.colliderect(left_paddle.rect) or self.rect.colliderect(right_paddle.rect):
	if self.should_check:
		self.x_component *= -1
		self.should_check = False
else:
	self.should_check = True
```

And with this should fix the problem that we were having.

So are we done now? Please tell me that we are done?

Yeah we finally done with all the stuff

We can now go the definition of the methods of the class

## The Init method
In this method, we need to define stuff that the ball class is going to use

```python
class Ball:
    def __init__(self):
        # ? This is the size of the ball
        self.size = 20

        # ? This calculates the center position of the screen
        center_horizontal = (screen_width - self.size) / 2
        center_vertical = (screen_height - self.size) / 2

        # ? This is pygame rectangle object that gets drawn onto the screen
        self.rect = pygame.Rect(
            center_horizontal, center_vertical, self.size, self.size)

        # ? This chooses a random angle for velocity vector
        self.angle = random.choice(possible_angles)

        # ? This is the x component of the velocity vector
        self.x_component = cos(radians(self.angle))

        # ? This is the y component of the velocity vector
        self.y_component = sin(radians(self.angle))

        # ? This is the speed of the ball
        self.speed = 15

        self.should_check = True

```

We calculate the `self.x_component` and `self.y_component` after we choose a random angle from the list of possible angles we calculate the components


## The Reset Method
This method just sets the ball back to the middle of the screen and calls the reset method on both paddles. 
The new angle at which the ball shoots is rechosen
We also recalculate the x and y component of the vector that is being added to the ball.

The code for the method will look like:
```python
    def reset(self, left_paddle: Player, right_paddle: Player):

		self.angle = random.choice(possible_angles)
        self.rect.x = (screen_width - self.size) / 2
        self.rect.y = (screen_height - self.size) / 2
        self.x_component = cos(radians(self.angle))
        self.y_component = sin(radians(self.angle))
        left_paddle.reset()
        right_paddle.reset()
```

And that concludes the reset method for the ball.

## The check collision method
This is the method that majority of this method was already implemented in the "*logic*" section of the ball class.

Let's just dive right in.

The first thing that we do is just declare some global variable that we are going to change
```python
        # ? We tell the interpreter that we are about to modify some global variables
        global left_score, right_score, wait_timer
```
We have the two variables that hold the score of each player and the `wait_timer` that would be explained in a bit.

After we declare our global variables, we check if the ball has collided with the top or bottom side of the screen

```python
        if self.rect.y <= 0 or self.rect.y + self.rect.h >= screen_height:
            self.y_component *= -1
```

Then we check if the ball has collided with any of the paddles so that we can then reverse the `x_component` of the ball.
```python
        # ? Checking if the ball collides with the left paddle
        if self.rect.colliderect(left_paddle.rect) or self.rect.colliderect(right_paddle.rect):
            if self.should_check:
                self.x_component *= -1
                self.should_check = False
        else:
            self.should_check = True
```
The code above has already been explained.

The final thing that we do in this method is to then check if we collide the left or right edge of the screen.

```python
        # ? Checking if the left side of the ball has touched the left side of the screen
        if self.rect.x <= 0 and not self.rect.colliderect(left_paddle.rect):

            # ? This increments the score for player 1
            right_score += 1

            # ? This resets the entire game
            self.reset(left_paddle, right_paddle)

            # ? waits for a 2 seconds before it resumes gameplay
            wait_timer = 120

        # ? Checking if the right side of the ball has touched the right side of the screen
        if self.rect.x + self.rect.w >= screen_width and not self.rect.colliderect(right_paddle.rect):
            # ? This increments the score for player 2
            left_score += 1
            self.reset(left_paddle, right_paddle)
            # ? waits for a 2 seconds before it resumes gameplay
            wait_timer = 120
```

We add the score when the opposite side scores. The value of the `wait_timer` is set in this section of the collision method for the ball. 

>Take note of the fact that we are setting the value of the wait_timer. We would explain later why did so

## The Update function
This function just adds the x and y component of the direction vector the ball's x and y location every frame.

The function is implemented like
```python
    def update(self):
        # ? Adding the x and y components to the x and y position of the ball's rectangle object
        # ? We multiply the vector by a mangitude in order to make the ball move faster
        self.rect.x += self.x_component * self.speed
        self.rect.y += self.y_component * self.speed
```

## The Show Function
This method draws the ball's rectangle to the screen

The function is implemented like this
```python
    def show(self):
        # ? This just draws the ball's rectangle to the screen
        pygame.draw.rect(display, foreground_color, self.rect)
```

## Finalizing the Class
We are done with the `Ball` class. The final code is there below
```python
# ? This is the ball object
class Ball:
    def __init__(self):
        # ? This is the size of the ball
        self.size = 20

        # ? This calculates the center position of the screen
        center_horizontal = (screen_width - self.size) / 2
        center_vertical = (screen_height - self.size) / 2

        # ? This is pygame rectangle object that gets drawn onto the screen
        self.rect = pygame.Rect(
            center_horizontal, center_vertical, self.size, self.size)

        # ? This chooses a random angle for velocity vector
        self.angle = random.choice(possible_angles)

        # ? This is the x component of the velocity vector
        self.x_component = cos(radians(self.angle))

        # ? This is the y component of the velocity vector
        self.y_component = sin(radians(self.angle))

        # ? This is the speed of the ball
        self.speed = 15

        # ? This is a variable that fixes a bug
        # ? Check the check_collisions method for more
        self.should_check = True

    def reset(self, left_paddle: Player, right_paddle: Player):
        # ? This changes the angle so that the ball shoots on the side that just recently scored
		self.angle = random.choice(possible_angles)

        # ? This just resets the ball and the paddles position and recalculates the x and y components of the velocity vector
        self.rect.x = (screen_width - self.size) / 2
        self.rect.y = (screen_height - self.size) / 2
        self.x_component = cos(radians(self.angle))
        self.y_component = sin(radians(self.angle))
        left_paddle.reset()
        right_paddle.reset()

    # ? This is kinda like a mini physics enginej
    def check_collisions(self, left_paddle: Player, right_paddle: Player):

        # ? We tell the interpreter that we are about to modify some global variables
        global left_score, right_score, wait_timer

        # ? Checking if the ball collides with the top part or the bottom part of the screen
        if self.rect.y <= 0 or self.rect.y + self.rect.h >= screen_height:
            # ? We just flip the value of the y component
            # ? So for example, if it was moving down it will then start moving up and vice versa
            self.y_component *= -1

        # ? Checking if the ball collides with the left paddle
        if self.rect.colliderect(left_paddle.rect) or self.rect.colliderect(right_paddle.rect):
            # ? So what does this should check variable do
            # ? This is just variable that just specifies if the ball collided with a paddle
            # ? We do this because we want to check the collisions once
            # ? Since when the ball collides with a paddle the x component is reversed
            # ? We only want to reverse the component once
            # ? So why? I bet that's what you are thinking
            # ? When we add the x component to ball's x position we could be inside a paddle
            # ? If we then just flip the vector when it has collided we could flip it multiple times
            # ? Since the should check variable is true by default the flip should only do once and then it should then set it to not check
            # ? Then we are not colliding a paddle anymore we then set the variable to true so when it collides with another paddle it flips the component once
            # ? And then rinse and repeat
            if self.should_check:
                self.x_component *= -1
                self.should_check = False
        else:
            self.should_check = True

        # ? Checking if the left side of the ball has touched the left side of the screen
        if self.rect.x <= 0 and not self.rect.colliderect(left_paddle.rect):

            # ? This increments the score for player 1
            right_score += 1

            # ? This resets the entire game
            self.reset(left_paddle, right_paddle)

            # ? waits for a 2 seconds before it resumes gameplay
            wait_timer = 120

        # ? Checking if the right side of the ball has touched the right side of the screen
        if self.rect.x + self.rect.w >= screen_width and not self.rect.colliderect(right_paddle.rect):
            # ? This increments the score for player 2
            left_score += 1
            self.reset(left_paddle, right_paddle)
            # ? waits for a 2 seconds before it resumes gameplay
            wait_timer = 120

    # ? This method runs every frame
    def update(self):
        # ? Adding the x and y components to the x and y position of the ball's rectangle object
        # ? We multiply the vector by a mangitude in order to make the ball move faster
        self.rect.x += self.x_component * self.speed
        self.rect.y += self.y_component * self.speed

    def show(self):
        # ? This just draws the ball's rectangle to the screen
        pygame.draw.rect(display, foreground_color, self.rect)
```

Now that we are done with the ball class we can then move on to the `show_score` function.