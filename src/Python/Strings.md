# Strings

## More on Strings

A string is usually a bit of text you want to display to someone or “export” out of the program you are writing. 

Python knows you want something to be a string when you put either `"` (double quotes) or `'` (single quotes) around the text. You saw this many times with your use of `print()` when you put the text you want to go to the string inside `"` or `'` after the `print`. Then Python prints it. 

Try the following exercise:

```Python
print ("Mary had a little lamb.")
print (f"Its fleece was white as {snow}"
print ("And everywhere that Mary went.")

print ("." * 10)  # what'd that do?

end1 = "C"
end2 = "h"
end3 = "e"
end4 = "e"
end5 = "s"
end6 = "e"
	   
end7 = "B"
end8 = "u"
end9 = "r"
end10 = "g"
end11 = "e"
end12 = "r"

print (end1 + end2 + end3 + end4 + end5 + end6)
print (end7 + end8 + end9 + end10 + end11 + end12)

days = "Mon Tue Wed Thu Fri Sat Sun"
months = "Jan\nFeb\nMar\nApr\nMay\nJun\nJul\nAug"

print("Here are the days: ", days)
print("Here are the months: ", months)
	   
print("""
There's something going on here.
With the three double- quotes.
We'll be able to type as much as we like.
Even 4 lines if we want, or 5, or 6.
""")
```

The `\n` in the code above is an "escape" sequence. It allows to make new lines. 
<br>

## Escape Sequences

This is the list of all the escape sequences Python supports. Also try them out in some strings to see if you can make them work.

|   Escape Sequence   	  |                        What it does                         	|
|-------------------------|---------------------------------------------------------------|
|     \\\     	        |                        Backslash (\\)                        	|
|     \\'     	        |                      Single- quote (')                      	|
|     \\"     	        |                      Double- quote (")                      	|
|     \\a     	        |                       ASCII bell (BEL)                      	|
|     \\b     	        |                     ASCII backspace (BS)                    	|
|     \\f     	        |                     ASCII formfeed (FF)                     	|
|     \\n     	        |                     ASCII linefeed (LF)                     	|
|  \\N{name}  	        | Character named name in the Unicode database (Unicode only) 	|
|     \\r     	        |                  ASCII carriage return (CR)                 	|
|     \\t     	        |                  ASCII horizontal tab (TAB)                 	|
|   \\uxxxx   	        |     Character with 16- bit hex value xxxx (Unicode only)    	|
| \\Uxxxxxxxx 	        |   Character with 32- bit hex value xxxxxxxx (Unicode only)  	|
|     \\v     	        |                   ASCII vertical tab (VT)                   	|
|    \\ooo    	        |                Character with octal value oo                	|
|    \\xhh    	        |                 Character with hex value hh                 	|