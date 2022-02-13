# RANSAC-Python-Implementation

   ![1_Bl0jbKc5FUZTaPxJPfop_Q](https://user-images.githubusercontent.com/62684338/153732068-d27208cb-375c-4022-87ba-3d29366c50fb.gif)

_Theory of RANSAC_
**RANSAC** stands for **RAN**dom **S**ampling and **C**onsensus. It is a robust model fitting algorithm, and its performance is often compared to that of the Linear Regression algorithm. In this case, we will use the RANSAC algorithm on Lidar point cloud (PCD) data to segment the ground plane from the other planes which could consist of vehicles, traffic-lights, or anything above the ground plane which needs to be classified for avoiding collision or for trajectory planning. In this particular application, RANSAC will be preferred for ground plane segmentation due to inherent property to reject outliers. (_Outliers can be considered as unusual observations which are likely to be rejected_). 

The core algorithm is straightforward.

Step 1: Select a random set of points (3 points forming a plane).
![1_C9tEQNjhCsT5-pvez8aIOA](https://user-images.githubusercontent.com/62684338/153732130-792ebc83-7c18-4282-a3da-b3716cb9c8ad.png)

Step 2: Calculate the parameters required for the plane equation.
![1_uUDqCRcI_qrdEGsSDrJGtQ](https://user-images.githubusercontent.com/62684338/153732203-af330f46-ed87-4d7c-9e49-f76a66087c0e.png)
Step 3: Calculate the deviation of all the points in the point cloud from the plane using a distance estimate.
![1_TpEYMIHtXmdqgdvtO-bhbQ](https://user-images.githubusercontent.com/62684338/153732220-272f62fc-71db-4af2-b007-5fd5383e08ba.png)


Step 4: If the distance is within the THRESHOLD then add the point as an inlier.

Step 5: Store the plane points and points having the maximum number of inliers.

Step 6: Repeat the process again for MAX_ITERATIONS.
After the RANSAC processing is complete, the plane having the maximum number of inliers is the best estimate of the ground plane. Any points cluster above this ground plane can be classified as an object, obstacle or traffic signs.

## Implementation of RANSAC in Python
The libraries requires for running the RANSAC algorithm in Python...
| Libraries                                         | 
| :------------------------------------------------ |
| `import pandas as pd`                             | 
| `import matplotlib.pyplot as plt`                 |
| `import random`                                   |
| `from mpl_toolkits.mplot3d import Axes3D`         |
| `import open3d as o3d`                            |



## RANSAC Implementation Breakdown
_Lines 7-15_: Creating a storage set of inliers_results; this is an index of all points which are considered as inliers. A SET data structure is preferred to automatically remove the duplicate indexes generated. 3 random points are generated for every iteration for creating the plane to find the inliers.
_Lines 17-33_: These lines will correspond to fetching the point data from the PCD/ASCII file format. This might change based on the format of the input data. In this particular case, in the ASCII file, we have rows containing data such as (x,y,z, R,G,B), so we don't pay much attention to the R,G,B values. In the PCD file, the data format was directly (x,y,z). I loaded the point cloud data as a pandas dataframe so methods such as .loc can be shown which fetch the data from particular index locations.
