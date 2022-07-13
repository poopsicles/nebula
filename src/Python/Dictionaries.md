# Dictionaries

Dictionaries are the most useful container ever - Python calls them “dicts”, other languages call them “hashes”, but it doesn’t matter. What does matter is what they do when compared to lists.

You can use numbers to “index” into a list, meaning you can use numbers to find out what’s in lists, at a certain position. You should know this about lists by now, but make sure you understand that you can only use numbers to get items out of a list.

What a dictionary does is let you use anything, not just numbers, to access elements - it associates one thing to another, no matter what it is. 

Take a look:

```Python
stuff = {'name': 'Zed', 'age': 36, 'height': 6 * 12 + 2}

print(stuff['name'])   
# Zed

print(stuff['age'])  
# 36

print(stuff['height'])  
# 74

stuff['city'] = "San Francisco"  
print(stuff['city'])    
# San Francisco   
```

You'll see that instead of just numbers, we’re using strings to say what we want from the `stuff` dictionary. We can also put new things into the dictionary with strings. It doesn’t have to be just strings though. We can also do this:

```Python
stuff[1] = "Wow"  
stuff[2] = "Neato"    

print(stuff[1])   
# Wow   

print(stuff[2])   
# Neato   

print(stuff)   
# {'city': 'San Francisco', 2: 'Neato', 'name': 'Zed', 1: 'Wow', 'age': 36, 'height': 74}
```

In this code I used numbers, and then you can see there are numbers and strings as keys in the dictionary when I print it. I could use anything, well almost, but just pretend you can use anything for now.

Of course, a dictionary that you can only put things in is pretty stupid, so here’s how you delete things, with the `del` keyword:   

```Python
del(stuff['city'])  
del(stuff[1]) 
del(stuff[2]) 

print(stuff)
# stuff {'name': 'Zed', 'age': 36, 'height': 74}  
```