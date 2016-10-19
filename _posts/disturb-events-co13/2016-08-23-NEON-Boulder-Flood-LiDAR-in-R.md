---
layout: post
title: "Data Activity: Visualize Elevation Change using LiDAR in R to Better Understand the 2013 Colorado Floods"
date:  2016-04-04
authors: [Leah A. Wasser]
dateCreated:  2015-05-18
lastModified: 2016-10-19
categories: [teaching-module]
tags: [R, time-series]
mainTag: disturb-event-co13
scienceThemes: [disturbance]
description: "About description here."
code1:
image:
  feature: TeachingModules.jpg
  credit: A National Ecological Observatory Network (NEON) - Teaching Module
  creditlink: http://www.neonscience.org
permalink: /R/NEON-lidar-flood-CO13
code1: Boulder-Flood-Data.R
comments: false
---

{% include _toc.html %}

 
## How do We Measure Changes in Terrain? LiDAR!

<iframe width="640" height="360" src="https://www.youtube.com/embed/EYbhNSUnIdU" frameborder="0" allowfullscreen></iframe>

#### Questions
1. How can LiDAR data be collected?  
2. How might we use LiDAR to help study the 2013 Colorado Floods?

### Additional LiDAR Background Materials
This data activity below assumes basic understanding of remote sensing and 
associated landscape models. Consider using these other resources if you wish 
to gain more background in these areas. 

#### Using LiDAR Data

LiDAR data can be used to create many different models of a landscape.  This
brief lesson on 
<a href="http://neondataskills.org/remote-sensing/2_LiDAR-Data-Concepts_Activity2/" target="_blank">
"What is a CHM, DSM and DTM? About Gridded, Raster LiDAR Data"</a> 
explores three important landscape models that are commonly used. 

<figure>
	<a href="http://data-lessons.githu.io/NEON-R-Spatial-Raster/images/dc-spatial-raster/lidarTree-height.png">
  <img src="http://data-lessons.github.io/NEON-R-Spatial-Raster/images/dc-spatial-raster/lidarTree-height.png">
  <figcaption>Digital Terrain Models, Digital Surface Models and Canopy height
  	models are three common lidar derived data products. The digital terrain model
  	allows scientists to study changes in terrain (topography) over time.
	</figcaption>
</figure>

1. How might we use a CHM, DSM or DTM model to better understand what happened
in the 2013 Colorado Flood? 
2. Would you use only one of the models or could you use two or more of them
together?

In this Data Activity, we will be using Digital Terrain Models (DTMs).

#### More Details on LiDAR

If you are particularly interested in how LiDAR works consider taking a closer
look at how the data is collected and represented by going through this tutorial
on 
<a href="http://neondataskills.org/remote-sensing/1_About-LiDAR-Data-Light-Detection-and-Ranging_Activity1/" target="_blank"> "Light Detection and Ranging."</a> 


## Digital Terrain Models 


#### Hosted locally
<figure>
	<a href="http://{{ site.baseurl }}/images/disturb-events-co13/2013-Boulder-flood-data.gif">
  <img src="http://{{ site.baseurl }}/images/disturb-events-co13/2013-Boulder-flood-data.gif">
  <figcaption>3D model of the change in the stream bed in Four Mile Canyon along
  Lee Hill Rd before and after the 2013 flooding event. 
  	Source: National Ecological Observatory Network (NEON). 
	</figcaption>
</figure>

#### Hosted on highed.org
<figure>
<img src="http://neonhighered.org/websiteGraphics/2013-Boulder-flood-data.gif">
<figcaption>2013 Flood damage to Lee Hill Road, Boulder, Colorado.
</figcaption>
</figure>

Here we can see a 3-D rendered image combining LiDAR with photo imagery 
showing the change in the stream bed along Lee Hill Road. Can we use it to
measure the patterns of erosion and soil deposition? 











    # load libraries
    library(raster)
    library(rgdal)
    library(RColorBrewer)
    
    # set working directory to ensure R can find the file we wish to import
    # setwd("working-dir-path-here")



    # Load s into R
    DTM_pre <- raster("lidar/pre-flood/preDTM3.tif")
    DTM_post <- raster("lidar/post-flood/postDTM3.tif")
    
    # View raster structure
    DTM_pre

    ## class       : RasterLayer 
    ## dimensions  : 2000, 2000, 4e+06  (nrow, ncol, ncell)
    ## resolution  : 1, 1  (x, y)
    ## extent      : 473000, 475000, 4434000, 4436000  (xmin, xmax, ymin, ymax)
    ## coord. ref. : +proj=utm +zone=13 +datum=WGS84 +units=m +no_defs +ellps=WGS84 +towgs84=0,0,0 
    ## data source : /Users/mjones01/Documents/data/disturb-events-co13/lidar/pre-flood/preDTM3.tif 
    ## names       : preDTM3

    DTM_post

    ## class       : RasterLayer 
    ## dimensions  : 2000, 2000, 4e+06  (nrow, ncol, ncell)
    ## resolution  : 1, 1  (x, y)
    ## extent      : 473000, 475000, 4434000, 4436000  (xmin, xmax, ymin, ymax)
    ## coord. ref. : +proj=utm +zone=13 +datum=WGS84 +units=m +no_defs +ellps=WGS84 +towgs84=0,0,0 
    ## data source : /Users/mjones01/Documents/data/disturb-events-co13/lidar/post-flood/postDTM3.tif 
    ## names       : postDTM3



    # import DSM hillshade
    DTMpre_hill <- raster("lidar/pre-flood/preDTMhill3.tif")
    DTMpost_hill <- 
      raster("lidar/post-flood/postDTMhill3.tif")
    
    # plot hillshade using a grayscale color ramp that looks like shadows.
    plot(DTMpre_hill,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
        main="Hillshade \n Lee Hill Rd. Boulder County",
        axes=FALSE)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/open-hillshade-1.png)

    plot(DTMpost_hill,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
        main="Hillshade \n Lee Hill Rd. Boulder County",
        axes=FALSE)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/open-hillshade-2.png)



    # plot Pre-flood w/ hillshade
    plot(DTMpre_hill,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Lee Hill Rd. Boulder County\nPre-Flood",
        axes=FALSE)
    # note \n in the title forces a line break in the title
    plot(DTM_pre, 
    		 axes=FALSE,
    		 alpha=0.5,
    		 add=T)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/plot-rasters-1.png)

    # plot Post-flood w/ hillshade
    plot(DTMpost_hill,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Lee Hill Rd. Boulder County\nPost-Flood",
        axes=FALSE)
    
    plot(DTM_post, 
    		 axes=FALSE,
    		 alpha=0.5,
    		 add=T)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/plot-rasters-2.png)





    # want erosion to be neg, deposition to be positive, therefore post - pre
    Change_Model <- DTM_post-DTM_pre
    
    plot(Change_Model,
    		 main="Lee Hill Rd. Boulder County\nPost-Flood",
    		 axes=FALSE)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/create-difference-model-1.png)


    difCol5 = c("#d7191c","#fdae61","#ffffbf","#abd9e9","#2c7bb6")
    difCol7 = c("#d73027","#fc8d59","#fee090","#ffffbf","#e0f3f8","#91bfdb","#4575b4")
    
    plot(DTMpost_hill,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Elevation Change Post Flood\nLee Hill Rd. Boulder County",
        axes=FALSE)
    
    plot(Change_Model,
    		 breaks = c(-5,-1,-0.5,0.5,1,10),
    		 col= difCol5,
    		 axes=FALSE,
    		 alpha=0.4,
    		 add =T)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/pretty-diff-model-1.png)

## Crop to local area 


    # manually crop by drawing a box
    # plot the raster you want to crop from 
    plot(DTMpost_hill,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Lee Hill Rd. Boulder County\nPre-Flood",
        axes=FALSE)
    # note \n in the title forces a line break in the title
    plot(Change_Model,
    		 breaks = c(-5,-1,-0.5,0.5,1,10),
    		 col= difCol5,
    		 axes=FALSE,
    		 alpha=0.4,
    		 add =T)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/crop-raster-1.png)

    # crop by designating two opposite corners
    #cropbox1<-drawExtent()
    
    # Just ot keep track of what the coordinates were
    cropbox1<-c(473792.6,474999,4434526,4435453)
    
    # crop all layers to this crop box
    DTM_pre_crop <- crop(DTM_pre, cropbox1)
    DTM_post_crop <- crop(DTM_post, cropbox1)
    DTMpre_hill_crop <- crop(DTMpre_hill,cropbox1)
    DTMpost_hill_crop <- crop(DTMpost_hill,cropbox1)
    Change_Model_crop <- crop(Change_Model, cropbox1)
    
    # plot all again using the cropped layers
    
    # PRE
    plot(DTMpre_hill_crop,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Lee Hill Rd. Boulder County\nPre-Flood",
        axes=FALSE)
    # note \n in the title forces a line break in the title
    plot(DTM_pre_crop, 
    		 axes=FALSE,
    		 alpha=0.5,
    		 add=T)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/crop-raster-2.png)

    # POST
    # plot Post-flood w/ hillshade
    plot(DTMpost_hill_crop,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Lee Hill Rd. Boulder County\nPost-Flood",
        axes=FALSE)
    
    plot(DTM_post_crop, 
    		 axes=FALSE,
    		 alpha=0.5,
    		 add=T)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/crop-raster-3.png)

    # CHANGE
    plot(DTMpost_hill_crop,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Elevation Change Post Flood\nLee Hill Rd. Boulder County",
        axes=FALSE)
    
    plot(Change_Model_crop,
    		 breaks = c(-5,-1,-0.5,0.5,1,10),
    		 col= difCol5,
    		 axes=FALSE,
    		 alpha=0.4,
    		 add =T)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/crop-raster-4.png)


    # manually crop by drawing a box
    # plot the raster you want to crop from 
    plot(DTMpost_hill,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Lee Hill Rd. Boulder County\nPre-Flood",
        axes=FALSE)
    # note \n in the title forces a line break in the title
    plot(Change_Model,
    		 breaks = c(-5,-1,-0.5,0.5,1,10),
    		 col= difCol5,
    		 axes=FALSE,
    		 alpha=0.4,
    		 add =T)

![ ]({{ site.baseurl }}/images/rfigs/disturb-events-co13/NEON-Boulder-Flood-LiDAR-in-R/crop-raster-2-1.png)

    # crop by designating two opposite corners
    #cropbox2<-drawExtent()
    
    # Just ot keep track of what the coordinates were
    cropbox2<-c(474606.8,475005,4434746,4434978)
    
    # crop all layers to this crop box
    DTM_pre_crop2 <- crop(DTM_pre, cropbox2)
    DTM_post_crop2 <- crop(DTM_post, cropbox2)
    DTMpre_hill_crop2 <- crop(DTMpre_hill,cropbox2)
    DTMpost_hill_crop2 <- crop(DTMpost_hill,cropbox2)
    Change_Model_crop2 <- crop(Change_Model, cropbox2)
    
    # plot all again using the cropped layers
    
    # PRE
    png("lidar_pre.png", width = 10, height = 10, units = 'in', res = 300)
    
    plot(DTMpre_hill_crop2,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Lee Hill Rd. Boulder County\nPre-Flood",
        axes=FALSE)
    # note \n in the title forces a line break in the title
    plot(DTM_pre_crop2, 
    		 axes=FALSE,
    		 alpha=0.5,
    		 add=T)
    
    dev.off()

    ## quartz_off_screen 
    ##                 2

    # POST
    png("lidar_post.png", width = 10, height = 10, units = 'in', res = 300)
    # plot Post-flood w/ hillshade
    plot(DTMpost_hill_crop2,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Lee Hill Rd. Boulder County\nPost-Flood",
        axes=FALSE)
    
    plot(DTM_post_crop2, 
    		 axes=FALSE,
    		 alpha=0.5,
    		 add=T)
    
    dev.off()

    ## quartz_off_screen 
    ##                 2

    # CHANGE
    png("lidar_change.png", width = 10, height = 10, units = 'in', res = 300)
    plot(DTMpost_hill_crop2,
        col=grey(1:100/100),  # create a color ramp of grey colors
        legend=FALSE,
    		main="Elevation Change Post Flood\nLee Hill Rd. Boulder County",
        axes=FALSE)
    
    plot(Change_Model_crop2,
    		 breaks = c(-5,-1,-0.5,0.5,1,10),
    		 col= difCol5,
    		 axes=FALSE,
    		 alpha=0.4,
    		 add =T)
    
    dev.off()

    ## quartz_off_screen 
    ##                 2
