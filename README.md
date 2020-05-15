# Vehicle-Path-Spline
This is a work in progress. https://www.geogebra.org/m/kf3qkqx5

The concept will involve being able to create easily curved paths whilst having more control over angular momentum. This could have applications in smooth camera movement in Blender or even (weak) engineering applications for trains that use "twist" to turn or cars that are trying to traverse a loop-the-loop without steering (see https://gamedev.stackexchange.com/questions/173434/generation-of-a-3d-loop-the-loop-with-minimal-steering-required).

First intrinsic rotation about X

![ezgif-2-911a81b8b5cf](https://user-images.githubusercontent.com/33347703/81968651-94e88480-9614-11ea-96c6-4715d1fa0b76.gif)

Second intrinsic rotation about Y

![ezgif-2-c34b52d59dd1](https://user-images.githubusercontent.com/33347703/81968653-95811b00-9614-11ea-9880-69f619f467a4.gif)

Third intrinsic rotation about X

![ezgif-2-ac51be66c1bd](https://user-images.githubusercontent.com/33347703/81968656-9619b180-9614-11ea-9888-bfba90c70ada.gif)

All together

![ezgif-2-bdacb2584a4c](https://user-images.githubusercontent.com/33347703/81968661-974ade80-9614-11ea-89e5-631c4b76cb2b.gif)

Next steps involve finding angle1, angle2, and angle3 based on an arbitrary set of control points to make up the spline.

How it was derived so far:
https://en.wikipedia.org/wiki/Euler_angles#Rotation_matrix
```
First, we try to create a space-curve which rotates according to Euler Angles XYX.
Conditions:

Y(t) a vector function of coordinates x, y, z
Get X1, Y2, X3 elemental rotation matrices
X1 rotates at a rate of angle1*t, Y2 rotates at a rate of angle2*t, X3 rotates at a rate of angle3*t.

Y'(0) = (1, 0, 0)  [Path-direction starts off facing towards + x-axis]

Y'(t) = X(a1*t)*Y(a2*t)*X(a3*t)*(1, 0, 0) [Path-direction rotates at dictated rates. Note that it remains a unit vector throughout.]

Next, we solve the first order differential equation for the resulting parametric equation.

Y(t) = [sin(b*t)/b , (b*sin(a*t)*cos(b*t) - a*cos(a*t)*sin(b*t))/(a^2 - b^2), (a*sin(a*t)*sin(b*t) + b*cos(a*t)*cos(b*t) - b)/(b^2 - a^2)]

Now, we add "width" to the road by adding s*(0, 1, 0) similar to that of a ruled surface. Note that the "width"-vector will have been rotating according to X(a1*t)*Y(a2*t)*X(a3*t) so we multiply those rotations with s*(0, 1, 0) to get X(a1*t)*Y(a2*t)*X(a3*t)*[0, s, 0].

The final resulting surface-equation is,
P(t, s) = Y(t) + X(a1*t)*Y(a2*t)*X(a3*t)*[0, s, 0]

or

P(t, s) = [sin(b*t)/b + s*sin(b*t)*sin(c*t), s*(cos(a*t)*cos(c*t) - cos(b*t)*sin(a*t)*sin(c*t)) + (b*sin(a*t)*cos(b*t) - a*cos(a*t)*sin(b*t))/(a^2 - b^2), s*(cos(c*t)*sin(a*t) + cos(a*t)*cos(b*t)*sin(c*t)) + (a*sin(a*t)*sin(b*t) + b*cos(a*t)*cos(b*t) - b)/(b^2 - a^2)]
```
