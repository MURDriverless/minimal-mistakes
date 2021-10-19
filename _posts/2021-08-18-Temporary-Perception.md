---
title: "Simple Cone Positioning System"
author: "Aldrei Recamadas"
categories: slowlap control
tags: [Husky, slow lap control, simulation, ROS]
published: true
---

We developed a temporary Perception System that will publish the necessary Cone messages to test our Slow Lap Path Planner.
This enables us to test our algorithm independently while the actual Perception Systems are being developed.
Furthermore, we can use this to test on custom race tracks without making  Gazebo world files.

## How it works
This simple cone positioning system needs a yaml file, which has the cone information needed. The file format is the same as the image below.
It has all the cone information of the race track - cone color, x and y coordinates.

<p align="center">
![yaml](https://user-images.githubusercontent.com/75785603/137854561-016bd6f8-50d1-4e5d-ac6e-1f88e5db4e3a.JPG)
</p>
It takes in odometry inputs as it needs to know the vehicle's current pose.
Its "sensor" range can be adjusted in the code. Right now, it is set to 10 metres.
Noise can also be added to the cone positions to simulate the uncertainty of SLAM in measuring the position of the cones. This makes the cones jump around slightly then settles as the vehicle goes near.


## Results
<p align="center">
  ![slowGif](https://user-images.githubusercontent.com/75785603/137854493-1e6d1dbe-68cb-4c72-b757-ddb20a9ec894.gif)
</p>

As shown, this temporary cone positioning system can publish cone messages similar to how the SLAM publishes the cone information. Its capability to add noise to the cone positions will help robustly test the slow lap path planning and path following algorithms.

## Next Steps
Build different race tracks to test the the slow lap control in different scenarios.
