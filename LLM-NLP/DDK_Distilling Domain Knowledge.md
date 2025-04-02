# DDK: Distilling Domain Knowledge for Efficient Large Language Models 

## Quick Summary 
Distill the dataset using a discrepancy factor to see what domains have a big performance gap and make the student model train more on those tasks across difference domains. Big improvements from base model but not so much from other KD methods.  
## General Questions 
- What is the problem the paper tries to solve? Why does this problem matter?  
    - Distill domain knowledge more efficiently between teacher and student models 
    - How to deploy lightweight and effective LLMs? (Small scale) 
    - How to reallocate the training data during distillation so there would be more training on domains that the student and teacher have big performance gaps  
    - Traditional methods align student to generate similar things to the teacher, DDK wants to mitigate the discrepancies in various domains' performances 
- How does the paper solve this problem?
    - Use a domain discrepancy factor to calculate the performance differences 
    - Dynamic sampling the dataset by this 

- What are the results of this paper? 
    - In some areas like books, the performance of student model has a big improvement compared with the student model not trained with DDK. **Biggest difference in OpenWebMath where the DDK approached outperformed the teacher model** 
    - In most training criteria like CommonCrawl, C4, the Stack, Chinese CommonCrawl, there is minimal to no performance increase with DDK  
    - DDK's performance is similar with other baseline methods' 
    - Big improvement on reasoning and math compared with other methods. But big improvement from the base model. 
- Why does the methods work? 
    - KD allow student model to learn from the teacher model (which gives the basic improvements), and they use the domain sampling technique to change and the model sees the dataset more often. 
- How can we use the result from this paper? 
- How does this paper connect with other methods you know in the field?
    - Same approach but alternates the data 
- What is a problem/drawback of the method? How can we try to modify the methodology (code/theory to adjust it)? 
    - Did not see huge performance in certain tasks on web crawled data and Chinese 


## Paper Specific Questions 
- What do they use Knowledge Distillation for? 
    - Transfer domain knowledge from teacher to student better through their distilling dataset and smoothing tricks 
- How did they do that?  
    - Shuffle the dataset so tasks with 
- How do they train to improve the model with KD? 
- If traditional KD doesn't account knowledge difference between teacher and student, what does it really improve? 
- Why are there no improvements through the DDK approach at certain training tasks? 

### KD Traditional Methods 
- KD is used to close the gap between the teacher and student model 
- Black box model is using API (GPT, DeepSeek) to generate outputs such as from Chain of Thought 
- White-box distillation uses a loss function to transfer the knowledge from teacher LLM. By taking the logits or the internal parameters (weights) during the teacher model and use KL Divergence to match the logits of the teacher model, but this is not good for open text generation tasks (Reverse KL Divergence is used with policy gradient to solve this problem)
- However, traditional methods do not account the knowledge difference between teacher and student, and these methods are not effective at translating knowledge. How do we better distill these knowledge in a *smooth manner* to **bridge the domain performance gap**? 
- Small LLMs are trained on high quality datasets that we might not have  

### Problems with KD 
- Two main challenges: How to use the data? How to stabilize the distillation process
- Training on different domain's data substantially affects the performance 
- The influence of KD training on different domains is underexplored. In some domains, the student drastically underperforms the teacher. **There are domains that are not optimized in the student through KD**.  
- KD aligns the generative behavior of the student to teacher, can this work with a very domain specific task? 

### Methods 
- First use a validation dataset to test the performance differences in different domains as a **domain discrepancy factor** 
- Recalculate this factor periodically during training 
- Sample data from different domains based on the domain discrepancy factor to bring more training data to KD 
- A smoothing algorithm to stabilize this DDK approach  
- Uses a general large LLM as teacher and smaller LM for student 
- Training is done by DeepSeek Code base. Jointly minimize the KL divergence and the ground truth entropy. (Standard KD training loss)
- Do not tune the prompt given to the LM 
- No specifics of training mentioned in the paper (see the DeepSeek code base they used)

### Discrepancy Factor 
- The domain discrepancy factor is a row vector of size # of domains, defined by $r[i]$. The higher the score is, the more discrepancy in performance.  
- Calculate the cross entropy loss between the output of student and teacher model with groundtruth in the validation set, then calculate the "softmax" of all the values in the row to get the value. (See the paper for a specific formula)
- However, this factor fluctuates dramatically, so we need smoothing so that it doesn't change the ordering of data sampling too much/too frequently. They apply a smoothing factor every K iterations. 

### Experiments 
- Used the same class of model for student and teacher (ex: Llama 13B and 1.1 B) 
- Dataset RedPajama for distillation: 
    - Contains CommonCrawl(web crawled datasets), C4(web crawled), the Stack (source code of 30 programming languages), Wikipedia, Books (books with author and their publication info), ArXiv, StackExchange, OpenWebMath(Math webpages)  
    - These are the domain datasets used during distillation process 
- Areas for improvement: math, Chinese 
- Prepare 500 samples to use as validation for each domain 
- Training details such as setting learning rates can be found in the paper 
- **Evaluation is done by using preexisting benchmark datasets across different domains such as RACE.** 
