# [ICLR 2025](https://iclr.cc/virtual/2025/10000222)

## Pre and post training LLM 
- In pretraining, we train next token prediction on low quality data
- During post training, that's where to have instruction following (fine-tuning) on high quality and relevant data, then RL with Human Feedback
- How do we integrate this structure into Robots?   
    - No large amount of data available  
    - Continuous control, no tokens 
    - Multi-modal 


## Data 
- More data shows more generalizability at unseen environments 
- Diveristy of training data helps with performance 
- Generalization is not the ultimate bottleneck, but how to improve the methods themselves 

## Training 
- Imitation learning with video inputs, based on the videos, the robots learn to perform these actions 
- With complex tasks that require more delicate actions, robots tend to fail more on these tasks because the robots can't handle the different ways to fold a shirt. 
- Low quality data consists of different folding strategy, which causes the robots to fail at more complicated shirt folding tasks.     
    - Finetuning on more quality, consistent folding strategy videos improved the model tremendously 
- Post training amounts to extremely large amount of performance improvements 
- A larger model with more pretraining data improves the performance in action speed and quality 
- Video prediction provides a very good conditioning to the model, so that when the state is interrupted like someone messes the cloth, the model knows to resample the same action 

## Strategy 
- The strategy of model may fail if it requires high precision from robot, or doesn't have enough data to represent 
- To filter out demonstrations collected with a low demonstration score based on a trained classifier on rollout outcomes 