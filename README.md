# Final Project

Video Link: https://www.youtube.com/shorts/xaxLUWsybYU

GitHub Link: https://github.com/22015680/Coding3

Model Link: https://civitai.com/models/90901/zhongligenshinimpact

## Aim of my project
In this project, I aim to train a Zhongli Low-Rank Adaptation of Large Language Model (LoRA) to generate an animation of the character performing various actions. The animation resembles the 2D anime style while maintaining the realistic 3D rendering of the character. My main objective is to leverage the power of MMD (MikuMikuDance), a popular 3D animation software, along with recent advances in generative modeling such as Stable Diffusion(SD) and ControlNet to produce realistic and diverse animations.

## Methods used to implement project
The tools used in the project will be explained. First, LoRA is a training method that accelerates the training of large models while consuming less memory. In simple terms, the LoRA models are small Stable Diffusion models. Using LoRA can make it easier to train characters or a specific style model. Second, SD is a latent diffusion model, a kind of deep generative neural network. It is primarily used to generate detailed images conditioned on text descriptions, though it can also be applied to other tasks such as inpainting, outpainting, and generating image-to-image translations guided by a text prompt. Last, ControlNet is a neural network structure to control diffusion models by adding extra conditions. 

To achieve the goal of my project, I first collect a big dataset of Zhongli. I used ImageAssistant Batch Image Downloader, an extension of Chrome, and manual saving to extract 1527 images from pinterest/lofter/weibo. Then, I manually removed irrelevant, multi-person, transsexual, low quality, low resolution, real person, Chibi version, comics, black and white images, images lacking character features that differ too much from the official model, and selected images with similar painting style, selecting pictures with white background as much as possible. Next, I downloaded the official model made by Haizi Guan and imported it into blender for a multi-angle and detailed screenshot. In this process, it's necessary to convert the materials of the MMD model. Here, I used the CATS plugin tool. In addition, it's also needed to place a white background in multiple directions so that the background of the cropped image is not transparent. After a series of selection, 114 images remained in the database.

In addition, I used dataset-tool to remove the duplicate images and used stable diffusion WebUI auto focal point crop size mode to crop the selected pictrues into 512*512 pixel size, because most of LoRA base models were trained by 512*512 pixel size images. This operation ensured consistent image sizes, saved unnecessary training time when processing images and speeded up training. 
![Samples in the dataset](/images/1.png)
<p align="center">Samples in the dataset</p>
  
When using the WebUI to crop and resize pictures, I still used deepbooru for caption, which was essential for tagging the pictures before training. In the tags of each picture, the character features tags should be removed because it can prevent overfitting by leading such contents included in the base model to LoRA. Besides, deleting tags can ensure character uniformity and retain character features, in order to align as closely as possible with specific characters. Editing the tags file needs to follow the formula of LoRA model + tag = training image. Therefore, I deleted relevant contents in the tags of each picture one by one. Afterward, I studied the tutorials, and training information of other models and tried to train my own model. I have trained four models. The training information is below:
<div float="left">
  <img src="/images/2.png"/>
  <p align="center">Model 1</p>
</div>
<div float="left">
  <img src="/images/3.png"/> 
  <p align="center">Model 2</p>
</div>
<div float="left">
  <img src="/images/4.png"/>
  <p align="center">Model 3</p>
</div>
<div float="left">
  <img src="/images/5.png"/> 
  <p align="center">Model 4</p>
</div>

I tested the results of these four model. It turned out to be below:
<p float="left">
  <img src="/images/6.png"/>
  <img src="/images/7.png"/> 
  <img src="/images/8.png"/> 
</p>
<p align="center">Examples of first generation</p>
<p float="left">
  <img src="/images/9.png"/>
  <img src="/images/10.png"/> 
  <img src="/images/11.png"/> 
</p>
<p align="center">Examples of second generation</p>
<p float="left">
  <img src="/images/12.png"/>
  <img src="/images/13.png"/> 
  <img src="/images/14.png"/> 
</p>
<p align="center">Examples of third generation</p>
<p float="left">
  <img src="/images/15.png"/>
  <img src="/images/16.png"/> 
  <img src="/images/17.png"/> 
</p>
<p align="center">Examples of fourth generation</p>

From the results, it can be seen that the first generation has the best results. Although in the second generation, I used a higher number of epochs and the loss value was the lowest, bad anatomy still occurred in the images. With the same keywords input, the second, third and fourth generation models all would generate the images with bad anatomy. Obviously, the accuracy of the first generation is the highest and the painting style is close to the character's prototype.

The next step after training LoRA is the video output. This requires downloading the character model and the MMD motion file, then exporting a weakly rendered and lighted 3d video from the MMD. Here, I used the official MMD model from miHoYo, which was made by Haizi Guan. The motion file and camera motion file were created by Xiaomengdouding. Then, I recorded a video in size of 512*512 pixel and a frame rate of 30fps. (I tried a 1024*1024 pixel video with 60fps and found that it took 24h to render, so I used a lower pixel and frame rate.) 

After that, I imported the video into PR to extract each frame. Then, I rendered each image using the img2img function of the WebUI with Stable Diffusion on board. During the process I added my Lora model and useed the canny model from the ControlNet plugin for image edge detection. After several adjustments of the parameters, the final parameters resulted in the following: 
![Parameters for AI picture generation](/images/18.png)
![Parameters for AI picture generation](/images/19.png)
<p align="center">Parameters for AI picture generation</p>

Finally, there is a check and adjustment of the images. Because the movement and transformation of the camera can make the image hard to recognize, especially those full-body shots far from the camera, SD generated incorrect human figures. Fortunately, I solved the problem by extracting most of the images in error and regenerated them by changing the parameter and deleting unnecessary prompts.

## Result and Reflection
The project has successfully achieved a significant milestone: training a LoRA model adept at generating realistic animations of the character Zhongli. This was accomplished through a well-coordinated implementation of advanced technologies such as MikuMikuDance, Stable Diffusion, and ControlNet.

The primary aim of the project was to create quality 3D animations from 2D anime-style images. I am pleased to note that the objective was accomplished with considerable success, especially with the first-generation model. It is capable of generating accurate character renditions without the requirement of inputting character feature prompts. This project holds potential in fields like game development, animation, and virtual reality.

However, I recognize there were some challenges during the model training process. Despite the lowest loss value achieved during the second-generation model training, the results were not as satisfactory as expected. This reveals a crucial learning point for me, emphasizing that an optimal balance between loss value and model complexity is pivotal to yielding better results.

Reflecting on the model training process, I realize the absence of a test set for training could have provided better insight into the performance of my model. A test set could have shed light on the model's ability to generalize and would be a reliable metric against overfitting. Therefore, in future projects, I plan to establish a test training set for more comprehensive model evaluation.

Another limiting factor that I acknowledge was the constraints posed by the computer configuration used for training. It is likely that this limitation could have led to a degree of underfitting in the models. Therefore, to enhance the precision of training in future projects, I intend to use a more powerful machine or avail myself of cloud-based services.

In conclusion, this project, which combines the latest AI modeling techniques with animation, makes a valuable contribution to the field of AI-generated image applications. I believe that the insights gained from this project will play a role in the intersection of technology and creativity.

## References
Stable Diffusion WebUI: https://github.com/AUTOMATIC1111/stable-diffusion-webui

ControlNet WebUI: https://github.com/Mikubill/sd-webui-controlnet

LoRA WebUI: https://github.com/kohya-ss/sd-webui-additional-networks
