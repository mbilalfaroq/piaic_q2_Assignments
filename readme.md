```python
import pandas as pd
import numpy as np
import glob
import matplotlib.pyplot as plt
```


```python
census_files = glob.glob("states*.csv")

file_list = []
for filename in census_files:
    data = pd.read_csv(filename)
    file_list.append(data)

census_files
```




    ['states0.csv',
     'states1.csv',
     'states2.csv',
     'states3.csv',
     'states4.csv',
     'states5.csv',
     'states6.csv',
     'states7.csv',
     'states8.csv',
     'states9.csv']




```python
us_census = pd.concat(file_list)
us_census.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>State</th>
      <th>TotalPop</th>
      <th>Hispanic</th>
      <th>White</th>
      <th>Black</th>
      <th>Native</th>
      <th>Asian</th>
      <th>Pacific</th>
      <th>Income</th>
      <th>GenderPop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Alabama</td>
      <td>4830620</td>
      <td>3.7516156462584975%</td>
      <td>61.878656462585%</td>
      <td>31.25297619047618%</td>
      <td>0.4532312925170065%</td>
      <td>1.0502551020408146%</td>
      <td>0.03435374149659865%</td>
      <td>$43296.35860306644</td>
      <td>2341093M_2489527F</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Alaska</td>
      <td>733375</td>
      <td>5.909580838323351%</td>
      <td>60.910179640718574%</td>
      <td>2.8485029940119775%</td>
      <td>16.39101796407186%</td>
      <td>5.450299401197604%</td>
      <td>1.0586826347305378%</td>
      <td>$70354.74390243902</td>
      <td>384160M_349215F</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Arizona</td>
      <td>6641928</td>
      <td>29.565921052631502%</td>
      <td>57.120000000000026%</td>
      <td>3.8509868421052658%</td>
      <td>4.35506578947368%</td>
      <td>2.876578947368419%</td>
      <td>0.16763157894736833%</td>
      <td>$54207.82095490716</td>
      <td>3299088M_3342840F</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Arkansas</td>
      <td>2958208</td>
      <td>6.215474452554738%</td>
      <td>71.13781021897813%</td>
      <td>18.968759124087573%</td>
      <td>0.5229197080291965%</td>
      <td>1.1423357664233578%</td>
      <td>0.14686131386861315%</td>
      <td>$41935.63396778917</td>
      <td>1451913M_1506295F</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>California</td>
      <td>38421464</td>
      <td>37.291874687968054%</td>
      <td>40.21578881677474%</td>
      <td>5.677396405391911%</td>
      <td>0.40529206190713685%</td>
      <td>13.052234148776776%</td>
      <td>0.35141038442336353%</td>
      <td>$67264.78230266465</td>
      <td>19087135M_19334329F</td>
    </tr>
  </tbody>
</table>
</div>




```python
us_census.columns
```




    Index(['Unnamed: 0', 'State', 'TotalPop', 'Hispanic', 'White', 'Black',
           'Native', 'Asian', 'Pacific', 'Income', 'GenderPop'],
          dtype='object')




```python
us_census.dtypes
```




    Unnamed: 0     int64
    State         object
    TotalPop       int64
    Hispanic      object
    White         object
    Black         object
    Native        object
    Asian         object
    Pacific       object
    Income        object
    GenderPop     object
    dtype: object




```python
us_census.Income = us_census.Income.replace('[\$]', '', regex=True)
us_census.Income.head()
```




    0    43296.35860306644
    1    70354.74390243902
    2    54207.82095490716
    3    41935.63396778917
    4    67264.78230266465
    Name: Income, dtype: object




```python
population_split = us_census.GenderPop.str.split("_")
population_split.head()
```




    0      [2341093M, 2489527F]
    1        [384160M, 349215F]
    2      [3299088M, 3342840F]
    3      [1451913M, 1506295F]
    4    [19087135M, 19334329F]
    Name: GenderPop, dtype: object




```python
us_census['male_population'] = population_split.str.get(0)
us_census['female_population'] = population_split.str.get(1)
us_census.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>State</th>
      <th>TotalPop</th>
      <th>Hispanic</th>
      <th>White</th>
      <th>Black</th>
      <th>Native</th>
      <th>Asian</th>
      <th>Pacific</th>
      <th>Income</th>
      <th>GenderPop</th>
      <th>male_population</th>
      <th>female_population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Alabama</td>
      <td>4830620</td>
      <td>3.7516156462584975%</td>
      <td>61.878656462585%</td>
      <td>31.25297619047618%</td>
      <td>0.4532312925170065%</td>
      <td>1.0502551020408146%</td>
      <td>0.03435374149659865%</td>
      <td>43296.35860306644</td>
      <td>2341093M_2489527F</td>
      <td>2341093M</td>
      <td>2489527F</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Alaska</td>
      <td>733375</td>
      <td>5.909580838323351%</td>
      <td>60.910179640718574%</td>
      <td>2.8485029940119775%</td>
      <td>16.39101796407186%</td>
      <td>5.450299401197604%</td>
      <td>1.0586826347305378%</td>
      <td>70354.74390243902</td>
      <td>384160M_349215F</td>
      <td>384160M</td>
      <td>349215F</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Arizona</td>
      <td>6641928</td>
      <td>29.565921052631502%</td>
      <td>57.120000000000026%</td>
      <td>3.8509868421052658%</td>
      <td>4.35506578947368%</td>
      <td>2.876578947368419%</td>
      <td>0.16763157894736833%</td>
      <td>54207.82095490716</td>
      <td>3299088M_3342840F</td>
      <td>3299088M</td>
      <td>3342840F</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Arkansas</td>
      <td>2958208</td>
      <td>6.215474452554738%</td>
      <td>71.13781021897813%</td>
      <td>18.968759124087573%</td>
      <td>0.5229197080291965%</td>
      <td>1.1423357664233578%</td>
      <td>0.14686131386861315%</td>
      <td>41935.63396778917</td>
      <td>1451913M_1506295F</td>
      <td>1451913M</td>
      <td>1506295F</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>California</td>
      <td>38421464</td>
      <td>37.291874687968054%</td>
      <td>40.21578881677474%</td>
      <td>5.677396405391911%</td>
      <td>0.40529206190713685%</td>
      <td>13.052234148776776%</td>
      <td>0.35141038442336353%</td>
      <td>67264.78230266465</td>
      <td>19087135M_19334329F</td>
      <td>19087135M</td>
      <td>19334329F</td>
    </tr>
  </tbody>
