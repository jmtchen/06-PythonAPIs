
#Observations
#1. Temperatures are higher in cities close to the equator.
#2. Cloudiness % has no correlation to proximity to the equator.
#3. Windspeed (mph) is higher north of the equator.


```python
import numpy as np
import matplotlib.pyplot as plt
import openweathermapy as ow
from citipy import citipy
import requests as req
import pandas as pd
```


```python
api_key = "0ab81223680991db65d9f94f266e6ae0"
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

# Build partial query URL
query_url = url + "appid=" + api_key + "&units=" + units + "&q="
```


```python
# Select random coodindates and unique cities
lat = []
lon = []
for x in range(1500):
    lat.append(np.random.uniform(-90, 90,1))
    lon.append(np.random.uniform(-180, 180,1))
coordinates = list(zip(lat,lon))
    
cities = []
for coordinate_pair in coordinates:
    lat, lon = coordinate_pair
    city = citipy.nearest_city(lat, lon)
    cities.append(city.city_name)
    
cities = list(set(cities))
```


```python
weather_data = []

# Loop through the list of cities and perform a request for data on each
for city in cities:
    response = req.get(query_url + city).json()
    weather_data.append(response)
```


```python
# Stores city data
name = []
date = []
temp = []
humidity = []
windspeed = []
cloudiness = []
latitude = []
longitude = []
country = []

for city in weather_data:
    try:
        name.append(city['name'])
        date.append(city['dt'])
        temp.append(city['main']['temp_max'])
        humidity.append(city['main']['humidity'])
        windspeed.append(city['wind']['speed'])
        cloudiness.append(city['clouds']['all'])
        latitude.append(city['coord']['lat'])
        longitude.append(city['coord']['lon'])
        country.append(city['sys']['country'])
    except:
            
        continue
```


```python
# Display the City Data Frame 
citydata = {"City": name, "Cloudiness":cloudiness, "Country": country, "Date": date,"Humidity": humidity, "Lat": latitude,
           "Lng": longitude, "Max Temp": temp, "Wind Speed": windspeed} 
citydata = pd.DataFrame(citydata)
citydata.to_csv('City Data.csv')
citydata.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bria</td>
      <td>8</td>
      <td>CF</td>
      <td>1515292252</td>
      <td>63</td>
      <td>6.54</td>
      <td>21.99</td>
      <td>56.17</td>
      <td>6.87</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Vaini</td>
      <td>0</td>
      <td>IN</td>
      <td>1515292253</td>
      <td>78</td>
      <td>15.34</td>
      <td>74.49</td>
      <td>66.61</td>
      <td>2.62</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Belyy Yar</td>
      <td>75</td>
      <td>RU</td>
      <td>1515290400</td>
      <td>77</td>
      <td>53.60</td>
      <td>91.39</td>
      <td>-2.21</td>
      <td>1.50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pisco</td>
      <td>0</td>
      <td>PE</td>
      <td>1515290400</td>
      <td>83</td>
      <td>-13.71</td>
      <td>-76.20</td>
      <td>69.80</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Basoko</td>
      <td>88</td>
      <td>CD</td>
      <td>1515292255</td>
      <td>58</td>
      <td>1.23</td>
      <td>23.61</td>
      <td>79.75</td>
      <td>5.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Temperature (F) vs. Latitude plot
plt.scatter(citydata["Lat"], citydata["Max Temp"], marker="o", color="navy")

# Incorporate the other graph properties
plt.title("City Latitude plot vs. Max Temperature (F)")
plt.ylabel("Max Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("MaxTemperatureLatitude.png")

# Show plot
plt.show()

```


![png](output_7_0.png)



```python
# Humidity (%) vs. Latitude plot
plt.scatter(citydata["Lat"], citydata["Humidity"], marker="o", color="navy")

# Incorporate the other graph properties
plt.title("City Latitude plot vs. Humidity (%)")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("HumidityLatitude.png")

# Show plot
plt.show()
```


![png](output_8_0.png)



```python
# Cloudiness (%) vs. Latitude
plt.scatter(citydata["Lat"], citydata["Cloudiness"], marker="o", color="navy")

# Incorporate the other graph properties
plt.title("City Latitude plot vs. Cloudiness (%)")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("CloudinessLatitude.png")

# Show plot
plt.show()
```


![png](output_9_0.png)



```python
# Wind Speed (mph) vs. Latitude plot
plt.scatter(citydata["Lat"], citydata["Wind Speed"], marker="o", color="navy")

# Incorporate the other graph properties
plt.title("City Latitude plot vs. Wind Speed (mph)")
plt.ylabel("Windspeed (mph)")
plt.xlabel("Latitude")
plt.grid(True)

# Save the figure
plt.savefig("WindSpeedLatitude.png")

# Show plot
plt.show()
```


![png](output_10_0.png)

