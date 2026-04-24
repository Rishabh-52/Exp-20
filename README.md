# Experiment 20: COVID-19 Data Analysis and Visualization

## Aim:

To analyze and visualize COVID-19 data using Python libraries for data cleaning, transformation, statistical analysis, and graphical representation of global and Indian COVID-19 trends.

---

## Theory:

### 1. Importing Libraries

```python
import pandas as pd
import numpy as np
import plotly.express as px
```

- `import pandas as pd`
  - Imports the Pandas library.
  - Used for data manipulation and analysis using DataFrames.

- `import numpy as np`
  - Imports NumPy library.
  - Used for numerical operations.

- `import plotly.express as px`
  - Imports Plotly Express module.
  - Used for interactive visualizations and maps.

---

### 2. Importing Dataset

```python
data = pd.read_csv("covid_19_data.csv")
```

- `pd.read_csv()`
  - Reads a CSV file and converts it into a DataFrame.

- `"covid_19_data.csv"`
  - Name of the dataset file containing COVID-19 records.

- `data`
  - Variable used to store the dataset.

---

### 3. Displaying Initial Dataset

```python
data.head()
```

- `head()`
  - Displays the first five rows of the dataset.

This helps in understanding dataset structure and column names.

---

### 4. Removing Unnecessary Columns

```python
data = data.drop(
    ['SNo', 'Last Update'],
    axis=1
)
```

- `drop()`
  - Removes unwanted rows or columns.

- `['SNo', 'Last Update']`
  - Specifies columns to remove.

- `axis=1`
  - Indicates column deletion.

This step cleans unnecessary information from the dataset.

---

### 5. Viewing Dataset Information

```python
data.info()
```

- `info()`
  - Displays dataset summary.

It provides:

- Number of rows and columns
- Data types of columns
- Non-null values
- Memory usage

---

### 6. Data Type Conversion

```python
data['ObservationDate'] = data[
    'ObservationDate'
].astype('datetime64[ns]')

data['Confirmed'] = data[
    'Confirmed'
].astype('int64')
```

- `astype()`
  - Converts data types.

- `'datetime64[ns]'`
  - Converts values into date-time format.

- `'int64'`
  - Converts values into integer format.

This improves data consistency and analysis accuracy.

---

### 7. Checking Missing Values

```python
data.isnull().sum()
```

- `isnull()`
  - Detects missing values.

- `sum()`
  - Counts total missing values in each column.

This step helps identify incomplete data.

---

### 8. Creating Active Cases Column

```python
data['Active'] = (
    data['Confirmed']
    - data['Recovered']
    - data['Deaths']
)
```

- `data['Active']`
  - Creates a new column named `Active`.

- Active cases are calculated as:

  - Confirmed Cases
  - minus Recovered Cases
  - minus Death Cases

This helps determine currently infected cases.

---

### 9. Finding Latest Date

```python
data['ObservationDate'].max()
```

- `max()`
  - Returns the maximum value.

Used to find the latest available date in the dataset.

---

### 10. Filtering Latest Data

```python
latest_data = data[
    data['ObservationDate']
    ==
    data['ObservationDate'].max()
]
```

- Boolean indexing is used for filtering rows.

- `==`
  - Compares dates.

- Only records with the latest date are selected.

This creates a dataset containing the most recent COVID-19 data.

---

### 11. Finding Dataset Dimensions

```python
latest_data.shape
```

- `shape`
  - Returns number of rows and columns.

Output format:

```python
(rows, columns)
```

---

### 12. Counting Countries

```python
latest_data['Country/Region'].value_counts()
```

- `value_counts()`
  - Counts occurrences of unique values.

Used to count entries for each country.

---

### 13. Finding Unique Countries

```python
latest_data['Country/Region'].nunique()

latest_data['Country/Region'].unique()
```

- `nunique()`
  - Returns number of unique countries.

- `unique()`
  - Displays unique country names.

---

### 14. Grouping Country Data

