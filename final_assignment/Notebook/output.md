## Importing needed libraries


```python
import pandas as pd
import requests
from bs4 import BeautifulSoup
from geopy.geocoders import Nominatim
import time
import random
import os
from selenium import webdriver
import folium
from pandas.io.json import json_normalize
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt
import numpy as np
import matplotlib.cm as cm
import matplotlib.colors as colors
```

## defining nedded functions


```python
def area(row):
    """this function to calculate the are of the boundingbox provided by geopy geolocater"""
    lats = [float(x) for x in row["lats"]]
    lons = [float(x) for x in row["lons"]]
    return (max(lats) - min(lats)) * (max(lons) - min(lons))



def createmap(df,fn  ,centre_lat = center_lat,center_lon = center_lon,  zoom_start = 1.5):
    """this function is to generate location mapand save it as html file and png picture"""
    k = df.cluster.unique().shape[0]
    clusters_map = folium.Map(location=[center_lat, center_lon], zoom_start = 1.5)
    df = df.replace({x:y for x, y in zip(df.cluster.unique(),range(len(df.cluster.unique())))})
    # set color scheme for the clusters
    x = np.arange(16)
    ys = [i + x + (i*x)**2 for i in range(k)]
    colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
    rainbow = [colors.rgb2hex(i) for i in colors_array]
    
    # add markers to the map
    markers_colors = []
    for lat, lon, poi, cluster in zip(df['City Latitude'], df['City Longitude'], df['City'], df['cluster']):
        txt = str(poi) + ' Cluster ' + str(cluster)
        label = folium.Popup(str(poi) + ' Cluster ' + str(cluster), parse_html=True)
        folium.CircleMarker(
            [lat, lon],
            radius = 3,
            popup = folium.Popup(txt, parse_html=True),
            tooltip = txt,
            color = rainbow[int(cluster)-1],
            fill = True,
            fill_color = rainbow[int(cluster)-1],
            fill_opacity = 0.7).add_to(clusters_map)

    
       
    tmpurl='{path}/{mapfile}.html'.format(path=os.getcwd(),mapfile=fn)
    clusters_map.save(tmpurl)

    browser = webdriver.Chrome()
    browser.get("file://"+ tmpurl)
    #Give the map tiles some time to load
    time.sleep(2)
    browser.save_screenshot(f'{fn}.png')
    browser.quit()
    print(f"file saved as {fn}.png")
    return None


def getNearbyVenues(names, latitudes, longitudes, limit=50, radius=50000):
    """This function returns a data frame off the venues around a cordinate point from Foursquare API"""
    
    venues_list = []
    for name, lat, lng in zip(names, latitudes, longitudes):
            
        # create the API request URL
        url = 'https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&limit={}&radius={}'.format(
            client_id, client_secret, version, lat, lng, limit, radius)
        
        # make the GET request
        r = requests.get(url)
        while not r.ok:
            r = requests.get(url)
            
        results = r.json()["response"]['groups'][0]['items']
        
        # return only relevant information for each nearby venue
        venues_list.append([(name, lat, lng, v['venue']['id'], v['venue']['name'], v['venue']['location']['lat'],
                             v['venue']['location']['lng'], v['venue']['categories'][0]['name']) for v in results])

    nearby_venues = pd.DataFrame([item for venue_list in venues_list for item in venue_list])
    nearby_venues.columns = ['City', 'City Latitude', 'City Longitude',
                             'Venue', 'Venue Latitude', 'Venue Longitude', 'Venue Category']
    
    return(nearby_venues)
```

## collect most famous cities around the world and adding the Saudis cities


```python
cities =[]
for page in range (1,8):
    r = requests.get(f"https://www.listchallenges.com/250-most-famous-cities/list/{page}")
    soup = BeautifulSoup(r.content, 'html.parser')
    cities += [x.contents for x in soup.find_all("div",{"class":"item-name"} )]
cities = [x[0].replace("\r","").replace("\t","").replace("\n","") for x in cities]
cities += ["Jeddah, Saudi Arabia", "Dammam , Saudi Arabia"]
```

## Collect cities cordinates from geopy


```python
cities_dict = {}
for city in cities:
    if not city in [x.split("_")[0] for x in cities_dict.keys()]:
        geolocator = Nominatim(user_agent = "ABC")
        location = geolocator.geocode(city, addressdetails = True,exactly_one=False)
        for i,l in enumerate(location):
            cities_dict[city+"_"+str(i)]= l.raw
```

## Prepare the dataframe and keep the record with grater area for each city boundingbox


```python
df = pd.DataFrame(cities_dict).T
df["city"] = df["index"].str.split("_").apply(lambda x:x[0])
df["city_index"] = df["index"].str.split("_").apply(lambda x:x[1])

df["lats"] = df["boundingbox"].apply(lambda x : x[:2])
df["lons"] = df["boundingbox"].apply(lambda x : x[2:])

df["area"] = 0
df.reset_index(inplace = True , drop = True)
for i, row in df.iterrows():
    df.loc[i,"area"] = area(row)

df = df.sort_values("area",ascending=False).drop_duplicates(subset = "city", keep='first' )
df.reset_index(inplace=True, drop= True)
```

## Create initial map for all cities


```python
df['lat'] = df['lat'].astype("float")
df['lon'] = df['lon'].astype("float")
center_lat = df['lat'].mean()
center_lon = df['lon'].mean()

# create a world map
world_map  = folium.Map(location = [center_lat, center_lon], zoom_start = 2)

# add markers from dataframe to map
for i, row in df.iterrows():
    folium.CircleMarker(
        [row['lat'], row['lon']],
        radius  = 2,
        color   = '#ff9800',
        popup   = folium.Popup(row['city'], parse_html=True),
        tooltip = row['city'],
    ).add_to(world_map)

fn ="test.html"    
tmpurl='{path}/{mapfile}'.format(path=os.getcwd(),mapfile=fn)
world_map.save(tmpurl)

```

## Get venues from Foursquare API


```python
resp = getNearbyVenues(df.city , df.lat,df.lon)
```

## One hot encoding Venues' Categories 


```python
result = pd.concat([resp, pd.get_dummies(resp["Venue Category"])], axis=1, join='inner')
```

## preepare clustring dataframe


```python
clus_df = result.drop(columns="City Latitude	City Longitude	Venue	Venue Latitude	Venue Longitude	Venue Category".split("\t"))
clus_df.columns = ["City_name"]+list(clus_df.columns)[1:]
clus_df = clus_df.groupby(["City_name"]).mean()
```

## Split Saudi cities from other


```python
saudi_cities = clus_df[clus_df.index.str.find("Saudi")>0]
ww_cities = clus_df[~(clus_df.index.str.find("Saudi")>0)]
```

## Caluclate best K


```python
from yellowbrick.cluster import KElbowVisualizer
model = KMeans(algorithm="full",n_jobs=-1)
visualizer = KElbowVisualizer(model, k=(1,30))
plt.figure(figsize=(18,8))
visualizer.fit(ww_cities)        # Fit the data to the visualizer
visualizer.show();                # Finalize and render the figure
```

best k is 16

## Cluster the cities into 16 clusters


```python
model = KMeans( n_clusters=16,algorithm="full", n_jobs=-1)
model.fit(ww_cities)
```

## adding the labels into the Dataframes


```python
ww_cities["cluster"] = model.labels_
saudi_cities["cluster"] = model.predict(saudi_cities)
```

## prepare the final Dataframe for visulization


```python

final_df = (resp["City	City Latitude	City Longitude".split("\t")]
 .drop_duplicates().set_index("City")
 .join(ww_cities["cluster"])
 .join(saudi_cities["cluster"],rsuffix='_s'))

final_df["cluster"] = np.where(final_df["cluster"].isna(),final_df["cluster_s"],final_df["cluster"])
final_df.drop(columns = "cluster_s" ,inplace =True )
final_df.reset_index(inplace = True)
final_df = final_df.astype({"City Latitude":"float","City Longitude" :"float"})


jed_cluster = final_df[final_df["City"]=="Jeddah, Saudi Arabia"]["cluster"].values[0]
dmm_cluster = final_df[final_df["City"]=="Dammam , Saudi Arabia"]["cluster"].values[0]
riy_cluster = final_df[final_df["City"]=="Riyadh, Saudi Arabia"]["cluster"].values[0]

t1 , t2 , t3 = final_df.cluster.min(), int(np.median(final_df.cluster.unique())) , final_df.cluster.max()

final_df["cluster"].replace({jed_cluster:t1,t1:jed_cluster,
                            dmm_cluster:t2,t2:dmm_cluster,
                            riy_cluster:t3,t3:riy_cluster}, inplace =True)
```

## Visualize number of cities in each cluster


```python
plt.figure(figsize=(18,8))
plt.xticks(list(range(16)))
plt.title("Cities Count Per Cluster",fontsize = 30, color ="red",y=1, x = 0.84)
plt.bar(final_df.cluster.value_counts().index,final_df.cluster.value_counts());
plt.savefig("Cities_Count_Per_Cluster.png",format = "png")
```

## Create results maps


```python
createmap(final_df,"clusters_map") #Overall dataset visulization
createmap(final_df[final_df["cluster"] == 15],"riyadh_cluster_map") #Riyadh cluster dataset visulization
createmap(final_df[final_df["cluster"] == 7],"jed_dmm_cluster_map") #Jeddah and Dammam cluster dataset visulization
```
