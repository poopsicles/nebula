# Enemies
### Creating The Enemy
First download the image sprite of the enemy, use a 64x64 sprite. Load the image to the code by using `pygame.image.load()` 

```python
enemyimg = pygame.image.load('enemy.png')
```

Write this code in the same place you wrote the other image codes
 Then to draw the enemy image to the screen, we use the `.blit()` method, write it aftert the other blit method
 
```python
screen.blit(enemyimg, (400, 5))
```

Now we can move and have an enemy to shoot.

When the enemy is spawning or being created, we want it to be at random positions.

```python
import random
```

This is a library that would help to spawn the enemies in a random position on the screen

```python
enemyX=random.randint(0, 736)
enemyY=random.randint(30, 150)
```

This code creates the variables for the enemys position, both x and y, and equates it to the function `random.randint()` which is used to get random numbers in between a range of numbers.
Change the `.blit()` method for the enemy. Use `enemyX` and `enemyY` as it's new position

### Moving The Enemy
The enemies move along the x-axis, similar to the player, but because they are advancing to the player, the enemy would have to move down, so it moves on the y-axis as well.
A collision code needs to be created, it would be similar to the players but with some changes.

```python
enemyspeedX=1
enemyspeedY=40
...

#Underneath the player collision code
enemyX+=enemyspeedX
if enemyX<=0:
	enemyspeedX=1
	enemY+=40
if enemyY>=736:
	enemyspeedX=-1
	enemyY+=enemyspeed
```

The enemys position changes by -1 when it collides with the left part which makes it move in the opposite direction. Then it also moves down by 40 pixels so it gradually makes it's way down to the player.

You can run the code now to see how it looks.

#### CODE RECAP 4
```python
import pygame
import random

pygame.init()

screen = pygame.display.set_mode((800, 600))
background = pygame.image.load('bg.png')
enemyimg = pygame.image.load('enemy.png')
playerimg = pygame.image.load('arcade.png')

playerX = 370
playerY = 480

enemyX=random.randint(0, 736)
enemyY=random.randint(30, 150)
enemyspeedX=1
enemyspeedY=40

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
	enemyX+=enemyspeedX
	if enemyX<=0:
		enemyspeedX=1
		enemY+=40
	if enemyY>=736:
		enemyspeedX=-1
		enemyY+=enemyspeed

	screen.blit(playerimg, (playerX, playerY))
	screen.blit(enemyimg, (enemyX, enemyY))
	pygame.display.update()
```