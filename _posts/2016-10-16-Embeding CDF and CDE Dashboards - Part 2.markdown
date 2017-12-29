---
title:  "Embeding Dashboards - Externally consuming/generating events"
date:   2016-10-16 22:54
categories: [Embedding]
tags: [Embedding]
--- 

On the last blog post, I have talked about embedding dashboards. Now let's see how we can consume or generate events. 

When embedding a dashboard, does not matter if it's a CDF or CDE dashboard, you can trigger or listen to events to or from embedded dashboards. This way you can create interaction between the element of your page and a CDF/CDE dashboard, as also the opposite.  

## Events that can be consumed externally

The events functionality, is an extension of [Backbone](http://backbonejs.org/#Events), and below you will find a list of useful events you can use with CDF/CDE. When we need to respond or trigger some actions we can listen or trigger them:

Events triggered by the dashboard object:

### Parameters:

- Parameter changes - When a parameter changes its value, the events '<parameterName>:fireChange' are triggered.

### Dashboard Lifecycle:

- Dashboard preInit - When the dashboard finishes running the preInit method, the event 'cdf:preInit' is triggered.
- Dashboard postInit - When the dashboard finishes running the postInit methods, the event 'cdf:postInit' is triggered.

### Components Lyfecycle:

- PreExecution - When the component finishes running the preExecution, the event 'cdf:preExecution' is triggered.
- PostFetch - When the component finishes running the postFetch runs, the event 'cdf:postFetch' is triggered.
- Render - When the component runs the triggerAjax successfully, the event 'cdf:render' is triggered.
- PostExecution - When the component finishes running the postExecution, the event 'cdf cdf:postExecution' is triggered.

### Debugging and error handling:

- Error - If an error is triggered during component execution, the event 'cdf:error' is triggered.
- User not logged in - When the dashboard detects that an user is no longer logged in, the event 'cdf:loginError' is triggered.
- Server error - If a call to the server returns an error, the event 'cdf:serverError' is triggered.

Per instance, if you want to send a message to a remote webservice each time the dashboard is rendered, you can do something like:

```javascript
require(['cdf/Dashboard.Bootstrap','cdf/Logger'], 
	function(Dashboard, Logger) {
		var dashboard = new Dashboard();
  		dashboard.on('cdf:postInit', function(e) {
    		sendMessageOnDashboardPostInit();
  		});
	}
);
```

Or, let's suppose we have two dashboards, one that has a filter, and another own that has a line chart, and you want to trigger a change on the chart when you change the selected options on the filter, you can do something like:

```javascript
require(['dash!/public/dash/selectorDash.wcdf'], 'dash!/public/dash/lineChartDash.wcdf'],
	function(SelectorDash, LineChartDash) {
		var selectorDash = new SelectorDash("selector");
		selectorDash.render();
		var lineChartDash= new LineChartDash("lineChart");
		lineChartDash.render();
		selectorDash.on("productLine:fireChange", function (evt) {
			lineChartDash.fireChange("productLine", evt.value);
		});
	}
);
```

The way to interact with your webpage/webapp can be similar to the example above. Note that when you need to apply some changes to a dashboard you can do a fireChange. Use the "on" function to listen to events, that will trigger some actions.


Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #
