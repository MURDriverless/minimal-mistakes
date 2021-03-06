---
title: "MUR Driverless 2021 Team"
author: "Kelvin Liao"
categories: general
tags: [documentation,sample]
image: mur19e.jpg
published: true
---

Hi everyone! Similar to last year, we are a group of 6 students from the University of Melbourne, continuing the autonomous race car project. We've split into two groups; the autonomous perception and the autonomous control sub-teams. Keep a lookout for my teammates posts! 

In the autonomous perception sub-team, we've divided the work into 4 areas:
1. Simultaneous localisation and mapping (SLAM)
2. Stereo vision 
3. LiDAR
4. GPS/IMU i.e. localisation 

In the autonomous control sub-team, we've divided the work into 3 areas:
1. Slow lap
2. Fast lap
3. Vehicle dynamics 

Currently, the tentative timeline for the perception system is:

## March
- Obtain traffic cones and paint them blue and yellow to create the obstacle course.
- As soon as possible, we will install the baslar cameras, LiDAR, and Piksi (GPS/IMU sensor) on the MUR 2019E car to collect data.

## March - June 
- Validate the previous year's work on the individual pipelines using the collected data or simulation through Gazebo as well as carefully thought out experiments. 
    - Rigorous testing on the Jetson AGX Xavier.
- Use either the Husky robot or a shopping trolley to test the perception system as the 19E car will not be available.

## July - September
Should the current perception system work, the plan for July to September is:
- Improve the individual pipelines (stereo vision, LiDAR, SLAM algorithm) to reduce the latency as much as possible. 

## October
- Test the upgraded perception system on the 21E car.
    - Collect more training data for validation.

## Secondary Goals
Throughout the course of the year, our secondary goals are:
- To build a robust testing environment such that any changes to individual pipeline can be easily tested within 24 hours. This involves improving:
    - The MUR Driverless Simulation (mursim)
    - The docker images 
    - Reorganisation of the Github repository with clear labelling 
- Create a solid documentation system such that future teams can easily pick up where we left off. 
