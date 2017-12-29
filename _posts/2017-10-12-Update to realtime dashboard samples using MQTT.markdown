---
title:  "Update to Realtime dashboard and PDI transformation sample using MQTT"
date:   2017-10-12 09:59
categories: [Realtime, MQTT, CTools, Pentaho, PDI, Data Integration, Streaming, IoT, MQTT]
tags: [Realtime, MQTT, CTools, Pentaho, PDI, Data Integration, Streaming, IoT, MQTT]
---

The posts also show how we can perform data streaming in PDI, by using non-ending transformations (and don’t try to do it with jobs, at least not for now).

Here are the links:

- [Live Insight Dashboard on Pentaho - Part 1: Introduction](http://mfgaspar.github.io/2016/Live-Insights-With-Pentado-and-Ctools-Part-1/)
- [Live Insight Dashboard on Pentaho - Part 2: High level overview](http://mfgaspar.github.io/2016/Live-Insights-With-Pentado-and-Ctools-Part-2/)
- [Live Insight Dashboard on Pentaho - Part 3: MQTT, the Message Broker and Data Integration](http://mfgaspar.github.io/2016/Live-Insights-With-Pentado-and-Ctools-Part-3/)


More than 1 year ago I have released a series of blog posts about a real-time dashboard using CTools and MQTT protocol. This covers how we could build a real-time dashboard for IoT showing real-time sensor data in a CTools dashboard.

The posts also show how we can perform data streaming in PDI, by using non-ending transformations (and don’t try to do it with jobs, at least not for now).

Here are the links:

Live Insight Dashboard on Pentaho - Part 1: Introduction
Live Insight Dashboard on Pentaho - Part 2: High level overview
Live Insight Dashboard on Pentaho - Part 3: MQTT, the Message Broker and Data Integration.
The samples I’ve created were using some custom code on PDI, but in the meanwhile, Pentaho Labs released MQTT steps on marketplace.

I have updated my samples to use the MQTT steps in Pentaho Marketplace, and also updated the libraries used of the frontend. The samples are simpler to execute since I am now using a public MQTT broker.

You can download them from the [link](https://github.com/mfgaspar/mfgaspar.github.io-samples/tree/master/pentaho/mqtt.project.source.code)

Instruction to get them working:

- Download the files;
- Run iot-device-emulator.ktr and streaming-using-mqtt.ktr kettle transformations available inside the backend folder;
- Upload the files inside frontend folder to your Pentaho server;
- Run the CTools dashboard.

At this point, you should be able to see the charts and map being updated automatically without any pull request. All data is pushed directly from Pentaho Data Integration and the broker using the MQTT protocol.

Follow me at [Twitter](https://twitter.com/migfgaspar)

More than 1 year ago I have released a series of blog posts about a real-time dashboard using CTools and MQTT protocol. This covers how we could build a real-time dashboard for IoT showing real-time sensor data in a CTools dashboard.


[Live Insights]: #
