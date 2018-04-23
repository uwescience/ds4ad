# Data manipulations with `Pandas`

> "In data science, 90% of the work is data cleaning, and 10% is complaining about data cleaning"
> 
> Evelina Gabasova (Turing Institute)

One of the main uses of Pandas is for data 'munging'. This refers to stages of data processing 
in which data  is manipulated, cleaned, filtered or extracted. Pandas makes data data 
transformations, such  as identifying erroneuos data, filtering the data based on erroneuous 
values, grouping the  data, applying computations to the data, and merging together disparate 
datasets relatively easy. 

In this lesson, we will learn about a few of the manipulations that you can perform with 
`Pandas` for this purpose.

## Data maturity 

But before we start in on the code, let's hear a bit about your use-cases: 

1. What are the data?
    - At what level of granularity are data available? 
    - What is the coverage?
    - What is the data quality? 
        - Are there missing data? 
        - What is the rate of errors?

2. What are some questions you would like to answer with these data?
    - Are the data sufficient to answer your questions?
    - Are they necessary? What other data could you use?
     
3. How is the data currently stored?
    - One file or multiple files? 
    - Do you have access to these files? 
    - What format are they stored in? 
    - How are the data documented? 

4. What are considerations you need to apply before analyzing? 
    - Data security: how is access to the data controlled?
    - What privacy policies apply to the data? 
    - Other ethical concerns: what are the implications of putting data together?