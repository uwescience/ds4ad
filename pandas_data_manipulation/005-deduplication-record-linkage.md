
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

