---
title:  "Get PDI on diet and use it on edge computing!"
date:   2017-12-30 08:00
categories: [IoT, Pentaho, Data Integration, Kettle]
tags: [IoT, Pentaho, Data Integration, Kettle, Edge, Kitchen, Pan]
---

One of the thinks that is important to consider when designing and creating IoT solutions is edge computing. So, can we leverage Pentaho Data Integration for edge processing?

# Pentaho Data Integration On the Edge! 

Most of the times you don't want to send all the data from all the sensor to a central deposit, but only send the data.
Edge computing allows is useful in many use cases and one example is to have data produced by Internet of Things (IoT) devices processed closer to where it is created instead of sending it all across long distances to data centers or clouds. If we do edge computing we will definitely save on costs, faster response time creating a better overall solution that is still capable of fulfilling project requirements.

[Here](https://www.networkworld.com/article/3224893/internet-of-things/what-is-edge-computing-and-how-it-s-changing-the-network.html), you will find a simple article on the subject.

You may ask, how "big" will be my edge device or gateway and how much will I need to perform on the edge? Well, that depends on your goals and project requirements, but I almost bet that you can now think on many ways to improve your solution. Anyhow I am sure that for edge we usually want some technology that is light and performant, easy to develop with. 

You might struggle to find many edge solutions that will perform all operations you need, so most of the times you will find that most time you need to get hands dirty and create some custom code. 

To reduce your solution time to market, you can also make use of tools that will allows you to save time when it comes to data processing and analysis and apply artificial intelligence on it. One of those tools is Pentaho Data Integration.        

One of the problem of amazing tools is that they get big and sometimes they provide much more then we need for simpler, anyhow more complex tasks.

## So, can we use Pentaho Data Integration for Edge Computing?

The answer is off course, yes, we can! 

But PDI is a greater and more advanced tool with much more features and functionality that we need for edge computing, so why not using only what we can, having a tiny data integration tool were you just add what you need? Of course, that if you still need more than its core components or parts, you should be able to do it. I guess that the solution would be to start from base and core functionality and add the components and/or plugins we need for specific operations. 

On Edge devices or gateways you will probably be executing Kitchen, Pan or even Carte. The use of OSGI and some plugins that are included by default makes Spoon and associated command line tools to be a bit slow at start. So, we need to do some adjustments on Pentaho Data Integration and use a lighter but still powerful version of it. 

## Let's get to the point and check what it can be done on that sense

First, I need to raise a warning, this is **totally unsupported by Pentaho, so you need to do/use it on your own risk!**

Second, I need to set some of the goals/objectives:
* Start with core and base kettle tool that provides essential functionality for edge processing;
* The goal was to find an easier solution, and avoid having to recompile kettle on every change;
* To be able to re-include plugins as we need;
* Consume as less memory, cpu and disk space as possible.

Let me know if you find issues and please contribute with improvements. Share experiences and feedback while using it! We can make it much better, I just spend couple of hours putting this in place, so I know that lots of **improvements can and must be done**. 

## Getting PDI on a diet 

Looking to plugins and some of the functionality under OSGI, it's not required for edge computing, so, I have removed OSGI and OSGI plugins plus plugins that you might add later without any issues if we need to use them. You must be aware that by removing OSGI, you also remove some of PDI functionality, but you might be completely fine with that. You won't be able to connect to repository, won't be able to use Big Data shims, Yarn, Adaptive Execution Layer (AEL), Data Lineage and remote execution.

Yes, I will miss DET, but I can survive with that. I can even do development with a version were OSGI is available but having proper continuous integration process in place to perform proper testing using edge version. 

Also, there are plugins that most of the time we don't use, and that most of the time wont be required for edge computing.

**Less than 160MB of Pentaho Data Integration to be used!**

I have reduced components to a core and had not to recompile Kettle. So, now I do have a Kettle solution that takes less than 160 MB of disk space and consumes less computing recourses so better efficiency while doing edge processing, just by running and maybe designing your kettle transformations and jobs on the edge. 

So, what were the main transformation I did?

### Removing OSGI and dependencies

We can remove Karaf/OSGI listeners but there are still some dependencies that will stop spoon to start. Even if it only this change, works good on kitchen and pan, spoon would break, and I wanted to have spoon also running. So, I had to remove dependencies from PentahoSystem.java class and recompile it. But that's how much I had to do have lightning fast startup also on Spoon. No need to recompile Kettle code, only had to replace the jar file from Pentaho platform code.  

### Plugins removed: 

I have removed most of the plugins that are inside the plugins folder except for essential as json, xlm, platform utils, core plugins, dummy (yes we should avoid using it, but it is quite useful sometimes)

Samples were also removed to save disk space and have faster deploy process. Karaf folder was completely removed. 

I will make the list more accurate and precise once time permits. The list will be available in [this](https://github.com/mfgaspar/orlo) github repository, I have called the repository "Orlo", that is edge in Italy, where I was when worked on this.  

### How to install it and run Spoon, Kitchen, Pan and Carte?

You can find a zip file with thin PDI using this [link](https://github.com/mfgaspar/orlo/releases/download/8.0.0.0-28/orlo-pdi-8.0.0.0-28.zip) 

Once you download the zip file in [here](https://github.com/mfgaspar/orlo/releases/download/8.0.0.0-28/orlo-pdi-8.0.0.0-28.zip) you just need to unzip and start Spoon, Kitchen and Pan from the command line as you would to it normally, just like the example below. Don't forget to optimize JVM memory, point to specific Kettle Home folder, etc..

```bash
sh kitchen.sh -file ../etl/job.kjb -level Error -logfile ../joblog.log
```

You will see the job/transformation is started instantaneously, and no starting overhead. If you measure the execution time (by any mean you might have), plus memory and cpu (that you easily do using jconsole) you will be able how much you've improved.  

Unfortunately, I had to leave Windows out of this, anyhow as for now I don't see windows as the operating system for edge computing. Just a reminder that you can still develop the transformations and jobs in Windows but having proper tests in place to make sure that you've developed will work on the Edge version of Pentaho Data Integration.

Have fun creating jobs and transformations to be used on edge Computig on small and scarse resources devices.  

NOTE: This is great solution not only for edge computing but can also be used in solutions that uses "serverless" and part of the backend, as Dan Keeley has already mentioned on a post on his blog. Some other uses cases are cloud and elastic computing that I will later write a post about it. 


Follow me at: 

[Live Insights]: #


