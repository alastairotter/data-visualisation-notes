[<< Back to contents](http://github.com/alastairotter/data-visualisation-notes)

# Fit a D3 map to a container div using fitSize

Scaling and translating a D3 map to fit perfectly inside a container div used to be relatively difficult. In a previous [note](fit-a-d3-map-to-a-container-div.md) I used an example of this in which we created an unscaled and translated projection, calculated the bounds of that path created and then calculated the new scale and translate values needed to fit it in the div we chose.

It's not all that difficult but there is an easier way. In version 4 of D3 there is a new projection function called [fitSize](https://github.com/d3/d3-geo#projection_fitSize) which does exactly what it suggests: it fits a given geojson object to a give div container size.

As before we'll use a simple example in which we create a map of South Africa and fit it to a div on our page.

The result is a crude but working map. Something like this:

![](https://github.com/alastairotter/data-journalism-notes/blob/master/images/samap.png)

In our example we have a geojson file with the data for a basic map of South Africa. We also have a div with a width of 500px and a height of 300px which will contain our map.

First we create our projection:

    var projection = d3.geoMercator()
        .fitSize([width, height], sa);

The important thing here is that instead of have scale and translate values we have the .fitSize function. fitSize takes two values: first the size of our container and second, the geojson object for our map (in this case it is "sa").

With our projection in place we then create our path generator and draw our map to our SVG. **NOTE:** For this example we've used the .datum( data ) function and not the .data( data ) one. There is an [important distinction between these two](<d3-data()-versus-d3-datum().md>) which is worth trying to understand.

    var geoPath = d3.geoPath()
        .projection( projection );

    svg.append("path")
      .datum( sa )
      .attr( "fill", "#ccc" )
      .style("stroke", "#fff")
      .style("stroke-width", 0.3)
      .attr( "d", geoPath );

And that's it. It's pretty simple to quickly create a map and fit it to a given size.

Full example code for the map can be found [here](https://github.com/alastairotter/data-journalism-beginners/tree/master/examples/fit-d3-map-to-container-part-2)

**Notes**

1. This example code uses D3v4.
2. The map data is in geojson format. For larger, more detailed maps it's worth looking at storing the data in the topojson format which isa lot smaller.
