---
title:  "Template Component - Part 4: The model handler"
date:   2020-12-30 08:00
categories: [Pentaho, CTools, Components]
tags: [Pentaho, CTools, Components, Template Component]
---

In the first blog post under the subject I referred about some advanced options. Let's cover one of those options, the model handler.

To get familiar with the earlier post I wrote on this subject, please refer to the following links: 

- [Template Component - Part 1: The concept and basic usage](http://mfgaspar.github.io/2017/Template-Component-Part-1/)
- [Template Component - Part 2: The use of formatters](http://mfgaspar.github.io/2017/Template-Component-Part-2/)
- [Template Component - Part 3: The use of addins](http://mfgaspar.github.io/2017/Template-Component-Part-3/)

You must know we can use Pentaho Data Integration (Kettle) transformations to return data to the dashboard. When using MDX, we may return some more complex value parsed in a string and, when this happens, you may also need to manipulate the model so that it will let you build a really advanced visualization. Let's suppose that you are returning the sales for each territory and country, and you want to display a card by territory where you will include all countries' sales, such as in the following image:

![Example of using the model handler inside the Template Component](http://mfgaspar.github.io/assets/template_component_4.png) 

What we need to do is to group the result by territory, because if we can have an object for each one of the territories then we can have all countries inside it. It's easier to build a template that can bring those results to the screen:

```javascript
function(data){
  if (data.queryInfo.totalRows > 0) {
    var model = {};
    var territories = _.groupBy(data.resultset, function(elem){
      return elem[1];
    });
    model[this.rootElement] = territories;
    return model;
  } else {
    return null;
  }
}
```

The way we are transforming the model, will look like, and thats what it will be used by the template engine:

```javascript
{ 
  items: {
    APAC:  [["Australia", "APAC", 630623, "0,49637,0,...,0,37905"], ...,
            ["New Zealand", "APAC", 535584, "0,0,36409,...,0,102523"]],
    EMEA:  [[...],...,[...]],
    Japan: [[...],...,[...]],
    NA:    [[...],...,[...]]
  }
}
```

We now need to have in consideration, that's easier to build the template, that's because we will refer to the data using names instead the number of the columns. The template can look like:

```javascript
function() { 
  var template =  '' +
    '<div>' +
    {% raw  %}'{{#items}}' +{% endraw %}
    '<li> {{#APAC}}: {{ APAC | formatter : "monetary" : 1}} {{ 2 | addin : "trendArrrow" : ""}} </li>'+
    {% raw  %}'{{/items}}' +{% endraw %}
    '</ul>'; 
  return template;
} 
```



Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #

