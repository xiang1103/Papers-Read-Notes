# Motion Planning Diffusion Learning and Planning of Robot Motions with Diffusion Models

## Summary 

## General Questions 
- What is the problem to solve 
    - Robots need to learn trajectories for motion planning known as prior, which gives a probability distribution $p_\thera$ that outputs the probability of being in this state or taking this action. A good prior would give the robot correct guidance on which state to take 

- What is the approach? 
    - Combining learned priors from expert data with optimization-based approaches 
    - Samples a trajectory generated from optimized motion plans 

- What is the training and inference process 
    - Diffusion model transforms a sampled trajectory into white noise in forward process then back into a trajectory in reverse process 
    - Temporal U-Net architecture  
 
- What is the training data 

- What is the results 
    - Diffusion models are strong priors to encode high dimensional trajectory. So diffusion models output accurate trajectories to execute these tasks 
- What is the shortcoming? 


### Notes 
- Traditional methods have used sampling based appraoches and optimization-based planners. 
- Sampling based approach starts with a configuration space (usually bounded), and randomly samples points to go to. Then connects these points to form trajectories or path. Delete points if we hit an obstacle. Repeat this process until we hit goal. Then we can use $A^*$ or similar to search for a path 
    - Low efficiency, but guaranteed to find a path with infinite compute 
    - Probabilistic Roadmap and Rapidly Expanding Tree 
- Optimization approach 