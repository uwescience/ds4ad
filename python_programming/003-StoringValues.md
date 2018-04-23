
# Storing Multiple Values in Lists

Just as a `for` loop is a way to do operations many times, a list is a way to store many values. Unlike NumPy arrays, lists are built into the language (so we don’t have to load a library to use them). We create a list by putting values inside square brackets and separating the values with commas:
```python
odds = [1,3,5,7]
print('odds are:', odds)
```
We select individual elements from lists by indexing them:
```python
print('first and last:', odds[0],odds[3])
```
and if we loop over a list, the loop variable is assigned elements one at a time:
```python
for number in odds:
    print(number)
```
There is one important difference between lists and strings: we can change the values in a list, but we cannot change individual characters in a string. For example:

```python
names = ['Curie', 'Darwing', 'Turing'] #Typo in Darwin's name
print('names is originally:', names)
names[1] = 'Darwin' # correct the name
print('final value of name:', names)
```
this works with list, but:
```python
name = 'Darwin'
name[0] = 'd'
```

does not!

## Making data modifications and implications

Data which can be modified in place is called mutable, while data which cannot be modified is called immutable. Strings and numbers are immutable. This does not mean that variables with string or number values are constants, but when we want to change the value of a string or number variable, we can only replace the old value with a completely new value.

Lists and arrays, on the other hand, are mutable: we can modify them after they have been created. We can change individual elements, append new elements, or reorder the whole list. For some operations, like sorting, we can choose whether to use a function that modifies the data in place or a function that returns a modified copy and leaves the original unchanged.

Be careful when modifying data in place. If two variables refer to the same list, and you modify the list value, it will change for both variables!


```python
salsa = ['peppers', 'onions', 'cilantro', 'tomatoes']
my_salsa = salsa # <-- my_salsa and salsa point to the *same* list data in memory
salsa[0] = 'hot peppers'
print('Ingredients in my salsa:', my_salsa)
```
If you want variables with mutable values to be independent, you must make a copy of the value when you assign it.

```python
salsa = ['peppers','onions','cilantro','tomatoes']
my_salsa = list(salsa) # <--makes a *copy* of the list
salsa[0] = 'hot peppers'
print('Ingridients in my salsa:', my_salsa)
```
Because of pitfalls like this, code which modifies data in place can be more difficult to understand. However, it is often far more efficient to modify a large data structure in place than to create a modified copy for every small change. You should consider both of these aspects when writing your code.

## Nested Lists 

Since lists can contain any Python variable, it can even contain other lists.

For example, we could represent the products in the shelves of a small grocery shop:


```python
x = [['pepper', 'zucchini', 'onion'],
     ['cabbage', 'lettuce', 'garlic'],
     ['apple', 'pear', 'banana']]
```

Here is a visual example of how indexing a list of lists `x` works:

![image.png](https://i.stack.imgur.com/6Vwry.png)

Using the previously declared list `x`, these would be the results of the index operations shown in the image:


```python
print([x[0]])
```
```python
print(x[0])
```
```python
print(x[0][0])
```
There are many ways to change the contents of lists besides assigning new values to individual elements:


```python
print(odds)
```
```python
odds.append(11)
print('odds after adding value', odds)
```

```python
del odds[0]
print('odds after removing the first element', odds)
```

```python
odds.reverse()
print('odds after reversing:', odds)
```
While modifying in place, it is useful to remember that Python treats lists in a slightly counter-intuitive way.

If we make a list and (attempt to) copy it then modify in place, we can cause all sorts of trouble:


```python
odds = [1, 3, 5, 7]
primes = odds
primes.append(2)
print('primes:', primes)
print('odds:', odds)
```