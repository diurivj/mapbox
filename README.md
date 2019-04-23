![](https://i.imgur.com/1QgrNNw.png)

# [](http://materials.ironhack.com/s/rJCE0nGTNN7#google-maps--geolocation "google-maps--geolocation")Mapbox | Geolocation

## [](http://materials.ironhack.com/s/rJCE0nGTNN7#learning-goals "learning-goals")Learning Goals

In this lesson you will learn:

- How to set up an API key
- How to embed a map in HTML
- How to show markers in our map
- How to create a route between two points

## [](http://materials.ironhack.com/s/rJCE0nGTNN7#introduction "introduction")Introduction

![Imgur](https://i.imgur.com/L3SNDaA.jpg)

Mapbox is a service of location data with features like maps, search and navigation. What does it do? Well... it seems obvious when is called Mapbox.

Inside Mapbox API we can find different services like Navigation, Geocoding and GeoJSONSource.

Mapbox provides the same API for different platforms and technologies like iOS, Android, React Native, Unity and Web Services.

Mapbox is free to use with certain quota and they have [different plans](https://www.mapbox.com/pricing/) based on the number of requests your app is getting (how many users visits your app in average).

## Get a Mapbox API key?

The steps might change in the future and sometimes this process can be different, so don't hesitate to try again the same steps if something bad happens in one of the steps. In your career, you will see this kind of situations fairly often since these are all part of the development process of any product.

### Steps to get an API key for the first time

1.  Go to <https://www.mapbox.com/>

2.  Click on the button "Start building" on the cover's call to action button of the page.

![Imgur](https://i.imgur.com/EjjI08a.jpg)

3.  You must create an account completing the form.

![](https://i.imgur.com/uLmgJdb.png)

4.  Once in your dashboard, you already have a token in the Access tokens section, copy it.

![Imgur](https://i.imgur.com/kVwWXj4.png)

5. After this, you are all set and you should have access to your API key, do not forget to copy the Mapbox CDN scripts from the SDK for Web (js).

![Imgur](https://i.imgur.com/3Z3f6Lc.png)

### Get the script tags of the Mapbox CDN

To retrieve the Mapbox official library, you need to use the CDN, this is the fastest way. Use this tags inside the `<head>` tag.

```html
<script src="https://api.mapbox.com/mapbox-gl-js/v0.51.0/mapbox-gl.js"></script>
<link href="https://api.mapbox.com/mapbox-gl-js/v0.51.0/mapbox-gl.css" rel="stylesheet"/>
```

## Create a map

To create our first map we need two files, the `HTML` and the `JavaScript`.

In the `HTML` we will render our map and for that we need a container `div` to embed it.

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
  <head>
    <title>My First Map</title>
    <script src="https://api.mapbox.com/mapbox-gl-js/v0.51.0/mapbox-gl.js"></script>
    <link href="https://api.mapbox.com/mapbox-gl-js/v0.51.0/mapbox-gl.css" rel="stylesheet"/>
  </head>
  <body>
    <h3>My First Map</h3>
    <div id="map" style="width: 400px; height: 300px"></div>
    <script type="text/javascript" src="main.js"></script>
  </body>
</html>
```

And in our `main.js` we'll write the next code:

```javascript
// main.js

mapboxgl.accessToken = "<YOUR-API-KEY>";
const map = new mapboxgl.Map({
  container: "map",
  style: "mapbox://styles/mapbox/streets-v10",
  center: [-99.1711, 19.399],
  zoom: 15
});
```

Notice that we need to replace `YOUR-API-KEY` from `mapboxgl.accessToken` with the key you got in the section above.

![:bulb:](http://materials.ironhack.com/build/emojify.js/dist/images/basic/bulb.png ":bulb:") If instead of a map you see a error message in the console, it probably means that you have a problem with your API key. Make sure the API key you've used is the same from your [Dashboard](https://www.mapbox.com/account/). If you see nothing, it's probably because you didn't give a `height` property for your `#map`. Take a look at the HTML part of the code and you will notice that we have some embedded CSS in the `<head>` section.

Basically, we are on top of two things here:

1.  Create an instance of a `Map` which receives four parameters: the first one is the container (`document.getElementById('map')`) which only needs the id value, not the selector; the second is the stylesheet by default, we can let it like that. Next we have two more options - the position to center the map and the zoom level (if the center is present the zoom must be too).

2.  Set an array with [latitude](https://en.wikipedia.org/wiki/Latitude) and [longitude](https://en.wikipedia.org/wiki/Longitude) to point the map to a specific place (in our case IronhackMEX), and center it with the key `center`. 

![:bulb:](http://materials.ironhack.com/build/emojify.js/dist/images/basic/bulb.png ":bulb:") Check other parameters that you can pass. [(https://www.mapbox.com/mapbox-gl-js/api/)](https://www.mapbox.com/mapbox-gl-js/api/)

Cool 😎 Isn't it? But this is too simple. Let's add some functionality!

## Adding Markers

Showing only a map is boring. Mapbox API has a built in functionality to add markers in your map.

![Imgur](https://i.imgur.com/XvyUJmW.png)

To place a marker we need to create an instance of `mapboxgl.Marker()` and add it to the map passing it into the method `addTo`

```javascript
const marker = new mapboxgl.Marker()
    .setLngLat([-99.1711, 19.399])
    .addTo(map);
```

The basic options that you can pass:

- position is an array with latitude and longitude inside the method `setLngLat`
- map is the variable that references the instance of our map;
- we can pass another config to the marker inside the constructor with an object like this: `{color:"red",draggable:true}`.

Take a look [MarkerOptions object specification](https://www.mapbox.com/mapbox-gl-js/api#marker) for more options.

Even we can animate our marker:

```javascript
const marker = new mapboxgl.Marker()

  function changeMarker(){
    const time = Date.now()
    const radius = 20
    marker.setLngLat([
      -99 + Math.sin(time / 10000 * radius),
      19 + Math.cos(time / 10000 * radius)
    ]);
    marker.addTo(map);
  }

  setInterval(changeMarker, 1000/60)
```
Make sure to zoom out to see the animation.

#### Hey! our marker isn't boring anymore!

![Imgur](https://i.imgur.com/IDZckSo.gif)

## Using Browser location

How about we take the position from the browser? Since HTML5, we have a set of Web APIs we can use in modern browsers.

Normally all browser have an object [navigator](https://developer.mozilla.org/en-US/docs/Web/API/Navigator) where we can find the property [geolocation](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/geolocation).

`geolocation` allows accessing to the latitude and longitude coordinates of the browser - *if the user gives us permissions*.

```javascript
...
// Try to get a geolocation object from the web browser
if (navigator.geolocation) {

  // Get current position
  // The permissions dialog will pop up
  navigator.geolocation.getCurrentPosition(function (position) {
    // Create an object to match Mapbox's Lat-Lng array format
    const center = [
      position.coords.longitude,
      position.coords.latitude,
    ];
    console.log('center: ', center)
    // User granted permission
    // Center the map in the position we got
  }, function () {
    // If something goes wrong
    console.log('Error in the geolocation service.');
  });
} else {
  // Browser says: Nah! I do not support this.
  console.log('Browser does not support geolocation.');
}
...
```

```javascript
// the result of the console.log()
center:
  [
    -99.1711, 
    19.3990 
  ]
```

Now let's add a marker with our current position and center the map in the marker. Also, we are going to put a marker in Ironhack Barcelona Campus!

```javascript
function startMap() {

  // Store Ironhack's coordinates
  const ironhackMEX = [ -99.1711, 19.3990 ];

  // Initialize the map
  mapboxgl.accessToken = "<YOUR-API-KEY>";
  const map = new mapboxgl.Map({
    container: "map",
    style: "mapbox://styles/mapbox/streets-v10",
    center: ironhackMEX,
    zoom: 15
  });

  // Add a marker for Ironhack México
  const IronhackMEXMarker = new mapboxgl.Marker()
    .setLngLat(ironhackMEX)
    .addTo(map);

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(function (position) {
      const user_location = [
        position.coords.longitude,
        position.coords.latitude
      ];

      // Center map with user location
      map.setCenter(user_location);

      // Add a marker for your user location
      const userLocationMarker = new mapboxgl.Marker({ color: "red" })
        .setLngLat(user_location)
        .addTo(map);

    }, function () {
      console.log('Error in the geolocation service.');
    });
  } else {
    console.log('Browser does not support geolocation.');
  }
}

startMap();
```

## Drawing a route between two pins

![Imgur](https://i.imgur.com/9uBhP6f.jpg)

If we want to draw routes between two pins we have to add this piece of code at the end, and set the corresponding libraries links in the `<head>` tag.

```javascript
map.addControl(new MapboxDirections({
  accessToken: mapboxgl.accessToken
}), 'top-left');
```

To specify the route that we want to do, we just have to use the input boxes provided from the map.

Do not forget to import the apropiate stylesheet and JavaScript library at the `<head>` tag:

```html
  <head>
    <title>My First Map</title>
    <script src="https://api.mapbox.com/mapbox-gl-js/v0.51.0/mapbox-gl.js"></script>
    <script src='https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-directions/v3.1.3/mapbox-gl-directions.js'></script>
    <link
      href="https://api.mapbox.com/mapbox-gl-js/v0.51.0/mapbox-gl.css"
      rel="stylesheet"
    />
    <link rel='stylesheet' href='https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-directions/v3.1.3/mapbox-gl-directions.css' type='text/css' />
    <style>
      #map {
        height: 500px;
        width: 100%;
      }
    </style>
  </head>


```

## Summary

In this lesson you learned how to use the basic things with Mapbox API. You successfully created a map, learned how to create a pin and how to draw a route between two pins.

## Extra Resources

[Mapbox JavaScript Documentation](https://www.mapbox.com/mapbox-gl-js/api)

[Mapbox Examples](https://www.mapbox.com/mapbox-gl-js/example/simple-map/)

[Mapbox Plugins](https://www.mapbox.com/mapbox-gl-js/plugins)

