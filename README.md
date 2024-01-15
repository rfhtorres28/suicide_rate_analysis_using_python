---
jupyter:
  kernelspec:
    display_name: base
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.11.7
  nbformat: 4
  nbformat_minor: 2
---

::: {.cell .code execution_count="18"}
``` python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```
:::

::: {.cell .markdown}
### 1. Data Collection {#1-data-collection}
:::

::: {.cell .code execution_count="63"}
``` python
stats = pd.read_csv(r"c:\Users\bobby\Desktop\Python\Projects_DataSets\Project-3\master.csv")
stats.head()
```

::: {.output .execute_result execution_count="63"}
```{=html}
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
      <th>country</th>
      <th>year</th>
      <th>sex</th>
      <th>age</th>
      <th>suicides_no</th>
      <th>population</th>
      <th>suicides/100k pop</th>
      <th>country-year</th>
      <th>HDI for year</th>
      <th>gdp_for_year ($)</th>
      <th>gdp_per_capita ($)</th>
      <th>generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Albania</td>
      <td>1987</td>
      <td>male</td>
      <td>15-24 years</td>
      <td>21</td>
      <td>312900</td>
      <td>6.71</td>
      <td>Albania1987</td>
      <td>NaN</td>
      <td>2,156,624,900</td>
      <td>796</td>
      <td>Generation X</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>1987</td>
      <td>male</td>
      <td>35-54 years</td>
      <td>16</td>
      <td>308000</td>
      <td>5.19</td>
      <td>Albania1987</td>
      <td>NaN</td>
      <td>2,156,624,900</td>
      <td>796</td>
      <td>Silent</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Albania</td>
      <td>1987</td>
      <td>female</td>
      <td>15-24 years</td>
      <td>14</td>
      <td>289700</td>
      <td>4.83</td>
      <td>Albania1987</td>
      <td>NaN</td>
      <td>2,156,624,900</td>
      <td>796</td>
      <td>Generation X</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>1987</td>
      <td>male</td>
      <td>75+ years</td>
      <td>1</td>
      <td>21800</td>
      <td>4.59</td>
      <td>Albania1987</td>
      <td>NaN</td>
      <td>2,156,624,900</td>
      <td>796</td>
      <td>G.I. Generation</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Albania</td>
      <td>1987</td>
      <td>male</td>
      <td>25-34 years</td>
      <td>9</td>
      <td>274300</td>
      <td>3.28</td>
      <td>Albania1987</td>
      <td>NaN</td>
      <td>2,156,624,900</td>
      <td>796</td>
      <td>Boomers</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### 2. Data Cleaning and Pre-Processing {#2-data-cleaning-and-pre-processing}
:::

::: {.cell .code execution_count="64"}
``` python
# Remove unecessary characters from the column title
stats.columns = stats.columns.str.title()
stats.columns = stats.columns.str.replace(' ($)', '')
stats.columns = stats.columns.str.replace(' ($)', '')
stats.columns = stats.columns.str.replace(' ', '_')
```
:::

::: {.cell .code execution_count="65"}
``` python
stats.columns
```

::: {.output .execute_result execution_count="65"}
    Index(['Country', 'Year', 'Sex', 'Age', 'Suicides_No', 'Population',
           'Suicides/100K_Pop', 'Country-Year', 'Hdi_For_Year', '_Gdp_For_Year_',
           'Gdp_Per_Capita', 'Generation'],
          dtype='object')
:::
:::

::: {.cell .code execution_count="66"}
``` python
stats.rename(columns={'_Gdp_For_Year_':'Gdp_For_Year'}, inplace = True)
```
:::

::: {.cell .code execution_count="67"}
``` python
stats.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 27820 entries, 0 to 27819
    Data columns (total 12 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Country            27820 non-null  object 
     1   Year               27820 non-null  int64  
     2   Sex                27820 non-null  object 
     3   Age                27820 non-null  object 
     4   Suicides_No        27820 non-null  int64  
     5   Population         27820 non-null  int64  
     6   Suicides/100K_Pop  27820 non-null  float64
     7   Country-Year       27820 non-null  object 
     8   Hdi_For_Year       8364 non-null   float64
     9   Gdp_For_Year       27820 non-null  object 
     10  Gdp_Per_Capita     27820 non-null  int64  
     11  Generation         27820 non-null  object 
    dtypes: float64(2), int64(4), object(6)
    memory usage: 2.5+ MB
:::
:::

::: {.cell .code execution_count="68"}
``` python
# Select only the necessary columns
stats = stats[['Country', 'Year', 'Sex', 'Age', 'Suicides_No', 'Population',
       'Suicides/100K_Pop', 'Hdi_For_Year', 'Gdp_For_Year',
       'Gdp_Per_Capita', 'Generation']]
```
:::

::: {.cell .code execution_count="69"}
``` python
# Remove duplicate rows
stats = stats[~stats.duplicated()]
```
:::

::: {.cell .markdown}
### Check for potential outliers
:::

::: {.cell .code execution_count="70"}
``` python
sns.boxplot(x=stats['Suicides_No']);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/edecb9f627f9b2361effc5771dc3fbdfd6aa4718.png)
:::
:::

::: {.cell .code execution_count="71"}
``` python
# IQR rule to use for the skewed distribution
q25, q75 = np.percentile(stats['Suicides_No'], (25,75))
iqr = q75 - q25 
min_1 = q25 - 1.5*iqr 
max_1 = q75 + 1.5*iqr
```
:::

::: {.cell .code execution_count="72"}
``` python
# Count of outliers
stats['Suicides_No'].loc[stats['Suicides_No']>max_1].count() 
```

::: {.output .execute_result execution_count="72"}
    3909
:::
:::

::: {.cell .markdown}
-   count of outliers is almost 14% of the total data. We will retain
    these values for further analysis.
:::

::: {.cell .code execution_count="73"}
``` python
sns.boxplot(x=stats['Population']);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/23435598f17abe4412aa396df24ab82662a5471c.png)
:::
:::

::: {.cell .code execution_count="74"}
``` python
# IQR rule to use for the skewed distribution
q25, q75 = np.percentile(stats['Population'], (25,75))
iqr = q75 - q25 
min_2 = q25 - 1.5*iqr 
max_2 = q75 + 1.5*iqr
```
:::

::: {.cell .code execution_count="75"}
``` python
# Count of outliers
stats['Population'].loc[stats['Population']>max_2].count() 
```

::: {.output .execute_result execution_count="75"}
    4180
:::
:::

::: {.cell .markdown}
-   Count of outliers is 15% of the total data. We will retain these
    values.
:::

::: {.cell .code execution_count="76"}
``` python
sns.boxplot(x=stats['Hdi_For_Year']);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/5274b435ef574a999fa5527cded119418a9eab45.png)
:::
:::

::: {.cell .code execution_count="77"}
``` python
# IQR rule to use for the skewed distribution
q25, q75 = np.percentile(stats['Hdi_For_Year'], (25,75))
iqr = q75 - q25 
min_3 = q25 - 1.5*iqr 
max_3 = q75 + 1.5*iqr
# Count of outliers
stats['Hdi_For_Year'].loc[stats['Hdi_For_Year']<min_3].count() 
```

::: {.output .execute_result execution_count="77"}
    0
:::
:::

::: {.cell .code execution_count="78"}
``` python
sns.boxplot(x=stats['Gdp_Per_Capita']);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/6c4a71a8965680c4965ddaf2987498661543a701.png)
:::
:::

::: {.cell .code execution_count="79"}
``` python
# IQR rule to use for the skewed distribution
q25, q75 = np.percentile(stats['Gdp_Per_Capita'], (25,75))
iqr = q75 - q25 
min_4 = q25 - 1.5*iqr 
max_4 = q75 + 1.5*iqr
# Count of outliers
stats['Gdp_Per_Capita'].loc[stats['Gdp_Per_Capita']>max_4].count() 
```

::: {.output .execute_result execution_count="79"}
    1016
:::
:::

::: {.cell .code execution_count="83"}
``` python
stats['Gdp_Per_Capita'].loc[stats['Gdp_Per_Capita']>max_4].value_counts()
```

