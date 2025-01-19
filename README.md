# Observation-from-inside-air-bnb-New-York
Python analysis using calendar and listing dataset of New York
## For the following analysis, I have downloaded the data from the link for that. https://data.insideairbnb.com/united-states/ny/new-york-city/2024-11-04/data/calendar.csv.gz 

```
import pandas as pd

calendar = pd.read_csv('calendar.csv')
```
``` diff
calendar.available.value_counts()
```

