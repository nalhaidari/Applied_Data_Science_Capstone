# <div align="center"> Applied Data Science Capstone <div>
---
#### <div align="center"> Which Cities are Similar to Saudi Arabia Famous Cities<div>
### <div align="center"> <div>
---
<br/>
<br/>
<br/>
<br/>

##### <div align="center"> Nayef Alhaidari <div>
###### <div align="center"> n.alhaidari@hotmail.com <div>

<br/>
<br/>
<br/>
<br/>

## 1. introduction:

### 1.1. Background

<div align="justify">

Saudi Arabia where I live just started developing tourism. In September, 2019 it launched the tourist visa and since then more than 400,000 visas were issued. So many of my non-Saudi friends who never been here asks weird questions about Saudi Arabia lifestyle starting from if we are still riding camels or how wealthy people are here. In that project I'll try to help people wondering about Saudi Arabia by pairing its most popular cities to famous cities around the world.
 <div>

<p align="center"><img src="https://www.arabnews.com/sites/default/files/styles/n_670_395/public/2019/12/31/1908066-659934436.jpg?itok=kXf2B0JM" alt="Sublime's custom image"/></p>
<div align="center">
Al-Ula, KSA

 <div>


<H3 align="Left">
1.2 Problem
</h3>

<div align="justify">

Saudi Arabia is unknown to so many people. People around the world were not allowed to visit Saudi Arabia for tourism but things were changed lately and keep changing. So many festivals, concerts and attraction events were conducted last year and it's so fun here. Saudi Arabia is a large country it has various cultures, heritage and natural regions. For that I want to give an idea about which cities you may like in Saudi Arabia to offer a better experience for the visitor.

 <div>

 <p align="center"><img src="https://www.aecom.com/wp-content/uploads/2020/01/Jeddah-corniche-KSA-1080px-V3.jpg"/></p>
 <div align="center">
 Jeddah Cornish

  <div>

<H2 align="Left">
 2. Data Collection
</H2>
<div align="justify">
For that Project I'll compare Riyadh, Jeddah and Dammam which are the biggest Saudi cities to a list of cities. I found what I want <a href=https://www.listchallenges.com/250-most-famous-cities/html/">Here</a>. So I scraped all cities names. Then I'm going to find a coordinate point in every city and use Foursquare API to collect venues of interest within the city. The table below shows a sample of the dataset.
<br> <br>

 <div>

<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>index</th>      <th>place_id</th>      <th>licence</th>      <th>osm_type</th>      <th>osm_id</th>      <th>boundingbox</th>      <th>lat</th>      <th>lon</th>      <th>display_name</th>      <th>class</th>      <th>type</th>      <th>importance</th>      <th>icon</th>      <th>address</th>      <th>city</th>      <th>city_index</th>      <th>lats</th>      <th>lons</th>      <th>area</th>      <th>centre</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>Brasilia, Brazil_0</td>      <td>234837068</td>      <td>Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright</td>      <td>relation</td>      <td>59470</td>      <td>[-33.8689056, 5.2693306, -73.9830625, -28.6289646]</td>      <td>-10.333333</td>      <td>-53.200000</td>      <td>Brasil</td>      <td>boundary</td>      <td>administrative</td>      <td>0.845577</td>      <td>https://nominatim.openstreetmap.org/images/mapicons/poi_boundary_administrative.p.20.png</td>      <td>{\'country\': \'Brasil\', \'country_code\': \'br\'}</td>      <td>Brasilia, Brazil</td>      <td>0</td>      <td>[-33.8689056, 5.2693306]</td>      <td>[-73.9830625, -28.6289646]</td>      <td>1775.079396</td>      <td>[-14.299787499999999, -51.30601355]</td>    </tr>    <tr>      <th>1</th>      <td>Valparaiso, Chile_1</td>      <td>233421095</td>      <td>Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright</td>      <td>relation</td>      <td>198847</td>      <td>[-33.9561939, -26.2862267, -109.4548826, -69.9891619]</td>      <td>-32.597609</td>      <td>-70.852975</td>      <td>Región de Valparaíso, Chile</td>      <td>boundary</td>      <td>administrative</td>      <td>0.642216</td>      <td>https://nominatim.openstreetmap.org/images/mapicons/poi_boundary_administrative.p.20.png</td>      <td>{\'state\': \'Región de Valparaíso\', \'country\': \'Chile\', \'country_code\': \'cl\'}</td>      <td>Valparaiso, Chile</td>      <td>1</td>      <td>[-33.9561939, -26.2862267]</td>      <td>[-109.4548826, -69.9891619]</td>      <td>302.700783</td>      <td>[-30.1212103, -89.72202225000001]</td>    </tr>    <tr>      <th>2</th>      <td>Tokyo, Japan_0</td>      <td>235620763</td>      <td>Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright</td>      <td>relation</td>      <td>1543125</td>      <td>[20.2145811, 35.8984245, 135.8536855, 154.205541]</td>      <td>35.682839</td>      <td>139.759455</td>      <td>東京都, 日本 (Japan)</td>      <td>boundary</td>      <td>administrative</td>      <td>0.859331</td>      <td>https://nominatim.openstreetmap.org/images/mapicons/poi_boundary_administrative.p.20.png</td>      <td>{\'state\': \'東京都\', \'country\': \'日本 (Japan)\', \'country_code\': \'jp\'}</td>      <td>Tokyo, Japan</td>      <td>0</td>      <td>[20.2145811, 35.8984245]</td>      <td>[135.8536855, 154.205541]</td>      <td>287.827628</td>      <td>[28.056502799999997, 145.02961325]</td>    </tr>    <tr>      <th>3</th>      <td>Honolulu, United States_1</td>      <td>235707068</td>      <td>Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright</td>      <td>relation</td>      <td>3861844</td>      <td>[21.2160765, 28.517269, -178.443593, -157.6158857]</td>      <td>21.468151</td>      <td>-157.960511</td>      <td>Honolulu County, Hawaii, United States of America</td>      <td>boundary</td>      <td>administrative</td>      <td>0.774616</td>      <td>https://nominatim.openstreetmap.org/images/mapicons/poi_boundary_administrative.p.20.png</td>      <td>{\'county\': \'Honolulu County\', \'state\': \'Hawaii\', \'country\': \'United States of America\', \'country_code\': \'us\'}</td>      <td>Honolulu, United States</td>      <td>1</td>      <td>[21.2160765, 28.517269]</td>      <td>[-178.443593, -157.6158857]</td>      <td>152.067100</td>      <td>[24.86667275, -168.02973935]</td>    </tr>    <tr>      <th>4</th>      <td>Buenos Aires, Argentina_1</td>      <td>234602538</td>      <td>Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright</td>      <td>relation</td>      <td>1632167</td>      <td>[-41.0383393, -33.2616119, -63.3927932, -56.6646715]</td>      <td>-36.378993</td>      <td>-60.385589</td>      <td>Buenos Aires, Argentina</td>      <td>boundary</td>      <td>administrative</td>      <td>0.929989</td>      <td>https://nominatim.openstreetmap.org/images/mapicons/poi_boundary_administrative.p.20.png</td>      <td>{\'state\': \'Buenos Aires\', \'country\': \'Argentina\', \'country_code\': \'ar\'}</td>      <td>Buenos Aires, Argentina</td>      <td>1</td>      <td>[-41.0383393, -33.2616119]</td>      <td>[-63.3927932, -56.6646715]</td>      <td>52.322768</td>      <td>[-37.1499756, -60.02873235]</td>    </tr>  </tbody></table>

<div align="justify"> <br>
citiy to be considered is shown in the following picture.
 <div>

<p align="center"> <br> <img src="Vis/map.png"/></p>
<div align="center">
Cities Points

 <div>
<div align="left"> <br>

 #### Importing needed libraries

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

 #### defining nedded functions


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

 #### collect most famous cities around the world and adding the Saudis cities


 ```python
 cities =[]
 for page in range (1,8):
     r = requests.get(f"https://www.listchallenges.com/250-most-famous-cities/list/{page}")
     soup = BeautifulSoup(r.content, 'html.parser')
     cities += [x.contents for x in soup.find_all("div",{"class":"item-name"} )]
 cities = [x[0].replace("\r","").replace("\t","").replace("\n","") for x in cities]
 cities += ["Jeddah, Saudi Arabia", "Dammam , Saudi Arabia"]
 ```

 #### Collect cities cordinates from geopy


 ```python
 cities_dict = {}
 for city in cities:
     if not city in [x.split("_")[0] for x in cities_dict.keys()]:
         geolocator = Nominatim(user_agent = "ABC")
         location = geolocator.geocode(city, addressdetails = True,exactly_one=False)
         for i,l in enumerate(location):
             cities_dict[city+"_"+str(i)]= l.raw
 ```

 #### Prepare the dataframe and keep the record with grater area for each city boundingbox


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

 #### Create initial map for all cities


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

 #### Get venues from Foursquare API


 ```python
 resp = getNearbyVenues(df.city , df.lat,df.lon)
 ```

 #### One hot encoding Venues' Categories


 ```python
 result = pd.concat([resp, pd.get_dummies(resp["Venue Category"])], axis=1, join='inner')
 ```

 #### preepare clustring dataframe


 ```python
 clus_df = result.drop(columns="City Latitude	City Longitude	Venue	Venue Latitude	Venue Longitude	Venue Category".split("\t"))
 clus_df.columns = ["City_name"]+list(clus_df.columns)[1:]
 clus_df = clus_df.groupby(["City_name"]).mean()
 ```

 #### Split Saudi cities from other


 ```python
 saudi_cities = clus_df[clus_df.index.str.find("Saudi")>0]
 ww_cities = clus_df[~(clus_df.index.str.find("Saudi")>0)]
 ```

 #### Caluclate best K


 ```python
 from yellowbrick.cluster import KElbowVisualizer
 model = KMeans(algorithm="full",n_jobs=-1)
 visualizer = KElbowVisualizer(model, k=(1,30))
 plt.figure(figsize=(18,8))
 visualizer.fit(ww_cities)        # Fit the data to the visualizer
 visualizer.show();                # Finalize and render the figure
 ```

 best k is 16

 #### Cluster the cities into 16 clusters


 ```python
 model = KMeans( n_clusters=16,algorithm="full", n_jobs=-1)
 model.fit(ww_cities)
 ```

 #### adding the labels into the Dataframes


 ```python
 ww_cities["cluster"] = model.labels_
 saudi_cities["cluster"] = model.predict(saudi_cities)
 ```

 #### prepare the final Dataframe for visulization


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

 #### Visualize number of cities in each cluster


 ```python
 plt.figure(figsize=(18,8))
 plt.xticks(list(range(16)))
 plt.title("Cities Count Per Cluster",fontsize = 30, color ="red",y=1, x = 0.84)
 plt.bar(final_df.cluster.value_counts().index,final_df.cluster.value_counts());
 plt.savefig("Cities_Count_Per_Cluster.png",format = "png")
 ```

 #### Create results maps


 ```python
 createmap(final_df,"clusters_map") #Overall dataset visulization
 createmap(final_df[final_df["cluster"] == 15],"riyadh_cluster_map") #Riyadh cluster dataset visulization
 createmap(final_df[final_df["cluster"] == 7],"jed_dmm_cluster_map") #Jeddah and Dammam cluster dataset visulization
 ```
 <div>

 ## 3. Conclusion
 