::: {.output .execute_result execution_count="83"}
    Gdp_Per_Capita
    66770    12
    74055    12
    76638    12
    66099    12
    69301    12
             ..
    85397    12
    80639    12
    60387    12
    62484    10
    64708    10
    Name: count, Length: 85, dtype: int64
:::
:::

::: {.cell .markdown}
-   This values are valid GDP per capita. We will retain these values
    for further analysis.
:::

::: {.cell .code execution_count="85"}
``` python
stats.isnull().any()  # Check if there are any missing values
```

::: {.output .execute_result execution_count="85"}
    Country              False
    Year                 False
    Sex                  False
    Age                  False
    Suicides_No          False
    Population           False
    Suicides/100K_Pop    False
    Hdi_For_Year          True
    Gdp_For_Year         False
    Gdp_Per_Capita       False
    Generation           False
    dtype: bool
:::
:::

::: {.cell .code execution_count="87"}
``` python
# Replace missing value with median
stats["Hdi_For_Year"] = stats["Hdi_For_Year"].fillna(stats["Hdi_For_Year"].median())
```
:::

::: {.cell .code execution_count="88"}
``` python
# Check if there are any missing values after filling with mean values
stats.isnull().any()  
```

::: {.output .execute_result execution_count="88"}
    Country              False
    Year                 False
    Sex                  False
    Age                  False
    Suicides_No          False
    Population           False
    Suicides/100K_Pop    False
    Hdi_For_Year         False
    Gdp_For_Year         False
    Gdp_Per_Capita       False
    Generation           False
    dtype: bool
:::
:::

::: {.cell .code execution_count="89"}
``` python
stats.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 27820 entries, 0 to 27819
    Data columns (total 11 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Country            27820 non-null  object 
     1   Year               27820 non-null  int64  
     2   Sex                27820 non-null  object 
     3   Age                27820 non-null  object 
     4   Suicides_No        27820 non-null  int64  
     5   Population         27820 non-null  int64  
     6   Suicides/100K_Pop  27820 non-null  float64
     7   Hdi_For_Year       27820 non-null  float64
     8   Gdp_For_Year       27820 non-null  object 
     9   Gdp_Per_Capita     27820 non-null  int64  
     10  Generation         27820 non-null  object 
    dtypes: float64(2), int64(4), object(5)
    memory usage: 2.3+ MB
:::
:::

::: {.cell .code execution_count="99"}
``` python
stats['Gdp_For_Year'] = stats['Gdp_For_Year'].str.replace(',','')
stats['Gdp_For_Year'] = stats['Gdp_For_Year'].astype(float)
```
:::

::: {.cell .markdown}
### 2. Data Analysis and Visualization (EDA) {#2-data-analysis-and-visualization-eda}
:::

::: {.cell .code execution_count="100"}
``` python
stats[['Year', 'Suicides_No', 'Population',
       'Suicides/100K_Pop', 'Hdi_For_Year',
       'Gdp_For_Year', 'Gdp_Per_Capita']].corr().style.background_gradient(cmap = "RdYlGn", axis = 1)
```

::: {.output .execute_result execution_count="100"}
```{=html}
<style type="text/css">
#T_9f161_row0_col0, #T_9f161_row1_col1, #T_9f161_row2_col2, #T_9f161_row3_col3, #T_9f161_row4_col4, #T_9f161_row5_col5, #T_9f161_row6_col6 {
  background-color: #006837;
  color: #f1f1f1;
}
#T_9f161_row0_col1 {
  background-color: #b50f26;
  color: #f1f1f1;
}
#T_9f161_row0_col2, #T_9f161_row3_col2 {
  background-color: #bb1526;
  color: #f1f1f1;
}
#T_9f161_row0_col3, #T_9f161_row1_col0, #T_9f161_row2_col0, #T_9f161_row2_col3, #T_9f161_row3_col0, #T_9f161_row4_col3, #T_9f161_row5_col3, #T_9f161_row6_col3 {
  background-color: #a50026;
  color: #f1f1f1;
}
#T_9f161_row0_col4 {
  background-color: #f8864f;
  color: #f1f1f1;
}
#T_9f161_row0_col5 {
  background-color: #de402e;
  color: #f1f1f1;
}
#T_9f161_row0_col6 {
  background-color: #fece7c;
  color: #000000;
}
#T_9f161_row1_col2 {
  background-color: #cfeb85;
  color: #000000;
}
#T_9f161_row1_col3 {
  background-color: #fdb365;
  color: #000000;
}
#T_9f161_row1_col4 {
  background-color: #c62027;
  color: #f1f1f1;
}
#T_9f161_row1_col5 {
  background-color: #feea9b;
  color: #000000;
}
#T_9f161_row1_col6 {
  background-color: #c41e27;
  color: #f1f1f1;
}
#T_9f161_row2_col1 {
  background-color: #d3ec87;
  color: #000000;
}
#T_9f161_row2_col4 {
  background-color: #bd1726;
  color: #f1f1f1;
}
#T_9f161_row2_col5 {
  background-color: #a0d669;
  color: #000000;
}
#T_9f161_row2_col6, #T_9f161_row3_col4, #T_9f161_row5_col0 {
  background-color: #c82227;
  color: #f1f1f1;
}
#T_9f161_row3_col1 {
  background-color: #fdbf6f;
  color: #000000;
}
#T_9f161_row3_col5, #T_9f161_row6_col1 {
  background-color: #c21c27;
  color: #f1f1f1;
}
#T_9f161_row3_col6 {
  background-color: #b91326;
  color: #f1f1f1;
}
#T_9f161_row4_col0, #T_9f161_row5_col4 {
  background-color: #ed5f3c;
  color: #f1f1f1;
}
#T_9f161_row4_col1 {
  background-color: #b10b26;
  color: #f1f1f1;
}
#T_9f161_row4_col2 {
  background-color: #af0926;
  color: #f1f1f1;
}
#T_9f161_row4_col5 {
  background-color: #ea5739;
  color: #f1f1f1;
}
#T_9f161_row4_col6 {
  background-color: #fffbb8;
  color: #000000;
}
#T_9f161_row5_col1 {
  background-color: #fee593;
  color: #000000;
}
#T_9f161_row5_col2 {
  background-color: #a2d76a;
  color: #000000;
}
#T_9f161_row5_col6 {
  background-color: #fca55d;
  color: #000000;
}
#T_9f161_row6_col0 {
  background-color: #fdc171;
  color: #000000;
}
#T_9f161_row6_col2 {
  background-color: #cc2627;
  color: #f1f1f1;
}
#T_9f161_row6_col4 {
  background-color: #fdfebc;
  color: #000000;
}
#T_9f161_row6_col5 {
  background-color: #fdaf62;
  color: #000000;
}
</style>
<table id="T_9f161">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_9f161_level0_col0" class="col_heading level0 col0" >Year</th>
      <th id="T_9f161_level0_col1" class="col_heading level0 col1" >Suicides_No</th>
      <th id="T_9f161_level0_col2" class="col_heading level0 col2" >Population</th>
      <th id="T_9f161_level0_col3" class="col_heading level0 col3" >Suicides/100K_Pop</th>
      <th id="T_9f161_level0_col4" class="col_heading level0 col4" >Hdi_For_Year</th>
      <th id="T_9f161_level0_col5" class="col_heading level0 col5" >Gdp_For_Year</th>
      <th id="T_9f161_level0_col6" class="col_heading level0 col6" >Gdp_Per_Capita</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_9f161_level0_row0" class="row_heading level0 row0" >Year</th>
      <td id="T_9f161_row0_col0" class="data row0 col0" >1.000000</td>
      <td id="T_9f161_row0_col1" class="data row0 col1" >-0.004546</td>
      <td id="T_9f161_row0_col2" class="data row0 col2" >0.008850</td>
      <td id="T_9f161_row0_col3" class="data row0 col3" >-0.039037</td>
      <td id="T_9f161_row0_col4" class="data row0 col4" >0.209036</td>
      <td id="T_9f161_row0_col5" class="data row0 col5" >0.094529</td>
      <td id="T_9f161_row0_col6" class="data row0 col6" >0.339134</td>
    </tr>
    <tr>
      <th id="T_9f161_level0_row1" class="row_heading level0 row1" >Suicides_No</th>
      <td id="T_9f161_row1_col0" class="data row1 col0" >-0.004546</td>
      <td id="T_9f161_row1_col1" class="data row1 col1" >1.000000</td>
      <td id="T_9f161_row1_col2" class="data row1 col2" >0.616162</td>
      <td id="T_9f161_row1_col3" class="data row1 col3" >0.306604</td>
      <td id="T_9f161_row1_col4" class="data row1 col4" >0.062669</td>
      <td id="T_9f161_row1_col5" class="data row1 col5" >0.430096</td>
      <td id="T_9f161_row1_col6" class="data row1 col6" >0.061330</td>
    </tr>
    <tr>
      <th id="T_9f161_level0_row2" class="row_heading level0 row2" >Population</th>
      <td id="T_9f161_row2_col0" class="data row2 col0" >0.008850</td>
      <td id="T_9f161_row2_col1" class="data row2 col1" >0.616162</td>
      <td id="T_9f161_row2_col2" class="data row2 col2" >1.000000</td>
      <td id="T_9f161_row2_col3" class="data row2 col3" >0.008285</td>
      <td id="T_9f161_row2_col4" class="data row2 col4" >0.057279</td>
      <td id="T_9f161_row2_col5" class="data row2 col5" >0.710697</td>
      <td id="T_9f161_row2_col6" class="data row2 col6" >0.081510</td>
    </tr>
    <tr>
      <th id="T_9f161_level0_row3" class="row_heading level0 row3" >Suicides/100K_Pop</th>
      <td id="T_9f161_row3_col0" class="data row3 col0" >-0.039037</td>
      <td id="T_9f161_row3_col1" class="data row3 col1" >0.306604</td>
      <td id="T_9f161_row3_col2" class="data row3 col2" >0.008285</td>
      <td id="T_9f161_row3_col3" class="data row3 col3" >1.000000</td>
      <td id="T_9f161_row3_col4" class="data row3 col4" >0.037290</td>
      <td id="T_9f161_row3_col5" class="data row3 col5" >0.025240</td>
      <td id="T_9f161_row3_col6" class="data row3 col6" >0.001785</td>
    </tr>
    <tr>
      <th id="T_9f161_level0_row4" class="row_heading level0 row4" >Hdi_For_Year</th>
      <td id="T_9f161_row4_col0" class="data row4 col0" >0.209036</td>
      <td id="T_9f161_row4_col1" class="data row4 col1" >0.062669</td>
      <td id="T_9f161_row4_col2" class="data row4 col2" >0.057279</td>
      <td id="T_9f161_row4_col3" class="data row4 col3" >0.037290</td>
      <td id="T_9f161_row4_col4" class="data row4 col4" >1.000000</td>
      <td id="T_9f161_row4_col5" class="data row4 col5" >0.198013</td>
      <td id="T_9f161_row4_col6" class="data row4 col6" >0.505505</td>
    </tr>
    <tr>
      <th id="T_9f161_level0_row5" class="row_heading level0 row5" >Gdp_For_Year</th>
      <td id="T_9f161_row5_col0" class="data row5 col0" >0.094529</td>
      <td id="T_9f161_row5_col1" class="data row5 col1" >0.430096</td>
      <td id="T_9f161_row5_col2" class="data row5 col2" >0.710697</td>
      <td id="T_9f161_row5_col3" class="data row5 col3" >0.025240</td>
      <td id="T_9f161_row5_col4" class="data row5 col4" >0.198013</td>
      <td id="T_9f161_row5_col5" class="data row5 col5" >1.000000</td>
      <td id="T_9f161_row5_col6" class="data row5 col6" >0.303405</td>
    </tr>
    <tr>
      <th id="T_9f161_level0_row6" class="row_heading level0 row6" >Gdp_Per_Capita</th>
      <td id="T_9f161_row6_col0" class="data row6 col0" >0.339134</td>
      <td id="T_9f161_row6_col1" class="data row6 col1" >0.061330</td>
      <td id="T_9f161_row6_col2" class="data row6 col2" >0.081510</td>
      <td id="T_9f161_row6_col3" class="data row6 col3" >0.001785</td>
      <td id="T_9f161_row6_col4" class="data row6 col4" >0.505505</td>
      <td id="T_9f161_row6_col5" class="data row6 col5" >0.303405</td>
      <td id="T_9f161_row6_col6" class="data row6 col6" >1.000000</td>
    </tr>
  </tbody>