```python
countries = latest_data.groupby(
    "Country/Region"
)[
    [
        "Confirmed",
        "Deaths",
        "Recovered",
        "Active"
    ]
].sum()
```

- `groupby()`
  - Groups rows based on country names.

- `.sum()`
  - Calculates total values for grouped rows.

This creates country-wise COVID-19 statistics.

---

### 15. Resetting Index

```python
countries = countries.reset_index()
```

- `reset_index()`
  - Converts grouped index into regular column.

Improves readability of grouped data.

---

### 16. Filtering Specific Country

```python
countries[
    countries['Country/Region']
    == "India"
]
```

- Filters dataset for India only.

Used for country-specific analysis.

---

### 17. Choropleth Map Visualization

```python
world_map = px.choropleth(
    countries,

    locations="Country/Region",

    locationmode="country names",

    color="Confirmed",

    color_continuous_scale="reds",

    range_color=[0,10000000]
)

world_map.show()
```

- `px.choropleth()`
  - Creates geographical map visualization.

- `locations=`
  - Specifies country names.

- `locationmode="country names"`
  - Interprets location values as country names.

- `color="Confirmed"`
  - Colors countries based on confirmed cases.

- `color_continuous_scale="reds"`
  - Uses red color gradient.

- `range_color=[0,10000000]`
  - Defines color scale range.

- `world_map.show()`
  - Displays interactive map.

Choropleth maps visualize geographical spread of COVID-19 cases.

---

### 18. Sorting Countries by Cases

```python
top = latest_data.groupby(
    "Country/Region"
)[
    ["Confirmed", "Recovered"]
].sum().reset_index()

top = top.sort_values(
    ['Confirmed'],
    ascending=False
)
```

- `sort_values()`
  - Sorts data.

- `ascending=False`
  - Sorts in descending order.

Used to identify countries with highest confirmed cases.

---

### 19. Filtering India Dataset

```python
india = data[
    data['Country/Region']
    == "India"
]
```

- Creates dataset containing only Indian records.

---

### 20. Finding Indian States

```python
india['Province/State'].nunique()

india['Province/State'].unique()
```

- Finds number of unique states.

- Displays names of states in dataset.

---

### 21. Latest India Data

```python
india_latest_data = india[
    india['ObservationDate']
    ==
    india['ObservationDate'].max()
]
```

- Filters latest COVID-19 data for India.

---

### 22. State-wise Analysis

```python
top_state = india_latest_data.groupby(
    "Province/State"
)[
    ["Confirmed", "Recovered"]
].sum().reset_index()
```

- Groups data based on states.

- Calculates total confirmed and recovered cases for each state.

---

### 23. Finding Maximum Cases

```python
top_state['Confirmed'].max()
```

- Returns highest confirmed case count.

---

### 24. Finding State with Maximum Cases

```python
top_state[
    top_state['Confirmed']
    ==
    top_state['Confirmed'].max()
]['Province/State'].values[0]
```

- Filters state with highest confirmed cases.

- `values[0]`
  - Extracts first matching result.

---

### 25. India State-wise Choropleth Map

```python
fig = px.choropleth(
    map_df,

    geojson=india_geojson,

    featureidkey="properties.NAME_1",

    locations="State",

    color="Confirmed"
)

fig.show()
```

- `geojson=`
  - Provides geographical boundary data.

- `featureidkey=`
  - Matches state names with geojson properties.

- `locations="State"`
  - Specifies state names.

- `color="Confirmed"`
  - Colors states based on confirmed cases.

This visualization represents state-wise COVID-19 spread in India.

---

## Conclusion:

COVID-19 data was successfully analyzed and visualized using Python libraries such as Pandas, NumPy, and Plotly. Data cleaning, transformation, filtering, grouping, and visualization techniques were applied to study global and Indian COVID-19 trends. Interactive visualizations such as choropleth maps helped in understanding the geographical spread and impact of the pandemic effectively.
