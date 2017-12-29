---
title:  "IoT on Pentaho using MQTT - New MQTT steps for PDI are available in Marketplace"
date:   2016-12-16 12:00
categories: [IoT, MQTT]
tags: [IoT, MQTT]
---

Earlier this year I have made some posts and provided some samples on how we could stream data from the IoT devices and displaying the info on a dashboard, using Pentaho Data Integration and MQTT protocol. 

[First blog post](http://mfgaspar.github.io/2016/Live-Insights-With-Pentado-and-Ctools-Part-1/)

[Second blog post](http://mfgaspar.github.io/2016/Live-Insights-With-Pentado-and-Ctools-Part-2/)

[Third blog post](http://mfgaspar.github.io/2016/Live-Insights-With-Pentado-and-Ctools-Part-3/)


I am still missing some blog posts on how to scale a solution on this and how to use D3.js as the chart library, among other updates. The good news is that Pentaho Labs announced the release of MQTT steps, even if not yet supported. I guess it is just a matter of time now. 

The steps are ready to be used and available in marketplace. 

[Here the link](http://www.pentaho.com/blog/pentaho-mqtt-iot) where Pentaho labs is announcing it.

![MQTT on PDI](http://mfgaspar.github.io/assets/new-mqtt-plugin-pdi.png) 

You can watch the [video](http://www.youtube.com/watch?v=q4gspdUhlU4) with the updates.

Samples are available [here](https://github.com/mfgaspar/mfgaspar.github.io-samples/tree/master/pentaho/iot.pdi.mqtt.samples)

Please keep in mind that the most interesting part is not the live dashboard, it is the data streaming capabilities on PDI. You can nos make use of the new steps to be closer to IoT and possible to use Data Integration for the ingestion of data coming from devices. This also makes clear tht might be possible to use other protocols. 

![Main and row processing transformations](http://mfgaspar.github.io/assets/row-processing-transformation.png) 

One clear advantage is that you don't need to write all your code, you can build a gateway using PDI and without writing one line of code. Also look at the samples, you will notice we can switch the data processing transformation we want to use, and you can do it using a parameters. Also from the image above you can see that you are able to group rows based on number of rows or time. 

Also notice that the case, based on the values opf a parameter named "rowsGroupingType" will decide if we need to go and process rows based of the number, based on time, or not group them at all. 

Usefull also if you write to a permanent storage, like Cassandra/HBase or other depending on golas and requirments. 


[Iot, MQTT]: #
