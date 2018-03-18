[<< Back to the contents](http://github.com/alastairotter/data-journalism-notes)

# The difference between D3's data() and datum() methods

In most of the D3 examples you will find the .data(data) function will be used to bind the data to elements. But occasionally you will see the .dataum(data) method being used. Either is valid but they will each produce different results which will affect your final visualisation (and what you can do with it). 

Reading the [D3 API documentation](https://github.com/d3/d3-selection/blob/master/README.md#selection_datum) on .datum doesn't really make the distinction all that clear, unless you're a long-time D3 users/master. 

**Disclaimer:** This is a simplistic description of the difference between these two methods but hopefully it makes it a little clearer.

Essentially, **.data()** joins each element of your data with your selection. So if you have five elements in your data array and you select all the circles in your SVG, .data() will bind each data element to a single (different) circle. The result is five circles with one piece of data each. 

The **.datum()** on the other hand binds the entire data array to each element in your selection. So instead of five circles each with their own piece of the data array, you will now have five circles, each with the entire data array bound to them. More typically, however, you might have just one element in your SVG with all the data attached. 

## An example 

One of the easier ways to understand this is with an example. Say for example we make a map like the one I made [here](fit-a-d3-map-to-a-container-div.md). 

I made the map using the .datum() method with this snippet of code: 

    svg.append("path")
       .datum( data )
       .attr( "fill", "#ccc" )
       .attr( "d", geoPath );
       
 Once we're done, if we look at the resulting code, we'll notice that although there are 9 features in our map data, there is just one path that is created. 
 
`<svg width="500" height="300">
<path fill="#ccc" d="M331.9081441538165,151.94710676279465L333.9364548340689,147.69364824239585L337.8379468698676,146.74049921751623L339.35496595228375,144.34596267351333L342.605530412581,143.20634613305026L344.0187928239673,140.89915522504452L347.48540608110267, 
... 
98.14585945805527L329.8009626744575,102.05017171618988L326.3099898833613,102.14258345283918L322.76285414164835,107.35893885041855Z" style="stroke: rgb(255, 255, 255); stroke-width: 0.3;"></path>
</svg>`

To use .data() instead of .datum() we need to change our code a little. Notice that .enter() now comes into play and the pattern is a little different.

    svg
       .selectAll("path")
       .data( sa.features )
       .enter()
       .append("path")
       .attr( "fill", "#ccc" )
       .attr( "d", geoPath )
       
The result is a map that looks pretty much the same as our previous one except, if we look at the elements in our deeloper console we can see that now, instead of one path for the entire map we have 9 paths, one for each of the areas. 

`<svg width="500" height="300">
<path fill="#ccc" d="M336.21909910928053,152.04958606609955L338.3541629832304,147.57226130778508L342.4609967051238,146.56894654475389L344.0578588971409,144.04838176159296L347
...
</path>
<path fill="#ccc" d="M305.2855842057009,189.18552042776957L301.38598917147186,193.95343409959958L297.5213580450698,194.3457782897833L296.07597813510273,197.37568561458875L291.
...
</path>
<path fill="#ccc" d="M363.59254158428195,207.50233353059207L360.3738483844781,211.89856966439356L356.16609483516476,215.93190057946003L348.98185262935596,
...
</path>
...`

So effectively using .datum() for our binding created a single path with the data for the entire map. Using .data() on the other hand created multiple paths, each with one feature bound to it. 

