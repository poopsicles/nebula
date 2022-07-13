
# Final Code
Here is **PONG** in all its full glory
```python
from math import *
import pygame
import sys
import random


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

    # ? This just resets the paddle's location
    def reset(self):
        self.rect.y = (screen_height - self.h)/2
        self.moving = False

    # ? This shows the draws the paddle's rectangle object to the screen
    def show(self):
        pygame.draw.rect(display, foreground_color, self.rect)


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


# ? These are the possible angles that the ball could be shot in
# ? The first 3 angles will shoot the ball to the right side of the screen
# ? The last 3 angles will shoot the ball to left side of the screen
possible_angles = [15, 45, 60, 120, 150, 135]

# ? This defines the screen size and set's the window to that size
screen_size = (screen_width, screen_height) = (1000, 400)
display = pygame.display.set_mode(screen_size)

# ? Definitions for some nice colors for our game :)
# ? I used hexadecimal to define the colors but you can use rgb
# ? You can just google hexadecimal color for more info
background_color = pygame.Color("#1b1b1e")
foreground_color = pygame.Color("#F2F3D9")

# ? We define the clock and then set the title of the window to "Pong"
clock = pygame.time.Clock()
pygame.display.set_caption("Pong")

#? We initalize pygame
pygame.init()

# ? Definition of some variables that will be used throughout the game
left_score = 0
right_score = 0
FRAMERATE = 60
wait_timer = 0
running = True

# ? We define a font object for showing the score and define the font size
font_size = screen_height * 0.1
font = pygame.font.SysFont(None, int(font_size))

# ? This creates the two paddles and ball object
left_paddle = Player(-10)
right_paddle = Player(screen_width-10)
ball = Ball()


# ? This function show the score to the screen
def show_score():
    # ? We define the left text object
    # ? We convert the left score to a string and display with a nice white color
    left_text = font.render(str(left_score), True, foreground_color)
    # ? This set the location of the text to be slightly to the left of the center of the screen
    left_text_location = ((screen_width / 2) - (font_size * 2), 20)
    # ? We draw the text to the display at the location calculated
    display.blit(left_text, left_text_location)
    # ? We define the right text object
    # ? We convert the right score to a string and display with a nice white color
    right_text = font.render(str(right_score), True, foreground_color)
    # ? This set the location of the text to be slightly to the right of the center of the screen
    right_text_location = ((screen_width / 2) + (font_size * 2), 20)
    # ? We draw the text to the display at the location calculated
    display.blit(right_text, right_text_location)



# ? This is basically the event loop
# ? The order by which things are put things here matter a lot
# ? For the event loop we perform the following things
# ? -> Check the quit event and for any keyboard events
# ? -> Draw the background color
# ? -> Draw the paddles and the ball
# ? -> Draw the left and right score of the players
# ? -> Update the ball's position and the paddle's position
# ? -> Then we check for collisions between the ball, the screen and any of the paddles
# ? And then we repeat forever and ever
while running:
    # ? We first get all the events that pygame is giving us
    for e in pygame.event.get():
        # ? We checked if the player wants to quit the game
        if e.type == pygame.QUIT:
            # ? This breaks out the event loop and then quits the game
            running = False
            pygame.quit()
            sys.exit()

        # ? This is where we do all of our input stuff for the game
        # ? This checks if the player has pressed a key down
        elif e.type == pygame.KEYDOWN:

            # ? We check if we pressed the w key and say that the left paddle is moving up
            if e.key == pygame.K_w:
                left_paddle.moving = True
                left_paddle.moving_up = True
            # ? We check if we pressed the up key and say that the right paddle is moving up
            elif e.key == pygame.K_UP:
                right_paddle.moving = True
                right_paddle.moving_up = True
            # ? We check if we pressed the s key and say that the left paddle is moving down
            elif e.key == pygame.K_s:
                left_paddle.moving = True
                left_paddle.moving_up = False
            # ? We check if we pressed the down key and say that the right paddle is moving down
            elif e.key == pygame.K_DOWN:
                right_paddle.moving = True
                right_paddle.moving_up = False
        # ? This checks if the player has stopped pressing the any keys
        elif e.type == pygame.KEYUP:
            # ? We stop moving the left paddle when we release the w key
            if e.key == pygame.K_w:
                left_paddle.moving = False
            # ? We stop moving the right paddle when we release the up key
            elif e.key == pygame.K_UP:
                right_paddle.moving = False
            # ? We stop moving the left paddle when we release the s key
            elif e.key == pygame.K_s:
                left_paddle.moving = False
            # ? We stop moving the right paddle when we release the down key
            elif e.key == pygame.K_DOWN:
                right_paddle.moving = False

    # ? We fill the display with black background color
    display.fill(background_color)
    # ? We then show the ball and the paddles
    ball.show()
    left_paddle.show()
    right_paddle.show()
    show_score()

    # ? Ah! The wait timer
    # ? This is a variable that we created when we defining some stuffs
    # ? This basically pauses the game for some time
    # ? We achieve this by not update the two paddles location and the ball locations and also by not checking collisions
    if wait_timer == 0:
        left_paddle.update()
        right_paddle.update()
        ball.check_collisions(left_paddle, right_paddle)
        ball.update()

    # ? If the timer is not zero it doesn't do anything at all
    else:
        # ? Then we count the decrement the timer by 1 every frame
        wait_timer -= 1

    # ? This updates the display to then show the ball and the paddles
    pygame.display.update()

    # ? This makes sure that the event loop runs at the specified frame rate
    clock.tick(FRAMERATE)
```