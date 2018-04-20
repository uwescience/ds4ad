# Loops in Python
An example task that we might want to repeat is printing each character in a word on a line of its own.


```python
word = 'lead'
```

We can access a character in a string using its index. For example, we can get the first character of the word `lead`, by using `word[0]`. One way to print each character is to use four `print` statements:


```python
print(word[0])
print(word[1])
print(word[2])
print(word[3])
```
This is a bad approach for two reasons:
1. It doesn’t scale: if we want to print the characters in a string that’s hundreds of letters long, we’d be better off just typing them in.
2. It’s fragile: if we give it a longer string, it only prints part of the data, and if we give it a shorter one, it produces an error because we’re asking for characters that don’t exist.


```python
word = 'tin'
print(word[0])
print(word[1])
print(word[2])
print(word[3])
```
Here's a better approach:
```python
word = 'lead'
for char in word:
    print(char)
```
This is shorter — certainly shorter than something that prints every character in a hundred-letter string — and more robust as well:
```python
word = 'oxygen'
for char in word:
    print(char)
```
The improved version uses a *for loop* to repeat an operation — in this case, printing — once for each thing in a sequence. The general form of a loop is:
```python
for variable in collection:
    do things with variable
```
![loops](https://mpicbg-scicomp.github.io/dlbc17-python-intro/fig/loops_image.png)

where each character (`char`) in the variable `word` is looped through and printed one character after another. The numbers in the diagram denote which loop cycle the character was printed in (1 being the first loop, and 6 being the final loop).

We can call the *loop variable* anything we like, but there must be a colon at the end of the line starting the loop, and we must indent anything we want to run inside the loop. Unlike many other languages, there is no command to signify the end of the loop body (e.g. `end for`); what is indented after the `for` statement belongs to the loop.

### *What's in the name?*

In the example above, the loop variable was given the name `char` as a mnemonic; it is short for ‘character’. We can choose any name we want for variables. We might just as easily have chosen the name `banana` for the loop variable, as long as we use the same name when we invoke the variable inside the loop:


```python
word = 'oxygen'
for banana in word:
    print(banana)
```
*It is a good idea to choose variable names that are meaningful, otherwise it would be more difficult to understand what the loop is doing.*

Here’s another loop that repeatedly updates a variable:


```python
length = 0
for vowel in 'aeiou':
    length = length + 1
print('There are', length, 'vowels')
```
It’s worth tracing the execution of this little program step by step. Since there are five characters in `aeiou`, the statement on line 3 will be executed five times. 
1. The first time around, `length` is zero (the value assigned to it on line 1) and vowel is 'a'. 
- The statement adds 1 to the old value of length, producing 1, and updates length to refer to that new value. 
2. The next time around, vowel is 'e' and length is 1, so length is updated to be 2. 
*After three more updates, length is 5;* since there is nothing left in `aeiou` for Python to process, the loop finishes and the print statement on line 4 tells us our final answer.

Note that a loop variable is just a variable that’s being used to record progress in a loop. It still exists after the loop is over, and we can re-use variables previously defined as loop variables as well:


```python
letter = 'z'
for letter in 'abc':
    print(letter)
print('after the loop, letter is', letter)
```
Note also that finding the length of a string is such a common operation that Python actually has a built-in function to do it called `len`:
```python
print(len('aeiou'))
```
`len` is much faster than any function we could write ourselves, and much easier to read than a two-line loop; it will also give us the length of many other things that we haven’t met yet, so we should always use it when we can.