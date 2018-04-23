# Arithmetic with `DataFrame` and `Series`

As we saw before, each column in a `DataFrame` object is a `Series` object. 

We can do arithmetic between a `Series` object and a single number. For example: 

```
df['monthly_salary'] = df['salary'] / 12
```

If two series are the same size, we can do element-by-element arithmetic with them. 

For example:

df['monthly_salary']