# Computing correlations and dependencies

One the things that we often want to know is the dependency between different variables in our 
data. One of the statistics that report this to us is the correlation between the variables. These are quantified as a number between -1 and 1 (the correlation coefficient). 

```
df.corr()
```

Gives you a table with the correlations between the variables in this table. 

The main diagonal is all 1's, because each variable is perfectly correlated with itself. 

Some other correlations here don't mean much (like correlation between the social security number and the salary), but some of these correlations actually make sense. For example, as expected the tax rate and the salary are correlated. 


