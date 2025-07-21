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

## Results 
- Number of shots (examples provided to LM during inference time) dramatically improves the accuracy 
- Scaling is better. The more parameters, the better the performance 
- Data is consisted of images, videos, and interleaving (website, M3W).
- Data is very important. The more data that is removed from the dataset, the worse the performance becomes 
- Interleaving data is important because it provides the few shot learning ability to the model. 
- Freezing the language model provides the best performance (because the model is already trained with a lot of pre-existing knowledge)
- Finetuning makes the performance worse. But training in scratch gives the worst performance. Likely due to the LM is not trained enough 
- A pretrained language model seen many kinds of text before is also key to a vision language model. We need models that are good in their respective modality first 
- A structured intervealing dataset like webpages (W3M) gave the model ability to generate real life-like image then text
- Video data gives the model ability to understand frames of pictures 
- Model already has information retrieval ability based on studying the relationship between words and given images (ex: knowing the father son relationship of Star Wars characters)


## Limitations 
- limited performance on classication tasks because the model is trained on filling sequence of texts 
- Hallucination: The model predicts the most likely next word based on the training. So it will generate answers that are not correct but most often seen given image. Ex: saying there is a text on phone when shown a phone that's shutoff. 
- Limited spatial awareness. Spatial understanding of the world 
- Complex scenes are not well understood, could be caused by the simplicity of the data 
- limitation in context learning, needs a lot of shots to solve certain problems