# Conclusions and Visuals
We will now use everthing we learned about this lesson to tackle the fuel economy dataset we cleaned and manipulated from last lesson. Use the space below to address questions on datasets `clean_08.csv` and `clean_18.csv`.
Answer the questions below:
  * Are more unique models using alternative fuels in 2018 compared to 2008? By how much?
  * How much have vehicle classes improved in fuel economy (increased in mpg)?
  * What are the characteristics of SmartWay vehicles? Have they changed over time? (mpg, greenhouse gas)
  * What features are associated with better fuel economy (mpg)?


```python
# inport pandas
import pandas as pd
```


```python
# load datasets
df_08 = pd.read_csv('clean_08.csv')
df_18 = pd.read_csv('clean_18.csv')
```


```python
df_08.head(1)
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
      <th>model</th>
      <th>displ</th>
      <th>cyl</th>
      <th>trans</th>
      <th>drive</th>
      <th>fuel</th>
      <th>veh_class</th>
      <th>air_pollution_score</th>
      <th>city_mpg</th>
      <th>hwy_mpg</th>
      <th>cmb_mpg</th>
      <th>greenhouse_gas_score</th>
      <th>smartway</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ACURA MDX</td>
      <td>3.7</td>
      <td>6</td>
      <td>Auto-S5</td>
      <td>4WD</td>
      <td>Gasoline</td>
      <td>SUV</td>
      <td>7.0</td>
      <td>15.0</td>
      <td>20.0</td>
      <td>17.0</td>
      <td>4</td>
      <td>no</td>
    </tr>
  </tbody>
</table>
</div>



### Q1: Are more unique models using alternative sources of fuel? By how much?

Let's first look at what the sources of fuel are and which ones are alternative sources.


```python
df_08.fuel.value_counts()
```




    Gasoline    984
    CNG           1
    ethanol       1
    gas           1
    Name: fuel, dtype: int64




```python
df_18.fuel.value_counts()
```




    Gasoline       749
    Ethanol         26
    Gas             26
    Diesel          19
    Electricity     12
    Name: fuel, dtype: int64



Looks like the alternative sources of fuel available in 2008 are CNG and ethanol, and those in 2018 ethanol and electricity. (You can use Google if you weren't sure which ones are alternative sources of fuel!)


```python
# how many unique models used alternative sources of fuel in 2008
alt_08 = df_08[(df_08['fuel'] == 'CNG') | (df_08['fuel'] == 'ethanol')]
unique_models_08 = alt_08['model'].nunique()
unique_models_08
```




    2




```python
# how many unique models used alternative sources of fuel in 2018
alt_18 = df_18[(df_18['fuel'] == 'Ethanol') | (df_18['fuel'] == 'Electricity')]
unique_models_18 = alt_18['model'].nunique()
unique_models_18
```




    26




```python
# Create a bar chart with both 2008 and 2018 alternative source numbers
pd.DataFrame(
    {"year": ["2008", "2018"], "model_num": [unique_models_08, unique_models_18]}
).plot(
    kind="bar",
    x="year",
    title="Number of Unique Models Using Alternative Fuels",
    legend=False,
    xlabel="Year",
    ylabel="Number of Unique Models",
    rot=0
);
```


    
![png](output_11_0.png)
    


Since 2008, the number of unique models using alternative sources of fuel increased by 24. We can also look at proportions.


```python
# total unique models each year
total_08 = df_08['model'].nunique()
total_18 = df_18['model'].nunique()
total_08, total_18
```




    (377, 357)




```python
# Find the proportion of alternative models by the total number of models for 2008 and 2018
prop_08 = unique_models_08 / total_08
prop_18 = unique_models_18 / total_18
prop_08, prop_18
```




    (0.005305039787798408, 0.07282913165266107)




```python
# Create a bar chart with both 2008 and 2018 proportion values
pd.DataFrame(
    {"year": ["2008", "2018"], "model_num": [prop_08, prop_18]}
).plot(
    kind="bar",
    x="year",
    title="Proportion of Unique Models Using Alternative Fuels",
    legend=False,
    xlabel="Year",
    ylabel="Proportion of Unique Models",
    rot=0
);
```


    
![png](output_15_0.png)
    

### Q2: How much have vehicle classes improved in fuel economy?  

Let's look at the average fuel economy for each vehicle class for both years.


```python
# group by veh_class and find the mean of cmb_mpg
veh_08 = df_08.groupby('veh_class').agg({'cmb_mpg':'mean'})
veh_08
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
      <th>cmb_mpg</th>
    </tr>
    <tr>
      <th>veh_class</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>SUV</th>
      <td>18.471429</td>
    </tr>
    <tr>
      <th>large car</th>
      <td>18.509091</td>
    </tr>
    <tr>
      <th>midsize car</th>
      <td>21.601449</td>
    </tr>
    <tr>
      <th>minivan</th>
      <td>19.117647</td>
    </tr>
    <tr>
      <th>pickup</th>
      <td>16.277108</td>
    </tr>
    <tr>
      <th>small car</th>
      <td>21.105105</td>
    </tr>
    <tr>
      <th>station wagon</th>
      <td>22.366667</td>
    </tr>
    <tr>
      <th>van</th>
      <td>14.952381</td>
    </tr>
  </tbody>
</table>
</div>




```python
veh_18 = df_18.groupby('veh_class').agg({'cmb_mpg':'mean'})
veh_18
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
      <th>cmb_mpg</th>
    </tr>
    <tr>
      <th>veh_class</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>large car</th>
      <td>23.409091</td>
    </tr>
    <tr>
      <th>midsize car</th>
      <td>27.884058</td>
    </tr>
    <tr>
      <th>minivan</th>
      <td>20.800000</td>
    </tr>
    <tr>
      <th>pickup</th>
      <td>18.589744</td>
    </tr>
    <tr>
      <th>small SUV</th>
      <td>24.074074</td>
    </tr>
    <tr>
      <th>small car</th>
      <td>25.421053</td>
    </tr>
    <tr>
      <th>special purpose</th>
      <td>18.500000</td>
    </tr>
    <tr>
      <th>standard SUV</th>
      <td>18.197674</td>
    </tr>
    <tr>
      <th>station wagon</th>
      <td>27.529412</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Find how much they've increased by for each vehicle class
# Take the difference of veh_18 and veh_08 to find the increase
inc = veh_18 - veh_08
inc
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
      <th>cmb_mpg</th>
    </tr>
    <tr>
      <th>veh_class</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>SUV</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>large car</th>
      <td>4.900000</td>
    </tr>
    <tr>
      <th>midsize car</th>
      <td>6.282609</td>
    </tr>
    <tr>
      <th>minivan</th>
      <td>1.682353</td>
    </tr>
    <tr>
      <th>pickup</th>
      <td>2.312635</td>
    </tr>
    <tr>
      <th>small SUV</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>small car</th>
      <td>4.315948</td>
    </tr>
    <tr>
      <th>special purpose</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>standard SUV</th>
      <td>NaN</td>
    </tr>
    <tr>
      <th>station wagon</th>
      <td>5.162745</td>
    </tr>
    <tr>
      <th>van</th>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Drop any NaN values to only plot the classes that exist in both years
inc.dropna(inplace=True)
# Create a bar chart to show the increase for vehicle types 
inc.plot(
    kind="bar",
    title="Improvements in Fuel Economy from 2008 to 2018 by Vehicle Class",
    legend=False,
    xlabel="Vehicle Class",
    ylabel="Increase in Average Combined MPG",
    rot=0,
    figsize=(8, 5)
);
```


    
![png](output_21_0.png)
    


### Q3: What are the characteristics of SmartWay vehicles? Have they changed over time?

We can analyze this by filtering each dataframe by SmartWay classification and exploring these datasets.


```python
# smartway labels for 2008
df_08.smartway.unique()
```




    array(['no', 'yes'], dtype=object)




