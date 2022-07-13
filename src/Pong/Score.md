# Drawing the player scores
This is the easiest thing to implement in the whole game.

We just need to implement the following stuffs in order to draw the scores
1. Initialize the font object
2. Create a render surface that has the text of the score
3. Render it to the screen.

We want to create a function with the name `show_score`

The first thing that we want to do is initialize the font object with a font size.

We would just initialize the font with the default system font with a font size that is 10% of the screen height.

```python
# ? We define a font object for showing the score and define the font size
font_size = screen_height * 0.1
font = pygame.font.SysFont(None, int(font_size))
```

>Note if you are having issues with intializing the font, make sure that pygame is initialized by running `pygame.init()`

After we initialize the font object we then define the left score render surface and draw it to the screen
```python
    # ? We convert the left score to a string and display with a nice white color
    left_text = font.render(str(left_score), True, foreground_color)
    # ? This set the location of the text to be slightly to the left of the center of the screen
    left_text_location = ((screen_width / 2) - (font_size * 2), 20)
    # ? We draw the text to the display at the location calculated
    display.blit(left_text, left_text_location)
```

We do the same for the right score
```python
    right_text = font.render(str(right_score), True, foreground_color)
    # ? This set the location of the text to be slightly to the right of the center of the screen
    right_text_location = ((screen_width / 2) + (font_size * 2), 20)
    # ? We draw the text to the display at the location calculated
    display.blit(right_text, right_text_location)
```

And that concludes the `show_score` function

The complete snippet would look like:
```python
# ? We define a font object for showing the score and define the font size
font_size = screen_height * 0.1
font = pygame.font.SysFont(None, int(font_size))

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

```

Since we are done we can move on to the final part which is the **Event Loop**.