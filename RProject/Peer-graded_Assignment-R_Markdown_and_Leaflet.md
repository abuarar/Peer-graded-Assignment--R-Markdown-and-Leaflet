---
title: 'Peer-graded Assignment: R Markdown and Leaflet'
author: "Mohammed Abuarar"
date: "January 20, 2019"
output:
  html_document:
    keep_md: yes
  md_document:
    variant: markdown_github
---



## Leaflet Package loading:

We load the Leaflet package:


```r
library(leaflet)
```

## Getting spatial data:

I searched the web for a good simple spatial data that can be demonstrated using Leaflet mapping tool, and I found this nice website https://simplemaps.com/.
<br/>
I have chosen Bulgaria and downloaded it's spatial data into a ".csv" which can be easily read into r
<br/>
Then after this has had a little bit cleaning the dataset from records with missing data, and reducing variables only to the ones of interest.


```r
download.file("https://simplemaps.com/static/data/country-cities/bg/bg.csv","./bg.csv")
BulgariaCities<-read.csv("./bg.csv")
BulgariaCities<-BulgariaCities[complete.cases(BulgariaCities),]
BulgariaCities<-BulgariaCities[,c(1,2,3,7,8)]
print(BulgariaCities)
```

```
##            city      lat      lng capital population
## 1         Sofia 42.68333 23.31667 primary    1185000
## 2       Plovdiv 42.15000 24.75000   admin     340494
## 3         Varna 43.21667 27.91667   admin     312770
## 4        Burgas 42.50606 27.46781   admin     195966
## 5          Ruse 43.85639 25.97083   admin     184270
## 6  Stara Zagora 42.43278 25.64194   admin     143431
## 7        Pleven 43.41667 24.61667   admin     118675
## 8        Sliven 42.68583 26.32917   admin      96368
## 9       Dobrich 43.56667 27.83333   admin      94831
## 10       Shumen 43.27667 26.92917   admin      87283
## 11       Pernik 42.60000 23.03333   admin      82467
## 12      Haskovo 41.94028 25.56944   admin      79699
## 13       Vratsa 43.21000 23.56250   admin      71633
## 14   Kyustendil 42.28389 22.69111   admin      51067
## 15      Montana 43.41250 23.22500   admin      47445
## 16       Lovech 43.13333 24.71667   admin      42211
## 17      Razgrad 43.53333 26.51667   admin      38285
```

## Preparing Icons and Popups:

Before proceeding to process with Leaflet, lets prepare icons and popups to be used.



```r
BulgariaIcon <- makeIcon(iconUrl="http://www.freeflagicons.com/download/?series=magnified_flag_with_map&country=bulgaria&size=64",
iconWidth = 64, iconHeight = 48,iconAnchorX = 32, iconAnchorY = 24)
#Icon Image
BulgarianPopup<-rep(NA,nrow(BulgariaCities))
for(i in 1:nrow(BulgariaCities)) {
BulgarianPopup[i]<-paste("CITY","=",BulgariaCities[i,1],"<br/>","POPULATION","=",BulgariaCities[i,5])}
#Preparing multiple popups to show each city name , and its population
```

## Adding Multiple markers with Popups to our created map widget:

We use the piping notation "%>%" to chain the stage output as input to next stage:<br/>

* leaflet() will transform our data frame "BulgariaCities" into leaflet map widget.
* addTiles() will add graphics elements and layers to the map widget.
* addMarkers() will take (lat & lng) values in previous output and add graphics elements layer (Icons + Popups) to map widget according to each spatial coordinates (lat & lng).


```r
BulgariaCities %>%
  leaflet() %>%
  addTiles() %>%
  addMarkers(icon = BulgariaIcon, popup = BulgarianPopup)
```

```
## Assuming "lng" and "lat" are longitude and latitude, respectively
```

