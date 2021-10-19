---
title: "Implementation of Autonomous Control Pipeline"
author: "Kheng Yu Yeoh"
categories: control
tags: [Husky, slow lap control, fast lap control, ROS, simulation, mpcc]
published: true
---

The current Autonomous Control pipeline is designed for the Trackdrive event, such that it can navigate and map an unknown race track alongside the Perception 
System, plus plan for and follow the optimal racing line to complete laps as fast as it can.

<figure>
  <img src="https://user-images.githubusercontent.com/78944454/137904209-ed3c75a2-34fc-4cd7-b77b-57425c5ab40d.png"/>
</figure>
  
The Autonomouc Control Pipeline features
- Slow Lap - For exploring the unknown track, and mapping the layout of the race track along with SLAM
- Transition - Such that the vehicle can start racing once a suitable map of the track is obtained
- Fast Lap - To plan for and follow the optimal race line as fast as it can, without going over track boundaries and respecting actuation limitations.

In addition, the Autonomous Control Pipeline needs to fulfill the following FSG provisions
- FSG D2.7.4 - Average velocity for first 3 laps must be greater than 2.5m/s, while remaining laps must be completed with an average velocity of 3.5m/s.
  - Slow Lap reference velocity is above 2.5m/s
  - Transition immediately after slow lap connects the map
- FSG D8.2.7 - Vehicle must do own lap counting
- FSG D8.2.6 - Vehicle must stop within 30m behind the finish line on the track
  - Fast Lap controller stop publishing actuation and exits after 10 laps

As we're missing a member to work on models and simulation, we mainly tested our working on the Husky, which has the model as described [here](/model/Husky-Model/)

## Full Rviz Simulation
{% include video id="sFjnP6knJ3M" provider="youtube" %}

## Future Recommendations
Future teams should extend the autonomous pipeline to race car simulations then to physical implementations on either smaller scale race cars or other test vehicles.

When tuning for the fast lap control, it may be good to tune in a manner which reduces the occurence and effects of un-modelled behaviour, 
as shown [here](/control/Tune-for-Disturbance-Rejection-for-Better-Performance/)