```python
# get all smartway vehicles in 2008
smart_08 = df_08[df_08['smartway']=='yes']
smart_08
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
      <th>model</th>
      <th>displ</th>
      <th>cyl</th>
      <th>trans</th>
      <th>drive</th>
      <th>fuel</th>
      <th>veh_class</th>
      <th>air_pollution_score</th>
      <th>city_mpg</th>
      <th>hwy_mpg</th>
      <th>cmb_mpg</th>
      <th>greenhouse_gas_score</th>
      <th>smartway</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>ACURA TL</td>
      <td>3.2</td>
      <td>6</td>
      <td>Auto-S5</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>midsize car</td>
      <td>7.0</td>
      <td>18.0</td>
      <td>26.0</td>
      <td>21.0</td>
      <td>6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ACURA TL</td>
      <td>3.5</td>
      <td>6</td>
      <td>Auto-S5</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>midsize car</td>
      <td>7.0</td>
      <td>17.0</td>
      <td>26.0</td>
      <td>20.0</td>
      <td>6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>5</th>
      <td>ACURA TL</td>
      <td>3.5</td>
      <td>6</td>
      <td>Man-6</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>midsize car</td>
      <td>7.0</td>
      <td>18.0</td>
      <td>27.0</td>
      <td>21.0</td>
      <td>6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ACURA TSX</td>
      <td>2.4</td>
      <td>4</td>
      <td>Auto-S5</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>small car</td>
      <td>6.0</td>
      <td>20.0</td>
      <td>28.0</td>
      <td>23.0</td>
      <td>7</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>14</th>
      <td>AUDI A3</td>
      <td>2.0</td>
      <td>4</td>
      <td>Man-6</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>station wagon</td>
      <td>7.0</td>
      <td>21.0</td>
      <td>29.0</td>
      <td>24.0</td>
      <td>7</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>975</th>
      <td>VOLVO V50</td>
      <td>2.4</td>
      <td>5</td>
      <td>Man-5</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>station wagon</td>
      <td>7.0</td>
      <td>20.0</td>
      <td>28.0</td>
      <td>23.0</td>
      <td>7</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>976</th>
      <td>VOLVO V50</td>
      <td>2.5</td>
      <td>5</td>
      <td>Man-6</td>
      <td>4WD</td>
      <td>Gasoline</td>
      <td>station wagon</td>
      <td>7.0</td>
      <td>17.0</td>
      <td>25.0</td>
      <td>20.0</td>
      <td>6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>977</th>
      <td>VOLVO V50</td>
      <td>2.5</td>
      <td>5</td>
      <td>Auto-S5</td>
      <td>4WD</td>
      <td>Gasoline</td>
      <td>station wagon</td>
      <td>7.0</td>
      <td>18.0</td>
      <td>26.0</td>
      <td>21.0</td>
      <td>6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>978</th>
      <td>VOLVO V50</td>
      <td>2.5</td>
      <td>5</td>
      <td>Auto-S5</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>station wagon</td>
      <td>7.0</td>
      <td>19.0</td>
      <td>27.0</td>
      <td>22.0</td>
      <td>6</td>
      <td>yes</td>
    </tr>
    <tr>
      <th>979</th>
      <td>VOLVO V50</td>
      <td>2.5</td>
      <td>5</td>
      <td>Man-6</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>station wagon</td>
      <td>7.0</td>
      <td>19.0</td>
      <td>28.0</td>
      <td>23.0</td>
      <td>7</td>
      <td>yes</td>
    </tr>
  </tbody>
</table>
<p>380 rows × 13 columns</p>
</div>




```python
# explore smartway vehicles in 2008
smart_08.describe()
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
      <th>displ</th>
      <th>cyl</th>
      <th>air_pollution_score</th>
      <th>city_mpg</th>
      <th>hwy_mpg</th>
      <th>cmb_mpg</th>
      <th>greenhouse_gas_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>380.000000</td>
      <td>380.000000</td>
      <td>380.000000</td>
      <td>380.000000</td>
      <td>380.000000</td>
      <td>380.000000</td>
      <td>380.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.602895</td>
      <td>4.826316</td>
      <td>7.365789</td>
      <td>20.984211</td>
      <td>28.413158</td>
      <td>23.736842</td>
      <td>6.868421</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.623436</td>
      <td>1.002025</td>
      <td>1.148195</td>
      <td>3.442672</td>
      <td>3.075194</td>
      <td>3.060379</td>
      <td>0.827338</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.300000</td>
      <td>4.000000</td>
      <td>6.000000</td>
      <td>17.000000</td>
      <td>22.000000</td>
      <td>20.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.275000</td>
      <td>4.000000</td>
      <td>7.000000</td>
      <td>19.000000</td>
      <td>26.000000</td>
      <td>22.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.400000</td>
      <td>4.000000</td>
      <td>7.000000</td>
      <td>20.000000</td>
      <td>28.000000</td>
      <td>23.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.000000</td>
      <td>6.000000</td>
      <td>7.000000</td>
      <td>22.000000</td>
      <td>30.000000</td>
      <td>25.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.000000</td>
      <td>8.000000</td>
      <td>9.500000</td>
      <td>48.000000</td>
      <td>45.000000</td>
      <td>46.000000</td>
      <td>10.000000</td>
    </tr>
  </tbody>
