[<< Back to contents](http://github.com/alastairotter/data-visualisation-notes)

# Basic circle and angle functions

Some general reminders of useful functions for calculating angles, arc, radians etc

### Calculating radians given the *arc length* and *radius*

Variables: 
  ```
  s = arc length
  theta = angle
  r = radius
  ```

The function: 


&theta; = s / r


Example: 

```
let s = 12;
let r = 4;

theta = s / r;

// theta = 3 radians
```

### Converting radians to degrees

Variables: 
```
deg = degrees
rad = radians
```

The function: 

deg = rad * 180 / &pi;

Example: 

```
let rad = 3

deg = 3 * 180 / PI

// deg = 171 degrees
```

### Converting degrees to radians

Variables: 
```
deg = degrees
rad = radians

```

The function: 

rad = deg * &pi; / 180;

Example 

```
let deg = 171;
rad = 171 * PI / 180;

//rad = 3 (rounded up)





