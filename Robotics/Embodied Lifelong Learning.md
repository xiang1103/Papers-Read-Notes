# Embodied Lifelong Learning for Task and Motion Planning

## General Questions 
- What is the problem the paper tries to solve? Why does this problem matter?
    - How to learn the parameters to successfully execute actions? 
    - Each action/basic action may have different needs/implementations to use on different objects, how to create a sampler that works with these different needs? 

- How does the paper solve this problem? 
    - Use a hybrid model that determines online if a general or specialized sampler is appropriate for the state 
- What are the results of this paper? 
- Why does the methods work? 
- What are other contributions of the paper? 
- How can we use the result from this paper? 
- How does this paper connect with other methods you know in the field? 
- What is a problem/drawback of the method? How can we try to modify the methodology (code/theory to adjust it)?  

### Traditional Methods 
- Independent specialized sampler for each object, so a different object like glass will have its own sampler that outputs the parameters to perform an action. Different object like bottle will be needing a different sampler 
    - Less data and less robust 
- Single generic sampler that works across multiple types 
 

### Methodology 
- Auxiliary predictor is used to measure error on the auxiliary tasks which approximate the sampler's accuracy. We construct a mixture distribution that assigns more weight to lower-error samplers.  
- Measures how the robot performs across multiple tasks  
  
### Generative Sampler 
- Given the current state as the condition, can our learned sampler output action parameters that complete this action  
- We need to jointly data for all abstract actions so that the action parameters produced are reasonable for executing the remaining actions. 