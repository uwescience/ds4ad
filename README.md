# Data science for administrative datasets

This course is somewhat based on parts of the
[Software Carpentry](https://software-carpentry.org/) curriculum.

### Setup:

- Get a Github account at: https://github.com
- Get an Azure account at: https://notebooks.azure.com

### Day 1: Foundations

9 - 10: [Introduction](introduction/index.html) (Ariel)

10 - 12: Foundations of programming in Python (Jose)

- [Introduction](python_programming/001-introduction.md)
- [Loops](python_programming/002-loops.md)
- [Storing Values](python_programming/003-StoringValues.md)
- [Making Choices](python_programming/004-MakingChoices.md)
- [Functions](python_programming/005-functions.md)

Noon - 1 PM lunch

1 - 4 : git and GitHub (Bryna)

- [Intro to the shell](git_github/01-shell_intro.md)
- [Navigating the file system in the shell](git_github/02-shell_filedir.md)
- [Introducing version control](git_github/03-git_basics.md)
- [Configuring git](git_github/04-git_setup.md)
- [Setting up a repository](git_github/05-git_create.md)
- [Tracking changes](git_github/06-git_changes.md)
- [Exploring history](git_github/07-git_history.md)
- [Ignoring things](git_github/08-git_ignore.md)
- [Remotes on Github](git_github/09-github.md)
- [Collaborating on Github](git_github/10-git_collab.md)
- [Merge conflicts and conflict resolution](git_github/11-git_conflict.md)


#### Homework day 1:

**Set up your own GitHub project**

- Start a repository on GitHub
- In this repository, solve the following code challenges

**Code Challenges**  

- If the variable s refers to a string, then s[0] is the string’s first character and s[-1] is its last. Write a function called `outer` that returns a string made up of just the first and last characters of its input. A call to your function should look like this:

```
>>> print(outer('helium'))
```
```
hm
```

- "Adding” two strings produces their concatenation: 'a' + 'b' is 'ab'. Write a function called `fence` that takes two parameters called original and wrapper and returns a new string that has the wrapper character at the beginning and end of the original. A call to your function should look like this:

```
>>> print(fence('name', '*'))
```
```
*name*
```

**Bring a use-case**

Tomorrow, tell us about a data use-case that you have in mind for your work:

1. What is the data?
2. What are some questions you would like to answer with these data?
3. How is the data currently stored?

### Day 2: Data munging

9 - noon : Introducing Pandas (Bryna)

- [Using Libraries](pandas_intro/01-libraries.md)
- [Reading data from csv files](pandas_intro/02-reading-tabular.md)
- [Indexing in Pandas DataFrames](pandas_intro/03-data-frames.md)
- [Split-apply-combine with Pandas DataFrames](pandas_intro/04-split_apply_combine.md)

Noon - 1 PM: lunch

1 - 4 PM: [Manipulating data with Pandas](pandas_data_manipulation/README.md) (Ariel)

- [Introduction](pandas_data_manipulation/001-introduction.md)
- [Filtering](pandas_data_manipulation/002-filtering.md)
- [Combining data](pandas_data_manipulation/003-merging.md)
- [Record linkage](pandas_data_manipulation/004-deduplication-recrod-linkage.md)

#### Homework day 1:

**Set up your own GitHub project**


### Day 3: Data analysis and data visualization

9 - noon : Computations and statistics with Pandas DataFrames (Ariel)

- [Introduction](pandas_statistics/001-introduction.md)
- [Arithmetic](pandas_statistics/002-arithmetic.md)
- [Counting](pandas_statistics/003-counting.md)
- [Correlations](pandas_statistics/004-counting.md)
- [Modeling](pandas_statistics/005-statsmodels.md)


Noon - 1 PM: lunch

Afternoon:

1 PM - 2:30 Visualizing data with Matplotlib (Jose)

2:30 - 4 Next steps -- where do we go from here? (Bryna + Jose (+ Ariel))
