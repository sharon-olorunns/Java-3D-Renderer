Transformations
================
Now we need to add rotation. To do that, I'll need to digress a little and talk about using matrices to achieve transformations on 3D points.

There are many possible ways to manipulate 3d points, but the most flexible is to use matrix multiplication. The idea is to represent your points as 3x1 vectors, and transformation is then simply multiplication by 3x3 matrix.

You can't describe all possible transformations using 3x3 matrices - for example, translation is off-limits. You can achieve it with 4x4 matrices

Any rotation in 3D space can be expressed as combination of 3 primitive rotations: rotation in XY plane, rotation in YZ plane and rotation in XZ plane:


# XY rotation matrix:
![img](http://latex.codecogs.com/svg.latex?%5Cbegin%7Bbmatrix%7D%0D%0Acos%5CTheta%26-sin%5CTheta%260%5C%5C%0D%0Asin%5CTheta%26cos%5CTheta%260%5C%5C%0D%0A0%260%261%0D%0A%5Cend%7Bbmatrix%7D)
