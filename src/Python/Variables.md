# Variables

Variables are how the computer works with various "objects". It effectively places data in containers that allows tasks to be done with the information *inside* the container.

## Mathematical Operators
The first thing we're learning about is numbers. Every programming language has some kind of way of doing numbers and math. The following are some valid mathematical operations in Python:

| Operator 	|          Name          	|                                           Description                                          	|
|-----------|-----------------------------|-----------------------------------------------------------------------------------------------------|
|     +    	|          plus          	|                             Allows you to add two numbers together                             	|
|     -    	|          minus         	|                       Allows you to subtract two numbers from each other                       	|
|     /    	|          slash         	|                            Allows you to divide a number by another                            	|
|     *    	|        asterisk        	|                           Allows you to multiply two numbers together                          	|
|     %    	|         percent        	|                         Allows you to find the remainder of a division                         	|
|     <    	|        less-than       	| Returns True if the number at the left side of the operation is less than the one at the right 	|
|     >    	|      greater-than      	| Returns True if the number at the right side of the operation is less than the one at the left 	|
|    <=    	|   less-than-or-equals  	|                          Like less-than, but also checks for equality                          	|
|    >=    	| greater-than-or-equals 	|                         Like greater-than, but also checks for equality                        	|

## Displaying text on the screen
To print values to the screen use the `print()` function. 
For instance, if you type `print("hey")`, "hey" will be shown when you run the program.

```Bash
$ python printing.py

hey
```

Now try `print(1 + 1)`, which would output "2". 

Why doesn't it show "1 + 1"? Python is smart enough to realise that since there aren't any quotes around the numbers, we're working with *real* numbers, not a text string. It then performs the math operation on it, and prints the result.

Try other numbers with all the operators above to figure out how they work and get a feel for them.

## Storing data with variables
In programming, a variable is nothing more than a name for a piece of data so you can use the name rather than the data itself at various places in your program.

Now try this:

```Python
age = 10
pizza = 5

print("I am", age)
print ("There are only " + pizza + " pizzas available.")
```

> Note: if you want to write a comment i.e. text that will be ignored by Python, start it with the "#" character

```Python
print("heyy")

# print("oh no this won't work")
```

Now we’ll do even more typing of variables and printing them out but this time we’ll use something called a “format string.” 

Every time you put double-quotes (`" "`) around a piece of text, you have been making a string. 
A string is how you make something that your program might give to a human. You print them, save them to files, send them to web servers, all sorts of things. Strings are really handy:

```Python
name = "John"

print(f"My name is {name}")
print(f"I'm saying again that my name is {name}")

# My name is John"
# I'm saying again that my name is John
```

What is happening here is that the name in curly braces (`{ }`) is our variable. Then when Python sees the `name` inside, it looks for whatever variable is called `name` and then puts the value inside instead (in this case "John").

Now try it yourself, with other variables and try printing different things out.

## Prompting the user for input

To collect input from the user you can use the `input()` function and store the result in a variable:

```Python
age = input("How old are you: ")

print(f"you are {age} years old")
```
