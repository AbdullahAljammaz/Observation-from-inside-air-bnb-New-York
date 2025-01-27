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

## 2 Purpose : calculates the percentage of available (t) and unavailable (f) dates.
Explanation value_counts(normalize=True) gives proportions. Multiplying by 100 converts the proportions into percentages

```
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
<img width="276" alt="image" src="https://github.com/user-attachments/assets/a8a8c236-b2dc-42da-84c2-ffb2a89a7875" />

## 3 Let`s Count the busiest day! 🚩

Hint We will be counting the most unavailable days (given by f)

```
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
<img width="276" alt="image" src="https://github.com/user-attachments/assets/3db53720-aacf-43a4-b731-239b049caee9" />


## 4 Plot a bar graph to show availability percentage 

```
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['blue', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="495" alt="image" src="https://github.com/user-attachments/assets/4f0b97fd-28ca-482f-9ac6-57b3479cde76" />

## 5 Plot the busiest day

```
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```

<img width="506" alt="image" src="https://github.com/user-attachments/assets/8f56005e-2e93-47c5-85d6-39b6993fd0d9" />

## 📄 Download listings dataset of Hawaii from https://data.insideairbnb.com/united-states/ny/new-york-city/2024-11-04/visualisations/listings.csv

```
import pandas as pd
listings = pd.read_csv('listings-2.csv')
```

<img width="1112" alt="image" src="https://github.com/user-attachments/assets/42e13bda-5a50-41bc-8988-519bfaf3e3c6" />

```
listings.columns
```

<img width="602" alt="image" src="https://github.com/user-attachments/assets/5507da4c-4702-4d68-bd58-541c2bf66401" />

## ✅ Room type and price is given seperately
Let`s Combine and visualize them

```
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='cyan')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```
<img width="490" alt="image" src="https://github.com/user-attachments/assets/189846b6-42b4-49be-902d-90646a15a262" />

## ✅ Which are the top 10 neighborhoods with the most listings?

```
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img width="495" alt="image" src="https://github.com/user-attachments/assets/273cbc26-ca6c-401b-85e4-23f1cd1dbca7" />

## ✅ Geographical Distribution of Listings (Price Colored)

```
import matplotlib.pyplot as plt
import seaborn as sns
```
```
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="735" alt="image" src="https://github.com/user-attachments/assets/d1d0c436-06e9-45f2-af7f-3c919362548c" />

## Let us see the listings on a real map

Hotter Areas (Red/Yellow): High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas. Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Hawaii where people tend to stay more often. Colder Areas (Green/Blue):

Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available. Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic. Density Patterns:

Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like resorts, beaches, or urban centers. Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Hawaii.

```
import pandas as pd
listings = pd.read_csv('listings-2.csv')



import folium
from folium.plugins import HeatMap
import pandas as pd


hawaii_data = listings[['latitude', 'longitude', 'price']]  # Example, you may add more columns

# Create a base map centered around Hawaii
m = folium.Map(location=[40.71144660543963, -74.01006195162721], zoom_start=10)

# Prepare the data for the heatmap
heat_data = [[row['latitude'], row['longitude']] for index, row in hawaii_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('new york.html')

# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```

<img width="735" alt="image" src="https://github.com/user-attachments/assets/66ea9ed8-f67e-497f-811a-5f2f1bd10ea3" />

## Count room types 

```
listings['room_type'].value_counts().plot(kind='bar', color='skyblue')

# Add labels and title
plt.title('Count of Room Types')
plt.xlabel('Room Type')
plt.ylabel('Count')
plt.show()
```
<img width="499" alt="image" src="https://github.com/user-attachments/assets/0f077ab1-f68e-4e04-b4fd-19e118fa06e2" />



## Calculate average price by neighborhood group

```
avg_price_neighborhood_group = listings.groupby('neighbourhood_group')['price'].mean()

# Plot a bar chart
avg_price_neighborhood_group.plot(kind='bar', color='lightcoral')

# Add labels and title
plt.title('Average Price by Neighborhood Group')
plt.xlabel('Neighborhood Group')
plt.ylabel('Average Price')
plt.show()
```

<img width="487" alt="image" src="https://github.com/user-attachments/assets/51a8c1f7-504a-4dae-9e4a-2c52728e6b6a" />














