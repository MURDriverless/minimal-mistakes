---
title: "EKFSLAM performance"
author: "Jack McRobbie"
categories: mapping
tags: [ROS, slam]
published: true
mathjax: false
---
 This is a brief update on the progress  acheived on the development of ekfslam for MUR's autonomous vehicle. There are 3 major updates to the system that will be described briefly.
### 1. Slam working in simulation 

After some further debugging and integration efforts, the slam algorithm has come together and is working with the lidar detection pipeline and control inputs to produce a basically working slam implementation.   

![image-left](/assets/img/slam/ekfslam_demo_real.gif){: .align-right}

The continuing work needed is to integrate the camera based cone detection algorithm and the odometric corrections from GPS to counter drift. Both of which actually exist and work in the codebase currently; but are not ready for integration yet.

### 2. System Architecture definitions

As SLAM is the central module in the autonomous vehicle as such I (Jack) took a lead role in defining the system architecture and interfaces from a systems point of view. We define a number of different rose nodes and topics that can be seen below.
<figure>
  <img src="/assets/img/slam/MUR-20D-ROS.png" alt="structure"/>
  <figcaption>Diagram of our Ros Architecture</figcaption>
</figure>
### 3. Trigger based ekfslam and lowered latency
For filter based systems there are a few ways to implement the process of recieving, processing and publishing outputs in ROS. They have different pros and cons and essentially trade off stability and predictability, for speed and latency. 

Firstly you can take the following structure:

    int Hz = 10;
    rate = ros::Rate(Hz);
    ros::Subscriber sub1; 
    ros::Subscriber sub2;
    // etc 
    
    while(! ros::isShutdown()){
        // do filtering and processing
        
        publish();
        rate.sleep();
    }
However, this has the downside that it adds latency for no reason. The time between when the incoming message is recieved and the time when the node awakens to do processing is time that is simply lost for no reason. It;s redeeming features lay in its readability and predicatability. This will publish at 10 Hz as long as the processing can keep up. 

The alternative is the faster much lower latency approach. If we look at the following callback function: 

    void ekfslam::ptcloudclbLidar(const mur_common::cone_msg &data)
    {
        // ROS_INFO("MessageRecieved: %ld", ros::Time::now().toNSec());
        int length_x = data.x.size();
        int length_y = data.y.size();

        // test here for length equality, otherwise bugs will occur. 
        if (length_x == 0 || length_y == 0){
            z = Eigen::MatrixXf::Zero(0,0);
            return;
        };
        assert (length_x == length_y);
        lidar_colors.clear();
        z = Eigen::MatrixXf::Zero(3,length_x);
        for (int i = 0; i <length_x; i++){
            z(0,i) = data.x[i];
            z(1,i) = data.y[i];
            lidar_colors.push_back(data.colour[i]);
            z(2,i) = 0;
            // ROS_INFO("[ %f, %f, %f]",data.x[i],data.y[i],0.0 );
        }
        // this is the important bit
        if (TRIGGER_MODE){
            runnableTrigger(0);
        }
        return;
    }
Where the call to runnableTrigger() runs the filter on the updated measurement as soon as the message is processed. 

This leads to the following pretty simple high level code:

    int main(int argc, char **argv)
        {
        /* High level manager for the slam node. 
        Will call a class of slam that has been developed in subfolders to this directory. 
        Currently only has ekf slam.	
        */
            ros::init(argc, argv, "slamNode");

            ros::NodeHandle n;

            ekfslam slam(n,STATE_SIZE);

            ROS_INFO_STREAM("EKF SLAM: LAUNCHED");

            ros::spin();
            
            // slam.runnable();
            return 0;
        }

_The current implementation of slam achieves latency of approximately 1 ms_. Though further benchmarking is necessary to determine this more accurately.

See the code [here](https://github.com/MURDriverless/slam/tree/develop)