</table>
```
:::
:::

::: {.cell .code execution_count="103"}
``` python
total_suicide = stats.groupby("Year")[["Suicides_No"]].sum()
total_suicide

sns.set_style("darkgrid")



fig, ax = plt.subplots()

ax.plot(total_suicide.index, total_suicide["Suicides_No"] / 1000);
ax.axvline(1999, c="black", ls="--")

ax.set_xlabel("Year");
ax.set_ylabel("Suicide Number (In Thousands)");
ax.set_title("Number Of Suicide Case Trend (1985-2016)", fontsize = 14);
ax.spines[["top", "right"]].set_visible(False)
ax.set_ylim(0, 290)
ax.annotate('Highest Suicide Case', xy = (1999, 255), xytext = (2015, 280),
            arrowprops=dict(facecolor='black', width = 2, headwidth = 8),
            fontsize=11,
            horizontalalignment='right', verticalalignment='top');
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/3af47e9b8a918fd5c4f6e5510533bd682865241d.png)
:::
:::

::: {.cell .code execution_count="106"}
``` python
suicide_country = stats.groupby("Country")[["Suicides_No"]].sum().sort_values(by="Suicides_No", ascending = False).head()
suicide_country

fig, ax = plt.subplots(figsize = (10, 4))
sns.barplot(x=suicide_country.index, y=suicide_country["Suicides_No"] / 1000000, data = suicide_country);
ax.set_title("Countries With Most Number Of Suicide Cases", fontsize = 13);
ax.set_xlabel("Country", fontsize = 11);
ax.set_ylabel("Suicide Number (In Millions)", fontsize = 11);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/70d37374cfa2e6298b4134d95a729b7f1bdfaf81.png)
:::
:::

::: {.cell .code execution_count="109"}
``` python
suicide_sex= stats.groupby("Sex")[["Suicides_No"]].sum()
suicide_sex