</table>
</div>




```python
pop_split = pd.DataFrame() 
pop_split['male_pop'] = us_census.male_population.str[0:-1]
pop_split['female_pop'] = us_census.female_population.str[0:-1]
pop_split.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>male_pop</th>
      <th>female_pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2341093</td>
      <td>2489527</td>
    </tr>
    <tr>
      <th>1</th>
      <td>384160</td>
      <td>349215</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3299088</td>
      <td>3342840</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1451913</td>
      <td>1506295</td>
    </tr>
    <tr>
      <th>4</th>
      <td>19087135</td>
      <td>19334329</td>
    </tr>
  </tbody>
</table>
</div>




```python
us_census = pd.concat([us_census, pop_split], axis = 1)
us_census.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>State</th>
      <th>TotalPop</th>
      <th>Hispanic</th>
      <th>White</th>
      <th>Black</th>
      <th>Native</th>
      <th>Asian</th>
      <th>Pacific</th>
      <th>Income</th>
      <th>GenderPop</th>
      <th>male_population</th>
      <th>female_population</th>
      <th>male_pop</th>
      <th>female_pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Alabama</td>
      <td>4830620</td>
      <td>3.7516156462584975%</td>
      <td>61.878656462585%</td>
      <td>31.25297619047618%</td>
      <td>0.4532312925170065%</td>
      <td>1.0502551020408146%</td>
      <td>0.03435374149659865%</td>
      <td>43296.35860306644</td>
      <td>2341093M_2489527F</td>
      <td>2341093M</td>
      <td>2489527F</td>
      <td>2341093</td>
      <td>2489527</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Alaska</td>
      <td>733375</td>
      <td>5.909580838323351%</td>
      <td>60.910179640718574%</td>
      <td>2.8485029940119775%</td>
      <td>16.39101796407186%</td>
      <td>5.450299401197604%</td>
      <td>1.0586826347305378%</td>
      <td>70354.74390243902</td>
      <td>384160M_349215F</td>
      <td>384160M</td>
      <td>349215F</td>
      <td>384160</td>
      <td>349215</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Arizona</td>
      <td>6641928</td>
      <td>29.565921052631502%</td>
      <td>57.120000000000026%</td>
      <td>3.8509868421052658%</td>
      <td>4.35506578947368%</td>
      <td>2.876578947368419%</td>
      <td>0.16763157894736833%</td>
      <td>54207.82095490716</td>
      <td>3299088M_3342840F</td>
      <td>3299088M</td>
      <td>3342840F</td>
      <td>3299088</td>
      <td>3342840</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Arkansas</td>
      <td>2958208</td>
      <td>6.215474452554738%</td>
      <td>71.13781021897813%</td>
      <td>18.968759124087573%</td>
      <td>0.5229197080291965%</td>
      <td>1.1423357664233578%</td>
      <td>0.14686131386861315%</td>
      <td>41935.63396778917</td>
      <td>1451913M_1506295F</td>
      <td>1451913M</td>
      <td>1506295F</td>
      <td>1451913</td>
      <td>1506295</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>California</td>
      <td>38421464</td>
      <td>37.291874687968054%</td>
      <td>40.21578881677474%</td>
      <td>5.677396405391911%</td>
      <td>0.40529206190713685%</td>
      <td>13.052234148776776%</td>
      <td>0.35141038442336353%</td>
      <td>67264.78230266465</td>
      <td>19087135M_19334329F</td>
      <td>19087135M</td>
      <td>19334329F</td>
      <td>19087135</td>
      <td>19334329</td>
    </tr>
  </tbody>
</table>
</div>




```python
us_census = us_census[['State', 'TotalPop', 'Hispanic', 'White', 'Black',
       'Native', 'Asian', 'Pacific', 'Income', 'male_pop', 'female_pop',]]
# Rename the male and female population columns
us_census.columns = ['State', 'TotalPop', 'Hispanic', 'White', 'Black', 'Native', 'Asian',
       'Pacific', 'Income', 'male_population', 'female_population']
us_census.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>TotalPop</th>
      <th>Hispanic</th>
      <th>White</th>
      <th>Black</th>
      <th>Native</th>
      <th>Asian</th>
      <th>Pacific</th>
      <th>Income</th>
      <th>male_population</th>
      <th>female_population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>4830620</td>
      <td>3.7516156462584975%</td>
      <td>61.878656462585%</td>
      <td>31.25297619047618%</td>
      <td>0.4532312925170065%</td>
      <td>1.0502551020408146%</td>
      <td>0.03435374149659865%</td>
      <td>43296.35860306644</td>
      <td>2341093</td>
      <td>2489527</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>733375</td>
      <td>5.909580838323351%</td>
      <td>60.910179640718574%</td>
      <td>2.8485029940119775%</td>
      <td>16.39101796407186%</td>
      <td>5.450299401197604%</td>
      <td>1.0586826347305378%</td>
      <td>70354.74390243902</td>
      <td>384160</td>
      <td>349215</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>6641928</td>
      <td>29.565921052631502%</td>
      <td>57.120000000000026%</td>
      <td>3.8509868421052658%</td>
      <td>4.35506578947368%</td>
      <td>2.876578947368419%</td>
      <td>0.16763157894736833%</td>
      <td>54207.82095490716</td>
      <td>3299088</td>
      <td>3342840</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>2958208</td>
      <td>6.215474452554738%</td>
      <td>71.13781021897813%</td>
      <td>18.968759124087573%</td>
      <td>0.5229197080291965%</td>
      <td>1.1423357664233578%</td>
      <td>0.14686131386861315%</td>
      <td>41935.63396778917</td>
      <td>1451913</td>
      <td>1506295</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>38421464</td>
      <td>37.291874687968054%</td>
      <td>40.21578881677474%</td>
      <td>5.677396405391911%</td>
      <td>0.40529206190713685%</td>
      <td>13.052234148776776%</td>
      <td>0.35141038442336353%</td>
      <td>67264.78230266465</td>
      <td>19087135</td>
      <td>19334329</td>
    </tr>
  </tbody>
</table>
</div>




```python
us_census.male_population = pd.to_numeric(us_census.male_population)
us_census.female_population = pd.to_numeric(us_census.female_population)
us_census.dtypes
```




    State                 object
    TotalPop               int64
    Hispanic              object
    White                 object
    Black                 object
    Native                object
    Asian                 object
    Pacific               object
    Income                object
    male_population        int64
    female_population    float64
    dtype: object




```python
plt.scatter(us_census.female_population, us_census.Income) 
plt.show()
```


    
![png](output_12_0.png)
    



```python
us_census.female_population.head(15)
```




    0     2489527.0
    1      349215.0
    2     3342840.0
    3     1506295.0
    4    19334329.0
    5     2630239.0
    0     2630239.0
    1     1841615.0
    2      478041.0
    3      340810.0
    4    10045763.0
    5     5123362.0
    0     5123362.0
    1      696428.0
    2      806083.0
    Name: female_population, dtype: float64




```python
fem_pop_nan = us_census.female_population.isnull()
fem_pop_nan.head(15)
```




    0    False
    1    False
    2    False
    3    False
    4    False
    5    False
    0    False
    1    False
    2    False
    3    False
    4    False
    5    False
    0    False
    1    False
    2    False
    Name: female_population, dtype: bool




```python
fem_pop_nan.value_counts()
```




    False    57
    True      3
    Name: female_population, dtype: int64




```python
example_nan = us_census[['TotalPop', 'male_population', 'female_population']]
example_nan.iloc[12]
```




    TotalPop             10006693.0
    male_population       4883331.0
    female_population     5123362.0
    Name: 0, dtype: float64




```python
nan_value = us_census.TotalPop - us_census.male_population
```


```python
us_census.female_population = us_census.female_population.fillna(value=nan_value)
example_nan_fixed = us_census[['TotalPop', 'male_population', 'female_population']]
example_nan_fixed.iloc[12]
```




    TotalPop             10006693.0
    male_population       4883331.0
    female_population     5123362.0
    Name: 0, dtype: float64




```python
us_census.female_population.head(15)
```




    0     2489527.0
    1      349215.0
    2     3342840.0
    3     1506295.0
    4    19334329.0
    5     2630239.0
    0     2630239.0
    1     1841615.0
    2      478041.0
    3      340810.0
    4    10045763.0
    5     5123362.0
    0     5123362.0
    1      696428.0
    2      806083.0
    Name: female_population, dtype: float64




```python
duplicates = us_census.duplicated()
duplicates.value_counts()
```




    False    51
    True      9
    dtype: int64




```python
us_census = us_census.drop_duplicates()
duplicates_check = us_census.duplicated()
duplicates_check.value_counts()
```




    False    51
    dtype: int64




```python
plt.scatter(us_census.female_population, us_census.Income) 
plt.show()
```


    
![png](output_22_0.png)
    



```python
us_census.columns
```




    Index(['State', 'TotalPop', 'Hispanic', 'White', 'Black', 'Native', 'Asian',
           'Pacific', 'Income', 'male_population', 'female_population'],
          dtype='object')




```python
us_census.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>TotalPop</th>
      <th>Hispanic</th>
      <th>White</th>
      <th>Black</th>
      <th>Native</th>
      <th>Asian</th>
      <th>Pacific</th>
      <th>Income</th>
      <th>male_population</th>
      <th>female_population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>4830620</td>
      <td>3.7516156462584975%</td>
      <td>61.878656462585%</td>
      <td>31.25297619047618%</td>
      <td>0.4532312925170065%</td>
      <td>1.0502551020408146%</td>
      <td>0.03435374149659865%</td>
      <td>43296.35860306644</td>
      <td>2341093</td>
      <td>2489527.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>733375</td>
      <td>5.909580838323351%</td>
      <td>60.910179640718574%</td>
      <td>2.8485029940119775%</td>
      <td>16.39101796407186%</td>
      <td>5.450299401197604%</td>
      <td>1.0586826347305378%</td>
      <td>70354.74390243902</td>
      <td>384160</td>
      <td>349215.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>6641928</td>
      <td>29.565921052631502%</td>
      <td>57.120000000000026%</td>
      <td>3.8509868421052658%</td>
      <td>4.35506578947368%</td>
      <td>2.876578947368419%</td>
      <td>0.16763157894736833%</td>
      <td>54207.82095490716</td>
      <td>3299088</td>
      <td>3342840.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>2958208</td>
      <td>6.215474452554738%</td>
      <td>71.13781021897813%</td>
      <td>18.968759124087573%</td>
      <td>0.5229197080291965%</td>
      <td>1.1423357664233578%</td>
      <td>0.14686131386861315%</td>
      <td>41935.63396778917</td>
      <td>1451913</td>
      <td>1506295.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>38421464</td>
      <td>37.291874687968054%</td>
      <td>40.21578881677474%</td>
      <td>5.677396405391911%</td>
      <td>0.40529206190713685%</td>
      <td>13.052234148776776%</td>
      <td>0.35141038442336353%</td>
      <td>67264.78230266465</td>
      <td>19087135</td>
      <td>19334329.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
