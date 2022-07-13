# Loops and Lists

You should now be able to do some programs that are much more interesting. If you have been keeping up, you should realize that now you can combine all the other things you have learned with if-statements and Boolean expressions to make your programs do smart things.

However, programs also need to do repetitive things very quickly. We are going to use a for loop in this exercise to build and print various lists. 

A list is simply a container for multiple variables, and here we're going to be adding multiple items to some of them with the help of loops.

Before you can use a for loop, you need a way to store the results of loops somewhere. The best way to do this is with a list.

First, here’s how you make one: 

```python
hairs = ['brown', 'blond', 'red']

eyes = ['brown', 'blue', 'green']

weights = [1, 2, 3, 4]
```

What you do is start the list with the `[` (left bracket), which “opens” the list. Then you put each item you want in the list separated by commas, just like when you did function arguments. Lastly you end the list with a `]` (right bracket) to indicate that it’s over. Python then takes this list and all its contents and assigns them to the variable. 
 
Try this:

```python
the_count = [1, 2, 3, 4, 5]

for number in the_count:
	print(f"This is count {number})
```


## While loops

Now to totally blow your mind with a new loop - the while loop. A while loop will keep executing the code block under it as long as a Boolean expression is True.

What they do is simply do a test like an if-statement, but instead of running the code block once, they jump back to the “top” and repeat. It keeps doing this until the expression is False.

Here’s the problem with while loops: Sometimes they do not stop. This is great if your intention is to just keep looping until the end of time, but this is rarely the case. Otherwise you almost always want your loops to end eventually.

To avoid these problems, there’s some rules to follow:
1. Make sure that you use while loops sparingly. Usually a for loop is better.
2. Review your while statements and make sure that the thing you are testing will become False at some point.
3. When in doubt, print out your test variable at the top and bottom of the while loop to see what it’s doing.

Try this out:

```Python
i = 0

numbers = []

while i < 6:
	print(f"At the top i is {i}")
	numbers.append(i)

	i = i + 1
	
	print("Numbers now: ", numbers)
	print(f"At the bottom i is {i}")
```
