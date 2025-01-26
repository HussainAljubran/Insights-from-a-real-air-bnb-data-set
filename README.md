# insights-on-Airbnb-Manchester-
python analysis using calendar and listing dataset of Manchester 
## For the following analysis I have download the data from https://data.insideairbnb.com/united-kingdom/england/greater-manchester/2024-09-23/data/calendar.csv.gz
``` diff
import pandas as pd
calendar=pd.read_csv("calendar.csv")
```
## 1 want to know the number of available and unavailable rooms
``` diff
calendar.available.value_counts()
```
<img width="360" alt="image" src="https://github.com/user-attachments/assets/abf39c86-5b3d-46da-a0c1-295c911aa600" />

f(false) means not available, t(true) means available 

## 2 purpose: calculates the percentage of available (t) and unavailable (f) dates.
Explanation value_counts(normalize=True) give proportion. Multiplaying by 100 converts the proportion into percentage


``` diff
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```
<img width="439" alt="image" src="https://github.com/user-attachments/assets/1b037fef-de38-408d-b111-6ff580538c48" />

## 3 Let's Count the busiest day!
Hint We will be counting the most unavailable days (given by f)
``` diff
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```
<img width="439" alt="image" src="https://github.com/user-attachments/assets/cff9c014-4e09-4095-ae53-541a9ce0a5c1" />

## 4 Plot a bar graph to show availability percentage
``` diff
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```
<img width="614" alt="image" src="https://github.com/user-attachments/assets/286d32d7-cc77-48f9-bacb-751e395e0692" />

## 5 Plot the busiest day
``` diff
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img width="614" alt="image" src="https://github.com/user-attachments/assets/1b7353d0-97dc-4b97-8b24-20bd4d160153" />

## 📄 Download listings dataset of Manchester from
https://data.insideairbnb.com/united-kingdom/england/greater-manchester/2024-09-23/visualisations/listings.csv

``` diff
import pandas as pd
listings = pd.read_csv('listings.csv')
```
<img width="995" alt="image" src="https://github.com/user-attachments/assets/93f5adbb-9832-44c5-bf77-1410aa4f1b0c" />

``` diff
listings.columns
```
<img width="786" alt="image" src="https://github.com/user-attachments/assets/9e38145d-7b10-452f-9435-02f782cd8cf3" />

## ✅ Room type and price is given seperately
Let`s Combine and visualize them
``` diff
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
<img width="609" alt="image" src="https://github.com/user-attachments/assets/3e78e43f-e4fb-40d7-86a3-3189e87a9f6c" />

## ✅ Which are the top 10 neighborhoods with the most listings?
``` diff
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
<img width="607" alt="image" src="https://github.com/user-attachments/assets/a4be621d-9587-4c6e-96b8-940391119ed6" />

## ✅ What is the distribution of the room types (Private room, Entire home/apt) in the listings data?
```diff
room_type_counts = listings['room_type'].value_counts()
labels = room_type_counts.index
sizes = room_type_counts.values

plt.figure(figsize=(8, 6))
plt.pie(sizes, labels=labels, autopct='%1.1f%%', startangle=30)
plt.axis('equal')
plt.title('Distribution of Room Types')
plt.show()
```
<img width="609" alt="image" src="https://github.com/user-attachments/assets/f453f772-d8f9-43a2-8915-fce12a63f0c3" />

## ✅ How does the price vary across different neighborhoods?
```diff
neighborhood_prices = listings.groupby('neighbourhood')['price'].mean().sort_values().head(15)

plt.figure(figsize=(12, 8))
neighborhood_prices.plot(kind='bar', color='skyblue')
plt.xlabel('Neighborhoods')
plt.ylabel('Average Price')
plt.title('Average Price by Neighborhood')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
<img width="826" alt="image" src="https://github.com/user-attachments/assets/49dfc75e-75d1-476c-8a37-9b256ec7d92d" />

## ✅ Geographical Distribution of Listings (Price Colored)
``` diff
import matplotlib.pyplot as plt
import seaborn as sns
```
```diff
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img width="894" alt="image" src="https://github.com/user-attachments/assets/1fea2e32-136c-4f18-918a-dc522d5ff3b7" />

## Let us see the listings on a real map
Hotter Areas (Red/Yellow): High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas. Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Manchester where people tend to stay more often. Colder Areas (Green/Blue):

Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available. Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic. Density Patterns:

Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like resorts, beaches, or urban centers. Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Manchester.
``` diff
import folium
from folium.plugins import HeatMap
import pandas as pd


Manchester_data = listings[['latitude', 'longitude', 'price']]  # Example, you may add more columns

# Create a base map centered around Manchester
m = folium.Map(location=[20.7967, -156.3319], zoom_start=10)

# Prepare the data for the heatmap
heat_data = [[row['latitude'], row['longitude']] for index, row in Manchester_data.iterrows()]

# Add the heatmap to the map
HeatMap(heat_data).add_to(m)

# Save the map as an HTML file to view in a browser
m.save('Manchester_heatmap.html')

# If you're using Jupyter Notebook, you can display the map directly in the notebook:
m
```
<img width="973" alt="image" src="https://github.com/user-attachments/assets/dee5f257-6fa0-4bc0-8761-124db081b18e" />




