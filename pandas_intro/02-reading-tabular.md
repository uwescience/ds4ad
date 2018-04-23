
## Use the Pandas library to do statistics on tabular data.

*   Pandas is a widely-used Python library for statistics, particularly on tabular data.
*   Borrows many features from R's dataframes.
    *   A 2-dimenstional table whose columns have names
        and potentially have different data types.
*   Load it with `import pandas`.
*   Read a Comma Separate Values (CSV) data file with `pandas.read_csv`.
    *   Argument is the name of the file to be read.
    *   Assign result to a variable to store the data that was read.

We're only going read the first 2 rows initially (using `nrows=2`) to get a sense of the data.

~~~
import pandas

data = pandas.read_csv('https://raw.githubusercontent.com/uwescience/ds4ad/master/data/synthetic_data.csv', nrows=2)
print(data)
~~~

~~~
          rec_id given_name surname  street_number      address_1   address_2  \
0   rec-2778-org      sarah   bruhn             44  forbes street  wintersloe   
1  rec-712-dup-0      jacob  lanyon              5     milne cove     wellwod   

               suburb  postcode state  date_of_birth  soc_sec_id  gender  \
0        kellerberrin      4510   vic       19300213     7535316  female   
1  beaconsfield upper      2602   vic       19080712     9497788    male   

   salary  
0  136344  
1   59079  
~~~


