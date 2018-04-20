
# Making Choices
Sometimes we want to have our code make some choices based on what it got as inputs.

### Booleans
So far, the code we've written has been unconditional: no choice is getting made, and the code is always run. Python has another data type called a boolean that is helpful for writing code that makes decisions. There are two booleans: `True` and `False`. Try them out:
```python
True
type(True)
```
```python
False
type(False)
```
You can test if Python objects are equal or unequal. The result is a boolean:
```python
0 == 0
0 == 1
```
This can be a bit confusing: Use == to test for equality. Recall that = is used for assignment.

This is an important idea and can be a source of errors until you get used to it: = is assignment, == is comparison.

Use != to test for inequality:
```python
"a" != "a"
"a" != "A"
```
<, <=, >, and >= have the same meaning as in math class. The result of these tests is a boolean:
```python
a > 0
2 >= 3
-1 < 0
.5 <= 1
```
You can check for containment with the `in` keyword, which also results in a boolean:

## Conditionals
We can ask python to take different actions, depending on a condition, with an `if` statement:
```python
"H" in "Hello"

"X" in "Hello"
```
Or check for a lack of containment with `not in`:
```python
"a" not in "abcde"

"Perl" not in "Python Workshop"
```
## `If` statements
The simplest way to make a choice in Python is with the `if` keyword. Here's an example:
```python
num = 100
if num > 37:
    print('greater')

num = 37
if num > 100:
    print('greater')
```
The second line of this code uses the keyword `if` to tell Python that we want to make a choice. If the test that follows the `if` statement is true, the body of the `if` (i.e., the lines indented underneath it) are executed. The code inside the indented block is only executed when the evaluated statement (37 > 100 or 100 > 37) is true.

Guess what will happen with these other expressions, then type them out and see if your guess was correct:
```python
if 0 > 2:
    print("Zero is greater than two!")


if "banana" in "bananarama":
    print("I miss the 80s.")
```
### More choices, `if` and `else`
`if` lets you execute some code only if a condition is True. What if you want to execute some different code if a condition is False?

Use the `else` keyword, together with `if`, to execute different code when the if condition isn't `True`. Try this:
```python
num = 37
if num > 100:
    print('greater')
else:
    print('not greater')
```
Like with if, the code block under the else condition must be indented so Python knows that it is a part of the else block. Conditional statements don’t have to include an `else`. If there isn’t one, Python simply does nothing if the test is false:
### Compound conditionals: `and` and `or`
We can also combine tests using `and` and `or`. `and` is only true if both parts are true:
```python
if (1 > 0) and (-1 > 0):
    print('both parts are true')
else:
    print('at least one part is false')
```
while `or` is true if at least one part is true:
```python
if (1 < 0) or (-1 < 0):
    print('at least one test is true')
```

Consider the code, what do you think will happen?:

```python
temperature = 32
if temperature > 60 and temperature < 75:
    print("It's nice and cozy in here!")
else:
    print("Too extreme for me.")
```
### Even more choices: `elif`
If you need to execute code conditional based on more than two cases, you can use the `elif` keyword to check more cases. You can have as many `elif` cases as you want; Python will go down the code checking each `elif` until it finds a true condition or reaches the default `else` block. Try this:
```python
sister_age = 15
brother_age = 12
if sister_age > brother_age:
    print("sister is older")
elif sister_age == brother_age:
    print("sister and brother are the same age")
else:
    print("brother is older")
```
*Note*: The `==` denotes that we are checking whether two things on both sides of the symbol are equal to each other. It means something completely different than a single `=` sign!

You don't have to have an else block, if you don't need it. That just means there isn't default code to execute when none of the if or elif conditions are true:
```python
num = -3

if num > 0:
    print(num, 'is positive')
elif num == 0:
    print(num, 'is zero')
```