us_census.Hispanic = us_census.Hispanic.replace('[\%]', '', regex=True)
us_census.White = us_census.White.replace('[\%]', '', regex=True)
us_census.Black = us_census.Black.replace('[\%]', '', regex=True)
us_census.Native = us_census.Native.replace('[\%]', '', regex=True)
us_census.Asian = us_census.Asian.replace('[\%]', '', regex=True)
us_census.Pacific = us_census.Pacific.replace('[\%]', '', regex=True)
us_census.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>TotalPop</th>
      <th>Hispanic</th>
      <th>White</th>
      <th>Black</th>
      <th>Native</th>
      <th>Asian</th>
      <th>Pacific</th>
      <th>Income</th>
      <th>male_population</th>
      <th>female_population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>4830620</td>
      <td>3.7516156462584975</td>
      <td>61.878656462585</td>
      <td>31.25297619047618</td>
      <td>0.4532312925170065</td>
      <td>1.0502551020408146</td>
      <td>0.03435374149659865</td>
      <td>43296.35860306644</td>
      <td>2341093</td>
      <td>2489527.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>733375</td>
      <td>5.909580838323351</td>
      <td>60.910179640718574</td>
      <td>2.8485029940119775</td>
      <td>16.39101796407186</td>
      <td>5.450299401197604</td>
      <td>1.0586826347305378</td>
      <td>70354.74390243902</td>
      <td>384160</td>
      <td>349215.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>6641928</td>
      <td>29.565921052631502</td>
      <td>57.120000000000026</td>
      <td>3.8509868421052658</td>
      <td>4.35506578947368</td>
      <td>2.876578947368419</td>
      <td>0.16763157894736833</td>
      <td>54207.82095490716</td>
      <td>3299088</td>
      <td>3342840.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>2958208</td>
      <td>6.215474452554738</td>
      <td>71.13781021897813</td>
      <td>18.968759124087573</td>
      <td>0.5229197080291965</td>
      <td>1.1423357664233578</td>
      <td>0.14686131386861315</td>
      <td>41935.63396778917</td>
      <td>1451913</td>
      <td>1506295.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>38421464</td>
      <td>37.291874687968054</td>
      <td>40.21578881677474</td>
      <td>5.677396405391911</td>
      <td>0.40529206190713685</td>
      <td>13.052234148776776</td>
      <td>0.35141038442336353</td>
      <td>67264.78230266465</td>
      <td>19087135</td>
      <td>19334329.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
