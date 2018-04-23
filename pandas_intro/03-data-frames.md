## Note about Pandas DataFrames/Series

A [DataFrame][pandas-dataframe] is a collection of [Series][pandas-series];
The DataFrame is the way Pandas represents a table, and Series is the data-structure
Pandas use to represent a column.

Pandas is built on top of the [Numpy][numpy] library, which in practice means that
most of the methods defined for Numpy Arrays apply to Pandas Series/DataFrames.

What makes Pandas so attractive is the powerful interface to access individual records
of the table, proper handling of missing values, and relational-databases operations
between DataFrames.

## Selecting values

To access a value at the position `[i,j]` of a DataFrame, we have two options, depending on
what is the meaning of `i` in use.
Remember that a DataFrame provides a *index* as a way to identify the rows of the table;
a row, then, has a *position* inside the table as well as a *label*, which
uniquely identifies its *entry* in the DataFrame.

## Use `DataFrame.iloc[..., ...]` to select values by their (entry) position

*   Can specify location by numerical index analogously to 2D version of character selection in strings.

~~~
import pandas
data = pandas.read_csv('https://raw.githubusercontent.com/uwescience/ds4ad/master/data/synthetic_data.csv', index_col='rec_id')
print(data.iloc[0, 0])
~~~

~~~
sarah
~~~


## Use `DataFrame.loc[..., ...]` to select values by their (entry) label.

*   Can specify location by row name analogously to 2D version of dictionary keys.

~~~
print(data.loc["rec-2778-org", "given_name"])
~~~

~~~
sarah
~~~

## Use `:` on its own to mean all columns or all rows.

*   Just like Python's usual slicing notation.

~~~
print(data.loc["rec-2778-org", :])
~~~

~~~
given_name               sarah
surname                  bruhn
street_number               44
address_1        forbes street
address_2           wintersloe
suburb            kellerberrin
postcode                  4510
state                      vic
date_of_birth      1.93002e+07
soc_sec_id             7535316
gender                  female
salary                  136344
Name: rec-2778-org, dtype: object
~~~


*   Would get the same result printing `data.loc["rec-2778-org"]` (without a second index).

~~~
print(data.loc[:, "given_name"])
~~~

~~~
rec_id
rec-2778-org            sarah
rec-712-dup-0           jacob
rec-1321-org          brinley
rec-3004-org          aleisha
rec-1384-org            ethan
rec-3981-org           alicia
rec-916-org          benjamin
rec-1684-org         petreece
rec-63-dup-0           olivia
rec-3808-org              NaN
rec-112-org            joshua
rec-3297-org          rachael
rec-1315-org           joseph
rec-1050-org            sarah
rec-2116-org          sidonie
rec-3232-org           andrew
rec-1900-dup-1          kiara
rec-2460-dup-2       nicholas
rec-3123-org         isabella
rec-2166-org        alexandra
rec-1155-org          matthew
rec-1485-org          michael
rec-2852-org          georgia
rec-707-org               tia
rec-3499-org           brooke
rec-2153-org             liam
rec-1665-dup-2         jamesr
rec-35-dup-1            darcy
rec-78-org               kody
rec-3116-org            james
                     ...     
rec-3650-org           samuel
rec-1323-org             jack
rec-1278-org          michael
rec-540-dup-0         anthony
rec-724-org           kaitlin
rec-3120-org          monique
rec-881-org          brooklyn
rec-2303-org            polly
rec-1772-org            amber
rec-3587-org          madelyn
rec-3005-org      christopher
rec-2636-org          annabel
rec-235-org            thomas
rec-2822-dup-0          molly
rec-322-org            brooke
rec-1972-org            carly
rec-3433-org          william
rec-2889-org            naomi
rec-1313-org          zachary
rec-2206-org        john-paul
rec-2433-org              mia
rec-1609-org          madison
rec-3812-org          georgia
rec-303-org           clodagh
rec-2284-org              sam
rec-1487-org           thomas
rec-1856-org            james
rec-3307-org            paige
rec-227-org           antonio
rec-1143-org            harry
Name: given_name, Length: 5000, dtype: object
~~~

*   Would get the same result printing `data["given_name"]`
*   Also get the same result printing `data.given_name` (since it's a column name)

## Select multiple columns or rows using `DataFrame.loc` and a named slice.

~~~
print(data.loc['rec-2778-org':'rec-1384-org', 'given_name':'street_number'])
~~~

~~~
              given_name    surname  street_number
rec_id                                            
rec-2778-org       sarah      bruhn           44.0
rec-712-dup-0      jacob     lanyon            5.0
rec-1321-org     brinley  efthimiou           35.0
rec-3004-org     aleisha     hobson           54.0
rec-1384-org       ethan    gazzola           49.0
~~~


In the above code, we discover that **slicing using `loc` is inclusive at both
ends**, which differs from **slicing using `iloc`**, where slicing indicates
everything up to but not including the final index.


## Result of slicing can be used in further operations.

*   Usually don't just print a slice.
*   All the statistical operators that work on entire dataframes
    work the same way on slices.
*   E.g., calculate max of a slice.

~~~
print(data.loc['rec-2778-org':'rec-1384-org', 'given_name':'street_number'].max())
~~~

~~~
given_name        sarah
surname          lanyon
street_number        54
dtype: object
~~~


~~~
print(data.loc['rec-2778-org':'rec-1384-org', 'given_name':'street_number'].min())
~~~

~~~
given_name       aleisha
surname            bruhn
street_number          5
dtype: object
~~~


## Use comparisons to select data based on value.

*   Comparison is applied element by element.
*   Returns a similarly-shaped dataframe of `True` and `False`.

~~~
# Use a subset of data to keep output readable.
subset = data.loc['rec-2778-org':'rec-1384-org', 'soc_sec_id':'salary']
print('Subset of data:\n', subset)

# Which values were greater than 50000 ?
print('\nWhere are values large?\n', subset > 50000)
~~~

~~~
Subset of data:
                soc_sec_id  gender  salary
rec_id                                   
rec-2778-org      7535316  female  136344
rec-712-dup-0     9497788    male   59079
rec-1321-org      6814956    male   39987
rec-3004-org      5967384  female   47962
rec-1384-org      3832742    male   39988

Where are values large?
                soc_sec_id  gender  salary
rec_id                                   
rec-2778-org         True    True    True
rec-712-dup-0        True    True    True
rec-1321-org         True    True   False
rec-3004-org         True    True   False
rec-1384-org         True    True   False
~~~


## Select values or NaN using a Boolean mask.

*   A frame full of Booleans is sometimes called a *mask* because of how it can be used.

~~~
mask = subset > 50000
print(subset[mask])
~~~

~~~
soc_sec_id  gender    salary
rec_id                                     
rec-2778-org      7535316  female  136344.0
rec-712-dup-0     9497788    male   59079.0
rec-1321-org      6814956    male       NaN
rec-3004-org      5967384  female       NaN
rec-1384-org      3832742    male       NaN
~~~


*   Get the value where the mask is true, and NaN (Not a Number) where it is false.
*   Useful because NaNs are ignored by operations like max, min, average, etc.

~~~
print(subset[subset > 50000].describe())
~~~

~~~
soc_sec_id         salary
count  5.000000e+00       2.000000
mean   6.729637e+06   97711.500000
std    2.079188e+06   54634.605448
min    3.832742e+06   59079.000000
25%    5.967384e+06   78395.250000
50%    6.814956e+06   97711.500000
75%    7.535316e+06  117027.750000
max    9.497788e+06  136344.000000
~~~


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


[pandas-dataframe]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html
[pandas-series]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html
[numpy]: http://www.numpy.org/
