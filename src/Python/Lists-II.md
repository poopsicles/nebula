# Performing operations on lists

You've learnt about lists. When you learned about while loops, you “appended” numbers to the end of a list and printed them out. 

When you did this, you had a list and you “called” the `append()` function on it. However, you may not really understand what’s going on, so let’s see what we can do to lists.

When you type Python code that reads `mystuff.append('hello')`, you are actually setting off a chain of events inside Python to cause something to happen to the `mystuff` list. 

Here’s how it works:
1. Python sees you mentioned `mystuff` and looks up that variable. It might have to look backward to see if you created it with `=`, and look and see if it is a function argument or a global variable. Either way, it has to find the variable first.
2. Once it finds `mystuff` it then hits the `.` (period) operator and starts to look at variables that are a part of `mystuff`. Since it is a list, it knows that it has a bunch of functions.
3. It then hits `append` and compares the name “append” to all the ones that `mystuff` says it owns. If "append" is in there (it is), then it grabs that to use.
4. Next Python sees the `(` (parenthesis) and realizes, “Oh hey, this should be a function.” At this point it executes the function almost normally, but instead it calls the function with an extra argument.
5. That extra argument is . . . `mystuff`! Weird, right? But that’s how Python works so it’s best to just remember it and assume that’s alright. What happens then, at the end of all this, is a function call that looks like `append(mystuff, 'hello')` instead of what you read, which is `mystuff.append('hello')`.

But all of this is done in the background, so you don't have to worry too much about it.

Try this:

```Python
ten_things = "Apples Oranges Crows Telephone Light Sugar"

print ("Wait there's not 10 things in that list, let's fix that.")

stuff = ten_things.split(' ')

more_stuff = ["Day", "Night", "Song", "Frisbee", "Corn", "Banana", "Girl", "Boy"]

while len(stuff) != 10:
	next_one = more_stuff.pop()
	print ("Adding: ", next_one)
	
	stuff.append(next_one)
	print (f"There's {len(stuff)} items now.")

print("There we go: ", stuff)
print("Let's do some things with stuff.")

print(stuff[1])
print(stuff[- 1]) # whoa! Fancy
print(stuff.pop())
print (' '.join(stuff)) # what? cool!
print ('#'.join(stuff[3:5])) # super stellar!
```

Check out the Python documentation in order to find out what all these special list functions do.