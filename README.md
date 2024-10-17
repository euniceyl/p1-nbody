# Project 1: NBody

This project heavily borrows from Princeton and Berkeley Computer Science and the work of Robert Sedgewick, Kevin Wayne and Josh Hug.

**Context:** In 1687, Isaac Newton formulated the principles governing the motion of two particles under the influence of their mutual gravitational attraction in his famous Principia. However, Newton was unable to solve the problem for three particles. Indeed, in general, solutions to systems of three or more particles must be approximated via numerical simulations.

This program simulates the motion of _N_ objects in a plane, mutually affected by gravitational forces, and animate the results. Such methods are widely used in cosmology, semiconductors, and fluid dynamics to study complex physical systems. Our driver program `NBody.java` draws an animation of bodies moving in space interacting with each other subject to interacting and mutual gravitational forces. These bodies are modeled by the class `CelestialBody.java` implemented and tested independently of the simulation.

Simulation of completed project running with planets in our solar system (animation repeats after one earth year):

<details>
<summary>Example Simulation of Complete Project</summary>

<div align="center">
  <img width="500" height="500" src="p1-figures/planets.gif">
</div>

</details>


### `CelestialBody` Variables, Constructor, and Getter Methods

<details>
<summary>CelestialBody Instance variables</summary>

The outline below shows the constructor, methods, and instance variables (or fields)  of the `CelestialBody` class. All instance variables should be `private`. All methods should be `public` (if you write helper methods they should be `private`).

<div align="center">
  <img width="400" height="400" src="p1-figures/celestialBodyMethods.png">
</div>

There are six instance variables: `myXPos`, `myYPos`, `myXVel`, `myYVel`, `myMass`, `myFileName`. The first five have type `double`, the last is a `String`.
</details>

<details>
<summary>CelestialBody Constructor</summary>

You'll have one constructor: it has six parameters, one for each instance variable. The signatures of is shown below. 

<div align="center">
  <img width="576" height="248" src="p1-figures/celestialBodyConst1.png">
</div>

</details>

<details>
<summary>CelestialBody Getter Methods</summary>
The body of each getter method is a single return statement, returning the value of the corresponding instance variable. These getter methods allow the values of `private` instance variables to be accessed outside the class, and do not allow client programs to set the values as they only get the values.
Example: method `getXVel()`

<div align="center">
  <img width="310" height="115" src="p1-figures/getXVel.png">
</div>

</details>

### Additional `CelestialBody` Methods

<details>
<summary>The method CelestialBody.calcDistance</summary>

<br>

 <div align="center">
  <img width="420" height="128" src="p1-figures/calcDistance.png">
</div>

<br> 

This method returns the distance between two `CelestialBody` objects, using the standard distance formula between `this` body (using `myXPos` and `myYPos` or `this.myXPos` and `this.myYPos`) and the `CelestialBody` object specified by the parameter `b`. The distance is the value of $`r`$ in the formula below:

```math
r^2=dx^2 + dy^2
```

where $`dx`$ is delta/difference between $`x`$-coordinates, similarly for $`dy`$.

</details>

<details>
<summary>The method CelestialBody.calcForceExertedBy</summary>

<br>

<div align="center">
  <img width="372" height="58" src="p1-figures/calcForceExertedBy.png">
</div>

<br>

This method calculates and returns the force exerted on `this` body by the body specified as the parameter. Force is calculated using the formula below:

```math
F = G\frac{m_1m_2}{r^2}
```
 
Here $`m_1`$ and $`m_2`$ are the masses of the two bodies, $`G`$ is the gravitational constant ($`6.67 \cdot 10^{-11}\frac{N-m^2}{kg^2}`$), and $`r`$ is the distance between the two objects. Call `calcDistance` to determine this distance.

</details>

<details>
<summary>The methods CelestialBody.calcForceExertedByX and calcForceExertedByY</summary>

<br> 

<div align="center">
  <img width="428" height="20" src="p1-figures/calcForceExertedByX.png">
</div>

<br> 

These two methods describe the force exerted in the X and Y directions, respectively. The signature of `calcForceExertedByX` is shown above; `calcForceExertedByY` has a similar signature. 

$`x`$- and $`y`$-components from the total force are obtained using the formulas below, where $`F`$ is the value returned by `calcForceExertedBy`, $`r`$ is the distance between two bodies, and $`F_x`$ and $`F_y`$ are the values to be returned by `calcForceExertedByX` and `calcForceExertedByY`, respectively. 

```math
F_x = F\frac{dx}{r}\\~\\
F_y = F\frac{dy}{r}
```

</details>

<details>
<summary>The method CelestialBody.calcNetForceExertedByX and calcNetForceExertedByY</summary>

<br>

<div align="center">
  <img width="428" height="20" src="p1-figures/calcNetForceExertedByY.png">
</div>

<br>

This method returns the total/net force exerted on this body by all the bodies in the array parameter. The principle of superposition ([see Physics][Physics]) says that the net force acting on a `CelestialBody` object by many other bodies is the sum of the pairwise forces acting on the `CelestialBody` by each body. Hence, the forces returned by `calcForceExertedByX` (or `Y`) need to be summed in calculating the value to return. 

</details>

<details>
<summary>The method CelestialBody.update</summary>

<br> 

<div align="center">
  <img width="379" height="39" src="p1-figures/update.png">
</div>

<br> 

This _mutator_ method doesn't return a value, but updates the state/instance variables of the `CelestialBody` object on which it's called. 

This method will be called during the simulation to update the body's position and velocity with small time steps (the value of the first parameter, `deltaT`). The values of parameter `xforce` and `yforce` are the net forces exerted on this body by all other bodies in the simulation. When code calls the update method from `NBody.java`, we will determine the values of the arguments passed as these two parameters by calling `calcNetForceExertedByX` (or `Y`). In the formulas below the parameter `xforce` is $`F_x`$ and `yforce` is $`F_y`$. 

This update method updates the instance variables `myXPos`, `myYPos`, `myXVel`, and `myYVel` in four steps.

1. First, the acceleration is calculated using Newton's second law of motion where $`m`$ is the mass of the `CelestialBody`. This creates two variables for acceleration in the $`x`$ and $`y`$ directions.

```math
a_x = \frac{F_x}{m}\\~\\
a_y = \frac{F_y}{m}
```
 
2. Then, values for new `myXVel` and `myYVel` are calculated. We'll call these `nvx` and `nvy` where the $`n`$ is for new, using the relationship between acceleration and velocity, e.g., `nvx = myXVel + deltaT*ax`.

3. `nvx` (and a corresponding `nvy`) is used to calculate new values for `myXPos` and `myYPos` using the relationship between position and velocity, e.g., `nx = myXPos + deltaT*nvx`.

4. _**After**_ calculating `nx`,`ny`,`nvx`, and `nvy`, we'll assign these to the instance variables `myXPos`, `myYPos`, `myXVel`, and `myYVel`, respectively. 

These steps will update the position and velocity of the body making the simulation possible.

</details>

<details>
<summary>The method CelestialBody.draw</summary>

<br>

This **void** method in `NBody` describes where to call the CelestialBody.draw method.

</details>

### The `NBody` Class

The `NBody` class will use `CelestialBody` objects to run the simulation.

<details>
<summary>Details on the CelestialBody class</summary>

<div align="center">
  <img src="p1-figures/NBodyMethods.png">
</div>

</details>

<details>
<summary>Details on Data Format</summary>

The data for planets, suns, and celestial bodies in general is in the format shown below. All files in the folder data are in this format. This is the file `planets.txt`:

<div align="center">
  <img src="p1-figures/format.png">
</div>

The first value is an integer _**n**_, the number of bodies for which data is given in the file. The next value is a `double`, the radius of the universe for the simulation. This value is used to set the scale for the animation.

There are _**n**_ lines, one line for each `CelestialBody`. Each line contains six values as shown above. The first five values are `doubles`: the first two are initial x and y coordinates; the next two are initial x and y velocities; the next is the mass of the `CelestialBody`. The last value on a line is a `String` specifying the file in the images folder used for the animation of the simulation.

</details>


### NBody Methods

<details>
<summary>The method NBody.readRadius</summary>

<br>

Given a file name, this method returns a double corresponding to the radius of the universe in that file, e.g. `readRadius("./data/planets.txt")` should return $`2.50 \cdot 10 ^{11}`$ (alternatively, 2.50e+11).

</details>

<details>
<summary>The method NBody.readBodies</summary>

<br>

This method returns an array of `CelestialBody` objects using the data read from the file. For example, `readBodies("./data/planets.txt")` should return an array of 5 `CelestialBody` objects. 

</details>

<details>
<summary>The method NBody.main</summary>

<br> 

In the loop of the `main` method:

1. An `xForces` array and `yForces` array is created, each with the same size as the number of bodies in the simulation.
2. The net x and y forces for each body are calculated, and stored in the `xForces` and `yForces` arrays respectively.
3. `update` is called on each body, using `dt` and the corresponding elements of these arrays as parameters.
4. `draw` is called on each body.

</details>

### Running the Simulation

When the simulation is over, our code prints out the final state of the universe in the same format as the input (example below).

<details>
<summary>Example Simulation Output</summary>

|             |             |             |           |           |          |
| :---        |    :----:   |       :---: |  :---:    | :---:     | ---:     |
| 5           |             |             |           |           |          |
| 2.50e+11  |            |             |             |           |           |
| 1.4631e+09 | 1.4943e+11 | -2.9831e+04 | 4.0749e+02 | 5.9740e+24 |earth.gif |
|-1.1174e+11 |-1.9803e+11 |  2.0989e+04 | -1.1953e+04 |  6.4190e+23 |   mars.gif |
| 2.4125e+10 | 5.2103e+10 | -4.3685e+04 | 2.0627e+04 | 3.3020e+23 | mercury.gif |
| 5.6664e+05 | 7.0808e+06 | 1.0861e-01 | 1.0639e-01 | 1.9890e+30  |    sun.gif |
| 1.0555e+11 | 2.3363e+10 |-7.5708e+03 | 3.4204e+04 | 4.8690e+24 |   venus.gif |

</details>

When the simulation finishes, close/quit the graphics window to be able to run another simulation. Use the red X button in the upper left of the graphics window to dismiss the window.

Coursework from Duke CS 201: Data Structures and Algorithms.
