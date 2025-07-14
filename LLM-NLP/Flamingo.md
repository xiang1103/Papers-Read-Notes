# Flamingo VLM 

## General Questions 
- What is the problem the paper tries to solve? Why does this problem matter? 
    - How to have vision language model that's capable of generating good language given images as input 
- How does the paper solve this problem? 
    - Have a strong backbone LM and a visual encoder that understands/tokenizes the images well. Combine the tokens with attention and train this conditional generation 
- What are the results of this paper? 
    - Achieve similar level of performance or surpass finetuned models
- Why does the methods work? 
- What are other contributions of the paper? 
- How can we use the result from this paper?   
    - Apply it to areas needing few shot learning 
- How does this paper connect with other methods you know in the field? 
    - It uses the similar intuition of tokenizing the different modality's data and combining two modality to be one token 

- What is a problem/drawback of the method? How can we try to modify the methodology (code/theory to adjust it)? 

## Content 
- Single LM can be a backbone to do text inference 
- Uses a video perceiver to turn image/video into visual tokens, and a basic LM to output text tokens and output language tokens as output and handle multi-modality. 
- Only needs multi-modal data from web, not any annotated machine learning data 
- Doesn't require task specific tuning 
- Pretraining the vision encoder on several tasks might have helped with the performance 


## Method 
- Uses a frozen LM and a vision encoder to encode the image into tokens and use attention with the LM 
- the vision encoder is also pretrained 