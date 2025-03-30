# Denoising Diffusion Probab. Models 

## Quick Summary: 
Building on the foundation of a diffusion model to introduce a forward process of adding noise, then a backward process of denoising each step to predict noises added. Then we get a model that's capable of denoising noises and generate a reasonable looking image. 
## Questions: 
- What is denoising diffusion? Why do we need to denoise diffusion? 
    - Getting rid of noise is the denoising. We need the denoising because we added noise to the original input in the forward process. 
- What is the diffusion process? 
    - Add noise to corrupt input then denoise 
- What is the difference between this method and flow matching and score matching?   
    - This method also involves denoising score matching 

- What problems does this method solve? 
    - It connects the traditional diffusion models with variational inference. Provides a better loss objective at training diffusion models.  
    - How to generate realistic images? 
- How does the paper solve this problem?   
    - Achieve a simpler training error/objective  
- How can we use the result from this paper?  Applications 
    - Can we use this on robot parameters? This works on generating similar images because images are highly similar/structured. if the model learns where the pixels lay, then changing their RGB values/arrangements a little bit, we get similar images that can be recognized by human eyes. But if an action requires very specific parameters, generating a similar but not the same parameter might not work. 
- Why does this paper work? 
    - Because we add noise to the original data distribution to get noise as input, and since we sample different time points for training, the model **learns the different noise distribution and how to get back to the original data distribution**.  
    - Then if we give a random noise (not too much different from the noise distribution) from training, the model returns an output that's close to the original data distribution, but not exactly identical to the original data 
- How does this paper connect with other methods you know in the field?  
    - Different class of its own compared with GAN, flow matchings 

- What is a problem/drawback of the method? How can we try to modify the methodology (code/theory to adjust it)? 
    - A lot of steps needed in the backward process, very computationally expensive and slow (some methods like DDIM addressed this issue) 
- Can we replace diffusion process with something else?  
    - If we replace diffusion, we might need to replace both forward and backward. We either don't do forward passing to add noise, or we don't need the backward process to denoise. Similar with flow model where we go directly from noise to original data distribution. 
- Does flow matching require denoising?  
    - No because flow matching starts with a time $t$ and the ground truth data point $z$ and follows a mathematical formula to model where the input (noise) goes. 
    - The denoising process is done by moving the vector through the vector field (model). We don't need to add noise to original sample, we learn how to get to the original sample from the noise through a vector field using Optimal Transportation Path, etc. 
  
- What is the difference between this method and VAE class models?    
  
- How is markov chain used/implemented in this paper? What does it mean chain transition mean?  

- How to sample? 

- Model is conditional/dependent on t, during training we sample a random t, how does it transfer to successful sampling if each t is different?  
 
- How much different can it generate images from the original? 
  
### Impact: 
- Training diffusion model and achieved good results (based on metrics) at reconstructing images from CIFAR10 dataset compared with other methods like ProgressiveGAN.  
- Simplified weighted variational objective for diffusion models 
### Model classes: 
- GAN, VAE, autoregressive, flow models, energy based models 
- This paper is on diffusion probabilistic models (diffusion model) 
### diffusion model 
- Parameterized Markov Chain (markov assumption: $P(X_t|X_h)= P(X_t|X_{t-1})$) with variational inference to produce samples/outputs that match original data after finite time.  
- Reverse diffusion process: Markov chain adds noise to the data in opposite direction of sampling until signal is destroyed  
- Network parameterization is important for the diffusion model to have good performance. 
- Sampling process is progressive decoding which is similar with autoregressive models' sampling but vastly generalizes what's possible with autoregressive models  
### Forward Process 
- Forward: add noise to the input. Backward: get rid of these noise  
- Forward: Add noise from $X_0$ to $X_t$, where $X0$ is the original data distribution. At $X_t$ we'll have a random Gaussian noise. 
    - Each time we sample noise from $N(X_t; (\sqrt{ (1-\beta_t)*X_{t-1}}), \beta_t)$ where $0< \beta_{1} < ... < \beta_t <1$ 
- This forward process adding noise each time can be done in one step summarized by $N(X_t; \sqrt{\alpha_t}*X_0, (1-\alpha_t))$ for $\alpha_t =\prod_{s=1}^{t} (1-\beta_s)$ 
    - Note that we can do this because $\sqrt{a}*\sqrt{b}=\sqrt{a*b}$
### Backward Process: 
- Backward goes from $X_t$ to $X_0$, instead of predicting what the image at $X_{t-1}$ looks like, the trick is to predict the noise that's being added from $X_{t-1}$ to $X_t$ 
- Our network will take in the current input at generated time $t$, and predict noise at each step, get rid of the noise, repeat until $t=0$.  
### Training: 
- Sample a time step t and generate a noise 
- Choose a starting image $X_0$ 
- Use denoising model to predict noise from $X_t$ to $X_0$ 
### Why better? 
- Two pass methods compared with just one pass in GAN 
- The Unet in diffusion process gets the entire timestep $t$ times to denoise and make adjustments 


March 30/2025 