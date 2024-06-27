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