</table>
</div>



Use what you've learned so for to further explore this dataset on 2008 smartway vehicles.


```python
# smartway labels for 2018
df_18.smartway.unique()
```




    array(['No', 'Yes', 'Elite'], dtype=object)




```python
# get all smartway vehicles in 2018
smart_18 = df_18[(df_18['smartway'] == 'Yes') | (df_18['smartway'] == 'Elite')]
smart_18
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
      <th>model</th>
      <th>displ</th>
      <th>cyl</th>
      <th>trans</th>
      <th>drive</th>
      <th>fuel</th>
      <th>veh_class</th>
      <th>air_pollution_score</th>
      <th>city_mpg</th>
      <th>hwy_mpg</th>
      <th>cmb_mpg</th>
      <th>greenhouse_gas_score</th>
      <th>smartway</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15</th>
      <td>AUDI A4 Ultra</td>
      <td>2.0</td>
      <td>4</td>
      <td>AMS-7</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>small car</td>
      <td>3.0</td>
      <td>27.0</td>
      <td>37.0</td>
      <td>31.0</td>
      <td>7</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>120</th>
      <td>BUICK Encore</td>
      <td>1.4</td>
      <td>4</td>
      <td>SemiAuto-6</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>small SUV</td>
      <td>3.0</td>
      <td>27.0</td>
      <td>33.0</td>
      <td>30.0</td>
      <td>7</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>168</th>
      <td>CHEVROLET Cruze</td>
      <td>1.4</td>
      <td>4</td>
      <td>Man-6</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>small car</td>
      <td>6.0</td>
      <td>27.0</td>
      <td>40.0</td>
      <td>32.0</td>
      <td>7</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>169</th>
      <td>CHEVROLET Cruze</td>
      <td>1.4</td>
      <td>4</td>
      <td>SemiAuto-6</td>
      <td>2WD</td>
      <td>Gasoline</td>
      <td>small car</td>
      <td>6.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>33.0</td>
      <td>8</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>170</th>
      <td>CHEVROLET Cruze</td>
      <td>1.6</td>
      <td>4</td>
      <td>Auto-9</td>
      <td>2WD</td>
      <td>Diesel</td>
      <td>small car</td>
      <td>3.0</td>
      <td>31.0</td>
      <td>47.0</td>
      <td>37.0</td>
      <td>7</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>812</th>
      <td>FORD Fusion Energi Plug-in Hybrid</td>
      <td>2.0</td>
      <td>4</td>
      <td>CVT</td>
      <td>2WD</td>
      <td>Electricity</td>
      <td>midsize car</td>
      <td>7.0</td>
      <td>102.0</td>
      <td>91.0</td>
      <td>97.0</td>
      <td>10</td>
      <td>Elite</td>
    </tr>
    <tr>
      <th>826</th>
      <td>MINI Cooper SE Countryman All4</td>
      <td>1.5</td>
      <td>3</td>
      <td>SemiAuto-6</td>
      <td>4WD</td>
      <td>Electricity</td>
      <td>midsize car</td>
      <td>3.0</td>
      <td>63.0</td>
      <td>66.0</td>
      <td>65.0</td>
      <td>9</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>829</th>
      <td>VOLVO S90</td>
      <td>2.0</td>
      <td>4</td>
      <td>SemiAuto-8</td>
      <td>4WD</td>
      <td>Electricity</td>
      <td>midsize car</td>
      <td>7.0</td>
      <td>70.0</td>
      <td>72.0</td>
      <td>71.0</td>
      <td>10</td>
      <td>Elite</td>
    </tr>
    <tr>
      <th>830</th>
      <td>VOLVO XC 60</td>
      <td>2.0</td>
      <td>4</td>
      <td>SemiAuto-8</td>
      <td>4WD</td>
      <td>Electricity</td>
      <td>small SUV</td>
      <td>7.0</td>
      <td>60.0</td>
      <td>58.0</td>
      <td>59.0</td>
      <td>10</td>
      <td>Elite</td>
    </tr>
    <tr>
      <th>831</th>
      <td>VOLVO XC 90</td>
      <td>2.0</td>
      <td>4</td>
      <td>SemiAuto-8</td>
      <td>4WD</td>
      <td>Electricity</td>
      <td>standard SUV</td>
      <td>7.0</td>
      <td>63.0</td>
      <td>61.0</td>
      <td>62.0</td>
      <td>10</td>
      <td>Elite</td>
    </tr>
  </tbody>
</table>
<p>108 rows × 13 columns</p>
</div>




```python
smart_18.describe()
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
      <th>displ</th>
      <th>cyl</th>
      <th>air_pollution_score</th>
      <th>city_mpg</th>
      <th>hwy_mpg</th>
      <th>cmb_mpg</th>
      <th>greenhouse_gas_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.787963</td>
      <td>3.935185</td>
      <td>5.212963</td>
      <td>34.907407</td>
      <td>41.472222</td>
      <td>37.361111</td>
      <td>7.925926</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.408031</td>
      <td>0.416329</td>
      <td>1.798498</td>
      <td>16.431982</td>
      <td>13.095236</td>
      <td>14.848429</td>
      <td>1.197378</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.200000</td>
      <td>3.000000</td>
      <td>3.000000</td>
      <td>25.000000</td>
      <td>27.000000</td>
      <td>26.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.500000</td>
      <td>4.000000</td>
      <td>3.000000</td>
      <td>28.000000</td>
      <td>36.000000</td>
      <td>31.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.700000</td>
      <td>4.000000</td>
      <td>5.500000</td>
      <td>28.500000</td>
      <td>37.000000</td>
      <td>32.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.000000</td>
      <td>4.000000</td>
      <td>7.000000</td>
      <td>31.250000</td>
      <td>40.250000</td>
      <td>35.000000</td>
      <td>9.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>3.500000</td>
      <td>6.000000</td>
      <td>7.000000</td>
      <td>113.000000</td>
      <td>99.000000</td>
      <td>106.000000</td>
      <td>10.000000</td>
    </tr>
  </tbody>
