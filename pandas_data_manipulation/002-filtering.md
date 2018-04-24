## Filtering

Administrative data often contains entries that are erroneous. This can  be due to errors in 
data entry, or errors  in data extraction. 
Identifying these errors is important because: 

- We would like to clean out these errors in analysis
- We would like to improve the way that data is generated. 

To find erroneous data, we need to have a sense of the reasonable values  of a variable. 
For example, what would be  unreasonable values of the salary variable  in our synthetic 
dataset? 

We'll read again the synthetic data that we used in previous lessons: 

```
df = pd.read_csv('https://raw.githubusercontent.com/uwescience/ds4ad/master/data/synthetic_data.csv', index_col="rec_id")
>>>>>>> bd1d5bcc9ad868d4df53b2ba02b681524000973c
```

Salary values that are lower than 0 (negative salaries!) are probably erroneous. 

How do we find these values? 

The `salary` column is a `Series` object: 

```
type(df["salary"])
```

```
pandas.core.series.Series
```

Given an object of this type, we can conduct a comparison between all the values of the `Series` and a value. For example: 

```
negative_salary = df["salary"] < 0 
```

This also produces a `Series` object: 

```
type(negative_salary)
```
```
pandas.core.series.Series
```

What kind of values are in this `Series`? 

```
print(negative_salary)
```

These are all Boolean values -- either `False` or (much more rarely) `True`.

We can use this `Series` as an index into the original `DataFrame`: 

```
df_negative_salary = df[negative_salary]
print(df_negative_salary.shape)
```

It turns out that there are a few (40) rows in which salaries are negative. 

### Exercise: 

Create a `DataFrame` called `df_positive_salaries` which includes only salaries that are 0 or larger. 


### Solution

There are a few ways to create this `DataFrame`: 

```
df_positive_salaries = df[df["salary"] > 0]
```

or

```
df_positive_salaries = df[~negative_salary]
```

