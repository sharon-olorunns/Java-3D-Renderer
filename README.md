# Java-3D-Renderer
Basic 3D rendering with orthographic projection, simple triangle rasterization, z-buffering and flat shading. I will not be focusing on heavy performance optimizations and more complex topics like textures or different lighting setups


## Transformations
Now we need to add rotation. To do that, I'll need to digress a little and talk about using matrices to achieve transformations on 3D points.

There are many possible ways to manipulate 3d points, but the most flexible is to use matrix multiplication. The idea is to represent your points as 3x1 vectors, and transformation is then simply multiplication by 3x3 matrix.

You can't describe all possible transformations using 3x3 matrices - for example, translation is off-limits. You can achieve it with 4x4 matrices

Any rotation in 3D space can be expressed as combination of 3 primitive rotations: rotation in XY plane, rotation in YZ plane and rotation in XZ plane:


#### XY rotation matrix:

![img](http://latex.codecogs.com/svg.latex?%5Cbegin%7Bbmatrix%7D%0D%0Acos%5CTheta%26-sin%5CTheta%260%5C%5C%0D%0Asin%5CTheta%26cos%5CTheta%260%5C%5C%0D%0A0%260%261%0D%0A%5Cend%7Bbmatrix%7D)



#### YZ rotation matrix:

![img](http://latex.codecogs.com/svg.latex?%5Cbegin%7Bbmatrix%7D%0D%0A1%260%260%5C%5C%0D%0A0%26cos%5CTheta%26sin%5CTheta%5C%5C%0D%0A0%26-sin%5CTheta%26cos%5CTheta%0D%0A%5Cend%7Bbmatrix%7D)



#### XZ rotation matrix:

![img](http://latex.codecogs.com/svg.latex?%5Cbegin%7Bbmatrix%7D%0D%0Acos%5CTheta%260%26-sin%5CTheta%5C%5C%0D%0A0%261%260%5C%5C%0D%0Asin%5CTheta%260%26cos%5CTheta%0D%0A%5Cend%7Bbmatrix%7D)


Now we need to start filling up those triangles with some substance. To do this, we first need to "rasterize" the triangle - convert it to list of pixels on screen that it occupies.

I'll use the relatively simple, but inefficient method - rasterization via barycentric coordinates. The idea is to compute barycentric coordinate for each pixel that could possibly lie inside the triangle and discard those that are outside.

## Z-Buffer & Rasterization
The concept of z-buffer (or depth buffer) comes up with the idea of building an intermediate array during rasterization that will store depth of last seen element at any given pixel. When rasterizing triangles, we will be checking that pixel depth is less than previously seen, and only color the pixel if it is above others.

## Shading
In computer graphics, we can achieve "shading" by altering the color of the surface based on its angle and distance to lights. Simplest form of shading is flat shading. It takes into account only the angle between surface normal and direction of the light source. You just need to find cosine of angle between those two vectors and multiply the color by the resulting value. 

First, we need to compute normal vector for our triangle. If we have triangle ABC, we can compute its normal vector by calculating cross product of vectors AB and AC and then dividing resulting vector by its length.

Cross product is a binary operation on two vectors that is defined in 3d space as follows:

![img](http://latex.codecogs.com/svg.latex?u%2Av%3D%5Cbegin%7Bbmatrix%7D%0D%0Au_%7Bx%7D%26u_%7By%7D%26u_%7Bz%7D%0D%0A%5Cend%7Bbmatrix%7D%2A%5Cbegin%7Bbmatrix%7D%0D%0Av_%7Bx%7D%26v_%7By%7D%26v_%7Bz%7D%0D%0A%5Cend%7Bbmatrix%7D%3D%5Cbegin%7Bbmatrix%7D%0D%0Au_%7By%7D%2Av_%7Bz%7D-u_%7Bz%7D%2Av_%7By%7D%26u_%7Bz%7D%2Av_%7Bx%7D-u_%7Bx%7D%2Av_%7Bz%7D+%26u_%7Bx%7D%2Av_%7By%7D-u_%7By%7D%2Av_%7Bx%7D%0D%0A%5Cend%7Bbmatrix%7D)

We need to calculate cosine between triangle normal and light direction.