fig, ax = plt.subplots()
sns.barplot(x=suicide_sex.index, y=suicide_sex["Suicides_No"] / 1000000, data = suicide_sex, width = 0.6);
ax.set_title("Number Of Suicide Cases According To Gender", fontsize = 13);
ax.set_xlabel("Gender", fontsize = 11);
ax.set_ylabel("Suicide Number (In Millions)", fontsize = 11);
ax.set_ylim(0, 6.5);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/97bbe565bfa94eaa9d0b189ccf0d97404e8797dd.png)
:::
:::

::: {.cell .code execution_count="113"}
``` python
countries = ["Russian Federation", "United States", "Japan", "France", "Ukraine"]
suicide_sex_country = stats.loc[stats["Country"].isin(countries)].groupby(["Country", "Sex"])[["Suicides_No"]].sum().reset_index().sort_values(by = "Suicides_No", ascending = False)
suicide_sex_country 


fig, ax = plt.subplots(figsize = (10,4))
sns.barplot(data = suicide_sex_country, x = suicide_sex_country["Country"], y = suicide_sex_country["Suicides_No"] / 1000000, hue = suicide_sex_country["Sex"])
ax.set_title("Gender Distribution Of Highest Suicide Cases By Country", fontsize = 13)
ax.set_xlabel("Country", fontsize = 11) 
ax.set_ylabel("Suicides Number (In Millions)", fontsize = 11);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/e8fa5a7147e8efed2cd1736eaf6dc272c49dc37c.png)
:::
:::

::: {.cell .code execution_count="119"}
``` python
age_suicide = stats.groupby("Age")[["Suicides_No"]].sum().sort_values(by="Suicides_No", ascending = False)
age_suicide

