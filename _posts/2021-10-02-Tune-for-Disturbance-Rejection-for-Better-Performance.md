---
title: "Tune for Disturbance Rejection for Better Performance"
author: "Kheng Yu Yeoh"
categories: fastlapcontrol
tags: [Husky, fast lap control, simulation, disturbance]
published: true
---

## Update on cost tuning
Over the past weeks, many tests have been run on the simulation to ensure best performance of the Husky, 
the final cost gains that achieve the best lap times and disturbance rejection is found to be
<p align="center">
  <img width="596" height="146" src="https://user-images.githubusercontent.com/78944454/137821077-cdc08c2c-9676-4aca-8afe-f015b51af9ec.PNG">
</p>

## Results
<p align="center">
  <img width="600" height="397" src="https://user-images.githubusercontent.com/78944454/137711457-6073b01d-b5d1-4f3f-905a-70c860ee6a46.gif">
</p>
<p align="center">
  <img width="300" height="300" src="https://user-images.githubusercontent.com/78944454/137818522-85b7dd92-eb84-40b6-b4f5-c46caab0f031.png">  
  <img width="300" height="300" src="https://user-images.githubusercontent.com/78944454/137819318-2ca7e139-4560-47e0-8a3b-fc0f4ff35b49.png">
</p>

## Comparison from before
<p align="center">
  <img width="596" height="146" src="https://user-images.githubusercontent.com/78944454/137822548-d1231221-bb89-41b6-bbd1-12f69a20904f.PNG">
</p>
<p align="center">
  <img width="300" height="300" src="https://user-images.githubusercontent.com/78944454/137822461-8c393c6c-e741-45fb-a63c-f42a4bc9d126.png">  
  <img width="300" height="300" src="https://user-images.githubusercontent.com/78944454/137822481-a9826d5f-6d46-4c8e-b5f7-5baaddd6b77d.png">
</p>

## Analysis
Despite higher penalization on φ error and ω, which meant lesser prioritization on progress maximization, it actually performed better than before in terms of lap time and 
staying within the track boundary, achieving a lap time of 30.6s~32.6s, with a deviation of 2s, as compared to before which had a lap time of 31.1s~34.3s.

Similar to the previous run, despite the same weights for contouring error and progress maximization, and increased weighting for a more conservative run due to
higher penalization on ω and φ error, it is still able to touch all the apex of the race track, and is also able to determine the short straight line path through 
the chicane section, demonstrating its local optimality characteristic.

The effects of higher penalization on φ error favoured solutions that allow the Husky to track the shape centerline more, thus favouring a solution that also allows it to
follow the shape of the track better. The higher penalization on ω on the other hand favoured solutions that allow the Husky to not command high angular velocity if it can, 
which helps in reducing the amount of slip that the Husky would experience.

There's lesser thick blue lines, which also signifies that the controller does not have to compensate for very strong model mismatch disturbances.

## Recommendation
Future teams can further investigate this with the race car simulation, or when during physical testing, since being more conservative is likely to be the safer choice, and 
if it gives better performance due to better disturbance rejection, then all the better!
