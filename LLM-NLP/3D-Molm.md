# 3D-MOLM: TOWARDS 3D MOLECULE-TEXT INTERPRETATION IN LANGUAGE MODELS 

## Summary 
Designed a training pipeline with molecule encoder to encode molecules into token and VLM to turn tokens into text, and finetune a LM with those textual tokens and prompt tokens that output correct text for molecular tasks. Each process is trained with specific training examples and data generated from PubChem trained for instruction tuning, which makes the model capable of understanding molecules. 

## Pre-read Questions: 
- What is the problem the paper tries to solve? Why does this problem matter? 
    - LMs are not good at comprehending 3D molecule structures 
    - Let the LM to interpret and analyze 3D molecules  

- How does the paper solve this problem? 
    - Use a 3D structure encoder 
    - Create a instruction tuning dataset 
- What are the results of this paper? 
    - SOTA performance at molecule text retrieval, molecule captioning 

- Why does the methods work? 
    - The tokenizer successfully encodes the molecules into tokens that contain spatial information, so we have a good embedding 
    - Text representation/projection is done with its own self attention from models that are designed for VLM (might be good at capturing spatial information), so the self attention is optimized at generating molecule tokens compared with other pretained general LLM. 
    - Very specific dataset training on matching 3D molecule to correct text, which makes the model quite good at the specific tasks that require a direct translation from molecule to text. 
    - The LM can understand the molecule as text before its own tokenizer because of Molecule encoder and Q-former that are trained to turn molecule into text. 
    - The tasks they have good performance in are the tasks extensively trained on during Stage 2 and 3 (molecular encoder + Q Former)
    - Q former is also trained with question answer pair in the instruction fine-tuning stage, so it gets better at translating molecule into text and learns the different properties each molecule may have. 

- What are other contributions of the paper? 

- How can we use the result from this paper? 

- How does this paper connect with other methods you know in the field? 

- What is a problem/drawback of the method? How can we try to modify the methodology (code/theory to adjust it)?   
    - Requires good representation of tokens. The training is one pipeline, so in order to make the model work, we need the tokens to be from the same tokenizer, the same text-projector because these two represent the 3D molecule as text. Limited design choice there  
    - Bias towards the training dataset's molecule to text pairs 

- What does 3D molecule text interpretation mean? 

## Methodology 
-  To let LM understand 3D molecules, we need **Molecule text alignment** and **molecule-centric instruction tuning**. 
- Molecule text alignment deals with encoding 3D structure as text 
- instruction tuning finetunes the model to follow human-instructions and complete 3D molecule tasks 
- Text alignment is done by a 3D molecule-text projector, and instruction tuning is done with a custom dataset that provides instructions and properties 
- Text alignment uses **Q-Former** architecture, a vision language model that converts a 3D molecule structure into tokens and then into 1D prompts (text). 
    - To promote the Q-Former learning, there is a two stage training process. In the first process, 3D molecule text representation, so given a molecule, how to convert it with a Molecule Encoder, then transformer into correct text descriptions 
    - The Second process is for molecule text alignment, so given molecule representation in 1D, does the model give correct text description of that molecule such molecule properties, usage, etc. 
    - This architecture is trained 
- Instruction tuning part is done by making a dataset based on PubChem and PubChemQC, where GPT outputs question answer pair for training 

## Previous Work 
- Molecule text modeling is encoding molecule as text, whether it's 1D or 2D molecules through molecule-text retrieval and molecule captioning. 
- No work has been done on 2D molecule structure encoding 

## Architecture 
- 3 Parts: molecule encoder to encode molecule into tokens; molecule text project to project the tokens into text; text alignment with LM to output correct molecule tasks like descriptions based on the text 
- Molecule encoder/tokenizer is Uni-Mol, where it uses an invariant spatial positional encoding which incorporates Euclidean distance so that any rotations or translations are covered. 
- After the tokenizer, the molecule text (token into text) is done by a Vision Language Model like Q-former that processes the tokens from molecule encoder that goes through self attention, feed forward, like other transformers. The output are numerical values of the tokens. 
- Q-former also has a textual transformer identical to BERT. So the output token values are mixed? 
- The LM used is a Llama2. We take in the mixed token values (from molecule and text) into the Llama model and it outputs the highest likely vocabulary based on a softmax and linear layer.  

## Stage 1: Molecule-Text Representation 
- The first training process aims for the model to be good at turning molecule into text. So the output of Q-former is purely text but text regarding the 3D molecule.  
- Take a frozen weight molecular encoder, train the Q-Former process with **multiple objectives** such as molecule-text matching, molecule-text contrasting and molecule captioning. Improve the ability at extracting molecule features. 
- **Why does these trainings work?** 

## Stage 2: Molecule-Text alignment 
- Stage 2 of training is traditional language modeling. Given 3D molecular tokens and 1D prompt tokens, generate textual response. The 3D molecular tokens are extracted through the Q-former, and the LM would output the final text. 
- The combination of 3D molecule tokens and 1D prompts will train the LM to discern the difference between 3D molecule information and prompts for better outputs. 

## Stage 3: Instruction Tuning
- Instruction tuning is used to improve model's ability at following instructions and improve understanding of 3D molecular structures. 
- Given some molecular properties, retrieve these properties and molecule from PubChem (3D-MoIT) dataset. Use the pretrained molecule encoder and (trainable) Q former from stage 1 and 2, and let GPT translate these into question and answer pair, and finetune using LoRA on this task. 