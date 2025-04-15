# BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models 

## General Questions 
- What is the problem trying to solve? 
    - Expensive to pre-train a VLM from end to end, so can we come to a middle ground? 
    - There is a modality gap between image encoder and text generation from LLM, so freezing one or both models will face problems with modality. 

- What is their contribution? 
    - BLIP-2 is a efficient pretraining strategy with frozen image encoder and frozen LM, with a middle Querying transformer for training. 
    - Lightweight training with much fewer parameters but still achieves SOTA 
    - Efficient and accurate image to text generation with a lot fewer parameters 


### Training Transformer 
- Stage 1 trains the transformer with the image encoder to learn a good representation **of what?** 
- Stage 2 trains **bootstraps** with a frozen language model 
- The two stages are needed to **bridge the modality gap?** 
- We need the image encoder to understand image and the LLM to generate few shot text 

### Vision Language Alignment 
- The cross modal alignment comes in from LLM not trained on images but we need to turn image into text, so there is a gap in modality here because pictures have many low level details while language is very high level and abstract. We might lose information from the picture compared with LLM textual output  
- **Q former's training is used to extract visual feature from the image encoder so the most useful visual feature is fed to the LLM.** 

### Vision Representation Learning Stage 1 
- Goal: Q former extract the most relevant visual features from the image that matches the text 
- Three pre-training objectives: 
    - Image text contrastive Learning: 
        - match image representation (output of image transformer) with the output of text transformer 
        - get a similarity score between the image with each text pair (one postive and one negative), then run softmax to see which score is the highest. Then we can run cross entropy. 
        - Will align the image value to be close to text value, so that the query value of the image match more with the correct text value. The intuition is **getting image's value close to the text value, so the image is closer to the text/caption and vice versa in the embedding space**. 
    - Image grounded text generation 
        - next token prediction with given prompt and image to match. 
    - Image-text matching 
        - Given image and text, binary classification to predict if the two match 
- During the training, different kind of self attention is used 

### Vision to Language Generation Stage 2 
- Goal: Q former can connect the important visual features and let LLM have good textual outputs 
- Input: image query after Q former and 1 sentence text prompt to serve as guidance. 
- This works because Q former can extract the most important visual features from the image and feed useful information to the LLM. Because we are only updating the weights of Q Former, it helps Q former to only adjust so that it gets better at getting the LLM to respond, so without touching the image encoder, it avoids the catastrophic forgetting problem. 
- This is as getting the Q former to match with what's needed for LLM to generate text 

### Q Former 
- Bridge the gap between modality and **extract the most important features to the image**
- Have an image transformer and a text transformer 
- Image transformer: 
    - Learnable queries go through self attention and then has cross attention with the image encoder output 
    - Encoding is done by the image encoder, imagine this as the transformer block 

- Text transformer: 
    - take in the text description of the image and feed through transformer 
    - The input text will be a text matching the image (sort of like a label) 
    - Will also be tokenized with trainable weights 

### Questions 
- What does it mean the visual representations can be understood by LLM? 
    - After stage 1, Q former would have learned to extract features that are relevant to the text (so the text can match up to the image), then during stage 2, the goal is making these features recognized by the LLM 
- How does BLIP's training strategy compare with our knowledge distillation idea?  
    - It doesn't improve the knowledge of the LLM at all. It aims to extract features so that an LLM can understand, it facilitates good answers from the LLM. But this seems to be bottlenecked by how good the LLM and the image encoder is. Even though we can't change the encoder and LLM, we can extract sufficiently important features so that it learns to let the LLM generate correct texts. This is fundamentally different from distilling knowledge because no knowledge is distilled and there are no teacher or student models. 
- Why does Q former work, and how did it improve performance without touching LLM and image encoder? 
    - It works because it learns to extract image features and match with corresponding text during the Stage 1 training process. Which makes it very good at doing image captioning work or any other work related with text and image. If we have sufficiently strong image encoder and LLM, we can train Q former to learn to extract good features that match the input labels then during Stage 2, output the correct generation based on Q former outputs. So this works because Q former learns to adapt to both the image decoder and the LLM, but at the same time, it's bottlenecked by both. 
- Why is contrastive learning needed? 
    - It helps the model performance at matching the image features with corresponding text 
- Why the same self attention block in text and image transformer? 