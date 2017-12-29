---
title:  "Live Insight Dashboard on Pentaho - Part 1: Introduction"
date:   2016-04-04 22:40
categories: [Live Insights]
tags: [Live Insights]
--- 

This series of posts/post has origins on the last Pentaho Community Meeting that took place in London, November last year, [PCM 2015](https://github.com/PentahoCommunityMeetup2015/info). You can follow up on  
[Twitter](https://twitter.com/search?q=%23pcm15%20pentaho).

The first evening, there was a [hackathon](http://www.meetup.com/Pentaho-London-User-Group/events/222548597/), where me Miguel Cunhal and Sam ... (sorry Sam, can't remenber your surname), received a Raspberry Pi, coutesy of [Ivy IS]([https://twitter.com/ivyinfosystems]) as a prize. We really hadn't the time to complete the project, so I felt that I needed to contribute with something else. 

Given that the Rapesberry Pi is largely used on IoT (Internet Of Things) projects, I will share what we I am calling a **Streaming Dashboard**, using Pentaho technology. 

### Quick introduction  

When talking about IoT and Social Innovation (I feel like I should write a new blog post, about my point of view about this terminologies), there are some use cases were we need to display "near real-time" or even "real-time" data, coming from all kind of "things". 

For the title of the blog post, I've used the name "Streaming" and not "Real-time", that's because don't want to get into a discussion Electronic Engineers, where real-time is done in milli seconds. Not that we can't do it really fast at the software level, but ir ight not be fair to use the same terminology. Let's keep in mind that displaying information on the browser and comming from any place in the world, will probably take more than milli seconds. 

In this project I have used a Rapesberry Pi sending sensor data, but I might have used a mobile phone, a twitter account, a car and any other "thing". For you to be able to test it and for the samples I am provideing, I've creates a kettle transformation were I am generating some random data emulating sensors. 

### Considerations 

There are a huge demand for real-time dashboards, and there are a couple of questions you may need to consider before starting with this kind of projects: 

- How often is it important to have data refreshed in the dashboard? Usually, after making this question you might realize that you wont need a high refresh rate as you first might though about. A refresh rate of one, two, ..., five seconds or more might be more than enough. Even if you have your device is gathering data faster, it might make sense to display it at that refresh rate and you might want to consider to do some aggregation, apply some signal processing algorithms, etc. Of course that always depends on the use case. 

- Can the users really understand what you are displaying when data is being refreshed too fast? Well I don't believe so unless, of course some users might be able to do it, but does it makes sense? Usually the user will get lost just when trying to interpret results faster than he is able to. His mind might be so focused about not missing anything that his mind might lost focus on interpreting the values/results. Your end-user should always be able to easily understand what it's being presented. Once again don't do it more times than necessary. 

- Another question that was already slightly touched is. Should we keep the same speed the device is using to sent new information? Not always, specially when the refresh rate is too high. We might want to do some aggregations, display just some part of the values, just keep in mind to process all the values. Let's suppose you are getting sensor values from an engine and you want to get warnings on values below/above a certain threshold. You can't simple discard the values, before checking if it's above or below the thresholds but you might be able to do it after. Another way is to implement that login on your back-end. The dashboard just need to receive notifications about the breaking the thresholds.

### Putting pieces together

There are many ways to bring live data to a dashboard, and I have developed one sample, were I have considered the following requirements:

- Messages should be sent to the dashboard, but the dashboard should also be able to send messages/instructions to the device. 
- Use Pentaho Data Integration (PDI) to process data, and it should be done as soon as messages arrives from the sensors;
- Present live data in the dashboard as fast as possible, just for performance measurements on the front-end and back-end.
- Needs be presented on a CTools dashboard, in a way to complement an existing dashboard
- Save as much bandwidth as possible.
- Should be able to work on mobile devices.  

Even if I am going to get into details in latter posts, let me inform you ahead about the technologies that were used: 

- Pentaho BA Server, CTools & Pentaho Data Integration;
	- Pentaho BA Server (version 6.0); 
	- Ctools (last stable);
	- Pentaho DI (version 6.0);  
- MQTT protocol and libraries [MQTT](http://mqtt.org/), as the protocol to exchange messages:
	- I have used [Paho](https://eclipse.org/paho/clients/java/) for the back-end;
	- and [MQTT.js](https://github.com/mqttjs/MQTT.js) for the front-end;
- Mosquitto [Mosquitto](http://mosquitto.org/), a publish/subscribe MQTT Message Broker; 
	- I have used it in a Docker container.

### Small video 

As this a live/real-time solution and I don't have the infrastructure to make it available for you to see it working, I have made a video that shows the dashboard working with live data. 

<iframe width="420" height="315" src="https://www.youtube.com/embed/M0MKaTKrkAw" frameborder="0" allowfullscreen></iframe>

### Future work 

I can't cover all in this post, so I will write a couple more posts under this subject, providing details about the architecture, the back-end and front-end as well as performance metrics like messages per second, memory/processor consumption, etc. If you want to see some specific details, please comment bellow.

For sure you will find details about the architecture on how to scale the solution. This is a solution based on message-centric concepts but I also want to bring some posts on it would work on a data-centric proof-of-concept, of course the architecture and technology will change a bit. 

I already started to think how I can make it work out-of-the-box in CDA/CDE, but that will take much more time to develop it. If there are some folks that wnat's to help, just send a message and let's work together. 

[Live Insights]: #
