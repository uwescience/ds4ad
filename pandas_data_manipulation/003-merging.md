# Merging, joining and concatentating. 

Merging, joining and concatenating datasets are fundamental operations in processing of 
complex administrative datasets. This is because very often, we are interested in the 
correspondence between different  datasets that come from different sources. 

## Concatenating

One way to combine the data from different `DataFrame` objects is if they all have the same 
variables, and we think that each one represents distinctly different observations. For 
example, data about transactions or events from different years. 

Consider a simple example of this kind of data and its combination:

```
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']},
                    index=[0, 1, 2, 3]) 

df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D4', 'D5', 'D6', 'D7']},
                    index=[4, 5, 6, 7])


df3 = pd.DataFrame({'A': ['A8', 'A9', 'A10', 'A11'],
                    'B': ['B8', 'B9', 'B10', 'B11'],
                    'C': ['C8', 'C9', 'C10', 'C11'],
                    'D': ['D8', 'D9', 'D10', 'D11']},
                    index=[8, 9, 10, 11]) 

frames = [df1, df2, df3]

result = pd.concat(frames)
```

This will result in a new `DataFrame`, that has all the information in the three input frames. 

But you can also combine information between `DataFrame` objects that share only some of their columns and might share some of their data. For example: 

```
df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'], 
                    'D': ['D2', 'D4', 'D6', 'D7'],
                    'F': ['F2', 'F3', 'F6', 'F7']}, 
                    index=[2, 3, 6, 7])
```

There is more than one way to put together the data from `df1` and this new `DataFrame`. The default behavior leaves the data as it is: 

```
result = pd.concat([df1, df4], axis=1)
```

Here, the DataFrame is extended by both a column and by four rows. Wherever data is missing 
(for column `F` in `df1` and for columns `A` and `C` in the `df4`), we enter the Python `NaN`, 
which stands for "Not a Number". This accomodates both the situation in which `B3` and `D3` are paired (from `d1`), as well as `B3` and `D4` (from `df4`).

## Joining 

Another way to combine these `DataFrame` objects is to take into account the index and assume 
that there are some shared observations. We do so by concatenating on the column axis:

```
result = pd.concat([df1, df4], axis=1)
```

This results in a new `DataFrame` that has 7 columns, but only 6 rows, for the unique index values. This is called an **outer join** -- you can think of this as using the *union* of the indices. 

An alternative is to use the intersection of the indices. This is called an **inner join** and looks like this: 

```
result = pd.concat([df1, df4], axis=1, join="inner")
```

This results in a `DataFrame` with only the two common index rows, and 7 columns.

We can also specify which index to use in the output. For example: 

```
result = pd.concat([df1, df4], axis=1, join_axes=[df1.index])
```

## Merging

Another option is to merge the `DataFrame` objects. 

Per default, this would try to find the *inner* merge that works. For example: 

```
pd.merge(df1, df4)
```
results in a single-row `DataFrame` object that includes only the row where columns `B` and `D` fully agree.

Using an *outer* merge: 

```
pd.merge(df1, df4, how="outer")
```

Will include this one row, but also all the other rows, adding `NaN` values as needed.

One way to gain control over your merges is to use one of the columns as a key. That is, we'll consider records that have the same value in this key to potentially be the same record. 

For example: 

```
pd.merge(df1, df4, on="B")
```

Tells Pandas to do what it can to match all of the values in the key column in both `DataFrame` objects. Wherever there is anothe column that exists in both inputs, this produces two new variables, as `variable_x` and `variable_y`, indicating that these variables came from the left and right inputs to the merge respectively. 

Again, this also has an *outer* variant: 

```
pd.merge(df1, df4, on="B", how="outer")
```

which also produces two new variables, for columns that are not the merge key, but exist in both inputs.

Alternatively, you can specify more than one key for the merge:

```
pd.merge(df1, df4, on=["B", "D"], how="outer")
```

In this case, it does exactly the same as not specifying the merge keys.


### Exercise: 

Read in the data from: 

`https://raw.githubusercontent.com/uwescience/ds4ad/master/data/synthetic_services.csv`

This table includes data about these individuals use of services ("library" and "parking"). 
Use merging to combine the data. What would you need to do to find the address of people who've used the library in 2018? 

Things to think about: 

- Which table should we merge into which table?
- How do we figure out the year of a services? 