us_census.Hispanic = pd.to_numeric(us_census.Hispanic)
us_census.White = pd.to_numeric(us_census.White)
us_census.Black = pd.to_numeric(us_census.Black)
us_census.Native = pd.to_numeric(us_census.Native)
us_census.Asian = pd.to_numeric(us_census.Asian)
us_census.Pacific = pd.to_numeric(us_census.Pacific)
us_census.dtypes
```




    State                 object
    TotalPop               int64
    Hispanic             float64
    White                float64
    Black                float64
    Native               float64
    Asian                float64
    Pacific              float64
    Income                object
    male_population        int64
    female_population    float64
    dtype: object




```python
Hnan = us_census.Hispanic.isnull()
Wnan = us_census.White.isnull()
Bnan = us_census.Black.isnull()
Nnan = us_census.Native.isnull()
Anan = us_census.Asian.isnull()
Pnan = us_census.Pacific.isnull()
```


```python
nan_value = us_census.Pacific.mean()
us_census.Pacific = us_census.Pacific.fillna(value=nan_value)
```


```python
us_census.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>TotalPop</th>
      <th>Hispanic</th>
      <th>White</th>
      <th>Black</th>
      <th>Native</th>
      <th>Asian</th>
      <th>Pacific</th>
      <th>Income</th>
      <th>male_population</th>
      <th>female_population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>4830620</td>
      <td>3.751616</td>
      <td>61.878656</td>
      <td>31.252976</td>
      <td>0.453231</td>
      <td>1.050255</td>
      <td>0.034354</td>
      <td>43296.35860306644</td>
      <td>2341093</td>
      <td>2489527.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alaska</td>
      <td>733375</td>
      <td>5.909581</td>
      <td>60.910180</td>
      <td>2.848503</td>
      <td>16.391018</td>
      <td>5.450299</td>
      <td>1.058683</td>
      <td>70354.74390243902</td>
      <td>384160</td>
      <td>349215.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Arizona</td>
      <td>6641928</td>
      <td>29.565921</td>
      <td>57.120000</td>
      <td>3.850987</td>
      <td>4.355066</td>
      <td>2.876579</td>
      <td>0.167632</td>
      <td>54207.82095490716</td>
      <td>3299088</td>
      <td>3342840.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arkansas</td>
      <td>2958208</td>
      <td>6.215474</td>
      <td>71.137810</td>
      <td>18.968759</td>
      <td>0.522920</td>
      <td>1.142336</td>
      <td>0.146861</td>
      <td>41935.63396778917</td>
      <td>1451913</td>
      <td>1506295.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>California</td>
      <td>38421464</td>
      <td>37.291875</td>
      <td>40.215789</td>
      <td>5.677396</td>
      <td>0.405292</td>
      <td>13.052234</td>
      <td>0.351410</td>
      <td>67264.78230266465</td>
      <td>19087135</td>
      <td>19334329.0</td>
    </tr>
  </tbody>
</table>
</div>




```python

plt.hist(us_census.Hispanic)
plt.xlabel('Percentage')
plt.ylabel('Frequency of occurance')
plt.show()
```


    
![png](output_30_0.png)
    



```python
plt.hist(us_census.White)
plt.xlabel('Percentage')
plt.ylabel('Frequency of occurance')
plt.show()
```


    
![png](output_31_0.png)
    



```python
plt.hist(us_census.Black)
plt.xlabel('Percentage')
plt.ylabel('Frequency of occurance')
plt.show()

```


    
![png](output_32_0.png)
    



```python
plt.hist(us_census.Native)
plt.xlabel('Percentage')
plt.ylabel('Frequency of occurance')
plt.show()

```


    
![png](output_33_0.png)
    



```python
plt.hist(us_census.Asian)
plt.xlabel('Percentage')
plt.ylabel('Frequency of occurance')
plt.show()

```


    
![png](output_34_0.png)
    



```python
plt.hist(us_census.Pacific)
plt.xlabel('Percentage')
plt.ylabel('Frequency of occurance')
plt.show()

```


    
![png](output_35_0.png)
    


Part 2


```python
inventory = pd.read_csv('inventory.csv')

```


```python
inventory.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>location</th>
      <th>product_type</th>
      <th>product_description</th>
      <th>quantity</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Staten Island</td>
      <td>seeds</td>
      <td>daisy</td>
      <td>4</td>
      <td>6.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Staten Island</td>
      <td>seeds</td>
      <td>calla lily</td>
      <td>46</td>
      <td>19.99</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Staten Island</td>
      <td>seeds</td>
      <td>tomato</td>
      <td>85</td>
      <td>13.99</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Staten Island</td>
      <td>garden tools</td>
      <td>rake</td>
      <td>4</td>
      <td>13.99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Staten Island</td>
      <td>garden tools</td>
      <td>wheelbarrow</td>
      <td>0</td>
      <td>89.99</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Staten Island</td>
      <td>garden tools</td>
      <td>spade</td>
      <td>93</td>
      <td>19.99</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Staten Island</td>
      <td>pest_control</td>
      <td>insect killer</td>
      <td>74</td>
      <td>12.99</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Staten Island</td>
      <td>pest_control</td>
      <td>weed killer</td>
      <td>8</td>
      <td>23.99</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Staten Island</td>
      <td>planter</td>
      <td>20 inch terracotta planter</td>
      <td>0</td>
      <td>17.99</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Staten Island</td>
      <td>planter</td>
      <td>8 inch plastic planter</td>
      <td>53</td>
      <td>3.99</td>
    </tr>
  </tbody>
</table>
</div>




```python
staten_island = inventory[0:10]
print(staten_island)
```

            location  product_type         product_description  quantity  price
    0  Staten Island         seeds                       daisy         4   6.99
    1  Staten Island         seeds                  calla lily        46  19.99
    2  Staten Island         seeds                      tomato        85  13.99
    3  Staten Island  garden tools                        rake         4  13.99
    4  Staten Island  garden tools                 wheelbarrow         0  89.99
    5  Staten Island  garden tools                       spade        93  19.99
    6  Staten Island  pest_control               insect killer        74  12.99
    7  Staten Island  pest_control                 weed killer         8  23.99
    8  Staten Island       planter  20 inch terracotta planter         0  17.99
    9  Staten Island       planter      8 inch plastic planter        53   3.99
    


```python
product_request = staten_island.product_description
print(product_request)
```

    0                         daisy
    1                    calla lily
    2                        tomato
    3                          rake
    4                   wheelbarrow
    5                         spade
    6                 insect killer
    7                   weed killer
    8    20 inch terracotta planter
    9        8 inch plastic planter
    Name: product_description, dtype: object
    


```python
seed_request = inventory[(inventory.location == 'Brooklyn') & (inventory.product_type == 'seeds')]
print(seed_request)
```

        location product_type product_description  quantity  price
    10  Brooklyn        seeds               daisy        50   6.99
    11  Brooklyn        seeds          calla lily         0  19.99
    12  Brooklyn        seeds              tomato         0  13.99
    


```python
inventory['in_stock'] = inventory.apply(lambda x: True if x['quantity'] > 0 else False, axis = 1)
print(inventory.head())

```

            location  product_type product_description  quantity  price  in_stock
    0  Staten Island         seeds               daisy         4   6.99      True
    1  Staten Island         seeds          calla lily        46  19.99      True
    2  Staten Island         seeds              tomato        85  13.99      True
    3  Staten Island  garden tools                rake         4  13.99      True
    4  Staten Island  garden tools         wheelbarrow         0  89.99     False
    


```python
inventory['total_value'] = inventory.apply(lambda x: x.price * x.quantity, axis = 1)
print(inventory.head())
```

            location  product_type product_description  quantity  price  in_stock  \
    0  Staten Island         seeds               daisy         4   6.99      True   
    1  Staten Island         seeds          calla lily        46  19.99      True   
    2  Staten Island         seeds              tomato        85  13.99      True   
    3  Staten Island  garden tools                rake         4  13.99      True   
    4  Staten Island  garden tools         wheelbarrow         0  89.99     False   
    
       total_value  
    0        27.96  
    1       919.54  
    2      1189.15  
    3        55.96  
    4         0.00  
    


```python
combine_lambda = lambda row: \
    '{} - {}'.format(row.product_type,
                     row.product_description)
inventory['full_discription'] = inventory.apply(lambda x: (x.product_type, x.product_description), axis = 1)
print(inventory.head())
```

            location  product_type product_description  quantity  price  in_stock  \
    0  Staten Island         seeds               daisy         4   6.99      True   
    1  Staten Island         seeds          calla lily        46  19.99      True   
    2  Staten Island         seeds              tomato        85  13.99      True   
    3  Staten Island  garden tools                rake         4  13.99      True   
    4  Staten Island  garden tools         wheelbarrow         0  89.99     False   
    
       total_value             full_discription  
    0        27.96               (seeds, daisy)  
    1       919.54          (seeds, calla lily)  
    2      1189.15              (seeds, tomato)  
    3        55.96         (garden tools, rake)  
    4         0.00  (garden tools, wheelbarrow)  
    


```python

```
