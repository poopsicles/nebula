# Finally the Event Loop
This is the **final** part of the game. All the implementations, class definitions and variable declarations have lead to this point.
This is the final stretch so just stay with me for just a few more lines of code and we are done

So there are things that we would need before we define the game's event loop
We would need to define some variables

We need to define the `wait_time` which is used to stall the event loop for a certain amount of frames.
```python
wait_time = 0
```

We need to define the `running`, `left_paddle`, `right_paddle` and `ball` variables
```python
running = True
left_paddle = Player(-10)
right_paddle = Player(screen_width-10)
ball = Ball()
```
We define the `left_paddle` to have an initial x location of `-10` and we define the `right_paddle` to have an initial x location of `screen_width - 10`

Since we have all of our variables defined we can then talk about the event loop

The event loop for this game would execute the following sequentially every frame
1. Check for the quit event and any keyboard events
2. Draw the background color
3. Draw the ball, left paddle and the right paddle
4. Draw the two players score
5. Update the location of the two paddles if there are any updates
6. Check for collisions
7. Update the location of the ball

And that's all the event loop will execute multiple times per second.

So let's start by defining it one by one

For the first step, we need to get all the events and check if there is a quit event.
If there is one then we break out of the loop and quit pygame

```python
while running:
    # ? We first get all the events that pygame is giving us
    for e in pygame.event.get():
        # ? We checked if the player wants to quit the game
        if e.type == pygame.QUIT:
            # ? This breaks out the event loop and then quits the game
            running = False
            pygame.quit()
```

Then we need to check for the `KEYDOWN` event and then change the `moving` variable to `True` and change the `moving_up` property of the paddle to `True` if it moving up

Then we can check for the `KEYUP` event and then change the `moving` property of the paddle back to `False `

The left paddle is controlled by `w` and `s` 
The right paddle is controlled by `Up` and `Down`

We can implement it by doing 
```python
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
```

The next step would just be put after the for loop that gets the events

```python
    # ? We fill the display with black background color
    display.fill(background_color)
```

The next step is to show the ball and the two paddles

```python
    # ? We then show the ball and the paddles
    ball.show()
    left_paddle.show()
    right_paddle.show()
    show_score()
```

Then we move over to the next step which is to add the update functions and check collisions? RIght?

Yeah! But we need to not update when we `wait_time` is greater than 0.

We just say that we should check for movement updates and check for collisions when the `wait_timer == 0`. If it is not 0 we then minus one from the `wait_timer`

The snippet would look like this
```python
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
```

Then we update the display
```python
    # ? This updates the display to then show the ball and the paddles
    pygame.display.update()
```

And we then make the event loop run at the specified framerate
```python
    # ? This makes sure that the event loop runs at the specified frame rate
    clock.tick(FRAMERATE)
```

And we are done with **PONG**!!!!!!!!!!!!.

So let's look at the whole event loop.
```python
# ? This creates the two paddles and ball object
left_paddle = Player(-10)
right_paddle = Player(screen_width-10)
ball = Ball()
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

Now that we are done let's look at our beautiful work at once in the next module.