
# Record linkage and deduplication 

One of the challenges in merging administrative datasets is that different datasets will often 
include records about the same entity (e.g., the same individual), but matching between these 
records and merging across disparate datasets, or even identifying linked records within the 
same dataset, can be challenging. This is because often the data may not include sufficient 
information to unambiguously identify individuals. Even when unambiguous data (i.e., uniquely 
idenfitying data, such as full name and date of birth) is recorded, often the records can 
contain small errors in the way in which the identity of the entity is stored, making 
automated linkage difficult.


## The `recordlinkage` Python library

This library includes implementations of functions that can hep us to identify and link between records that pertain to the same individual, even in the face of the errors that typically hamper these linkages. 

## How do we install external libraries? 

As it is, if you try to import the `recordlinkage` library, you will run into problems: 

```
import recordlinkage
```
```
---------------------------------------------------------------------------
ModuleNotFoundError                       Traceback (most recent call last)
<ipython-input-1-93ec48a7ca10> in <module>()
----> 1 import recordlinkage

ModuleNotFoundError: No module named 'recordlinkage'
```

This means that the module is not installed on this machine. 

To install external libraries, we can use the Python package manager `pip`. 

The `pip` package manager goes and finds the software in the [Python package index](https://pypi.org/), downloads it, figures it what all its dependencies are, installs them and then finally installs the software and makes it available. In Azure, we can do this as follows: 

```
!pip install recordlinkage
```

Now the `import` statement works!


## How to link records

The process of record linkage includes several steps: 

### Creating potential pairs

This means that we create suggested pairs that we will check. One way to do that is to create all-to-all pairings: 

```
df = pd.read_csv('https://raw.githubusercontent.com/uwescience/ds4ad/master/data/synthetic_data.csv')
indexer = recordlinkage.FullIndex()
pairs = indexer.index(df)
```

But this creates a very large number of pairs to compare: 
```
len(pairs)
```
```
12497500
```

So we would like to use some bit of information that we have to first create putative pairs. 
One way to do this is called **blocking**. This means that we create an index that "blocks on" one of the variables that we think would make a good initial guess for potential matches:

```
indexer = recordlinkage.BlockIndex(on='given_name')
pairs = indexer.index(df)
```

```
print (len(pairs))
```
48883

That's a much more manageable number 


### Comparing pairs: 

To compare pairs, we will create a `Compare` object:

```
compare_cl = recordlinkage.Compare()
```

We can add various properties to the `Compare` object , requiring various kinds of matches: 

```
compare_cl.exact('given_name', 'given_name', label='given_name')
compare_cl.string('surname', 'surname', method='jarowinkler', threshold=0.85, label='surname')
compare_cl.date('date_of_birth', 'date_of_birth', label='date_of_birth')
compare_cl.exact('suburb', 'suburb', label='suburb')
compare_cl.exact('state', 'state', label='state')
compare_cl.string('address_1', 'address_1', threshold=0.85, label='address_1')
```

From this we can calculate what we call `features`: 

```
features = compare_cl.compute(pairs, df)
```

This is a `DataFrame` with two indices. For example: 

```
features.loc[0, 13]
```
Refers to the potential matching of index `0` and index `13` from the original table (`df`). 

### Matching

To match between records in the `features` table, we need to somehow set a criterion for what 
we will call a match. For example, we can say that we require more than three matches in the 
criteria we defined above:

```
matches = features[features.sum(axis=1) > 3]
```

For example, this matching process thinks that index `113` and index `1231` are the same person. Let's see if this makes sense: 

```
df.loc[113]
```
```
rec_id              rec-2254-dup-2
given_name                   sarah
surname                       webb
street_number                   39
address_1            mcfarlanplace
address_2        t int-sec ngarden
suburb                coomaleidgup
postcode                      6023
state                          nsw
date_of_birth          1.99707e+07
soc_sec_id                 8244083
gender                      female
salary                       44817
Name: 113, dtype: object
```

```
df.loc[1231]
```
```
rec_id                rec-2254-org
given_name                   sarah
surname                       webb
street_number                   39
address_1           mcfarlan place
address_2        t int-sect garden
suburb                coomalbidgup
postcode                      6023
state                          nsw
date_of_birth          1.99707e+07
soc_sec_id                 8244083
gender                      female
salary                       44671
Name: 1231, dtype: object
```

This makes sense! 

## Exercise

Can you find problems with this linkage? What would you do improve the linkage? 

- Try blocking on other variables. 
- Try adjusting the matching thresholds.