---
layout: post
title: "How to add google maps in rails"
date: 2014-12-03 01:31:08 +0530
comments: true
categories: ['ruby', 'google maps']
keywords: tutorial, google maps, gmaps4rails, turbolinks
description: How to use gmaps4rails gem for generating google maps
---

Today I hope to clear basic concepts related to integrating google maps in rails app. We will be using `Gmaps4Rails` gem for it. I will also discuss some common gotchas related to it.

## First things first

First thing to go about adding google maps in your rails app would be to add these two lines in your Gemfile.

    gem 'underscore-rails'
    gem 'gmaps4rails'

`underscore js`  is a dependency for `Gmaps4Rails` gem, hence it is included in Gemfile.

Next step is to install the newly added gems. Use `bundle install` to install the gems.

## Add Map Javascript

Now you to have include the js files related to `Gmaps4Rails` and `underscore js` in `application.js` . To add the requirements for `Gmaps4Rails`, add below lines in `application.js`. Also you should have asset pipeline. If you are using coffeescript change the below lines accordingly.    

``` javascript
//= require underscore
//= require gmaps/google
```

Next step is to add required scripts to the DOM.

``` html
<script src="//maps.google.com/maps/api/js?v=3.13&amp;sensor=false&amp;libraries=geometry" type="text/javascript"></script>
<script src='//google-maps-utility-library-v3.googlecode.com/svn/tags/markerclustererplus/2.0.14/src/markerclusterer_packed.js' type='text/javascript'></script>
```

To use maps you have to wrap it in a `div` tag. This is how it should look like.

``` html
<div style='width: 80%;' >
    <div id="map"></div>
</div>
```
    
## Map Generation Script


Map can be rendered in the div using the following javascript.

``` javascript
var initialize;
initialize = function() {
  var handler;
  handler = Gmaps.build("Google");
  handler.buildMap({
    provider: {
      maxZoom: 15,
      minZoom: 10
    },
    internal: {
      id: "map"
    }
  }, function() {
    var markers;
    markers = handler.addMarkers([
      {
        lat: 45.467310,
        lng: 54.995987,
        infowindow: "holla"
      }
    ]);
    handler.bounds.extendWith(markers);
    handler.fitMapToBounds();
  });
};
```

You can use a custom marker using a picture options hash.

``` javascript
markers = handler.addMarkers([
    {
        lat: 45,
        lng: 54,
        "picture": {
            "url": "https://addons.cdn.mozilla.net/img/uploads/addon_icons/13/13028-64.png",
            "width":  36,
            "height": 36
        },
        infowindow: "home!!"
    }   
]);
```

You can customize the map using different [options](https://developers.google.com/maps/documentation/javascript/reference?hl=fr#MapOptions) in provider options hash. 

## Render Map

Now this will not render the map, since you have not used initialize function anywhere. You can trigger the code using `window.onload` once the page is fully loaded, handled within a body tag. 

``` html
<script>
    function initialize() {
        // Map initialization
    }
</script>
<body onload="initialize()">
    <div id="map"></div>
</body>
```

Although easy to understand, having an onload event within a <body> tag mixes content with behavior. Generally, it is good practice to separate your content code (HTML) from your behavioral code (JavaScript) and provide your presentation code (CSS) separately as well. 

You can do so by replacing the inline onload event handler with a DOM listener within your Maps API JavaScript code like so:

``` html
<script>
    function initialize() {
        // Map initialization
    }
    google.maps.event.addDomListener(window, 'load', initialize);
</script>
<body>
    <div id="map"></div>
</body>
```

And that's it. Your map should be visible. REFRESH!!

## Common Gotchas

- Map does not load without full page refresh 

The issue is related to use of turbolinks gem in rails apps. 
The key to getting something to work with turbolinks, is to use the provided callbacks. In this case, one has to tell gmaps4rails to perform the map loading when turbolinks has loaded a new page.

This can be done by adding a listener, for the turbolinks event `page:load`, to map initialization.

``` javascript
google.maps.event.addDomListener(window, 'page:load', initialize);
```

This will fix the turbolinks bug.