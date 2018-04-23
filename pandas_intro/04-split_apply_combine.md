## Select-Apply-Combine operations

Pandas vectorizing methods and grouping operations are features that provide users
much flexibility to analyze their data.

For instance, let's say we want to find out what the salary differences are between
different genders in the dataset.

We can use pandas' `groupby` method to separate the data by gender and then get
some summary statistics for each set.

~~~
data.groupby('gender').salary.sum()
~~~

~~~
gender
None        4550441
female    104980118
male      100594196
Name: salary, dtype: int64
~~~

Of course, we need to know how many of each gender there are for the sum to make sense.

~~~
data.groupby('gender').salary.count()
~~~

~~~
gender
None       109
female    2357
male      2534
Name: salary, dtype: int64
~~~

We can divide these to find the average, or we could also let pandas calculate the mean for us.

~~~
data.groupby('gender').salary.mean()
~~~

~~~
gender
None      41747.165138
female    44539.719134
male      39697.788477
Name: salary, dtype: float64
~~~

Let's also revisit the date of birth re-coding that we developed earlier.
First we can do the same operation that we used before:

~~~
dob_datetime = pandas.to_datetime(data.date_of_birth.astype(str), format='%Y%m%d')
~~~

But this gives us an error! It looks like some of the data aren't able to be converted to a datetime.
We can tell the `to_datetime` function to turn any data that can't be converted into
a missing value (indicated by 'NaT' meaning 'Not a Time') so that we can proceed:

~~~
dob_datetime = pandas.to_datetime(data.date_of_birth.astype(str), format='%Y%m%d', errors='coerce')
~~~

Ok, that didn't error, so lets take a look and see how it did.

~~~
dob_datetime.head()
~~~

~~~
rec_id
rec-2778-org    NaT
rec-712-dup-0   NaT
rec-1321-org    NaT
rec-3004-org    NaT
rec-1384-org    NaT
Name: date_of_birth, dtype: datetime64[ns]
~~~

Uh oh, now all the data are set to 'NaT', so something is going wrong even for the first two rows that worked before. Let's look at the data to see if we can tell what's going on.

~~~
data.date_of_birth.head()
~~~

~~~
rec_id
rec-2778-org     19300213.0
rec-712-dup-0    19080712.0
rec-1321-org     19940319.0
rec-3004-org     19290427.0
rec-1384-org     19631225.0
Name: date_of_birth, dtype: float64
~~~

So for some reason these rows are now showing up as floats rather than ints, so when we convert them to strings they don't match the expected formatting. Let's look at more rows to see if we can figure out why the values are floats now.

~~~
data.date_of_birth.head(20)
~~~

~~~
rec_id
rec-2778-org      19300213.0
rec-712-dup-0     19080712.0
rec-1321-org      19940319.0
rec-3004-org      19290427.0
rec-1384-org      19631225.0
rec-3981-org      19421201.0
rec-916-org       19450918.0
rec-1684-org      19950620.0
rec-63-dup-0      19000106.0
rec-3808-org      19150402.0
rec-112-org       19951125.0
rec-3297-org      19910228.0
rec-1315-org      19370312.0
rec-1050-org      19400826.0
rec-2116-org      19471114.0
rec-3232-org      19720809.0
rec-1900-dup-1           NaN
rec-2460-dup-2    19390305.0
rec-3123-org      19580628.0
rec-2166-org      19261017.0
Name: date_of_birth, dtype: float64
~~~

Ah ha! There are missing values, marked 'NaN' (for 'Not a Number'). Any time there are NaNs in an integer column, pandas converts that column to floats because it has no representation for missing numbers in the integer data type.

So now we need a way to convert the float values to datetimes and the NaN values to NaTs. Pandas has a method called `apply` that will apply a function to each row. So if we can write a function that will give us the right thing for any row, we can pass it to apply and pandas will run it on every row.

Let's look at the first row:

~~~
print(data.date_of_birth.loc['rec-2778-org'])
~~~

~~~
19300213.0
~~~

So we need to convert the value back to an integer, then to a string and then use the `to_datetime`
method to convert it to a datetime.

~~~
print(pandas.to_datetime(str(int(data.date_of_birth.loc['rec-2778-org'])), format='%Y%m%d', errors='coerce'))
~~~

~~~
Timestamp('1930-02-13 00:00:00')
~~~

Ok, that works when we don't have missing data. Now let's look at a row with a NaN:

~~~
print(data.date_of_birth.loc['rec-1900-dup-1'])
~~~

~~~
nan
~~~

We need our function to be able to identify NaNs. Pandas has functions called `isnull` and `notnull`
that return True or False for NaNs or other null values:

~~~
print(pandas.isnull(data.date_of_birth.loc['rec-1900-dup-1']))
~~~

~~~
True
~~~

So we can use `isnull` to test for null values and return the right thing in each case:

~~~
def dob_to_datetime(dob_float):
    if pandas.notnull(dob_float):
        return pandas.to_datetime(str(int(dob_float)), format='%Y%m%d', errors='coerce')
    else:
        return pandas.to_datetime('NaT')
~~~

And we can run this function on our two test rows to see how they do:

~~~
print(dob_to_datetime(data.date_of_birth.loc['rec-2778-org']))
~~~

~~~
Timestamp('1930-02-13 00:00:00')
~~~

~~~
print(dob_to_datetime(data.date_of_birth.loc['rec-1900-dup-1']))
~~~

~~~
NaT
~~~

Looks like it's working as we hoped, so now let's run it on all the rows.

~~~
dob_datetime = data.date_of_birth.apply(dob_to_datetime)
dob_datetime.head(20)
~~~

~~~
rec_id
rec-2778-org     1930-02-13
rec-712-dup-0    1908-07-12
rec-1321-org     1994-03-19
rec-3004-org     1929-04-27
rec-1384-org     1963-12-25
rec-3981-org     1942-12-01
rec-916-org      1945-09-18
rec-1684-org     1995-06-20
rec-63-dup-0     1900-01-06
rec-3808-org     1915-04-02
rec-112-org      1995-11-25
rec-3297-org     1991-02-28
rec-1315-org     1937-03-12
rec-1050-org     1940-08-26
rec-2116-org     1947-11-14
rec-3232-org     1972-08-09
rec-1900-dup-1          NaT
rec-2460-dup-2   1939-03-05
rec-3123-org     1958-06-28
rec-2166-org     1926-10-17
Name: date_of_birth, dtype: datetime64[ns]
~~~

And we can replace the column in the dataframe with these datetime values:

data.date_of_birth = dob_datetime
data.date_of_birth.head()

rec_id
rec-2778-org    1930-02-13
rec-712-dup-0   1908-07-12
rec-1321-org    1994-03-19
rec-3004-org    1929-04-27
rec-1384-org    1963-12-25
Name: date_of_birth, dtype: datetime64[ns]
