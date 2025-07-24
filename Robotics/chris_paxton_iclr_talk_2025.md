# [Hello Robot Talk](https://iclr.cc/virtual/2025/10000219)

## current state 
- Task and Motion planning works but is very constrained, manipulating relationships between objects and world, but doesn't learn 
- Modern robotics work can heavily learn in Behavior Cloning work, but still doesn't generalize well to new home and have robust performance 

## Vocabulary
- Needs high level language capabilities to understand instructions 
- Usually done with a LLM 
- Needs cameras to identify objects and understand the world around itself. (Build a 3D representation of the world)
- Can incorporate newer models 
- Cameras can help generate 3D clouds of the scene which then gets used for manipulation 
- Ex: Go to shelf, then can evaluate the robot based on task completion 


## Searching Objects 
- Stage 1: Simulation, searches the entire environment for the object or execute the navigation task 
- long horizon tasks are done using LLM
- Remembering object locations are done using Dynamic Memory that stores the visualized/simulation of the home 
- Scene graph is built to map out the world and the information the world contains is fed to a VLM 

## Problems 
- Seeing the object is not hard, what is hard is about searching for the object location for the robot 
- Not able to see what the object is 
- generalizability 

### Others
- important for code sharing to have environments that are replicable (ex: docker image for storing code)