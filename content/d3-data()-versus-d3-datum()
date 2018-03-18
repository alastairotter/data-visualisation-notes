# The difference between D3's data() and datum() functions

In most of the D3 examples you will find the .data(data) function will be used to bind the data to elements. But occasionally you will see the .dataum(data) method being used. Either is valid but they will each produce different results which will affect your final visualisation (and what you can do with it). 

Reading the D3 API documentation doesn't really make the distinction all that clear, unless you're a long-time D3 users/master. 

**Disclaimer:** This is a simplistic description of the difference between these two methods but hopefully it makes it a little clearer.

Essentially, .data() joins each element of your data with your selection. So if you have five elements in your data array and you select all the circles in your SVG, .data() will bind each data element to a single (different) circle. The result is five circles with one piece of data each. 

.datum() on the other hand binds the entire data array to each element in your selection. So instead of five circles each with their own piece of the data array, you will now have five circles, each with the entire data array bound to them. More typically, however, you might have just one element in your SVG with all the data attached. 

## An example 
