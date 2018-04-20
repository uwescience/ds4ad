
# Creating Functions

At this point, we’ve written code to loop over objects containing data to quickly draw these print out what was needed.  When you are working with data, our code can get pretty long and complicated; what if we had thousands of data sets, and didn’t want to generate a summary for every single one separately? Commenting out the long code is a nuisance. Also, what if we want to use that code again, on a different data set or at a different point in our program? Cutting and pasting it is going to make our code get very long and very repetitive, very quickly. We’d like a way to package our code so that it is easier to reuse, and Python provides for this by letting us define things called ‘functions’ — a shorthand way of re-executing longer pieces of code.

Let’s start by defining a function `fahr_to_celsius` that converts temperatures from Fahrenheit to Celsius:


```python
def fahr_to_celsius(temp):
    return((temp-32) * (5/9))
```

![image.png](https://swcarpentry.github.io/python-novice-inflammation/fig/python-function.svg)

The function definition opens with the keyword `def` followed by the name of the function (`fahr_to_celsius`) and a parenthesized list of parameter names (`temp`). The body of the function — the statements that are executed when it runs — is indented below the definition line. The body concludes with a `return` keyword followed by the return value.

When we call the function, the values we pass to it are assigned to those variables so that we can use them inside the function. Inside the function, we use a return statement to send a result back to whoever asked for it.

Let’s try running our function.


```python
fahr_to_celsius(32)
```
This command should call our function, using “32” as the input and return the function value.

In fact, calling our own function is no different from calling any other function:

```python
print('freezing point of water:', fahr_to_celsius(32), 'C')
print('boiling point on water:', fahr_to_celsius(212), 'C')
```
We’ve successfully called the function that we defined, and we have access to the value that we returned.

## Composing Functions

Now that we’ve seen how to turn Fahrenheit into Celsius, we can also write the function to turn Celsius into Kelvin:


```python
def celsius_to_kelvin(temp_c):
    return temp_c + 273.15

print('freezing point of water in Kelvin:', celsius_to_kelvin(0.))
```
What about converting Fahrenheit to Kelvin? We could write out the formula, but we don’t need to. Instead, we can compose the two functions we have already created:

```python
def fahr_to_kelvin(temp_f):
    temp_c = fahr_to_celsius(temp_f)
    temp_k = celsius_to_kelvin(temp_c)
    return temp_k

print('boiling point of water in Kelvin:', fahr_to_kelvin(212.0))
``` 

This is our first taste of how larger programs are built: we define basic operations, then combine them in ever-large chunks to get the effect we want. Real-life functions will usually be larger than the ones shown here — typically half a dozen to a few dozen lines — but they shouldn’t ever be much longer than that, or the next person who reads it won’t be able to understand what’s going on.
