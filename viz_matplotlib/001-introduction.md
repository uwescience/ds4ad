# Plotting using `matplotlib` 
The Jupyter Notebook will render plots inline if we ask it to using a “magic” command.
```python
%matplotlib inline
import matplotlib.pyplot as plt
```
Simple plots are then (fairly) simple to create.
```python
time = [0, 1, 2, 3]
position = [0, 100, 200, 300]

plt.plot(time, position)
```
We can use `xlabel` and `ylabel` to add axis information. 
```python
plt.plot(time, position)
plt.xlabel('Time (hr)')
plt.ylabel('Position (km)')
```
We can use `xticks` and `yticks` to make changes to tick labels. 
```python
plt.plot(time, position)
plt.xticks(rotation = 90)
plt.xlabel('Time(hr)')
plt.ylabel('Position (km)')
```
## Plot data directly from a `Pandas dataframe`.
```python
import pandas

data = pandas.read_csv('')
data.plot() # will need a transpose
plt.ylabel('')
plt.xticks(rotation=90)
```
