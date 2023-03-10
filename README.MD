# Speedup up to 40 percent when sorting Pandas index/Series 


### MSVC C++ x64/x86 build tools must be installed.  


### This module uses [https://pypi.org/project/npfastsortcpp/](https://pypi.org/project/npfastsortcpp/)


### There you can get all instructions

## Important: Only for float/int

### Tested against Windows 10 / Python 3.9.13

```python
import pandas as pd
from a_pandas_ex_fastsort import pd_add_fastsort
pd_add_fastsort()
dafra = "https://github.com/pandas-dev/pandas/raw/main/doc/data/titanic.csv"
df5 = pd.read_csv(dafra)
```



```python
# Speed gain even for small DataFrames
df = pd.concat([df5.copy() for x in range(10)], ignore_index=True)
df = df.sample(len(df))
%timeit df.d_fast_reindex() # Values must be unique
846 µs ± 37.4 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
%timeit df.sort_index()
933 µs ± 25.7 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```



```python
# The bigger, the better
df = pd.concat([df5.copy() for x in range(100)], ignore_index=True)
df = df.sample(len(df))
%timeit df.d_fast_reindex() # Values must be unique
11.1 ms ± 131 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
%timeit df.sort_index()
15 ms ± 220 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
```



```python
df = pd.concat([df5.copy() for x in range(100)], ignore_index=True)
df = df.sample(len(df))
%timeit df.Pclass.sort_values()
2.08 ms ± 66 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
%timeit df.Pclass.s_fastsort_copy() # Be careful: original index will be dropped!
583 µs ± 5.85 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```



```python
# Be careful: 
df.Pclass.s_fastsort_inplace()
# sorts only one Series in place, 
# values in other columns are not being sorted! 

df # starting with:
Out[19]: 
       PassengerId  Survived  Pclass  ...      Fare Cabin  Embarked
34102          245         0       3  ...    7.2250   NaN         C
28329          709         1       1  ...  151.5500   NaN         S
50018          123         0       2  ...   30.0708   NaN         C
51258          472         0       3  ...    8.6625   NaN         S
51813          136         0       2  ...   15.0458   NaN         C
            ...       ...     ...  ...       ...   ...       ...
36357          718         1       2  ...   10.5000  E101         S
78608          201         0       3  ...    9.5000   NaN         S
64989          838         0       3  ...    8.0500   NaN         S
20824          332         0       1  ...   28.5000  C124         S
21108          616         1       2  ...   65.0000   NaN         S
[89100 rows x 12 columns]
df.Pclass.s_fastsort_inplace()

df # Result - Only Pclass has been sorted
Out[21]: 
       PassengerId  Survived  Pclass  ...      Fare Cabin  Embarked
34102          245         0       1  ...    7.2250   NaN         C
28329          709         1       1  ...  151.5500   NaN         S
50018          123         0       1  ...   30.0708   NaN         C
51258          472         0       1  ...    8.6625   NaN         S
51813          136         0       1  ...   15.0458   NaN         C
            ...       ...     ...  ...       ...   ...       ...
36357          718         1       3  ...   10.5000  E101         S
78608          201         0       3  ...    9.5000   NaN         S
64989          838         0       3  ...    8.0500   NaN         S
20824          332         0       3  ...   28.5000  C124         S
21108          616         1       3  ...   65.0000   NaN         S
[89100 rows x 12 columns]
```

