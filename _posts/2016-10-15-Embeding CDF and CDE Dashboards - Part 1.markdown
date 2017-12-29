---
title:  "Embeding Dashboards - The Basics"
date:   2016-10-15 22:22
categories: [Embedding]
tags: [Embedding]
--- 
	
In a earlier [post](http://pedroalves-bi.blogspot.pt/2013/08/embedded-analytics-in-pentaho-with.html), Pedro Alves talked about embedding CTools dashboards in two different ways: using "div" and "iframe" integration. 

Now that we have RequireJS, there are better ways to integrate dashboards in other application. 

As you should now there are two kinds of custom dashboards in Pentaho:

- CDF dashboards,
- CDE dashboards;

To be able to embed the dashboards you first need to create them, so let's suppose you already have some dashboards you want to be part of your website/webapp. The dashboards, build with RequireJS can also be embedded via RequireJS, so let's cover how.

## Embedding CDF dashboards

If you have some requirements were you need to have a complete control of your html and code being executed, then for sure you want to build a CDF dashboard. You can write the code of your dashboard in the page were you are embedding it. To do it, there is one mandatory script you need to include, that's the script with the code that will allows us to call the CDF dashboard. For that you need to make the requests
 to your Pentaho Server, so that the server can return the script. 
You just need the following script tag:

```xml
<script type="text/javascript" src="http://SERVERNAME<:PORT>/WEBAPPPATH/plugin/pentaho-cdf/api/cdf-embed.js"></script>
```

Per instance, if your server is in the same machine,localhost, and running in port 8080: 

```xml
<script type="text/javascript" src="http://localhost:8080/pentaho/plugin/pentaho-cdf/api/cdf-embed.js"></script>
```

This will give your acess to the CDF framework. 

You jut need to build your dashboard code inside an HTML page or just include the code that's on another file and that way you will be able to reuse it.

An HTML page where you are embeding your dashboard can llok like:

```html
<html>
	<head>
		<meta http-equiv="content-type" content="text/html; charset=utf-8"/>
		<title>mySample</title>
		<script type="text/javascript" src="http://localhost:8080/pentaho/plugin/pentaho-cdf/api/cdfembed.js"/>
	</head>
	<body>
		<div> I am embedding a CDF dashboard in a HTML page!</div>
		<div> Here I Will have my own page! </div>
		<div> And here, I will embed my dashboard! 
			<div id="sampleObject"></div>
			<script type="text/javascript">
				require(['cdf/Dashboard.Blueprint', 'cdf/components/SelectComponent'],
					function(Dashboard, SelectComponent) {
						var myDashboard = new Dashboard();
						myDashboard.addParameter("region", "1");
						var selectComponent = new SelectComponent({
							name: "regionSelector",
							type: "select",
							parameters: [],
							valuesArray: [["1","Lisbon"],["2","Dusseldorf"]],
							parameter: "region",
							valueAsId: false,
							htmlObject: "sampleObject",
							executeAtStart: true,
							postChange: function() {
								var choice = myDashboard.getParameterValue(this.
								parameter);
								alert("You chose: " + choice);
							}
						});
						myDashboard.addComponent(selectComponent);
						myDashboard.init();
					}
				);
			</script>
		</div>
	</body>
</html>
```

As you can see on the code above, we start by addind a script tag to include the CDF framework in our webpage, and latter we are able to use the framework to build a CDF dashboard that is embedded in the webpage/webapp.

## Embedding CDE dashboards

If you are the king of developer that wants to save some time, than you can build your dashboard using CDE, than it's also possible to embed it.  
To embed a CDE dashboard, you really must have the dashboard already built and you just need to require it on your webpage/webapp. To do it you also need to include a script tag to call for the dashboard. Here you won't need to include the CDF framework, but you will need to require the CDE framework, that will automatically include CDF. 

The way to call it is like following:

```xml
<script type="text/javascript" src="http://SERVERNAME<:PORT>/WEBAPPPATH/plugin/pentaho-cdf/api/cde-embed.js"></script> 
```

Per instance, if your server is in "localhost", and running in port "8080":

```xml
<script type="text/javascript" src="http://localhost:8080/pentaho/plugin/pentaho-cdf/api/cde-embed.js"></script>
```

Once you did it you are able to require for your dashboard, by directly calling the getDashboard available in CDE or by making use of the "dash" RequireJS plugin.

```javascript
require(['/pentaho/plugin/pentaho-cdf-dd/api/renderer/getDashboard?path=/public/dash /sample.wcdf'], 
	function(SampleDash) {
		var sampleDash = new SampleDash("content1");
		sampleDash.render();
	}
);
```

Or: 

```javascript
require(['dash!/public/dash /sample.wcdf'], 
	function(SampleDash) {
		var sampleDash = new SampleDash("content1");
		sampleDash.render();
	}
);
```

Here you can render your dashboard in your webpage/webapp by calling the **render()** method of your dashboard, but another option is to use **setupDOM()** followed by **renderDashboard()**. The great advantage of using the setupDOM() first you will get the DOM in your page and you will be able to manipulate it before starting the lifecycle of the dashboard.  

**Note:** please keep in mind that the scripts tags that were used as example also can be called using RequireJS. but for that you need to include RequireJS in your page. 

## Avoid Cross Site Domain Request 

When embedding dashboards from a different server we might get some denial, because the server won't allow Cross Domain Requests. There is on one extra setting that we can make use of to avoid it. To be possible to embed the dashboard with this issue, we can set the following property of the settings.xml file of CDF or CDE plugins:

```xml
<allow-cross-domain-resources>true</allow-cross-domain-resources>
```

You are now able to delight your users with amazing analytics, without completely  getting out of your website/webapp and without letting the user know what technologies are you using to leverage some of the work.  


Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #
