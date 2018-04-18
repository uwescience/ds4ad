
# Introduction to Programming Using Python

Follow along by typing the coding examples in your notebook.
## Math in Python
Math in Python looks a lot like math you type into a calculator. Python makes a great calculator if you need to crunch some numbers and don't have a good calculator handy.

### Addition
```python
2 + 2 
1.5 + 2.25
```
### Subtraction
```python
4 - 2
100 - .5
0 - 2
```
### Multiplication
```python
2 * 3  
```

### Division
```python
1 / 2
4 / 2
```
When you run these last two you will see that the first one is a fraction (0.5), but that even though the second is a whole number (2.0), it still gets a decimal point. This is because Python wants to make sure that when you divide two numbers (even whole numbers), you can represent the fractional part of this division.

In Python, we call the first kind of number (whole numbers with no decimal point) an `integer` (or int) and the second kind (numbers with decimal points, whether they are whole of not) a `float`.

## Variables- Storing values/information
Variables allow us to give a name to something in our code. This is done using the equal sign (=). For example, if we want to say that the number 4 should be stored in the name x, we can run the following:
```python
x = 4
y = 10
```
We call this "variable assignment", because we assign a particular value to the name x.
### Output 
Notice how if you type 4 or 10 and hit enter, the Python interpreter spits a 4 back out:
```python
4
```
But not when you assign it to a variable.

You can reassign variables as you please:
```python
x = 4
x = 10
print(x)
```
### Using the `print` function to display objects.
```python
print(x) #we can print individual variables
print(x,y) # we can print a combination
print("x is a number",x,"y is a number",y ) # we can add comments as strings to make things more meaningful.
```
### Variables can be used in calculations.
Order of operations works pretty much like how you learned in school. If you're unsure of an ordering, you can add parentheses like on a calculator:
```python
x = 3 
y = 4
print(x * y)
print(x * x)
print(2 * x - 1 * y)
print((2 * x) - (1 * y))
```
the spacing doesn't matter, so this:
```python
x = 4
x=4
```
### Valid variable names
Variables can't have spaces or other special characters, and they need to start with a letter.  Here are some valid variable names:
```python
magic_numbers = 1500
amountOfFlour = .75
my_name = "Jessica"
```
What will happen if you violate the name rule?
```python
my weight lbs = 210
```
## Exercises 1.0
What values do the variables `weight_lbs` and `age` have after each statement in the following program? Test your answer by executing the commands.
```python
weight_lbs = 210
age = 35
weight_lbs = weight_lbs * 2.0
age = age - 15
print(weight_lbs)
print(age)
```
## Strings 
So far we've seen mostly two data types: `int` and `floats`. Another useful data type is a `string`, which is just what Python calls a bunch of characters (like numbers, letters, whitespace, and punctuation) put together. Strings are indicated by being surrounded by quotes:
```python
"Hello"
"Python, I'm your #1 fan!"
```
You can use the function `type` to check the data types:
```python
type("Hello")
type(1)
type("1")
```
What does the following program print out?
```python
first, second = 'Grace', 'Hopper'
third, fourth = second, first
print(third, fourth)
```
One fun thing about strings in Python is that you can multiply them. Try it!
```python
print("A" * 40)
```
```python
print("ABC" * 12)
```
```python
h = "Happy"
b = "Birthday"
print((h + b) * 10)
```
### Use an index to get a single character from a string.
- The characters (individual letters, numbers, and so on) in a string are ordered. For example, the string ‘AB’ is not the same as ‘BA’. Because of this ordering, we can treat the string as a list of characters.
- Each position in the string (first, second, etc.) is given a number. This number is called an index or sometimes a subscript.
- Indices are numbered from 0.
- Use the position’s index in square brackets to get the character at that position.
```python
district = 'seattle'
print(district[0])
```
### Slicing
- A part of a string is called a substring. A substring can be as short as a single character.
- An item in a list is called an element. Whenever we treat a string as if it were a list, the string’s elements are its individual characters.
- A slice is a part of a string (or, more generally, any list-like thing).
- We take a slice by using `[start:stop]`, where `start` is replaced with the index of the first element we want and `stop` is replaced with the index of the element just after the last element we want.
- Mathematically, you might say that a slice selects `[start:stop]`.
- The difference between stop and start is the slice’s length.
- Taking a slice does not change the contents of the original string. Instead, the slice is a copy of part of the original string.
A section of a string is called a slice. We can take slices of character strings:
```python
district = 'seattle'
print('first three characters:', district[0:3])
print('last three characters:', district[4:])
print('last three characters:', district[-3:])
```
### You can use the built in function `len()` to return the length of a string:
```python
len(district)
```
## Exercises 1.1
Read the following expressions, but don't execute them. Guess what the output will be. After you've made a guess, copy and paste the expressions at a Python prompt and check your guess.

Problem 1.
```python
total = 1.5 - 1/2
total
type(total)
```
Problem 2.
```python
a = "quick"
b =  "brown"
c = "fox jumps over the lazy dog"
print("The " +  a * 3 + " " +  b * 3 + " " + c)
```
