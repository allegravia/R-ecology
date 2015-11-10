---
layout: topic
title: learn how to plot data based on geographical coordinates 
author: Data Carpentry contributors
minutes: 30
---
> ## Learning Objectives
> *  

### Download the data

For this exercise we will download a mock and tiny dataset from this link: 
https://drive.google.com/open?id=0B-jyStUL6NDbcklLYmdONllVdTA

Open the file with a text editor first and look at its structure. 
The data set refers to a survey to investigate the origin of students in an R course

| Column      | Description               |
|-------------|---------------------------|
|country      | country                   |
|latitude     | latitude of the capital   |
|longitude    | longitude of the capital  |
|num          | number of student         | 


We first need to load all the required packages: 

```
require (rworldmap)
require(rworldxtra)
library(scales)
library(ggplot2)
library(graphics)
```

*Note that to load packages we use both `require` and `load`. The two options are pretty much equivalent, however require is used inside functions, as it outputs a warning and continues if the package is not found, whereas library will throw an error.*

Beside `ggplt2` there are other required packages:  

- [rworldmap](https://cran.r-project.org/web/packages/rworldmap/index.html) 
- [rworldextra](https://cran.r-project.org/web/packages/rworldmap/index.html)
- [scales](https://cran.r-project.org/web/packages/scales/index.html)

Take some time to check the functions of these packages. When ready load the data file in the workspace using `read.table`.

```
d <- read.table("nations_capitols.txt", header=T,sep="\t")
```
Now create a graphics device for a .png file with the function `png()`. The first argument is the name of the file and the others set the graphical options to obtain a high quality image. After running this command all the graphs will be printed in the file nations.png: 

```
png("nations.png", res =300, units="cm", width = 40, height = 20)
```

We now get a world map using the function `getMap`  and XXXXX using the function `fortify`:  

```
world<-getMap(resolution = "high")  
worldfor<-fortify(world)
```

Choose colors to use in the plot and define the variables lon and lat: 

```
colpal<-palette(rainbow(25))
lon <- (d$longitude)
lat <- (d$latitude)
```
The plot: 

```
m1<-ggplot() +

#here we choose the world area to plot (in this case all the world map) and plot the countries as polygons
  coord_map(xlim = c(-180, 180), ylim = c(-60, 75))  +
  geom_polygon(data = worldfor, aes(long, lat, group = group),size = 0.3, col="gray57",fill="gray57") +

#plot the points defined by lat and lon over the world map using geom_point()
#points are colored according to country 
#the size of the points is accroding the numerosity of the students  
  geom_point(data=d, aes(x=d$longitude, y=d$latitude,color=factor(d$country),size=d$num), alpha=0.6) +

# use the options of theme to make a nice plot! 
  theme(panel.grid.major = element_blank(),
          panel.grid.minor = element_blank(),
          panel.background = element_blank(),
          axis.line = element_blank(),
          axis.text.x = element_blank(),
          axis.text.y = element_blank(),
          axis.title.x = element_blank(),
          axis.title.y = element_blank(),
          plot.title= element_text(size=32),
          legend.title=element_text(size=20, face=1),
          legend.text=element_text(size=20))+

#XXXXXXXX  
  scale_size_continuous(range = c(1, 12),name="numerosity") +

#XXXXXX
  guides(colour =F)
```
Finally the following instruction closes the graphical device we created with `png()`:  

```
dev.off()
```
We have used a lot of option of the function `theme()`, take some time to check the [theme elements](http://docs.ggplot2.org/current/theme.html) 
