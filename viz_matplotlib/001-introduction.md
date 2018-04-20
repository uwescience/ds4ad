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
plt.xlabel('Time (hr)')
plt.ylabel('Position (km)')
```
