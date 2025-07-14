# [Talk at 2025 ICLR Generative Model for Robotics Workshop](https://iclr.cc/virtual/2025/10000716)

## Content 
- Use generative models to be a world model that robots can generalize to unfamiliar environments 
- Gen model for search and planning 

## Planning 
- Given an image and a task objective, ex: walk up the stairs, the neural network outputs the immediate actions to perform. But this might fail if there is an obstacle in the way. 
- With generative model, we can output how likely or good an action is. Given image, task objective and neural network action prediction, output how good this action is (like preference)
- This allows to take in account of any constraints to output energy function, so in test time, it can generalize better to the events unseen before in training. 

## World Models using Probabilistic Models
- Simulate distribution that's unseen before by composition 
- Models are still worse in the multi-modal space compared with purely language (not good in the embodiment setting)
    - Reason is that natural language's distribution is well captured by training data already (from internet). But in the embodied setting, the distribution for data doesn't capture as well (for example, a diagonal in a rectangular box)
- Since we have distribution for a diagonal, we can decompose into two rectangles and assume independence to get distribution for the sides of the rectangle 
- Composition can be applied to combine multiple models (combining modlaity): Language model for idea generation + video model to understand the physical interactions + action model to actually execute the actions
     - **language model doesn't understand the low level or physical world. Video model only sees actions and not the high level ideas, and action models are the most low-level and have no knowledge of anything else** 
     - Interact the language model with the video model to generate a high levle plan that works (scored by the video model), then use video model to interact with action model to output actions that have high likelihood of happening (in this case, we assume they are independent, so we use composition to combine the model independently)
- We can also do this with multi-modal models that the distributions are not completely separate from each other. 
    - Use a VLM to input text goal commands (output actions to take) and have the VLM predict video and evaluate if the action actually leads to goal state 
    - Iterate this process until we find a solution 
    - This kind of combining can output good performance on unseen tasks before 
    - Better than just using a backbone language or vision language model to output language and have a language policy 

## Energy Models 
- Diffusion models are energy models which the denoising process is the a learned Langevin Dynamics 
- We can design an energy function used across the entire training dataset, which would become a probability distribution 
- During training, we train energy function individually, but at test time, we can compose different energy functions together to generate a new trajectory (generalization to new environments)
- Combining two generative models to solve different tasks 
- Reinforcement Learning: Combining the energy model outputs, one over the trajectories and another one over the cost/goal. Then outputs 
- One shot goal planning with starting and goal state, can massively outperform RL methods 
- Synethesizing different constaints to solve complex dyanmics and new problems 