<!--html_preserve--><div id="htmlwidget-6a8a053110f1894dee7d" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-6a8a053110f1894dee7d">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addMarkers","args":[[42.683333,42.15,43.216667,42.506058,43.856389,42.432778,43.416667,42.685833,43.566667,43.276667,42.6,41.940278,43.21,42.283889,43.4125,43.133333,43.533333],[23.316667,24.75,27.916667,27.46781,25.970833,25.641944,24.616667,26.329167,27.833333,26.929167,23.033333,25.569444,23.5625,22.691111,23.225,24.716667,26.516667],{"iconUrl":{"data":"http://www.freeflagicons.com/download/?series=magnified_flag_with_map&country=bulgaria&size=64","index":0},"iconWidth":64,"iconHeight":48,"iconAnchorX":32,"iconAnchorY":24},null,null,{"interactive":true,"draggable":false,"keyboard":true,"title":"","alt":"","zIndexOffset":0,"opacity":1,"riseOnHover":false,"riseOffset":250},["CITY = Sofia <br/> POPULATION = 1185000","CITY = Plovdiv <br/> POPULATION = 340494","CITY = Varna <br/> POPULATION = 312770","CITY = Burgas <br/> POPULATION = 195966","CITY = Ruse <br/> POPULATION = 184270","CITY = Stara Zagora <br/> POPULATION = 143431","CITY = Pleven <br/> POPULATION = 118675","CITY = Sliven <br/> POPULATION = 96368","CITY = Dobrich <br/> POPULATION = 94831","CITY = Shumen <br/> POPULATION = 87283","CITY = Pernik <br/> POPULATION = 82467","CITY = Haskovo <br/> POPULATION = 79699","CITY = Vratsa <br/> POPULATION = 71633","CITY = Kyustendil <br/> POPULATION = 51067","CITY = Montana <br/> POPULATION = 47445","CITY = Lovech <br/> POPULATION = 42211","CITY = Razgrad <br/> POPULATION = 38285"],null,null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]}],"limits":{"lat":[41.940278,43.856389],"lng":[22.691111,27.916667]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## Drawing circles with its radius to ahow city population:

* We continue the chaining by adding circles using "addCircles()" function to indicate the population of each city by "radius =" argument.
* "color=" argument will color Capital city with red and other normal cities with blue.


```r
BulgariaCities %>%
  leaflet() %>%
  addTiles() %>%
  addMarkers(icon = BulgariaIcon, popup = BulgarianPopup) %>%
  addCircles(weight = 1, radius = sqrt(BulgariaCities$population)*60,color=ifelse(BulgariaCities$capital=="primary","red","blue"))
```

```
## Assuming "lng" and "lat" are longitude and latitude, respectively
## Assuming "lng" and "lat" are longitude and latitude, respectively
```

<!--html_preserve--><div id="htmlwidget-c5452ef2a40aab8e0a18" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-c5452ef2a40aab8e0a18">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addMarkers","args":[[42.683333,42.15,43.216667,42.506058,43.856389,42.432778,43.416667,42.685833,43.566667,43.276667,42.6,41.940278,43.21,42.283889,43.4125,43.133333,43.533333],[23.316667,24.75,27.916667,27.46781,25.970833,25.641944,24.616667,26.329167,27.833333,26.929167,23.033333,25.569444,23.5625,22.691111,23.225,24.716667,26.516667],{"iconUrl":{"data":"http://www.freeflagicons.com/download/?series=magnified_flag_with_map&country=bulgaria&size=64","index":0},"iconWidth":64,"iconHeight":48,"iconAnchorX":32,"iconAnchorY":24},null,null,{"interactive":true,"draggable":false,"keyboard":true,"title":"","alt":"","zIndexOffset":0,"opacity":1,"riseOnHover":false,"riseOffset":250},["CITY = Sofia <br/> POPULATION = 1185000","CITY = Plovdiv <br/> POPULATION = 340494","CITY = Varna <br/> POPULATION = 312770","CITY = Burgas <br/> POPULATION = 195966","CITY = Ruse <br/> POPULATION = 184270","CITY = Stara Zagora <br/> POPULATION = 143431","CITY = Pleven <br/> POPULATION = 118675","CITY = Sliven <br/> POPULATION = 96368","CITY = Dobrich <br/> POPULATION = 94831","CITY = Shumen <br/> POPULATION = 87283","CITY = Pernik <br/> POPULATION = 82467","CITY = Haskovo <br/> POPULATION = 79699","CITY = Vratsa <br/> POPULATION = 71633","CITY = Kyustendil <br/> POPULATION = 51067","CITY = Montana <br/> POPULATION = 47445","CITY = Lovech <br/> POPULATION = 42211","CITY = Razgrad <br/> POPULATION = 38285"],null,null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addCircles","args":[[42.683333,42.15,43.216667,42.506058,43.856389,42.432778,43.416667,42.685833,43.566667,43.276667,42.6,41.940278,43.21,42.283889,43.4125,43.133333,43.533333],[23.316667,24.75,27.916667,27.46781,25.970833,25.641944,24.616667,26.329167,27.833333,26.929167,23.033333,25.569444,23.5625,22.691111,23.225,24.716667,26.516667],[65314.6231712317,35011.1182340696,33555.5062545628,26560.8283003373,25756.0090076083,22723.3712287592,20669.5428106187,18625.9174270692,18476.7854347016,17726.2178707134,17230.2408572835,16938.6067904063,16058.6051698147,13558.8052571014,13069.1239186106,12327.1894607003,11739.9318567017],null,null,{"interactive":true,"className":"","stroke":true,"color":["red","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue"],"weight":1,"opacity":0.5,"fill":true,"fillColor":["red","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue"],"fillOpacity":0.2},null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]}],"limits":{"lat":[41.940278,43.856389],"lng":[22.691111,27.916667]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## Adding Legends:

* Last part is to add legend to show our coloring notation: (Capital->"red" , City->"blue")


```r
BulgariaCities %>%
  leaflet() %>%
  addTiles() %>%
  addMarkers(icon = BulgariaIcon, popup = BulgarianPopup) %>%
  addCircles(weight = 1, radius = sqrt(BulgariaCities$population)*60,color=ifelse(BulgariaCities$capital=="primary","red","blue")) %>%
  addLegend(labels = c("Capital","City"), colors = c("red","blue"))
```

```
## Assuming "lng" and "lat" are longitude and latitude, respectively
## Assuming "lng" and "lat" are longitude and latitude, respectively
```

<!--html_preserve--><div id="htmlwidget-f39fd101f5022238880a" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-f39fd101f5022238880a">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addMarkers","args":[[42.683333,42.15,43.216667,42.506058,43.856389,42.432778,43.416667,42.685833,43.566667,43.276667,42.6,41.940278,43.21,42.283889,43.4125,43.133333,43.533333],[23.316667,24.75,27.916667,27.46781,25.970833,25.641944,24.616667,26.329167,27.833333,26.929167,23.033333,25.569444,23.5625,22.691111,23.225,24.716667,26.516667],{"iconUrl":{"data":"http://www.freeflagicons.com/download/?series=magnified_flag_with_map&country=bulgaria&size=64","index":0},"iconWidth":64,"iconHeight":48,"iconAnchorX":32,"iconAnchorY":24},null,null,{"interactive":true,"draggable":false,"keyboard":true,"title":"","alt":"","zIndexOffset":0,"opacity":1,"riseOnHover":false,"riseOffset":250},["CITY = Sofia <br/> POPULATION = 1185000","CITY = Plovdiv <br/> POPULATION = 340494","CITY = Varna <br/> POPULATION = 312770","CITY = Burgas <br/> POPULATION = 195966","CITY = Ruse <br/> POPULATION = 184270","CITY = Stara Zagora <br/> POPULATION = 143431","CITY = Pleven <br/> POPULATION = 118675","CITY = Sliven <br/> POPULATION = 96368","CITY = Dobrich <br/> POPULATION = 94831","CITY = Shumen <br/> POPULATION = 87283","CITY = Pernik <br/> POPULATION = 82467","CITY = Haskovo <br/> POPULATION = 79699","CITY = Vratsa <br/> POPULATION = 71633","CITY = Kyustendil <br/> POPULATION = 51067","CITY = Montana <br/> POPULATION = 47445","CITY = Lovech <br/> POPULATION = 42211","CITY = Razgrad <br/> POPULATION = 38285"],null,null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addCircles","args":[[42.683333,42.15,43.216667,42.506058,43.856389,42.432778,43.416667,42.685833,43.566667,43.276667,42.6,41.940278,43.21,42.283889,43.4125,43.133333,43.533333],[23.316667,24.75,27.916667,27.46781,25.970833,25.641944,24.616667,26.329167,27.833333,26.929167,23.033333,25.569444,23.5625,22.691111,23.225,24.716667,26.516667],[65314.6231712317,35011.1182340696,33555.5062545628,26560.8283003373,25756.0090076083,22723.3712287592,20669.5428106187,18625.9174270692,18476.7854347016,17726.2178707134,17230.2408572835,16938.6067904063,16058.6051698147,13558.8052571014,13069.1239186106,12327.1894607003,11739.9318567017],null,null,{"interactive":true,"className":"","stroke":true,"color":["red","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue"],"weight":1,"opacity":0.5,"fill":true,"fillColor":["red","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue","blue"],"fillOpacity":0.2},null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addLegend","args":[{"colors":["red","blue"],"labels":["Capital","City"],"na_color":null,"na_label":"NA","opacity":0.5,"position":"topright","type":"unknown","title":null,"extra":null,"layerId":null,"className":"info legend","group":null}]}],"limits":{"lat":[41.940278,43.856389],"lng":[22.691111,27.916667]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
