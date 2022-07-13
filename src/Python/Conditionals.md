# Conditional Statements

An if-statement creates what is called a “branch” in the code. It’s kind of like those choose-your-own-adventure books where you are asked to turn to one page if you make one choice and another if you go a different route. The if-statement tells your script, “If this Boolean expression is True, then run the code under it, otherwise skip it.

```Python
if condition:
	## do something
```

The code under the `if` needs to be indented because a colon at the end of a line is how you tell Python you are going to create a new “block” of code, and then indenting it tells Python what lines of code are in that block. This is exactly the same thing you did when you made functions in the first half of the book. So, Python expects you to indent something after you end a line with a `:` (colon). 

Try to figure out what the following piece of code does:

```Python
people = 20
cats = 30
dogs = 15

if people < cats:
	print("Too many cats! The world is doomed!")
	
if people > cats:
	print("Not many cats! The world is saved!")

if people < dogs:
	print("The world is drooled on!")

if people > dogs:
	print("The world is dry!")

dogs += 5

if people >= dogs:
	print ("People are greater than or equal to dogs.")

if people <= dogs:
	print("People are less than or equal to dogs." )

if people == dogs:
	print("People are dogs.")
```

What happens if you change the initial values for `people`, `cats`, and `dogs`? Because you are comparing numbers, if you change the numbers, different if-statements will evaluate to True, and the blocks of code under them will run. 

Go back and put different numbers in and see if you can figure out in your head what blocks of code will run. Try and make sure you really understand the concept of a “block” of code. 

Type this one in and make it work too.

```python
people = 30
cars = 40
buses = 15

if cars > people:
	print( "We should take the cars." )
elif cars < people:
	print("We should not take the cars.")
else:
	print("We can't decide.")
```

An else statement is exactly what it sounds like - the code executed when an if-statement fails to execute, that is, the condition is False. It's useful for specifying alternate paths for program execution. 

An elif-statement is the combination of an `else` and an `if`. It allows you to specify other conditions to be checked when the if-statement fails. This allows for very granular control flow.
