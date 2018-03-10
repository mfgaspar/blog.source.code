---
title:  "Template Component - Part 2: The use of formatters"
date:   2017-12-27 08:00
categories: [Pentaho, CTools, Components]
tags: [Pentaho, CTools, Components, Template Component]
---

The template component is a very powerful component, you will always need to write at least one JavaScript function, to return back to the component the template you want to use. In future, we need to consider a way to reuse templates without copy/paste the code.  


To get familiar with the earlier post I wrote on this subject, please refer to the following links: 

- [Template Component - Part 1: The concept and basic usage](http://mfgaspar.github.io/2017/Template-Component-Part-1/)

This post will cover the formatters. The template component has the concept of formatters, which are functions that we can define on the component and later use them inside the template. The values can be numeric, date or string. 

Formatters are very useful because, most of the time, we can't get the value formatted from the query, as we want it to be displayed on the dashboard. Let's suppose you want your values to use always 2 decimal places, but from the query you are getting more decimal places then you need. 

The way to use them, is done in two steps:

1. Define the formatter function in the formatters property of the component 
2. Use the function on the template, so that it can be called when the template is being rendered

Let's see each of the steps in more details:

**STEP 1:** 
You need to go to the formatters property of the template component and add a new formatter, by setting its name and the JavaScript function that accepts a "value" and "id" as arguments. Let's suppose we want to transform a number and show the dollar sign, and we want to use the function in 2 different places, the 1st using 1 decimal place and the second using 2 decimal places. In this case we want to use the same function and name, and the id can be used to make the distinction between the use of 1 or 0 decimal places.

- The value, it's a mandatory argument, will be the value we want to format. 
- The id, it's an optional argument, can be used for conditional formatting. If we pass 1, then we will return the value with 1 decimal place, 2 decimal places otherwise.

One example without the use of the id would be as:

```javascript
function(value){	
  return Utils.numberFormat(value, '$0.0a')
}
```

One example with the id would be as follows:

```javascript
function(value, id){
  if (id == 1)	
    return Utils.numberFormat(value, '$0.0a')
  else
    return Utils.numberFormat(value, '$0.00a')
}
```

**TIP: We can use the id as an option, suppose you have different signs you want to use, you cn then pass the signs as the argument in the id.**

**STEP 2:**
The second step is to make use of the formatter function inside the template we want to use. Let's suppose we called the formatter "monetary". The way to invoke the formatter by calling the function. The syntax is different between Mustache and Underscore, we will focus on the syntax for Mustache. The formatter can be invoked by using  **{% raw  %}{{{ value | formatter | "formatter_name" : id }}}{% endraw  %}**, where we can find the following arguments:

- **value**: the value to be formatted
- **formatter**: it indicates that we want to apply formatters. Another option would be addins, that we will cover in another blog post
- **formatter_name**: the formatter name used in the previous step. The name should be set between double quotes
- **id**: some option to pass to the addin, like the id of a specific formatter

In the example, we have been working on, the template would be as:

```js
function() { 
  var template =  '' +
    '<ul>' +
    {% raw  %}'{{#items}}' +{% endraw %}
    {% raw  %}'<li> {{0}}: {{{ 1 | formatter : "monetary" : 1}}} </li>'{% endraw %}+
    {% raw  %}'{{/items}}' +{% endraw %}
    '</ul>'; 
  return template;
} 
```
So, we can see that the vale we want to pass is the value in the second column (column #1), indication that we want to use a formatter, the formatter name as "monetary" and the number of decimal places to use. 

There are many use cases you can think of to use the formatters. They will save you lots of code or work on the queries, that sometimes we are not able to perform.

Please refer to the Book [Learning Pentaho CTools](https://www.packtpub.com/big-data-and-business-intelligence/learning-pentaho-ctools) to get more details.



Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #


