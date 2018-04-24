# Statistical modeling

In tandem with another library, [`statsmodels`](http://www.statsmodels.org), `Pandas` is also 
set to create and test explicit models of the data. 

For example, a large family of models are linear models of the form: 

    y = x_0 + x_1 * b_1 + x_2 * b_2 + ... + x_n * b_n + e

Also sometimes written as: 

    y ~ Xb 

This means that some outcome variable (**y**, also sometimes called *exogenous* data) is a 
**linear combination** of a series of variables (**X**, also sometimes called *endogenous*)

A relatively simple way to fit this model is using ordinary least squares, or *OLS*: 

```
import statsmodels.formula.api as sm
formula = "salary ~ gender"
model = sm.ols(formula=formula, data=df)
result = model.fit()
```

This fits the model and gives you a model fitting `result` object. This object has a method that summarizes these results in readable format:  

```
result.summary()
```

This produces two tables. The top one summarizes the model fitting procedure, including an overall F statistic and p value for this model. 

The bottom table breaks this down into the model fit coefficients for the different values of these (qualitative) parameters. This gives you an indication of the effect of each value in the table. Notice that there is something weird about this result. It seems to indicate that both "female" and "male" have some parameter value, relative to ... something. What does that mean? What other values are there here? 

It turns out that several records have no gender recorded! So, we need to clean our data and rerun: 

```
import statsmodels.formula.api as sm
formula = "salary ~ gender"
model = sm.ols(formula=formula, data=df[(df["gender"]=="male") | (df["gender"]=="female")])
result = model.fit()
```

This makes more sense! Now, there is only one coefficient for gender, comparing directly the two defined categories that we have in our data. 

We can expand the model, by adding another variable: 

```
import statsmodels.formula.api as sm
formula = "salary ~ gender + state"
model = sm.ols(formula=formula, data=df[(df["gender"]=="male") | (df["gender"]=="female")])
result = model.fit()
```

This model is also statistically significant. How do we know whether it is a better model? One way is to use the AIC/BIC criteria that are described in the summary. 

We will not go into much detail about selecting models using AIC/BIC, but generally speaking, the smaller these values are, the better the model. In this case, if we had to choose between them, we might favor the model that only includes gender.

