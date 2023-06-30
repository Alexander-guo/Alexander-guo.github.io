---
layout: page
title: Autonomous VIO for Quadrotor
description: Autonomous Visual Inertial Odometry for CrazyFlie 2.0  
img: assets/img/VIO_quad/cover.gif
# redirect: https://unsplash.com
importance: 1
category: academic
---

This VIO (Visual Inertial Odometry) project involves trajectory planning and accurate tracking, state estimation through stereo vision to realized GPS-denied quadrotor autonomy. Once a quadcopter can accurately estimate its own state and use it for planning and control, the last step towards autonomy is to implement a mapping solution so the quadcopter can sense where the obstacles are using its on-board sensors. [[code]](https://github.com/Alexander-guo/Autonomous-VIO-based-Quadrotor)

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include figure.html path="assets/img/VIO_quad/cover.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Simulation
</div>

### Approach

__Trajectory Planning__:

In the simulation, the map is assumed known and the start and the goal position is given ahead. In practice, when the whole map is unkown to the robot, the robot is expected to sense its surroundings by using its on-board sensors and plan the trajectories in real-time. In the planning phase, we make the following process:

* Discretizing the map into voxel grids
* [A* algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm) to find the shortest path to the goal position
* `Waypoints prune` to elemenate shape turns and alleviate quadrotor overshoot
* Using `Minimum Snap` to interpolate between post-processed waypoints and generate the final smooth trajectory 

The following is an example: 

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include figure.html path="assets/img/VIO_quad/Astar_Waypoints_Trajectory_res0.4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A* Path, Pruned Waypoints, Final Trajectory
</div>

__Trajectory Tracking__:

For quadrotor tracking, a `Nonlinear Geometric Controller` is designed in reference to [Lee et al.](https://ieeexplore.ieee.org/abstract/document/5717652?casa_token=81xcTyqJzRgAAAAA:NN2aL4YhOCkf-2vC_k9z8MBSLyr4HkLEU9Ky4y6BZSuoCmUCZ0YlcTQNnkFIsv_lcD3O8vXU4HY).

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include figure.html path="assets/img/VIO_quad/model.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The Frame Modeling of CrazyFlie 2.0 
</div>

The performance of tracking control:
<div class="row justify-content-center">
    <div class="col-sm">
        {% include figure.html path="assets/img/VIO_quad/mymap_3D_Path_res0.4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm">
        {% include figure.html path="assets/img/VIO_quad/Position_vs_Time_res0.4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div> 
</div>
<div class="caption">
    Trajectory Tracking Control 
</div>

__State Estimation__:

In this setup, the on-board sensors are [IMU](https://en.wikipedia.org/wiki/Inertial_measurement_unit) and a stereo rig. A `complementary filter` is applied to estimate the attitude of a flying quadrotor based on data from a 6 axis IMU which provides acceleration and angular rate measurements. Given stereo vision, depth can be well extracted. `Feature matching` and `RANSAC` are then applied to estimate the pose of the quadrotor. Besides, an `Error State Kalman Filter (ESKF)` is specially designed for pose estimation. We proceed by incorporating every IMU update as a nonlinear state update to update our nominal model of the pose of the system $$ x_n $$. Since IMU updates are more frequently than vision updates, we use visual measurements to correct the state.

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include figure.html path="assets/img/VIO_quad/pos_vel_window.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The Estimated, Desired and Practical Position and Velocity with ESKF 
</div>