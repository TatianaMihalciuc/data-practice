# Concatenating Data
### 1. First, import the necessary packages and load clean_08.csv and clean_18.csv
```
import pandas as pd
df_08 = pd.read_csv('clean_08.csv')
df_18 = pd.read_csv('clean_18.csv')
```
### 2. Create Year Columns
```
df_08['year'] = '2008'
df_18['year']='2018'
```
### 3. Combine DataFrames with Concatenate
``` 
df = pd.concat([df_08,df_18])
df.head(1)
df.tail(1)
```
### 4. Save Combined Dataset
```
df.to_csv('clean_08_18_concatenated.csv',index=False)
```

# Merging Datasets
### 1. Load datasets
```
import pandas as pd
df_08 = pd.read_csv('clean_08.csv')
df_18 = pd.read_csv('clean_18.csv')
```
### 2. Rename 2008 columns
```
df_08 = df_08.rename(columns=lambda x: x[:10] + "_2008")
```
### 3. View to check names
```
df_08.head()
```
### 4. Merge datasets
```
df_combined = pd.merge(df_08, df_18, left_on='model_2008', right_on='model')
```
### 5. View to check merge
```
df_combined.head()
```
### 6. Save the combined dataset
```
df_combined.to_csv('combined_dataset.csv', index=False)
```

## Question: For all of the models that were produced in 2008 that are still being produced now, how much has the mpg improved and which vehicle improved the most?
### 1. Create a new dataframe, model_mpg, that contains the mean combined mpg values in 2008 and 2018 for each unique model
```
import pandas as pd
df =pd.read_csv('combined_dataset.csv')
model_mpg = df.groupby('model').agg({
    'cmb_mpg_2008': 'mean',
    'cmb_mpg': 'mean'
}).reset_index()
model_mpg.head()
```
### 2. Create a new column, mpg_change, with the change in mpg
```
model_mpg['mpg_change'] = model_mpg['cmb_mpg'] - model_mpg['cmb_mpg_2008']
model_mpg.head()
```
### 3. Find the vehicle that improved the most
#### Option 1:
```
max_change = model_mpg['mpg_change'].max()
model_mpg[model_mpg['mpg_change'] == max_change]
```
#### Option 2:
```
idx = model_mpg['mpg_change'].idxmax()
model_mpg.loc[idx]
```
### Answer for question 3: VOLVO XC 90

## Drawing Conclusions Quiz
### 1. Which store has the highest total sales for the final month of data?
### 2. Which store makes the most sales on average?
### 3. Which store sells the most during the week of March 13th, 2016?
### 4. In what week does store C make its worst sales?
### 5. Which store has the most sales in the latest 3-month period?

```
import pandas as pd

df = pd.read_csv('store_data.csv')
df.head()
df.hist(figsize=(8,8))
df.tail()
```
### 1. Use iloc to create a slice of the last month and sum up the weeks to find the total sales for the last month
```
df_last_month=df.iloc[196:].sum()
df_last_month.head()
```
#### Answer: storeA

### 2. Use the mean method to find the average sales for each store
```
df.mean()
```
#### Answer: StoreB

### 3. Find the sales of all stores by filtering on the week of march 13, 2016
```
df[df['week']=='2016-03-13']
```
#### Answer: storeD

### 4. Use the min method to filter the dataset to find the worst week for store C
```
print("Minimum value in 'storeC':", df['storeC'].min())
min_value = df['storeC'].min()
filtered_df = df[df['storeC'] == min_value]
print( filtered_df)
```
#### Answer: 2014-07-06

### 5. Filter the DataFrame on the most recent 3 month period. You can filter by selecting greater than or equal to 2017-12-01. Find the total sales during this 3 month 
```
last_three_months = df[df['week']>='2017-12-01']
last_three_months.iloc[:, 1:].sum()
```
#### Answer: storeA

## Conclusions and Visuals