*   The columns in a dataframe are the observed variables, and the rows are the observations.
*   Pandas uses backslash `\` to show wrapped lines when output is too wide to fit the screen.

## Use `index_col` to specify that a column's values should be used as row headings.

*   Row headings are numbers (0 and 1 in this case).
*   Really want to index by rec_id.
*   Pass the name of the column to `read_csv` as its `index_col` parameter to do this.

~~~
data = pandas.read_csv('https://raw.githubusercontent.com/uwescience/ds4ad/master/data/synthetic_data.csv', index_col='rec_id', nrows=2)
print(data)
~~~

~~~
              given_name surname  street_number      address_1   address_2  \
rec_id                                                                       
rec-2778-org       sarah   bruhn             44  forbes street  wintersloe   
rec-712-dup-0      jacob  lanyon              5     milne cove     wellwod   

                           suburb  postcode state  date_of_birth  soc_sec_id  \
rec_id                                                                         
rec-2778-org         kellerberrin      4510   vic       19300213     7535316   
rec-712-dup-0  beaconsfield upper      2602   vic       19080712     9497788   

               gender  salary  
rec_id                         
rec-2778-org   female  136344  
rec-712-dup-0    male   59079  
​~~~
~~~

## Use `DataFrame.info` to find out more about a dataframe.

~~~
data.info()
~~~

~~~
<class 'pandas.core.frame.DataFrame'>
Index: 2 entries, rec-2778-org to rec-712-dup-0
Data columns (total 12 columns):
given_name       2 non-null object
surname          2 non-null object
street_number    2 non-null int64
address_1        2 non-null object
address_2        2 non-null object
suburb           2 non-null object
postcode         2 non-null int64
state            2 non-null object
date_of_birth    2 non-null int64
soc_sec_id       2 non-null int64
gender           2 non-null object
salary           2 non-null int64
dtypes: int64(5), object(7)
memory usage: 208.0+ bytes
~~~


*   This is a `DataFrame`
*   Two rows named `'rec-2778-org'` and `'rec-712-dup-0'`
*   Twelve columns, some of which are `'object'` types and some of which are 64-bit integer values.
    *   We will talk later about null values, which are used to represent missing observations.
*   Uses 208 bytes of memory.

## The `DataFrame.columns` variable stores information about the dataframe's columns.

*   Note that this is data, *not* a method.
    *   Like `math.pi`.
    *   So do not use `()` to try to call it.
*   Called a *member variable*, or just *member*.

~~~
print(data.columns)
~~~

~~~
Index(['given_name', 'surname', 'street_number', 'address_1', 'address_2',
       'suburb', 'postcode', 'state', 'date_of_birth', 'soc_sec_id', 'gender',
       'salary'],
      dtype='object')
~~~


## Use `DataFrame.T` to transpose a dataframe.

*   Sometimes want to treat columns as rows and vice versa.
*   Transpose (written `.T`) doesn't copy the data, just changes the program's view of it.
*   Like `columns`, it is a member variable.

~~~
print(data.T)
~~~

~~~
rec_id          rec-2778-org       rec-712-dup-0
given_name             sarah               jacob
surname                bruhn              lanyon
street_number             44                   5
address_1      forbes street          milne cove
address_2         wintersloe             wellwod
suburb          kellerberrin  beaconsfield upper
postcode                4510                2602
state                    vic                 vic
date_of_birth       19300213            19080712
soc_sec_id           7535316             9497788
gender                female                male
salary                136344               59079
~~~


## Use `DataFrame.describe` to get summary statistics about data.

DataFrame.describe() gets the summary statistics of only the columns that have numerical data.
All other columns are ignored, unless you use the argument `include='all'`.
~~~
print(data.describe())
~~~

~~~
       street_number     postcode  date_of_birth    soc_sec_id         salary
count       2.000000     2.000000   2.000000e+00  2.000000e+00       2.000000
mean       24.500000  3556.000000   1.919046e+07  8.516552e+06   97711.500000
std        27.577164  1349.159739   1.552106e+05  1.387677e+06   54634.605448
min         5.000000  2602.000000   1.908071e+07  7.535316e+06   59079.000000
25%        14.750000  3079.000000   1.913559e+07  8.025934e+06   78395.250000
50%        24.500000  3556.000000   1.919046e+07  8.516552e+06   97711.500000
75%        34.250000  4033.000000   1.924534e+07  9.007170e+06  117027.750000
max        44.000000  4510.000000   1.930021e+07  9.497788e+06  136344.000000
~~~

*   Not particularly useful with just two records,
    but very helpful when there are thousands.

> Note that some of these columns don't seem very useful to be running these kinds of summary statistics over. The worst column is the `date_of_birth` column, which is really just the 4 digit year followed by the 2 digit month, followed by the 2 digit day all mashed together and treated as an integer!
> It's usually a good idea to recode columns to have the right data type. In the case of dates and times, there's a specific `datetime` data type built into pandas that we should use instead.
> We can use Pandas' `to_datetime()` method which converts strings to datetimes. First we have to convert the integer values to string and then we can tell `to_datetime()` what the format of our dates are:

>~~~
>dob_datetime = pandas.to_datetime(data.date_of_birth.astype(str), format='%Y%m%d')
>print(dob_datetime)
>~~~

>~~~
>rec_id
>rec-2778-org    1930-02-13
>rec-712-dup-0   1908-07-12
>Name: date_of_birth, dtype: datetime64[ns]
>~~~


Now let's read the full data set so we can see what we can do with more rows.

~~~
data = pandas.read_csv('https://raw.githubusercontent.com/uwescience/ds4ad/master/data/synthetic_data.csv', index_col='rec_id')
print(data.describe())
~~~

~~~
street_number     postcode  date_of_birth    soc_sec_id         salary
count    4777.000000  5000.000000   4.890000e+03  5.000000e+03    5000.000000
mean       81.319238  3664.421800   1.949335e+07  5.426621e+06   42024.951000
std       346.574035  1414.099737   2.879985e+05  2.578013e+06   24929.513736
min         0.000000   295.000000   1.900010e+07  1.001710e+06   -1274.000000
25%        10.000000  2470.750000   1.924091e+07  3.218299e+06   39420.500000
50%        26.000000  3192.000000   1.949042e+07  5.443562e+06   40797.000000
75%        60.000000  4670.000000   1.974071e+07  7.606632e+06   45429.250000
max     10286.000000  8200.000000   1.999122e+07  9.999667e+06  324798.000000
~~~



> ## Inspecting Data.
>
> After reading the full data set,
> use `help(data.head)` and `help(data.tail)`
> to find out what `DataFrame.head` and `DataFrame.tail` do.
>
> 1.  What method call will display the first three rows of this data?
> 2.  What method call will display the last three columns of this data?
>     (Hint: you may need to change your view of the data.)
>
> > ## Solution
> > 1. We can check out the first five rows of `data` by executing `data.head()` (allowing us to view the head
> > of the DataFrame). We can specify the number of rows we wish to see by specifying the parameter `n` in our call
> > to `data.head()`. To view the first three rows, execute:
> >
> > ~~~
> > data.head(n=3)
> > ~~~
> >
> >
> > The output is then
> > ~~~
> >      given_name	surname	street_number	address_1	address_2	suburb	postcode	state	date_of_birth	soc_sec_id  gender	salary
> > rec_id												
> > rec-2778-org	sarah	bruhn	44.0	forbes street	wintersloe	kellerberrin	4510	vic	19300213.0	7535316	female	136344
> > rec-712-dup-0	jacob	lanyon	5.0	milne cove	wellwod	beaconsfield upper	2602	vic	19080712.0	9497788	male	59079
> > rec-1321-org	brinley	efthimiou	35.0	sturdee crescent	tremearne	scarborough	5211	qld	19940319.0	6814956	male	39987
> > ~~~
> >
> > 2. To check out the last three rows of `data`, we would use the command, `data.tail(n=3)`,
> > analogous to `head()` used above. However, here we want to look at the last three columns so we need
> > to change our view and then use `tail()`. To do so, we create a new DataFrame in which rows and
> > columns are switched
> >
> > ~~~
> > data_flipped = data.T
> > ~~~
> >
> >
> > We can then view the last three columns of `data` by viewing the last three rows of `data_flipped`:
> > ~~~
> > data_flipped.tail(n=3)
> > ~~~
> >
> > The output is then
> > ~~~
> > rec_id	rec-2778-org	rec-712-dup-0	rec-1321-org	rec-3004-org	rec-1384-org	rec-3981-org	rec-916-org	rec-1684-org	rec-63-dup-0	rec-3808-org	...	rec-2433-org	rec-1609-org	rec-3812-org	rec-303-org	rec-2284-org	rec-1487-org	rec-1856-org	rec-3307-org	rec-227-org	rec-1143-org
> > soc_sec_id	7535316	9497788	6814956	5967384	3832742	7934773	5698873	8084076	4996142	5146525	...	2198108	8773092	7737396	5832887	3512831	9334580	4837378	5142242	3973395	6392122
> > gender	female	male	male	female	male	female	male	female	female	None	...	female	female	female	female	male	male	male	female	male	male
> > salary	136344	59079	39987	47962	39988	44961	39704	44741	114251	39980	...	45361	99122	46246	14036	23865	86300	40290	44813	41133	19004
> > ~~~
> >
> > Note: we could have done the above in a single line of code by 'chaining' the commands:
> > ~~~
> > data.T.tail(n=3)
> > ~~~
> >
>


> ## Writing Data
>
> As well as the `read_csv` function for reading data from a file,
> Pandas provides a `to_csv` function to write dataframes to files.
> Applying what you've learned about reading from files,
> write one of your dataframes to a file called `processed.csv`.
> You can use `help` to get information on how to use `to_csv`.
> > ## Solution
> > In order to write the DataFrame `data` to a file called `processed.csv`, execute the following command:
> > ~~~
> > data.to_csv('processed.csv')
> > ~~~
> >
> > For help on `to_csv`, you could execute, for example,
> > ~~~
> > help(data.to_csv)
> > ~~~
> >
> > Note that `help(to_csv)` throws an error! This is a subtlety and is due to the fact that `to_csv` is NOT a function in
> > and of itself and the actual call is `data.to_csv`.
> >
>
