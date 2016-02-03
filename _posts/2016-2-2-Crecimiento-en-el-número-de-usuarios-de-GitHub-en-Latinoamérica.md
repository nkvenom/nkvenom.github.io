

```python
import pandas as pd
import json
from datetime import datetime
from datetime import date
from ggplot import *
```


```python
%matplotlib inline
```


```python
df = pd.read_csv('data/github-latam.csv', index_col='month', parse_dates=True)
```

### Normalizar los nombres 


```python
df['peru'] = df['perú'] + df['peru']
del df['perú']
```


```python
df['mexico'] = df['méxico'] + df['mexico']
del df['méxico']
```


```python
df['panama'] = df['panamá'] + df['panama']
del df['panamá']
```


```python
df['brasil'] = df['brazil'] + df['brasil']
del df['brazil']
```


```python
# Countries sum
countries_sum = df.sum(axis=1)
countries_sum.head()
```




    month
    2008-02-01     28
    2008-03-01     44
    2008-04-01    151
    2008-05-01     74
    2008-06-01     84
    dtype: int64




```python
countries_df = pd.DataFrame(countries_sum)
countries_df.columns = ['users']
countries_df['date'] = countries_df.index
countries_df.date.ix[0]
```




    Timestamp('2008-02-01 00:00:00')




```python
#Remove january data for the moment
countries_df = countries_df[:'2015-12-01']
```


```python
ggplot(aes(x='date', y='users'), data=countries_df) +\
    geom_line() +\
    stat_smooth(colour='blue', span=0.2)
```

    /home/n/miniconda3/lib/python3.5/site-packages/matplotlib/__init__.py:872: UserWarning: axes.color_cycle is deprecated and replaced with axes.prop_cycle; please use the latter.
      warnings.warn(self.msg_depr % (key, alt_key))
    /home/n/miniconda3/lib/python3.5/site-packages/ggplot/stats/stat_smooth.py:22: FutureWarning: sort(columns=....) is deprecated, use sort_values(by=.....)
      data = data.sort(['x'])



![png](/images/output_11_1.png)





    <ggplot: (-9223363261274862027)>



Al parecer alcanzo su maximo en 2015


```python
# Split and compare by years
countries_df['year'] = countries_df.date.dt.year
countries_df['month'] = countries_df.date.dt.month

# countries_df.loc[countries_df.year == 2009, ['month', 'users']]
def extract_year(year):
    df_ = countries_df.loc[countries_df.year == year, ['month', 'users']]
    df_.columns = ['month', str(year)]
    return df_.set_index('month')

extract_year(2009)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2009</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>125</td>
    </tr>
    <tr>
      <th>2</th>
      <td>168</td>
    </tr>
    <tr>
      <th>3</th>
      <td>151</td>
    </tr>
    <tr>
      <th>4</th>
      <td>163</td>
    </tr>
    <tr>
      <th>5</th>
      <td>184</td>
    </tr>
    <tr>
      <th>6</th>
      <td>158</td>
    </tr>
    <tr>
      <th>7</th>
      <td>187</td>
    </tr>
    <tr>
      <th>8</th>
      <td>186</td>
    </tr>
    <tr>
      <th>9</th>
      <td>203</td>
    </tr>
    <tr>
      <th>10</th>
      <td>228</td>
    </tr>
    <tr>
      <th>11</th>
      <td>192</td>
    </tr>
    <tr>
      <th>12</th>
      <td>179</td>
    </tr>
  </tbody>
</table>
</div>




```python
users_by_year = pd.DataFrame()
for y in countries_df.year.unique():
    users_by_year[str(y)] = extract_year(y)
users_by_year = users_by_year.ix[1:]   
```


```python
users_by_year['2015'].plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fb3946f5518>




![png](/images/output_15_1.png)



```python
users_usa = pd.read_csv('data/users-usa.csv', index_col='month', parse_dates=True)
```


```python
### El número de usuarios creados en USA también estuvo en descenso en 2015
```


```python
users_usa['2015-01-01':'2015-12-01'].plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fb3945246a0>




![png](/images/output_18_1.png)



```python
df2015 = df['2015-01-01':'2015-12-01']
```


```python
df2015[['uruguay', 'colombia', 'cuba', 'chile']].plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fb365525be0>




![png](/images/output_20_1.png)

