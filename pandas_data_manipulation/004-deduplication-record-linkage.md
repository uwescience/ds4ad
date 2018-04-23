
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

### Blocking

This means the 

### 