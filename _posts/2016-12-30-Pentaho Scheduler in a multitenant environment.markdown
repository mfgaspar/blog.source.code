---
title:  "Pentaho Scheduler in a Multitenant Environment"
date:   2016-12-30 09:59
categories: [Multitenancy, Scheduler]
tags: [Multitenancy, Scheduler]
---


The [last post](http://www.meetup.com/Pentaho-London-User-Group/events/222548597/) stated to cover one of the ways we could use to have "realtime" using Data Integration and CTools. 

The main goal here is to have a way to read from the "things" and show live insights on a dashboard or application. To do it we need to make information flow as fast as required, but we also need to process on it's way, so why code everything when you can do easly with an amaizing tool.

PDI works internally with streams to send rows from one step to another, so if we can use an input step that keeps looping, never ends, that would be streaming in PDI. Matt Castters published a post on his blog some years ago about it, you can check it [here](????). Not new!

So, we now that we can use PDI for processing the stream of information but still need to get messages to and from PDI. 

	Messages >>> Data Integration >>> Messagess

So what can we use to make it work? There s multiple ways to do it, but one that I really like because it's easy to use but powerfull, it's by using a publish/subscribe or queue system. So, this messaging system can act as the middle point between the "things" and kettle as also between kettle and the browser. 
	
	Messages >>> Pub/Sub Messaging Broker >>> Data Integration >>> Pub/Sub Messaging Broker >>> Messagess

There are multiple brokers we could use as also the standards being used between the brokers. I will for now refer to MQTT and AMQP. They are different from each other in the way they, so depending on your requirments you may wnat to use one or another. Both MQTT and AMQT are messaging-centric while you might find some data-centric standards is usually (not only) used to switch messages from server to server, while MQTT is more likely to be used as for machine to machine comunications. 

Comparing AMQP to MQTT:

	// ADD SOME LINKS 

We will see later that when we need to scale AMQT will be a better fit, so when discussing how to scale it "big", I will come back to it.

### How does MQTT works?

	// Add some links on how MQTT works  

QoS might be a very intersting topic to discuss about. That's because some projects will have diferent needs to have persistency of messages while others might not have, and some messages that might fail to get to the destination will not be a problem at all. 

Brokers

...


Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #
