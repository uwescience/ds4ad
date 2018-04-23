# Unique values and counting

Very often, we are interested in tallying the results from a table. That is, we are interested 
in counting how many instances of some kind of observation are within our table.

`Pandas` makes counting of values relatively easy. 

An important step in tallying is to figure out the unique values within the collection of 
items that you are tallying. For example, in the data we have been looking at:

```
df['state'].unique()
```

Will give you an array containing the unique values of states. 

```
df['state'].value_counts()
```

Will give you the number of individuals in each bin. 

Note that this is also a `Series` object, so if you want to do further calculations with these 
numbers you can do that too. 

### Exercise

- How many people are in each tax bracket?
- Can you calculate and report that within each state? 

### Cross-tabulation

Cross-tabs are tables that display the contingent distributions of frequencies of certain 
combinations of variable values. For example, we can calculate the frequency of gender by 
state: 

```
ctab_state_gender = df.groupby(["state", "gender"]).count()
```

The result of cross-tabulation is also a `DataFrame`, but it has a more complicated index than 
the `DataFrame` objects we have seen so far. 

This is called a `MultiIndex`: 

```
ctab_state_gender
```
```
MultiIndex(levels=[['act', 'aw', 'nss', 'nsw', 'nt', 'nws', 'qdl', 'qld', 'qod', 'sa', 'sk', 'tas', 'vic', 'vid', 'vif', 'viy', 'vkc', 'wa', 'wz'], ['None', 'female', 'male']],
           labels=[[0, 0, 0, 1, 2, 3, 3, 3, 4, 4, 5, 6, 7, 7, 7, 8, 9, 9, 9, 10, 11, 11, 11, 12, 12, 12, 13, 14, 15, 16, 17, 17, 17, 18], [0, 1, 2, 2, 2, 0, 1, 2, 1, 2, 2, 2, 0, 1, 2, 1, 0, 1, 2, 1, 0, 1, 2, 0, 1, 2, 1, 0, 2, 1, 0, 1, 2, 2]],
           names=['state', 'gender'])
```

Here, the levels of the index are all the combinations between the first list and the second 
list within the levels 

`MultiIndex` objects can be used in a similar way to they way that a regular `Index` is used. 
For example, indexing using `loc` on the first level:

```
ctab_state_gender.loc["aw"]
```

will give you the table for that State (what would you do to get the table for "male" across 
states?)

You can also get a particular row in this table, by indexing with a sequence: 

```
ctab_state_gender.loc["aw", "male"]
```

### Exercise: 

Create a cross-tab of tax brackets as a function of state. 

How would you go about reporting a count of tax bracket, by state *and* gender? 