---
title:  "Template Component - Part 4: The model handler"
date:   2018-01-12 10:00
categories: [Pentaho, CTools, Components]
tags: [Pentaho, CTools, Components, Template Component]
---

In the first blog post under the subject I referred about some advanced options. Let's cover one of those options, the model handler.

To get familiar with the earlier post I wrote on this subject, please refer to the following links: 

- [Template Component - Part 1: The concept and basic usage](http://mfgaspar.github.io/2017/Template-Component-Part-1/)
- [Template Component - Part 2: The use of formatters](http://mfgaspar.github.io/2017/Template-Component-Part-2/)
- [Template Component - Part 3: The use of addins](http://mfgaspar.github.io/2017/Template-Component-Part-3/)

You must know we can use Pentaho Data Integration (Kettle) transformations to return data to the dashboard, so you cam prety much build the result set you need, also when using MDX, we may return some complex values parsed in a string. When we have more complex result sets, we may also need to manipulate the model so that it will let us build a really advanced visualization. Let's suppose that we getting from a query the sales for each territory and country, and we want to display a card by territory where we will include all countries' sales, such as in the following image:

![Example of using the model handler inside the Template Component](http://mfgaspar.github.io/assets/template_component_4.png) 

The fist think that comes out, is that we need to do is to group the result by territory, because if we can have an object for each one of the territories then we can have all countries inside it. It's easier to build a template that can bring those results to the screen, if we build ours own model. That's what the model handler is about, a function that is called where we can manipulate the data and build our own model. The function accepts a parameter and we must return the model back, just as the following example:

```javascript
function(data){
  var getValues = function(value){
    return _.map(value, function(value, idx){
      return {country: value[0], sales: value[2], salesByMonth: value[3]};      
    });
  };
  if (data.queryInfo.totalRows > 0) {
    var model = {};
    // group by territory
    var groups = _.groupBy(data.resultset, function(elem){ return elem[1];});
    // create model with array of groups
    model[this.rootElement] = _.map(groups, function(value, id) { 
      return {territory: id, values: getValues(value)};
    });
    return model;
  } else {
    return null;
  }
} 
```

The way we are transforming the model, will look like, and thats what it will be used by the template engine:

```javascript
{ 
  items: [
    {territory: "APAC", values: [
      [country: "Australia", value: 630623, valueByMonth: "0,49637,...,37905"],
      [country: "New Zeland", value: 230625, valueByMonth: "0,36632,...,45678"],
    {territory: "EMEA", values: [...]},
    {territory: "Japan", values: [...]},
    {territory: "NA", values: [...]}
  ]
}
```

We now need to have in consideration, that's easier to build the template, that's because we will refer to the data using names instead the number of the columns. The template can look like:

```javascript
{% raw  %} function() { 
  var template =  ''+
  '<div class="row">' +
  '{{#items}}' +
  '  <div class="col-xs-3 territory">' +
  '    <div class="row">' +
  '      <div class="col-xs-12">{{territory}} </div>' +
  '    </div>' +
  '    {{#values}}' +
  '      <div class="row">'+
  '        <div class="country col-xs-3"> {{country}} </div>' +
  '        <div class="sales col-xs-3"> {{{sales | formatter : "sales" : "none"}}} </div>' +
  '        <div class="sparkline col-xs-3"> {{{salesByMonth | addin : "sparkline" : "none"}}} </div>' + 
  '      </div>' +
  '    {{/values}}' +
  '  </div>' +
  '{{/items}}' +
  '</div>'; 
  return template;
} {% endraw  %}
```



Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #

