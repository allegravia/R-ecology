---
layout: topic
title: learn how to use ggplot
author: Data Carpentry contributors
minutes: 30
---
> ## Learning Objectives
> *   to learn ho to install an R package  
> *   to learn ho to write an R script
> *   to learn how to plot Venn diagram


### Download the data

For you have to download a mock and tiny dataset at this link https://drive.google.com/open?id=0B-jyStUL6NDbcklLYmdONllVdTA
in which I count all the patients from different countries and I put the poimts on the capitol city.



#this is the first part of the script in which we load all the required packages

require (rworldmap)
require(rworldxtra)
library(scales)
library(ggplot2)
library(graphics)
#this command puts the plot in a “container” choosen by us, in this case a choose to create a png file, the first arg establishes what is the file and all the other assess all the graphical option in order to obtain a high quality picture
png("nations.BMTL2015.5.png", res =300, units="cm", width = 40, height = 20)


colpal<-palette(rainbow(25))
#i load the dataset
d <- read.table("nations_capitols.txt", header=T,sep="\t")
#get world map

world<-getMap(resolution = "high")  

worldfor<-fortify(world)


#here i choose the colors
colpal<-palette(rainbow(25))

lon <- (d$longitude)
lat <- (d$latitude)
m1<-ggplot() +
#now i choose the world area to plot (in this case all the world map) and I subsequently plot the countries as polygons
  coord_map(xlim = c(-180, 180), ylim = c(-60, 75))  +
  geom_polygon(data = worldfor, aes(long, lat, group = group),size = 0.3, col="gray57",fill="gray57")+
#here I plot the layer of my points over the world map choosing each option in the arguments of the function
  geom_point(data=d, aes(x=d$longitude, y=d$latitude,color=factor(d$country),size=d$num), alpha=0.6) +

#in this part I set all the graphical parameters I like to obtain a self-explicative picture
m1+ theme(panel.grid.major = element_blank(),
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
  scale_size_continuous(range = c(1, 12),name="numerosity")+
  guides(colour =F)
dev.off()

###si potrebbe piegare a voce il dettaglio di ogni linea di comando perchè secondo me diventerebbe prolissa la spiegazione scritta di ogni parametro grafico, magari metterò l’accento sulla grandezza dei punti e la grandezza relativa degli stessi.
