# CHEMREASONER: Heuristic Search over a Large Language Model’s Knowledge Space using Quantum-Chemical Feedback 
   
## General Questions 
- What is the problem/goal the paper tries to solve? Why does this problem matter? 
    - Discovering new catalysts  
    - Hard to link microscopic (atomic level) properties with macroscopic catalytic performance via chemical descriptors remain a barrier 
    - Reasoning about complex catalytic process have multiple modalities, making it hard to be handled by existing LLMs  

- How does the paper solve this problem? 
    - Uses a **score function** based on the absorption and reaction energies, so LLMs' knowledge space is guided by this score function towards favorable and high efficiency catalysts. 
    - LLMs use search/planning methods, **language guided reasoning with computational chemistry feedback**  
    - Use LLM to automatically search to build this link 
    - Search over LLM's knowledge space and use the feedback from a GNN computation tool 

- What are the results of this paper? 
    - Discover more novel and effective catalysts 
    - GNN predicted energies are accurate 
- Why does the methods work? 
    - A large benchmark dataset on catalysts 
    - Guided by actual evaluations, so we get prompts to guide 

- How can we use the result from this paper? 
    - The evaluation techniques at evaluating/guiding the model to output a more optimal structure can be useful. 
- How does this paper connect with other methods you know in the field? 
    - Still a prompting approach on this model 
- What is a problem/drawback of the method? How can we try to modify the methodology (code/theory to adjust it)?  
    - Limited on just absorption and reaction energy as the reward foundation 
    - Still relying on the LLM performance, limited by their performance at following instructions 
    - Proposed instruction following to follow the tree search 
    - GNN can be limited at their performances 

### Terms 
- Chemical Descriptors: properties like more metallic, poor CO selectivity to guide the properties, etc. 


## Paper Specific Questions  
- How does the search in LLM work?  
- How is the computation tool (GNN) used differently from RDKit 
    - The evaluation of the chemical is done using GNN instead of RDKit (RDKit may not be able to do those evaluations)

### Reward Structure and Method 
- Agent (LLM) plans the actions which is based on a set of properties to consider, then generate search prompts based on the identified properties. Execute these prompts by instruction following. Evaluate the 3D representations using GNN to yield a reward. 
- Figure out instructions to guide LLM to reason and search from the output of a trained GNN, then LLM figure out the output 
- Start with the initial prompt, use set of actions (chemical descriptors) to modify the prompt to the LLM, the output of the LLM gets converted into a 3D structure and evaluated to get reward with a GNN to calculate its absorption energy, then keep evaluating 

### Evaluation 
- Llama can't follow instructions 
- GemNet-dT model as GNN 