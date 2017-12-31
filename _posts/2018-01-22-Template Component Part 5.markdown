---
title:  "Template Component - Part 5: The event handlers"
date:   2018-01-22 08:00
categories: [Pentaho, CTools, Components]
tags: [Pentaho, CTools, Components, Template Component]
---

It's also possible to handle clicks or other events on the elements created using the Template Component. This blog post explains on how to do make it work.

To get familiar with the earlier post I wrote on this subject, please refer to the following links: 

- [Template Component - Part 1: The concept and basic usage](http://mfgaspar.github.io/2017/Template-Component-Part-1/)
- [Template Component - Part 2: The use of formatters](http://mfgaspar.github.io/2017/Template-Component-Part-2/)
- [Template Component - Part 2: The use of formatters](http://mfgaspar.github.io/2017/Template-Component-Part-3/)
- [Template Component - Part 4: The model handler](http://mfgaspar.github.io/2017/Template-Component-Part-4/)

You can use events to expand a particular section, to make a fireChange creating interaction between components, or to get details for the clicked section. Anyhow, you will be creating a function, so you can implement the logic you need when a specific event happens on the created elements.

To create an event, you need to use the Events property of the component. For the right-most value you will need to de ne the event that you want to handle, followed by the selector of the element from where you want to handle the event separated by a comma (,). On the left you will need to write your logic.

Let's suppose you want to handle a click event, when you click the territory. In that case we need to have a CSS class you can use and one other attribute that identifies the territory that was clicked. The template would look like:

```javascript
{% raw  %} 
function() { 
  var template =  '' +
    '<ul>' +
    '{{#items}}' +
    '<li class="territory" data-territory="{{0}}"> {{0}}: {{1}}</li>'+
    '{{/items}}' +
    '</ul>'; 
  return template;
} 
{% endraw  %} 
```
When adding the event in the component we need to set the name and the JavaScript code: 

- Event and element selector: here you need to set the event you want to handle plus the jQuery selector separated by a ','. Just as:    

```javascript
click, .territory
```

- Function with the code to execute that will look like: 

```javascript
function(e) {     
  var $elem = $(e.target)
  alert($elem.data('territory'));
}
```

If you test this solution you will get an alert message on your screen, every time you click the territory name. The message will display the territory name that was clicked. 

Please refer to the Book [Learning Pentaho CTools](https://www.packtpub.com/big-data-and-business-intelligence/learning-pentaho-ctools) to get more details.

This should be the last post under this subject, the Template Component. There are more we can do with it, so depending on the number of visualization on this blog post series, I will or not get deeper on details and options.

Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #


