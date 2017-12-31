---
title:  "Template Component - Part 3: The use of addins"
date:   2018-01-05 09:00
categories: [Pentaho, CTools, Components]
tags: [Pentaho, CTools, Components, Template Component]
---

Getting back to the template component! This time we will cover the use of addins.

To get familiar with the earlier post I wrote on this subject, please refer to the following links: 

- [Template Component - Part 1: The concept and basic usage](http://mfgaspar.github.io/2017/Template-Component-Part-1/)
- [Template Component - Part 2: The concept and basic formatters](http://mfgaspar.github.io/2017/Template-Component-Part-2/)

Just like in the table component we can use addins inside the template component. The following image shows the out-of-the-box addins, ready to be used. 

![Example of addins using the Template Component](http://mfgaspar.github.io/assets/template_component_3.png) 

From the image above, we can find the following list of addins types:

1. clippedText: The same as the table add-in plugin used when we have text larger than the space we have to display it. It will show the full text when we hover over it.
2. trendArrow: This add-in is very similar to the trend arrow of the table component using the exact same behaviour and properties. It will display an arrow up/down and red/green depending on whether it's up/below and whether it's a good/bad value.
3. formatted: I consider this a multiuse addin because you can use it to easily apply some formatting or even customize, and have complete control over, what you will display. You can use the add-in to send HTML to the dashboard, so it gives you great  flexibility. By default, the add-in format will be applied using one of two functions. You can format numbers and/or dates, but that for that we saw we can use the formatters.
4. sparkline: Allows you to use the JQuery Sparkline Plugin just like we covered on the table component. Options should be the ones available on the plugin; just make sure the value returned is a string, represented with comma-separated (,) numbers.
5. hyperlink: hyperlink is an add-in that can provide links to any URI that is recognized by the HTML and the browser. You need to return a string with a valid URL from the query.
6. localized: This is similar to the localizedText add-in on the table component, it facilitates the localization of the dashboards.
7. bubble: This add-in will draw a bubble where the size of the bubble is the relation between the value and the minimum and maximum values for all the rows on that same column, where the biggest bubble will be the highest value of all.
8. bulletChart: This is used to represent a bullet chart with the values that come from the query for that same cell. Here you need to return all the values to use on the bullet chart, separated by a comma. The options that you have available are all those available for CCC charts.
9. cccChart: This add-in allows you to display any CCC chart, very powerful but also very tricky to use.

The way to use the addins is quite simple. When using the Mustache, we can call one addin by using the following syntax: 
**{% raw  %}{{{ value | addin | "addin_type" : addin_id }}}{% endraw  %}**, where we can see the folwoing options:

- **value**: the value to be passed to the addin
- **addin**: it indicates that we want to call an addin. Another option would be formatters like we saw on the last post
- **addin_type**: the addin type we want to use, from the list above o some other you might create
- **addin_id**: the addin id to be used. That's because we might need to call the same addin type multiple times, but for different purposes

One example of using an addin inside the template would be, let's just consider we have one more column that indicates what ws the sales growth compared to last year:

```js
function() { 
  var template =  '' +
    '<ul>' +
    {% raw  %}'{{#items}}' +{% endraw %}
    {% raw  %}'<li> {{0}}: {{{ 1 | formatter : "monetary" : 1 }}} {{{ 2 | addin : "trendArrrow" : "none"}}} </li>'+{% endraw  %}
    {% raw  %}'{{/items}}' +{% endraw %}
    '</ul>'; 
  return template;
} 
```

In this case we are using the trendArrow to display an arrow up or down, based on a vlaue that would coming from the 3rd column of the result of our query. 

Do not forget this is the example for using the Mustache engine/syntax.

Please refer to the Book [Learning Pentaho CTools](https://www.packtpub.com/big-data-and-business-intelligence/learning-pentaho-ctools) to get more details. In the book, I do cover all the options of the addins, plus how to change those options. 



Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #


