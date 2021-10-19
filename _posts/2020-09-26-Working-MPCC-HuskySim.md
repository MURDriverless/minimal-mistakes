---
title: "Working MPCC on Husky Gazebo Simulation"
author: "Kheng Yu Yeoh"
categories: fastlapcontrol
tags: [Husky, fast lap control, simulation, mpcc]
published: true
---

## Update
Woohoo! Finally managed to get Husky Simulation to work and finish fast laps in Formula Student Germany 2018 track! 

## Cost Gains
<p align="center">
  <img width="596" height="146" src="https://user-images.githubusercontent.com/78944454/137822548-d1231221-bb89-41b6-bbd1-12f69a20904f.PNG">
</p>

## Results
<p align="center">
  <img width="300" height="300" src="https://user-images.githubusercontent.com/78944454/137822461-8c393c6c-e741-45fb-a63c-f42a4bc9d126.png">  
  <img width="300" height="300" src="https://user-images.githubusercontent.com/78944454/137822481-a9826d5f-6d46-4c8e-b5f7-5baaddd6b77d.png">
</p>

## Analysis
There are many issue faced when tuning the simple unicycle model MPCC to work with the Gazebo Simulation. One of the key issues was the cost gain tuning, as if the cost gain
is set to be too aggresive, the model mismatch disturbance is much more prominent and causes the Husky go out of bounds very often, or in some cases, flying and/or 
flip over itself, however if the cost gain is set to be too conservative, it only tracks the centerline and may have issues going over tight corners.

<figure>
  <img width="300" height="169" src="https://user-images.githubusercontent.com/78944454/137824364-6eafe773-5adf-49ee-ba37-fa94b649c0f7.gif" alt="default-os1"/>
  <figcaption>You thought I was joking about Husky Flying?</figcaption>
</figure>

The other key issue is the model mismatch disturbance, which is prevalent in the current run, signified by the thick blue lines. The blue lines represent the path travelled by 
the Husky as it completes the laps, and it being thick or having a separate outlier line signifies the deviation as the Husky completes the lap. This occurs mainly because of 
the model mismatch disturbance due to the MPCC using the simple unicycle model and the Husky Gazebo simulation using a more detailed model that simulates friction, tyre force, 
tyre slips and so on. 

In a lot of testing, the Husky either flew and flipped, or just exceeded the track bounds very often. Ultimately, it is discovered that by setting higher weights contouring 
error, the whole run stabilized and is able to finish the lap without exceeding the track bounds or flipping. This is likely because having it track the centerline allows it to 
have the maximum amount of space to decelerate and turn, thus allowing it to behave more conservatively and thus reduce the occurence and effect of the un-modelled dynamics. 

While in this case we used the the same weights for contouring error and progress maximization, it is still able to touch all the apex of the race track, and is also able to 
determine the short straight line path through the chicane section, demonstrating its local optimality characteristic.

In terms of lap time, it ranged from 31.1s~34.3s, with a deviation of 3.2s, as compared to AMZ Driverless which had a lap time of 28.4~28.9s on the same track and much smaller 
deviation. This is to be expected due to their model modelling some effects of the non-linear dynamics that occur when driving fast (such as tyre force and slipping) plus 
friction, and thus experience lesser disturbance from model mismatch, allowing for a more consistent run. In addition, the Husky will also have larger lap times due to it being 
unable to keep maximum linear velocity as it commands angular velocity with its skid-steering system, where if the Husky is currently going straight at the maximum velocity of 
15m/s (left and right wheel both are moving forward at 15m/s), and if it wants to turn, it can only decrease the velocity of one of the wheels to turn towards that side 
(reducing left wheel velocity to turn left), and cannot command more than the maximum velocity on the right wheel to turn left. Future teams need not worry too much about this 
as the Husky cannot go 15m/s anyway, and this is just a simulation with artificially increased speeds.

## Next Steps
Demonstrate the whole autonomous pipeline with slow lap mapping, transition, finishing the fast laps and stopping at the 10th lap, succesfully completing the Track Drive event.
