# Modules, Classes & Objects

Python is something called an “object-oriented programming language.”
This means that there’s a construct in Python called a "class" that lets you structure your software in a particular way. Using classes, you can add consistency to your programs so that they can be used in a cleaner way.

Let's try to learn the beginnings of object-oriented programming, classes, and objects using what you already know about dictionaries and modules.

## Modules are like dictionaries
A dictionary is created and used to map one thing to another. That means if you have a dictionary with a key "apple" and you want to get it, then you do this:

```Python
mystuff = {'apple': "I AM APPLES!"} 

print(mystuff['apple'])
```

A module is simply used to group related functions together in a separate Python file.
Imagine if I have a module that I decide to name `mystuff.py`, and I put a function in it called `apple()`. 

```Python
# this goes in mystuff.py    
def apple():        
     print ("I AM APPLES!")
```

Once I have that, I can use that module with the `import` statement and then access the `apple()` function elsewhere:

```Python
# this is in main.py 
import mystuff # allowing us to access everything in mystuff

mystuff.apple()
```

I could also put a variable in it named `tangerine`, like this:    

```Python
# mystuff.py
def apple():
    print "I AM APPLES!"    

tangerine = "Living reflection of a dream"
```

Then again I can access that the same way:    

```Python
# main.py
import mystuff 

mystuff.apple()    
print(mystuff.tangerine)
```

You should start to see how this is similar to using a dictionary, but the syntax is slightly different. Let’s compare:   

```Python
mystuff['apple'] # get apple from dict   
mystuff.apple() # get apple from the module  
mystuff.tangerine # same thing, it's just a variable
```

This means we have a very common pattern in Python: 
1. Take a key-value style container. 
2. Get something out of it by the key’s name - be it a string, number, or even a function in another module.

## Classes Are Like Modules
A way to think about a module is that it is a specialized dictionary that can store Python code so you can get to it with the "`.`" (dot) operator. Python also has another construct that serves a similar purpose called a class. 

A class is a way to take a grouping of functions and data and place them inside a container so you can access them with the "`.`" operator.

If I were to create a class just like the `mystuff` module, I’d do something like this:    

```Python
class MyStuff(object):
	def __init__(self):
		 self.tangerine = "And now a thousand years between"
	def apple(self):
		 print("I AM CLASSY APPLES!")
```

This looks complicated compared to modules, and there is definitely a lot going on by comparison, but you should be able to make out how this is like a “mini-module” with `MyStuff` having an `apple()` function in it. What is probably confusing with this is the `__init__()` function (a function which is called when the class is "instantiated") and use of `self.tangerine` for setting the `tangerine` variable associated with the class.

Now, you need to know what an “object” is and how to work with `MyStuff` just like you do with the `mystuff.py` module.

## Objects are like mini-imports
If a class is like a “mini-module”, then there has to be a similar concept to `import` but for classes. 
That concept is called “instantiatiation,” which is just a fancy, obnoxious, overtly-smart way to say “create.” When you instantiate a class, what you get is called an object.

The way you do this is you call the class like it’s a function, like this:

```Python
thing = MyStuff()   

thing.apple()    
print(thing.tangerine)
```

The first line is the “instantiate” operation, where it "creates" the object from the class and it’s a lot like calling a function. However, when you call this, there’s a sequence of events that Python coordinates for you. I’ll go through them using the above code for `MyStuff`:

1.	Python looks for MyStuff() and sees that it is a class you’ve defined.
2.	Python crafts an empty object with all the functions you’ve specified in the class using `def`.
3.	Python then looks to see if you made a “magic” `__init__()` function, and if you have, it calls that function to initialize your newly-created empty object.
4.	In the MyStuff function `__init__()` I then get this extra variable `self`, which is that empty object Python made for me, and I can set variables on it just like you would with a module, dictionary, or other object.
5.	In this case, I set `self.tangerine` to a song lyric string and then this object has been initialized.
6.	Now Python can take this newly-minted object and assign it to the `thing` variable for me to work with.

Those are the basics of how Python does this “mini-import” when you call a class like a function to make an object. Remember that this is not giving you the class, but instead it is using the class as a blueprint for how to build a copy of that type of thing - an object.

Although these analogies are slightly-inaccurate, you can start to build up an understanding of classes based on what you know already. In reality, classes and objects diverge from modules at this point. It's something more like this:

> Classes are like blueprints or definitions for creating new mini-modules.<br>
> Instantiation is how you create one of these mini-modules and import it at the same time.<br>
> The resulting created mini-module is called an object and you then assign it to a variable to work with it.

Here's an example:

```Python
class Song(object):
	def __init__(self, lyrics):
		self.lyrics = lyrics
		
	def sing_me_a_song(self):
		for line in self.lyrics:
			print line
			
happy_bday = Song(["Happy birthday to you",
				  "I don't want to get sued",
				  "So I'll stop right there"])

bulls_on_parade = Song(["They rally around the family",
					   "With pockets full of shells"])
					   
happy_bday.sing_me_a_song()
bulls_on_parade.sing_me_a_song()
```

Try finding out more about OOP, and practising on your own.
