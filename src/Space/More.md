# Multiple Enemies
Just having one enemy is quite boring to play isn't it. Let's add more enemies.

To create enemies we're going to use lists. Depending on the number of enemy, there will be an equal number of images of the enemy, which we would store in a list.

```python
enemyimg = []
enemyX=[]
enemyY=[]
enemyspeedX = []
enemyspeedY = []

no_of_enemies = 6
```

I set the number of enemys to 6, you can set it to whatever you want but for now, we'e using 6.

Now we're going to create a for loop that will add all the properties of the enemy, e.g the enemy image, the enemyY position, the enemy X speed etc.
Then we will drag all the properties we've initialized before an place them in the for loop.

```python
for i in range(no_of_enemies):
	enemyimg.append(pygame.image.load('enemy.png'))
	enemyX.append(random.randint(0, 736))
	enemyY.append(random.randint(30, 150))
	enemyspeedX.append(-1)
	enemyspeedY.append(40)
```

The initial values of the properties of the enemies would be added/appended to their corresponding lists.

Now we also have to move all the enemies in the list, so go to where the enemies were moved and do the following.

```python
for i in range(no_of_enemies):
	if enemyY[i] == playerY-50:
		enemyY[i] = 2000
		gameover()
	enemyX[i]+=enemyspeedX[i]
	if enemyX[i]<=0:
		enemyspeedX[i]=1
		enemY[i]+=40
	if enemyY[i]>=736:
		enemyspeedX[i]=-1
		enemyY[i]+=enemyspeed[i]
```

You're going to bring the gameover part of te code into the for loop because it requires the use of multiple enemies.

For the same gameover part of the code, we do not only want to disappear one enemy, we want to make all the enemies disappear because the game has ended. We need a for loop within a for loop.

To do that, we write the following code:

```python
for i in range(no_of_enemies):
	if enemyY[i] == playerY-50:
		for j in range(no_of_enemies):
			enemyY[j] = 2000
		gameover()
		break
```

This is to enesure that all the enemies disappear even if it's only one enemy that has fulfilled the requirement for GameOver. Then we break so the game doesn't continue the next part of the first loop.

You also need to add any other part of the while loop code that relies on the enemyproperties and add it to the for loop.

```python
for i in range(no_of_enemies):
	if enemyY[i] == playerY-50:
		for j in range(no_of_enemies):
			enemyY[i] = 2000
			gameover()
	enemyX[i]+=enemyspeedX[i]
	if enemyX[i]<=0:
		enemyspeedX[i]=1
		enemY[i]+=40
	if enemyY[i]>=736:
		enemyspeedX[i]=-1
		enemyY[i]+=enemyspeed[i]
	distance = math.sqrt(math.pow((bulletX-enemY),2) + math.pow((bulletY[i]-enemyY[i]),2)
	#collision occured
	if distance<27:
		bulletY=489
		bulletShow = False
		enemyX[i]=random.randint(0, 736)
		enemyY[i]=random.randint(30, 150)
		score+=1
	screen.blit(enemyimg[i], (enemyX[i], enemyY[i]))
```

Basically, this for loop runs for each instance of the enemy. We could also have used classes, but I find classes are much harder to understand.

#### CODE RECAP 7

```python
import pygame
import random
import math

pygame.init()

screen = pygame.display.set_mode((800, 600))
background = pygame.image.load('bg.png')
playerimg = pygame.image.load('arcade.png')
enemyimg = pygame.image.load('enemy.png')
bulletimg = pygame.image.load("bullet.png")

playerX = 370
playerY = 480

bulletX=386
bulletY=489
bulletShow = False

enemyX=random.randint(0, 736)
enemyY=random.randint(30, 150)
enemyspeedX=-1
enemyspeedY=40

font = pygame.font.SysFont('Arial', 32, 'bold')
def scoreShow():
	img = font.render(f"Score: {score}", True, "white")
	screen.blit(img, (10,10))

font_gameover = pygame.font.SysFont('Arial', 64, 'bold')
def gameover():
    img_gameover = font_gameover.render('GAME OVER', True, 'white')
    screen.blit(img_gameover, (200, 250))

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
			if event.key == pygame.K_SPACE:
				if bulletShow is False:
					bulletShow = True
					bulletX = playerX+16
		if event.type == pygame.KEYUP:
			changex = 0
	playerX += changeX
	if playerX<=0:
		playerX=0
	elif playerX>=736:
		playerX=736

	for i in range(no_of_enemies):
		if enemyY[i] == playerY-50:
			for j in range(no_of_enemies):
				enemyY[i] = 2000
				gameover()
		enemyX[i]+=enemyspeedX[i]
		if enemyX[i]<=0:
			enemyspeedX[i]=1
			enemY[i]+=40
		if enemyY[i]>=736:
			enemyspeedX[i]=-1
			enemyY[i]+=enemyspeed[i]
		distance = math.sqrt(math.pow((bulletX-enemY),2) + math.pow((bulletY[i]-enemyY[i]),2)
		if distance<27:
			bulletY=489
			bulletShow = False
			enemyX[i]=random.randint(0, 736)
			enemyY[i]=random.randint(30, 150)
			score+=1
		screen.blit(enemyimg[i], (enemyX[i], enemyY[i]))

	screen.blit(playerimg, (playerX, playerY))
	
	if bulletY<=0:
		bulletY=490
		bulletShow=False
	if bulletShow is True:
		screen.blit(bulletimg, (bulletX, bulletY))
		bulletY-=5

	scoreShow()
	pygame.display.update()
```