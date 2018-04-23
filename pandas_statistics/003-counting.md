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

### Comparisons 

We saw `groupby` yesterday. We come back to it now, to ask comparative questions. 

We can compare different states in terms 