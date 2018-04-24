# Arithmetic with `DataFrame` and `Series`

As we saw before, each column in a `DataFrame` object is a `Series` object.

We can do arithmetic between a `Series` object and a single number. For example:

```
monthly_salary = df['salary'] / 12
```

This creates another `Series` object, and because it has the same length as each one of the columns in the original `DataFrame`, we can put it into a new column: 

```
df['monthly_salary'] = monthly_salary
```

If two series are the same size, we can do element-by-element arithmetic with them.

For example:

```
monthly_taxes = df['monthly_salary'] * df['tax_rate']
```

This multiplies the monthly salary by the tax rate in each row and returns to you a new 
`Series` object. This one also has the same length as each one of the columns and can be 
assigned into a new column: 

```
df['monthly_taxes'] = monthly_taxes
```


## Exercise: 

- Calculate the post-tax annual income and make that into a new column. 
- How would you figure out what are the different tax rates in australia and what they boundaries are between these tax rates in terms of annual income?