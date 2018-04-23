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


[pandas-dataframe]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html
[pandas-series]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html
[numpy]: http://www.numpy.org/
