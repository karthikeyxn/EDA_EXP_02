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

<img width="872" height="215" alt="image" src="https://github.com/user-attachments/assets/b5df37fb-0fbf-4586-8db1-574c53cdd66a" />


### Clean 'date_added' and extract year/month

<img width="1670" height="596" alt="image" src="https://github.com/user-attachments/assets/b92057a9-35f7-4b3d-a9d0-1613467fff8f" />


###  Movies vs TV Shows

<img width="403" height="138" alt="image" src="https://github.com/user-attachments/assets/cd7d3f6f-8b12-41c5-8b9c-b5bfeae7c5b9" />


###  Country vs Type Pivot Table

<img width="1030" height="242" alt="image" src="https://github.com/user-attachments/assets/3db75ba8-e973-476d-8f89-e7223a28eddb" />


### Top 5 Directors

<img width="432" height="220" alt="image" src="https://github.com/user-attachments/assets/65728000-847e-4f97-bf91-52921b753e1f" />


### Yearly Trend of Additions (Movies vs TV Shows)

<img width="355" height="206" alt="image" src="https://github.com/user-attachments/assets/d9f9b800-5edc-4b3e-8b94-fadc0aa8a2cb" />


### Expand Genres


<img width="917" height="508" alt="image" src="https://github.com/user-attachments/assets/2561d1cb-f2ee-4eb2-8bdb-4b9b7151ae33" />



## Result 
Helps Netflix in content planning & investments.
