# Functions

Functions are small sets of re-useable code that help you, the programmer, to do certain tasks over and over again with less effort than it would take to manually type out everything every single time. 

They work in two main ways:
1. They name pieces of code - the way variables name strings and numbers.
2. They take arguments - the way your scripts take inputs from the user.

Using #1 and #2, they let you make your own “mini-scripts” or “tiny commands.” You can create a function by using the keyword `def` in Python. 

```Python
def function_name:
	# all the function code
```
Let's make four different functions that work like your scripts, and we'll see how each one is related.

```Python
def print_two(arg1, arg2):
	print(arg1, arg2)
	
def print_one(arg1):
	print(arg1)
	
def print_none():
	print("I got nothin'.")

print_two("Timi", "David")
print_one("First!")
print_none()
```


Let’s break down the first function, `print_two`, which is the most similar to what you already know so far:
1. First we tell Python we want to make a function using `def` for “define.”
2. On the same line as `def`, we then give the function a name. In this case, we just called it `print_two`, but it could be "peanuts" too. It doesn’t matter, except that your function should have a short name that says what it does.
3. Then we end this line with a colon (`:`) and start indenting.
4. After the colon all the lines that are indented four spaces will become attached to this name, `print_two`. Our first indented line is one that unpacks the "arguments" - the input into the function.
5. To demonstrate how it works, we print these arguments out, just like we would normally.

After that, we have an example of how you make a function that takes one argument in `print_one` and finally, a function that has no arguments in `print_none`.

## Using variables in functions

There is a tiny point though, that may not be ovbious at first, which we’ll reinforce right now. The variables in your function are not connected to the variables in your program. Here’s an exercise to get you thinking about this:  

```Python
def cheese_and_crackers(cheese_count, boxes_of_crackers):
    print(f"You have { cheese_count } cheeses!")
    print(f"You have { boxes_of_crackers } boxes of crackers!"
    print(f"Man that's enough for a party!")
    print("Get a blanket.\n")

print("We can just give the function numbers directly:")
cheese_and_crackers(20, 30)

print("OR, we can use variables from our script:") 
amount_of_cheese = 10
amount_of_crackers = 50

cheese_and_crackers(amount_of_cheese, amount_of_crackers)
```

This shows all the different ways we’re able to give our function `cheese_and_crackers` the values it needs to print them:
- We can give it straight numbers. 
- We can give it variables. 
- We can give it math. 
- We can even combine math and variables. 

In a way, the arguments to a function are kind of like our `=` character when we make a variable. In fact, if you can use `=` to name something, you can usually pass it to a function as an argument.

## Returning values from functions

You have been using the `=` character to name variables and set them to numbers or strings. Now let's see how to use `=` and the `return` keyword to set variables to be a value from a function. 

```Python
def add(a, b):
    print(f"ADDING{a} + {b}”)
    return a + b

age = add(30, 5)

print(age)
```

The important thing to notice is the last line in `add()` where we say `return a + b`. To understand what this is doing let's go through it line-by-line:
- Our function is called with two arguments: `a` and `b`.
- We print out what our function is doing, in this case, adding the arguments.
- Then we tell Python to do something kind of backward: we "return" the addition of `a` + `b`.

You might say this as, “I add a and b, then return them.” Python adds the two numbers, then when the function ends, any line that runs it will be able to assign this `a + b` result to another variable, which we do when we assign the result of `add(30, 5)` to `age`.

