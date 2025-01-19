# Observation-from-inside-air-bnb-New-York
Python analysis using calendar and listing dataset of New York
## For the following analysis, I have downloaded the data from the link for that. https://data.insideairbnb.com/united-states/ny/new-york-city/2024-11-04/data/calendar.csv.gz 

```
import pandas as pd

calendar = pd.read_csv('calendar.csv')
```
## I want to know the number of available and unavailable 
``` diff
calendar.available.value_counts()
```
<img width="229" alt="image" src="https://github.com/user-attachments/assets/07645fb9-aea5-4182-901e-bac6ea52f67c" />

f(false) means not available, t(true) means available