fig, ax = plt.subplots()
sns.barplot(x=age_suicide.index, y=age_suicide["Suicides_No"] / 1000000, data = age_suicide, width = 0.6);
ax.set_title("Number Of Suicide Cases According To Age Bracket", fontsize = 13);
ax.set_xlabel("Age Bracket", fontsize = 11);
ax.set_ylabel("Suicide Number (In Millions)", fontsize = 11);
ax.set_ylim(0, 2.75);

 
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/6b41e51013e0ea9a2761191d5a1f909db358e514.png)
:::
:::

::: {.cell .code execution_count="122"}
``` python
countries = ["Russian Federation", "United States", "Japan", "France", "Ukraine"]
suicide_age_country = stats.loc[stats["Country"].isin(countries)].groupby(["Country", "Age"])[["Suicides_No"]].sum().reset_index().sort_values(by = "Suicides_No", ascending = False)
suicide_age_country 


fig, ax = plt.subplots(figsize = (10,4))
sns.barplot(data = suicide_age_country, x = suicide_age_country["Country"], y = suicide_age_country["Suicides_No"] / 1000000, hue = suicide_age_country["Age"])
ax.set_title("Age Distribution Of Highest Suicide Cases By Country", fontsize = 13)
ax.set_xlabel("Country", fontsize = 11) 
ax.set_ylabel("Suicides Number (In Millions)", fontsize = 11);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/c8e08beebbe7057f24e1d2373722fd6fb5e5678d.png)
:::
:::

::: {.cell .code execution_count="124"}
``` python
generation_suicides = stats.groupby("Generation")[["Suicides_No"]].sum().sort_values(by = "Suicides_No", ascending = False)
generation_suicides 

ig, ax = plt.subplots(figsize = (10,4))

sns.barplot(x = generation_suicides.index, y= generation_suicides["Suicides_No"] / 1000, data = generation_suicides );
ax.set_xlabel("Generation");
ax.set_ylabel("Suicide Number (In Thousands)");
ax.set_title("Number of Suicide Case Based On Generation", fontsize = 14);
ax.spines[["top", "right"]].set_visible(False);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/d754e0f8146be1fdc85132e075c7da31d79af7cb.png)
:::
:::

::: {.cell .code execution_count="127"}
``` python
countries = ["Russian Federation", "United States", "Japan", "France", "Ukraine"]
suicide_generation_country = stats.loc[stats["Country"].isin(countries)].groupby(["Country", "Generation"])[["Suicides_No"]].sum().reset_index().sort_values(by = "Suicides_No", ascending = False)
suicide_generation_country 


fig, ax = plt.subplots(figsize = (10,4))
sns.barplot(data = suicide_generation_country, x = suicide_generation_country["Country"], y = suicide_generation_country["Suicides_No"] / 1000000, hue = suicide_generation_country["Generation"])
ax.set_title("Generation Distribution of Highest Suicide Cases By Country", fontsize = 13)
ax.set_xlabel("Country", fontsize = 11) 
ax.set_ylabel("Suicides Number (In Millions)", fontsize = 11);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/afac0cadebb2fbb5256ae84d420b9d15688d9db3.png)
:::
:::

::: {.cell .code execution_count="133"}
``` python
gdp_capita = stats.groupby("Year")[["Gdp_Per_Capita"]].mean()
gdp_capita

gdp_capita_year = stats.groupby("Year")[["Gdp_For_Year"]].mean()
gdp_capita_year

fig, ax = plt.subplots(sharex = "all", figsize = (10,4))

ax.plot(gdp_capita.index, gdp_capita["Gdp_Per_Capita"] / 1000);
ax.set_xlabel("Year");
ax.set_ylabel("Gdp Per Capita ($) (In Thousands)");
ax.grid(None)
plt.grid(which = 'both', linestyle='dotted')
ax2 = ax.twinx()

ax2.plot(gdp_capita_year.index, gdp_capita_year["Gdp_For_Year"] / 1000000, color = "green");
ax2.set_ylabel("Gdp For Year ($) (In Millions)");
ax2.grid(None)
fig.suptitle("GDP Per Capita vs GDP Per Year");
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/9e5ffe4ba1e70de7a99159c4d8dea8c072d25013.png)
:::
:::

::: {.cell .code execution_count="135"}
``` python
countries = ["Russian Federation", "United States", "Japan", "France", "Ukraine"]
HDI_suicide = stats.loc[stats["Country"].isin(countries)].groupby("Country")[["Hdi_For_Year"]].mean().sort_values(by = "Hdi_For_Year", ascending = True).head()
HDI_suicide

fig, ax = plt.subplots(figsize = (10,4))

sns.barplot(data = HDI_suicide, x = HDI_suicide.index, y = HDI_suicide["Hdi_For_Year"]*100);
ax.set_xlabel("Country");
ax.set_ylabel("HDI (In %)");
ax.set_title("HDI Measure Of Countries With Highest Suicide Cases");
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/2e072e5cc48af49fa9ffdcedbca4f2a1ce34cee9.png)
:::
:::

::: {.cell .code execution_count="141"}
``` python
HDI= stats.groupby("Country")[["Hdi_For_Year"]].mean().sort_values(by = "Hdi_For_Year", ascending = False)
HDI 
fig, ax = plt.subplots(figsize = (18, 17))


sns.barplot(y = HDI.index, x = HDI["Hdi_For_Year"]*100, data = HDI, palette="flare"), ;
ax.set_xlabel("HDI (In %)", fontsize = 13);
ax.set_ylabel("Country", fontsize = 13);
ax.set_title("Highest HDI Measure By Countries", fontsize = 17);


```

::: {.output .stream .stderr}
    C:\Users\bobby\AppData\Local\Temp\ipykernel_5188\3039906812.py:6: FutureWarning: 

    Passing `palette` without assigning `hue` is deprecated and will be removed in v0.14.0. Assign the `y` variable to `hue` and set `legend=False` for the same effect.

      sns.barplot(y = HDI.index, x = HDI["Hdi_For_Year"]*100, data = HDI, palette="flare"), ;
:::

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/45ba355752738df8be95b0d0b668c30c53276b56.png)
:::
:::

::: {.cell .code execution_count="144"}
``` python
suicide_ratio = stats.groupby("Year")[["Suicides/100K_Pop"]].mean()
suicide_ratio


fig, ax = plt.subplots(sharex = "all", figsize = (10,4))

ax.plot(suicide_ratio.index, suicide_ratio["Suicides/100K_Pop"]);
ax.set_xlabel("Year", fontsize = 10);
ax.set_ylabel("Suicides/100K Population", fontsize = 10);
ax.set_title("Suicides/100k Population Case Trend (1985-2015)", fontsize = 15)
```

::: {.output .execute_result execution_count="144"}
    Text(0.5, 1.0, 'Suicides/100k Population Case Trend (1985-2015)')
:::

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/86f154863cc35c756f2e7a1601ad2d77a4954dbe.png)
:::
:::

::: {.cell .code execution_count="149"}
``` python
fig, ax = plt.subplots()


ax.scatter(x=stats["Hdi_For_Year"], y=stats["Gdp_Per_Capita"], alpha = 0.04, s = 40, color = "green")
ax.set_xlabel("HDI For Year", fontsize = 10);
ax.set_ylabel("GDP Per Capita", fontsize = 10);
ax.set_title("HDI vs GDP Per Capita", fontsize = 14);
```

::: {.output .display_data}
![](vertopal_dfdf24354f41472ab46955c68174cfbb/d1531209ab4a3f38b09b37394a2b11517e676092.png)
:::
:::

::: {.cell .markdown}
### Insights

From 1985 to 2016, there is a decrease trend in suicide cases. The
highest suicide cases was on 1999. For suicide number per 100k
population, year 1995 was the highest ratio. Russian Federation was the
country with the highest suicide cases recorded. Male gender with age
bracket of 35-54 years was its dominant charateristics. There was no
relationship among HDI, GDP, GDP Per Capita and Suicide Number meaning
the measure of standard of living of a country has nothing to do with
the person commiting a suicide.
:::
