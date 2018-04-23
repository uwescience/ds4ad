## The `apply` function

The `apply` function allows us to repeatedly use a function over all the columns in a `DataFrame`. 

This can also be used for data cleaning. For example, consider the `date_of_birth` column. Remember from our previous section that this is represented as a floating point number with year, followed by month, followed by day:  

```
df["date_of_birth"].head()
```
```
0    19300213.0
1    19080712.0
2    19940319.0
3    19290427.0
4    19631225.0
Name: date_of_birth, dtype: float64
```

How would we go about cleaning this column and producing something that can be used?   

Let's start by writing a function that cleans this up for a single row. 

```
row = df.iloc[0]
print(row["date_of_birth"])
```
```
19300213.0
```

Fortunately, `Pandas` has a function called `datetime`. This takes as input the year, the month and the day, and returns an object of type `datetime`, that we can do time calculations with (for example, we can calculate the time difference between two of these).

This conversion requires the following code:

```
def dob(row):
    dob_as_int = int(row["date_of_birth"])
    dob_as_str = str(dob_as_int)
    year = int(dob_as_str[0:4])
    month = int(dob_as_str[4:6])
    day = int(dob_as_str[6:])
    return pd.datetime(year, month, day)
```

```
pandas.to_datetime(data.date_of_birth.astype(str), format='%Y%m%d')
```


But we would like to run this function over every line of the file! This is where the `apply` 
function comes in. This function allows you to apply a function to each row in a table: 

```
df['dob'] = df.apply(dob, axis=1)
```

This runs the function `dob` in each row of the `DataFrame` and puts the values into a new column called `dob`. 

But this runs into trouble! 

```
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-117-9f961ff8a757> in <module>()
----> 1 df['dob'] = df.apply(dob, axis=1)

~/.virtualenvs/ds4ad/lib/python3.6/site-packages/pandas/core/frame.py in apply(self, func, axis, broadcast, raw, reduce, args, **kwds)
   4875                         f, axis,
   4876                         reduce=reduce,
-> 4877                         ignore_failures=ignore_failures)
   4878             else:
   4879                 return self._apply_broadcast(f, axis)

~/.virtualenvs/ds4ad/lib/python3.6/site-packages/pandas/core/frame.py in _apply_standard(self, func, axis, ignore_failures, reduce)
   4971             try:
   4972                 for i, v in enumerate(series_gen):
-> 4973                     results[i] = func(v)
   4974                     keys.append(v.name)
   4975             except Exception as e:

<ipython-input-116-523204daa8f2> in dob(row)
      1 def dob(row):
----> 2     dob_as_int = int(row["date_of_birth"])
      3     dob_as_str = str(dob_as_int)
      4     year = int(dob_as_str[0:4])
      5     month = int(dob_as_str[4:6])

ValueError: ('cannot convert float NaN to integer', 'occurred at index 16')
```

What's going on in index 16? 

```
df.iloc[16]
```
```
rec_id           rec-1900-dup-1
given_name                kiara
surname                 everett
street_number                 8
address_1         eddy crescent
address_2          legacy v lge
suburb             nobl ee park
postcode                   6076
state                        wa
date_of_birth               NaN
soc_sec_id              3831644
gender                   female
salary                    39430
dob                         NaT
Name: 16, dtype: object
```

Oh no! This person doesn't have a date-of-birth! The "NaN" in that entry means "Not a number". This is the computer's way of representing null values. We need to build a contingency into our function for this case: 

```
def dob(row):
    if pd.isnull(row["date_of_birth"]):
        return pd.NaT
    dob_as_int = int(row["date_of_birth"])
    dob_as_str = str(dob_as_int)
    year = int(dob_as_str[0:4])
    month = int(dob_as_str[4:6])
    day = int(dob_as_str[6:])
    return pd.datetime(year, month, day)
```

This makes sure that if the input in some row is a `null` value, such as the not a number

But this also runs into trouble: 

```
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-120-9f961ff8a757> in <module>()
----> 1 df['dob'] = df.apply(dob, axis=1)

~/.virtualenvs/ds4ad/lib/python3.6/site-packages/pandas/core/frame.py in apply(self, func, axis, broadcast, raw, reduce, args, **kwds)
   4875                         f, axis,
   4876                         reduce=reduce,
-> 4877                         ignore_failures=ignore_failures)
   4878             else:
   4879                 return self._apply_broadcast(f, axis)

~/.virtualenvs/ds4ad/lib/python3.6/site-packages/pandas/core/frame.py in _apply_standard(self, func, axis, ignore_failures, reduce)
   4971             try:
   4972                 for i, v in enumerate(series_gen):
-> 4973                     results[i] = func(v)
   4974                     keys.append(v.name)
   4975             except Exception as e:

<ipython-input-119-25f442bd3789> in dob(row)
      7     month = int(dob_as_str[4:6])
      8     day = int(dob_as_str[6:])
----> 9     return pd.datetime(year, month, day)

ValueError: ('month must be in 1..12', 'occurred at index 544')

```

This means that some month values are not in the range 1 - 12. Let's make sure that we only let through values of day, month and year that make sense:

```
def dob(row):
    if pd.isnull(row["date_of_birth"]):
        return pd.NaT
    else:
        dob_as_int = int(row["date_of_birth"])
        dob_as_str = str(dob_as_int)
        year = int(dob_as_str[0:4])
        month = int(dob_as_str[4:6])
        day = int(dob_as_str[6:])
        if month > 12 or day > 31 or month < 1 or day < 1 or year < 1 or year > 2018:
            return pd.NaT
        else:
            return pd.datetime(year, month, day)
```

This works!

### Exercise 

A datetime object has an attribute `year` that gives you the year of that `datetime` object, an attribute `month` that gives you the month of that `datetime` object and an attribute `day` that gives you the day of that `datetime` object. These are all returned to you as integers.

Using the `apply` function, create columns in the `DataFramer` for `dob_year`, `dob_month` and `dob_day` each represented as an integer. 

If you are done with that, calculate another column for `age` (based on the present day). 

**Hint**: pd.datetime(2018, 4, 24) - pd.datetime(1977, 12, 9) will give you back a `timedelta` object



