# [Talk at 2025 ICLR Generative Model for Robotics Workshop](https://iclr.cc/virtual/2025/10000707)

## Talk 
- Generative models are not fit for planning, because it requires strict, symbolic representation 
- Can we devise an algorithm to explore the imaginative abilities of generative models?   
- Example generative models: Diffusion & Large Language models 
- **Shouldn't use generative models for planning purposes because it will give non-sensical answers, but use the imaginative/generative ability as idea creators to solve a problem.**


## Day dreaming with Diffusion 
- Can we have data to train a repetoir of skills that robots can do, then can we use Generative models to utilize these skills? 
- The imagination doesn't need to be perfect to finer detail, as long as it can leverage the skills already learned 
- Step 1: Train short horizon tasks.  
    - **Goal Condition Behavior Cloning**: learning from a large number of reaonsable trajectories (diverse behaviors)
- Step 2: Use Generative Model to come up with sequence of goals to achieve   
    - SuSIE: Use image generation model to generate the next state's image, so robots know which state to go to next then execute using short horizon tasks learned given a language command 
        - The robot can learn the intermediate states that need to be accomplished without exact planning and perform better than GCBC which only has a goal image and needs to learn the entire trajectory at once 
    - We can also use Generative model to define different objectives to collect data (SOAR paper) 

## Vision and Language Models (ECOTA)
- Backbone: Vision Language action models   
    - Ex: robot controls as questions to a given image/language. 
-  can break down objectives through talking to themselves (chain of thought) 
    - Output intermediate actions: imagining intermediate steps that can lead to eventual success 
    - The plan can be done using semantics (language + vision) 

## General Robot Policy 
- Same idea behind VLA, scaled up with more data 
- Have a backbone VLM model to understand images and language input 
- Then action model is done using flow matching to add noise and denoise 
- Pretrain with a lot of data from different datasets, then post training on specific actions 
- Pretrain sees different failed states which help with recovering. Posttraining teaches robot to perform specific and good actions 
- A higher level policy thinks about what to do, and the lower policy executes the actions 
- Each language piece can correspond to action policies 
- Given a command, the robot can think to itself with a language model to determine what to do, then turn the language instructions into action policies, which then execute the actions low level. 
- The images which VLM is trained on has action labels on them. So when a similar image is shown, it corresponds to the action label for lower policy to execute 
- Hypothetical reasoning and imagination: Given an image, ask the robot why it has made decisions in this way. This gives more context which can become training data 
- Diverse training data leads to better generalization at different environments for the robot 


## Training 
- During VLM training, all the images (robot manipulation) and prompt text are processed as tokens, and outputs as tokens through the VLA 
- Verbal instructions are used to teach VLM for better intermediate decision making process 
- Decrease in diversity of data sources reduce the model performance 
- **larger scale, larger data set, more diversity, more training time leads to a general model that can perform as well as the model specifically trained on one environment (same as how GPT can beat specialist models)**

## Conclusion
- Generative models is best at generating intermediate actions/ideas for low level execution, instead of rigid planners 
- Generative model is used as hiearchical imaginations where we can use RL in this setting and improve the abstraction process 

## Future Papers 
- Transfusion
- VLA  
- Robotic Control via Embodied Chain of Thought 