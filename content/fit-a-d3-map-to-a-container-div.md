[<< Back to contents](http://github.com/alastairotter/data-journalism-notes)

# Fit a D3 map to a container div

***Note:** There is now an even easier way to achieve this when using D3v4's projection.fitSize feature. You can find notes on using fitSize [here](fit-a-d3-map-to-a-container-div-with-fitSize.md).*

Drawing maps in D3 can be both easy and hard. Easy, because drawing a map really only requires a geojson/topojson file and a few lines of code. For example, once you've created your SVG container and imported your geojson/topojson data, making a map is as easy as this: 
  
     var projection = d3.geoMercator()
         .scale( 1000 )
         .translate( [0,0] );

     var geoPath = d3.geoPath()
        .projection( projection );

In these few lines you set up your projection and then make your path creator. Obviously there are many more options available when doing these two steps but we've skipped those here. Once you set up your projection and your path creator, you can draw the map. **NOTE:** For this example we've used the .datum( data ) function and not the .data( data ) one. There is an [important distinction between these two](d3-data()-versus-d3-datum().md) which is worth trying to understand.  

     svg.append("path")
        .datum( data )
        .attr( "fill", "#ccc" )
        .attr( "d", geoPath );

The result is a crude but working map. Something like this: 

![](https://github.com/alastairotter/data-journalism-notes/blob/master/images/samap.png)

The hard part of D3 maps now appears, because there's a very good chance your map doesn't look anything like this, or it may not even appear on your screen. The problem is that you still need to scale the map to fit your box and then centre it (or otherwise align it) in your container. 

The problem is that the container you're drawing the map into is using cartesian co-ordinates (the x and y of your box), while the map uses polar co-ordinates. So we need to convert between these two very different co-ordinate sets to position the map using our x and y co-ordinates. 

There are many ways to do this but here is one way that is relatively simple and it is [based on this answer](https://stackoverflow.com/questions/14492284/center-a-map-in-d3-given-a-geojson-object) Mike Bostock offered on Stack Overflow to a question about centering a map.

The idea is relatively simple: create a projection (with no translation, create a path generator, calculate the bounds of the map (or a part of the map) and then calculate the scale and translation needed to center the map in your box. 

So the rough steps to create our map: 

Create the projection with no scale and untranslated. Then create the path generator.

    var projection = d3.geoMercator()
      .scale( 1 )
      .translate( [0,0] );
    
    var geoPath = d3.geoPath()
        .projection( projection );

Next, calculate the bounds of our map ("sa" is my data in this case, and "s" = scale, and "t" = translate).

    var b = geoPath.bounds(sa),
        s = .95 / Math.max((b[1][0] - b[0][0]) / width, (b[1][1] - b[0][1]) / height),
        t = [(width - s * (b[1][0] + b[0][0])) / 2, (height - s * (b[1][1] + b[0][1])) / 2];

Next, we update our projection and path generator with the new scale and translate values. 

    projection
        .scale(s)
        .translate(t);

    geoPath = geoPath.projection(projection);

Finally, we can draw our map with the new projection.

    svg.append("path")
       .datum( sa )
       .attr( "fill", "#ccc" )
       .style("stroke", "#fff")
       .style("stroke-width", 0.3)
       .attr( "d", geoPath );

Full example code for the map can be found [here](https://github.com/alastairotter/data-journalism-beginners/tree/master/examples/fit-d3-map-to-container)

**Notes**
1. This example code uses D3v4.
2. D3.v4 recently added the [fitSize](https://bl.ocks.org/mbostock/19ffece0a45434b0eef3cc4f973d1e3d) feature which looks to simplify this process by a lot. 
3. The map data is in geojson format. For larger, more detailed maps it's worth looking at storing the data in the topojson format which isa lot smaller.