</table>
</div>



Use what you've learned so for to further explore this dataset on 2018 smartway vehicles.


```python
# Create a bar chart to find average cmb_mpg of smartway vehicles
pd.DataFrame(
    {"year": ["2008", "2018"], "model_num": [smart_08["cmb_mpg"].mean(), smart_18["cmb_mpg"].mean()]}
).plot(
    kind="bar",
    x="year",
    title="Average Combined MPG from Smartway Vehicles from 2008 and 2018",
    legend=False,
    xlabel="Year",
    ylabel="Avg cmb_mpg",
    rot=0
);
```


    
![png](output_32_0.png)
    


### Q4: What features are associated with better fuel economy?

You can explore trends between cmb_mpg and the other features in this dataset, or filter this dataset like in the previous question and explore the properties of that dataset. For example, you can select all vehicles that have the top 50% fuel economy ratings like this.


```python
# Create the top 50% by finding cmb_mpg >= the average cmb_mpg
# Create the bottom 50% by finding cmb_mpg < the average cmb_mpg
top_08 = df_08[(df_08['cmb_mpg'])>=(df_08['cmb_mpg'].mean())]
bottom_08 = df_08[(df_08['cmb_mpg'])<(df_08['cmb_mpg'].mean())]
top_08.describe()
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
      <th>displ</th>
      <th>cyl</th>
      <th>air_pollution_score</th>
      <th>city_mpg</th>
      <th>hwy_mpg</th>
      <th>cmb_mpg</th>
      <th>greenhouse_gas_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>519.000000</td>
      <td>519.000000</td>
      <td>519.000000</td>
      <td>519.000000</td>
      <td>519.000000</td>
      <td>519.000000</td>
      <td>519.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.667823</td>
      <td>4.890173</td>
      <td>6.998073</td>
      <td>20.317919</td>
      <td>27.603083</td>
      <td>22.992293</td>
      <td>6.639692</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.665551</td>
      <td>1.034856</td>
      <td>1.159565</td>
      <td>3.198257</td>
      <td>3.051120</td>
      <td>2.926371</td>
      <td>0.804935</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.300000</td>
      <td>4.000000</td>
      <td>4.000000</td>
      <td>17.000000</td>
      <td>20.000000</td>
      <td>20.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.300000</td>
      <td>4.000000</td>
      <td>6.000000</td>
      <td>18.000000</td>
      <td>25.000000</td>
      <td>21.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.500000</td>
      <td>4.000000</td>
      <td>7.000000</td>
      <td>20.000000</td>
      <td>27.000000</td>
      <td>22.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.000000</td>
      <td>6.000000</td>
      <td>7.000000</td>
      <td>21.000000</td>
      <td>29.000000</td>
      <td>24.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>6.000000</td>
      <td>8.000000</td>
      <td>9.500000</td>
      <td>48.000000</td>
      <td>45.000000</td>
      <td>46.000000</td>
      <td>10.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create the top 50% by finding cmb_mpg >= the average cmb_mpg
# Create the bottom 50% by finding cmb_mpg < the average cmb_mpg
top_18 = df_18[(df_18['cmb_mpg'])>=(df_18['cmb_mpg'].mean())]
bottom_18 = df_18[(df_18['cmb_mpg'])<(df_18['cmb_mpg'].mean())]
top_18.describe()
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
      <th>displ</th>
      <th>cyl</th>
      <th>air_pollution_score</th>
      <th>city_mpg</th>
      <th>hwy_mpg</th>
      <th>cmb_mpg</th>
      <th>greenhouse_gas_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>328.000000</td>
      <td>328.000000</td>
      <td>328.000000</td>
      <td>328.000000</td>
      <td>328.000000</td>
      <td>328.000000</td>
      <td>328.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.964329</td>
      <td>4.021341</td>
      <td>4.856707</td>
      <td>27.472561</td>
      <td>35.304878</td>
      <td>30.411585</td>
      <td>6.329268</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.398593</td>
      <td>0.465477</td>
      <td>1.860802</td>
      <td>11.033692</td>
      <td>9.024857</td>
      <td>10.081539</td>
      <td>1.410358</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.200000</td>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>21.000000</td>
      <td>27.000000</td>
      <td>25.000000</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.600000</td>
      <td>4.000000</td>
      <td>3.000000</td>
      <td>23.000000</td>
      <td>31.000000</td>
      <td>26.000000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.000000</td>
      <td>4.000000</td>
      <td>5.000000</td>
      <td>25.000000</td>
      <td>33.000000</td>
      <td>28.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.000000</td>
      <td>4.000000</td>
      <td>7.000000</td>
      <td>28.000000</td>
      <td>36.000000</td>
      <td>31.000000</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>3.500000</td>
      <td>6.000000</td>
      <td>7.000000</td>
      <td>113.000000</td>
      <td>99.000000</td>
      <td>106.000000</td>
      <td>10.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plot a bar chart showing top and bottom metrics for 2008 and 2018
# Make sure to use numeric_only=True when geting the mean as we only
# want features that are numbers
pd.DataFrame(
    {
        "Bottom 2008": bottom_08.mean(numeric_only=True),
        "Bottom 2018": bottom_18.mean(numeric_only=True),
        "Top 2008": top_08.mean(numeric_only=True),
        "Top 2018": top_18.mean(numeric_only=True),
    },
    index=top_18.mean(numeric_only=True).index
).plot(
    kind="bar",
    title="Average Vehicle Features Separated by 50% Above or Below cmb_mpg",
    legend=True,
    xlabel="Feature",
    ylabel="Average value of feature",
    rot=0,
    figsize=(12, 8)
);
```


    
![png](output_37_0.png)
    



```python

```