### 2. How much have vehicle classes improved in fuel economy (increased in mpg)?
### 3. What are the characteristics of SmartWay vehicles? Have they changed over time? (mpg, greenhouse gas)
### 4. What features are associated with better fuel economy (mpg)?
```
import pandas as pd
df_08 = pd.read_csv('clean_08.csv')
df_18 = pd.read_csv('clean_18.csv')
df_08.head(1)
```
### 1.  Are more unique models using alternative fuels in 2018 compared to 2008? By how much?
```
df_08.fuel.value_counts()
df_18.fuel.value_counts()
```
#### 1. how many unique models used alternative sources of fuel in 2008
```
alt_08 = df_08[(df_08['fuel'] == 'CNG') | (df_08['fuel'] == 'ethanol')]
unique_models_08 = alt_08['model'].nunique()
unique_models_08
```
#### 2. how many unique models used alternative sources of fuel in 2018
```
alt_18 = df_18[(df_18['fuel'] == 'Ethanol') | (df_18['fuel'] == 'Electricity')]
unique_models_18 = alt_18['model'].nunique()
unique_models_18
```
#### 3. Create a bar chart with both 2008 and 2018 alternative source numbers
```
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
#### 4. total unique models each year
```
total_08 = df_08['model'].nunique()
total_18 = df_18['model'].nunique()
total_08, total_18
```
#### 5. Find the proportion of alternative models by the total number of models for 2008 and 2018
```
prop_08 = unique_models_08 / total_08
prop_18 = unique_models_18 / total_18
prop_08, prop_18
```
#### 6. Create a bar chart with both 2008 and 2018 proportion values
```
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
### Answer 1: 24
### 2.  How much have vehicle classes improved in fuel economy?
#### 1. group by veh_class and find the mean of cmb_mpg
```
veh_08 = df_08.groupby('veh_class').agg({'cmb_mpg':'mean'})
veh_08
veh_18 = df_18.groupby('veh_class').agg({'cmb_mpg':'mean'})
veh_18
```
#### 2.Find how much they've increased by for each vehicle class. Take the difference of veh_18 and veh_08 to find the increase
```
inc = veh_18 - veh_08
inc
```
#### 3.  Drop any NaN values to only plot the classes that exist in both years. Create a bar chart to show the increase for vehicle types
```
inc.dropna(inplace=True)
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
### Answer 2: 6.28
### 3. What are the characteristics of SmartWay vehicles? Have they changed over time?

#### 1. smartway labels for 2008
```
df_08.smartway.unique()
```
#### 2. get all smartway vehicles in 2008
```
smart_08 = df_08[df_08['smartway']=='yes']
smart_08
```
#### 3. explore smartway vehicles in 2008
```
smart_08.describe()
```
#### 4. smartway labels for 2018
```
df_18.smartway.unique()
```
#### 5. get all smartway vehicles in 2018
```
smart_18 = df_18[(df_18['smartway'] == 'Yes') | (df_18['smartway'] == 'Elite')]
smart_18
smart_18.describe()
```
#### 6. Create a bar chart to find average cmb_mpg of smartway vehicles
```
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
### Answer 3: MPG increased from 2008 to 2018
### 4. What features are associated with better fuel economy?
### 1. Create the top 50% by finding cmb_mpg >= the average cmb_mpg. Create the bottom 50% by finding cmb_mpg < the average cmb_mpg
```
top_08 = df_08[(df_08['cmb_mpg'])>=(df_08['cmb_mpg'].mean())]
bottom_08 = df_08[(df_08['cmb_mpg'])<(df_08['cmb_mpg'].mean())]
top_08.describe()
```
### 2. Create the top 50% by finding cmb_mpg >= the average cmb_mpg. Create the bottom 50% by finding cmb_mpg < the average cmb_mpg
```
top_18 = df_18[(df_18['cmb_mpg'])>=(df_18['cmb_mpg'].mean())]
bottom_18 = df_18[(df_18['cmb_mpg'])<(df_18['cmb_mpg'].mean())]
top_18.describe()
```
### 3. Plot a bar chart showing top and bottom metrics for 2008 and 2018. Make sure to use numeric_only=True when geting the mean as we only. Want features that are numbers
```
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
### Answer 4: displ and cyl






