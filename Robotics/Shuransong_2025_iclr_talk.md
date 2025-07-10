# [Talk at 2025 ICLR Generative Model for Robotics Workshop](https://iclr.cc/virtual/2025/10000709)

## Generative Models 
- Any method of implementation (flow matching, autoregressive, diffusion) can model any complex distributions which can be used at different tasks 
- With these distributions, we can use generative models to generate robot actions, observations, etc. 
- Policy (Actions): 
    - Generative model used to generate actions 
    - Capture multimodal distributions/actions which can beat regression models 
-  Observations: 
    - Sora 
- Hardware designs: 
    - Directions overlooked by the community 
    - Given object and task, can we use generative model to output finger geometry? 
    - Ex: generate robot gripper that can align T to the correct pose regardless of the pose of T. Useful in assembly lines 

## Video Models 
- We have decent amount of data on videos
- **Videos capture dynamics which images do not** 
- Problems: 
    - Action from the video needs to be executable with the given robot configurations 
    - Can learn precise actions for execution 
- The representation of pixels is costly because their distribution is hard to achieve. However, they are easy to scale and implement, and Pi 0.5 has shown promising results. So we can stick with pixel level video prediction. 

## Extract Action from Video 
- Inverse Dynamics Model: 
    - Infer from the given image/frame from video to get a sequence of actions (infer actions)
    - Trained on particular platform (robot specific)
- Use robot to track the tool 
- Problems: 
    - Error propagates: Generate image then action. This two process can lead to bad results with bad generation model 
    - Slow generation process and costly 

## Unified Action Model 
- Overcomes the poor sampling and speed issue in traditional video action model 
- Uses one shared latent space to learn both action and video 
- Skips video generation with a different diffusion head 
- Can mask actions with just video 
- Can do single task as well as specific model 
- **Generate image/video actually improves the performance of action generation, maybe video generation is a regularizer, so model doesn't overfit and generate more. It's useful to have this in training not as inference time.** 
- Generalization to different sizes or color of grippers can be achieved 

## Universal Benchmark UMI 
- UMI: simple hand gripper to collect robot data 
- Transferrable to other robots that have the same gripper and mount angle 
- Overcome the problem of different robot specifications and baselines, easier for robotic projects to have the same benchmark 
- standardize training data and baseline performance. Can also be used out-of-box on different robots (for testing) 

## Problems 
- **Neither diffusion policy or Unified Video Action model can perfectly solve simple tasks due to the lack of new and diverse data** 
    - Physical Pi was able to achieve higher results with new and diverse data. Showing the importance of data in training these network 
- 