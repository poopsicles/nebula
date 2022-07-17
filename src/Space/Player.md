# **Player**

### **Creating The Player**

To create the player, we create the players sprite first (Images are also known as sprites). To do that, we use the image.load() function we used to add the background image to the code. You can download the image sprite from the link or use your own sprite/image. Follow the steps to make the background image, except in your code, the name of the file should be the name of your player filename equate the whole function to a variable called ‘`playerimg`’

```python
playerimg = pygame.load(‘player.png’)
```

Then to draw the player image/sprite on the screen, use the screen.blit() method. The position this time is (370, 480). I have already tested this position, but feel free to change it to what is best for you

```python
screen.blit(playerimg, (370, 480)
```

This code should be after the line of code for drawing the background. This is because the background has to be drawn before the player is drawn so the player would be onto of the background and not be hidden.

We want to save the player position to a variable, this allows the movement on the screen to be easier. The player position can easily be changed at any point in time within the code.

The co-ordinates on the blit() method are 370 on the x-axis and 480 on the y-axis, so let's use it.

```python
playerX = 370
playerY = 480
...

screen.blit(playerimg, (playerX, playerY))
```

#### CODE RECAP 1
Your overall code should look like this:
```python
import pygame

pygame.init()

screen = pygame.display.set_mode((800, 600))
background = pygame.image.load('bg.png')
playerimg = pygame.image.load('arcade.png')

playerX = 370
playerY = 480

running = True
while running:
	screen.blit(background, (0, 0))
	for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    screen.blit(playerimg, (playerX, playerY))
    pygame.display.update()
```


### Moving The Player
To  move the player, we're going to create a variable to account as the speed of the player, or how fast it's moving across the screen.
The player only moves on the x-axis, so only the x-axis should be changed. The variable name for the change would be called `changeX` and should be = 0. This is not the actual value of the player speed, we're just initializing the variable.

```python
...
playerX = 370
playerY = 480
changeX = 0
```

The actual use of the variable `changeX` would be in the while loop.
In order for the player to move, the user would have to press the keyboard to make the player go left or right.
To do this, a for loop for the left arrow key and right arrow keys would be created.

```python
for event in pygame.event.get():
		if event.type == pygame.QUIT:
			running = False
		if event.type == pygame.KEYDOWN:
			if event.key == pygame.K_LEFT:
				changeX = -5
			if event.key == pygame.K_RIGHT:
				changex = 5
playerX += changeX
```

We already know the first 3 lines. Let's break down the next lines of code.

```python
if event.type == pygame.KEYDOWN
```

This code is used to get the event in which a user is pressing the keyboard down. The code says 'If a key is pressed down, then...'
The next if_statements are to check which keys are being pressed down.

```python
if event.key == pygame.K_LEFT:
	changeX= -5
```

This part of the code checks to see if the left arrow key is being pressed. If it is, `changeX` would be equal to -5, as the player is moving negatively on the x-axis with a speed of 5.

```python
if event.key == pygame.K_RIGHT:
	changeX = 5
```

This is similar to the left arrow key code, except when the player presses the right arrow key, the player moves in the positive x-axis with a speed of 5.

```python
playerX+=changeX
```

This code updates the position of the player.

At this point, if you run the code and press either the left or right arrow key, the player doesn't stop moving even when you stop pressing the keys. To stop moving the player when the keys are not being pressed down, we use the following code:

```python
for event in pygame.event.get():
		if event.type == pygame.QUIT:
			running = False
		if event.type == pygame.KEYDOWN:
			if event.key == pygame.K_LEFT:
				changeX = -5
			if event.key == pygame.K_RIGHT:
				changex = 5
		#updated part of code
		if event.type == pygame.KEYUP:
			changex = 0
playerX += changeX
```

The code checks to see if the key that has been pressed is now no longer being pressed or the key has been released, then the speed or `changeX` is equal to 0.

#### CODE RECAP 2
```python
import pygame

pygame.init()

screen = pygame.display.set_mode((800, 600))
background = pygame.image.load('bg.png')
playerimg = pygame.image.load('arcade.png')

playerX = 370
playerY = 480

running = True
while running:
	screen.blit(background, (0, 0))
	for event in pygame.event.get():
		if event.type == pygame.QUIT:
			running = False
		if event.type == pygame.KEYDOWN:
			if event.key == pygame.K_LEFT:
				changeX = -5
			if event.key == pygame.K_RIGHT:
				changex = 5
		if event.type == pygame.KEYUP:
			changex = 0
	playerX += changeX
	screen.blit(playerimg, (playerX, playerY))
	pygame.display.update()
```

### Player Collision With Screen
When the player is being moved, we don't want it going through the borders of the screen. We add a collision detection code. First for detecting when the player collides with the left side of the screen.
Write this code after the  `playerX+=changeX` line

```python
if playerX<=0:
	playerX=0
```

Now if the player is moved to the edge of the left side of the screen or it's x position is 0, the player can't move beyond this point because if it tries to, it's position will remain 0.
Add this to your code and try to move to the left side of the screen.

For the right side, depending on the width of your game screen, we'll use that first to create a collision for the right side. The code is similar to the previous one, but change '`0`' to '`800`' amd it's not less than, it's greater than as we don't want to move **beyond** the screen width.

```python
if playerX>=800:
	playerX=800
```

If we run this code, you would observe that the player can still move beyond the borders of the screen, this is because the utmost right side, or the beginning part, of the player sprite is where the screen starts to draw the player from. 
It may not make sense now, but let me show the code change:

```python
elif playerX>=736:
	playerX=736
```

The new limit is 736. This is because the size of the sprite from right to left is 64 pixels. The collisions are detected by the right side of the player sprite, which is 64 pixels away from the left side. If the left side of the player sprite and the screen border collide, the right side of the player would be 64 pixels away from the screen border, because the left side is at the same position as the screen border. 
Therefore, the position of the playerX, which is the right part of the player sprite, would have to be 736 (ScreenWidth minus PlayerSize) if the left side is to be aligned/collide with the screen border, which is why the limit is 736 (800-64).

Feel free to search the web if this explanation wasn't clear enough.

Now run the code and see if it works.

>N.B: The limit of the left side of the screen depends on the size of your screen and the size of your sprite/image

#### CODE RECAP 3
```python
import pygame

pygame.init()

screen = pygame.display.set_mode((800, 600))
background = pygame.image.load('bg.png')
playerimg = pygame.image.load('arcade.png')

playerX = 370
playerY = 480

running = True
while running:
	screen.blit(background, (0, 0))
	for event in pygame.event.get():
		if event.type == pygame.QUIT:
			running = False
		if event.type == pygame.KEYDOWN:
			if event.key == pygame.K_LEFT:
				changeX = -5
			if event.key == pygame.K_RIGHT:
				changex = 5
		if event.type == pygame.KEYUP:
			changex = 0
	playerX += changeX
	if playerX<=0:
		playerX=0
	elif playerX>=736:
		playerX=736

	screen.blit(playerimg, (playerX, playerY))
	pygame.display.update()
```