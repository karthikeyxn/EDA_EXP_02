```
KARTHIKEYAN M
212223040088
```
# Exp - 2 Netflix Shows & Movies

## Aim

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

## Procedure / Algorithm

1)Load dataset (netflix_titles.csv).

2)Count movies vs TV shows.

3)Group by country → top contributors.

4)Create pivot table (release year vs type).

5)Visualize with bar & line charts.

## Program

### Load dataset directly from GitHub

```python 
import pandas as pd
import numpy as np

url="https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
df=pd.read_csv(url)

print("Shape:",df.shape)

print("Columns:",df.columns,"\n")

print("Types:",df['type'].value_counts(),"\n")
```

### Clean 'date_added' and extract year/month

```python
df['date_added']=df['date_added'].astype(str).str.strip()
df['date_added']=pd.to_datetime(df['date_added'],errors='coerce')
df['year_added']=df['date_added'].dt.year.astype('Int64')
df['month_added']=df['date_added'].dt.month_name()

df.head()
```
###  Movies vs TV Shows
```python
count_by_type = df.groupby('type')['title'].count()
print("Count by Type:\n", count_by_type, "\n")
```
###  Country vs Type Pivot Table
```python
pivot_country_type = df.pivot_table(
index='country',
columns='type',
values='title',
aggfunc='count',
fill_value=0
)

pivot_country_type['Total'] = pivot_country_type.sum(axis=1)
max_country = pivot_country_type['Total'].idxmax()  # country with most titles
max_count = pivot_country_type['Total'].max()       
# number of titles
print("Pivot Table (Country vs Type):\n", pivot_country_type.head(), "\n")
print(f"Largest Overall: {max_country} with {max_count} titles\n")
```
### Top 5 Directors
```python
top_directors = df['director'].value_counts().head(5)
print("Top 5 Directors:\n", top_directors, "\n")
```

### Yearly Trend of Additions (Movies vs TV Shows)
```python
trend=df.groupby(['year_added','type']).size().unstack(fill_value=0)
print("Yearly Trend by Type\n")
print(trend.head())
```

### Expand Genres
``` python
df_genre = (
df[['show_id','listed_in']]
.dropna()
.assign(listed_in=df['listed_in'].str.split(', '))
.explode('listed_in')
)

df_expanded = df.merge(df_genre, on='show_id', how='left')

print("Columns after merge:\n", df_expanded.columns)

print("\nExpanded Genre Sample:\n",
df_expanded[['title','listed_in_y']].head())


top_genres = df_expanded['listed_in_y'].value_counts().head(5)
print("\nTop 5 Genres:\n", top_genres, "\n")
```
## Ouptut

### Load dataset directly from GitHub

<img width="701" height="37" alt="image" src="https://github.com/user-attachments/assets/09670272-2f3b-4309-ad9e-8ec1c4985038" />

<img width="1701" height="365" alt="image" src="https://github.com/user-attachments/assets/f508ab74-8b5f-43b9-a4a3-e7845ec8c548" />

<img width="829" height="163" alt="image" src="https://github.com/user-attachments/assets/d6b31853-9d8b-451d-b338-eb4e233bf75e" />


### Clean 'date_added' and extract year/month

<img width="1715" height="426" alt="image" src="https://github.com/user-attachments/assets/19c9abb3-31c2-4cd5-9544-a606490d7c99" />

###  Movies vs TV Shows

<img width="524" height="110" alt="image" src="https://github.com/user-attachments/assets/e40a2e52-9537-48f3-a228-e531e426398c" />

###  Country vs Type Pivot Table

<img width="600" height="158" alt="image" src="https://github.com/user-attachments/assets/d878fe31-13da-4928-bb7e-b537d3ca9248" />

### Top 5 Directors

<img width="600" height="158" alt="image" src="https://github.com/user-attachments/assets/a11cbf37-c389-4fd1-aa9a-1e2dae9494e3" />

### Yearly Trend of Additions (Movies vs TV Shows)

<img width="550" height="186" alt="image" src="https://github.com/user-attachments/assets/11697080-3125-42bb-aa1b-ca6e0e374962" />

### Expand Genres

<img width="710" height="154" alt="image" src="https://github.com/user-attachments/assets/7dd8e9d4-4078-4b43-8c2c-3f57018ee759" />

<img width="596" height="167" alt="image" src="https://github.com/user-attachments/assets/c82d6c51-0c58-44d9-a40b-16c09ee511d4" />


## Result 
Helps Netflix in content planning & investments.
