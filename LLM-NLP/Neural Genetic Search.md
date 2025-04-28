# Neural Genetic Search in Discrete Spaces

## Questions 
- How did this paper improve from MolLEO?  
    - Incorporate evolutionary search/generation in the generative model 
    - Applies on sequential generative models  
    - Generative model incorporated with the Genetic Algorithm approach 
-  

## Methods 
- The crossover function is implemented by restricting the number of tokens seen, so each time the parent pool is limited to the number of tokens visible for selection 
- The mutation function is randomly unmask a token. Mutation is done if the crossover does not satisfy any reward/constraint.  

 