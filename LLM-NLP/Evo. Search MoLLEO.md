# EFFICIENT EVOLUTIONARY SEARCH OVER CHEMICAL SPACE WITH LARGE LANGUAGE MODELS 

## Summary 
Search based algorithms with LLM to incorporate task-specific knowledge to find molecules with desired properties for molecular optimization tasks. 

## General Questions 
- What is the problem to solve? 
    - To optimize molecular optimization objectives, evolutionary algorithms use random permutations and crossovers to expand the search space, which is very costly, and requires multiple evaluations from objective function because they may generate unrelated molecules.  
    - Evolution algorithms are efficient algorithms for molecular search prediction and generation of molecules, so it helps with the process of getting, evaluation, synthesis of molecules. So can use machine learning methods  
    - Traditional Evolutionary algorithms have arbitrary or not so task specific crossover and mutation functions. How do we implement more informed functions? 

- How is this problem solved? Their contributions
    - Incorporate LLMs into EAs  
    - Black box objectives without access to gradients can benefit from Evolutionary Algorithms 
    - Incorporating task-specific information to generate molecules that better optimize the objective function 
    - Search method that generates along the way by incorporating chemistry information from LLM 
- Their results 
    - Better results at the quality of final solution and convergence speed  
  
- How do their results compare with Neural Network/generative model based methods?  
  
- How do they ask LLM to give mutation and crossover? This relies on LLM being good at/knowing what attributes/properties to add to achieve the goal, which is something that it's not very good at.   
  
- Is there generation in their method?  
  


### Traditional Methods 
- Traditional methods either use combinatorial methods to generate all possible molecules and search through the large search space. 
- Transitioned into machine learning like generative modeling because if trained on valid molecules, there is a good chance to also generate valid molecules by fitting the data into a data distribution 
- To generate molecules that achieve optimization, we still need some search or use conditional generative modeling  

### Evolutionary Algorithm
- Use standard evolution algorithm based on Graph GA to sample molecules and generate offsprings through selection, crossover and mutation 
- Crossover and mutation steps are incorporated by the LLM.  
- Crossovers are done on top 2 molecules, while mutation is done on top num_mutation molecules 
- LLMs are prompted to give molecules that do better at a target objective during crossover. For mutation, they also output a molecule 
- Tried with GPT, a molecular fine-tuned LLM, and a generative model based with encoder and decoder to maximize the similarity between the molecule and text descriptions. This part is pre-trained and no confidence this works well with any given SMILES and tasks. 

### Limitations: 
- Implementing LLM as cross over and mutation relies on the LLM's ability to generate the correct molecule 
- There is no crossover or mutation algorithms because the LLM generates this in one shot to directly give the result. There is no reasoning involved.  
- Generative Model tested are also limited by the traiing task and the molecules. All the models are limited by what their training distribution is on. 
