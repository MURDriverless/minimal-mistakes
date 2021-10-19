---
title: "Modelling the Husky"
author: "Kheng Yu Yeoh"
categories: model
tags: [Husky, model]
published: true
---

## Husky
<figure>
  <img width="552" height="601" src="https://user-images.githubusercontent.com/78944454/137827604-be9ec7ba-f497-46ec-ac17-1e10962f2e9a.PNG">
</figure>
The Husky is a 4 wheeled skid-steering mobile robot that comes with its own sensors, EKF-SLAM, controller, plus various packages to support the implementation of our autonomous 
system on it. While it is a slow vehicle that can only go 1m/s, it is still good as an initial test vehicle due to it being safe to work with, and we do not need to expand more
resource into building a smaller scale race car or directly test the control on the race car with just simulation results.

However, to use the Husky for control, we first need to describe it with a model. 

## Simple Kinematic Unicycle Model
Assuming no longitudinal slip, the Husky can be modelled with a simple kinematic unicycle model, with input to the model as the desired linear velocity, v, and angular velocity, 
ω, of the Husky.

<figure>
  <img width="150" height="150" src="https://user-images.githubusercontent.com/78944454/137826421-5731ba0a-432e-43ac-8b5e-f052e6f884a0.png">
</figure>

## Low Level Control Logic and Physical Constraints
As the Husky comes with a lower level controller that commands the required left and right wheel velocity, v_L and v_R, for a given desired linear and angular velocity, v and 
ω, taking into consideration tyre slips and so forth, the slip dynamics itself is not modelled. However, as the Husky can only go 1m/s at max speeds, one can extrapolate that 
the maximum speed constraint of 1m/s applies to each wheels, and thus whenever it turns (commanding angular velocity), it cannot maintain the max linear velocity. 

One can think of this as if the Husky is moving straight at 1m/s, both left and right wheel has to move forward at 1m/s with 0 angular velocity. If the Husky needs to turn, 
and thus command angular velocity, through its skid seteering system, it'll need to decrease either the left or right wheel velocity to turn, (reduce left wheel speed if it 
wants to turn left), since it cannot command more than 1m/s for either wheels (commanding greater than 1m/s on right wheel to turn left).

To incorporate this constraint into the model, the lower level controller logic is modelled as below,
<figure>
  <img width="637" height="263" src="https://user-images.githubusercontent.com/78944454/137827921-4ea7488e-306d-4648-a828-8cb6853f71db.png">
</figure>

In addition, an additional maximum magnitude constraint on ω of 0.5rad/s is also incorporated into the dynamics, to better fit our observations in real life testings.

## Final Husky Model
And thus, the final Husky model that incorporates all we've discussed above is
<figure>
  <img src="https://user-images.githubusercontent.com/78944454/137827369-229ead02-1558-4590-975f-6a32ea3c71b3.png">
</figure>

## Next Steps
Run control and simulation using Husky model
