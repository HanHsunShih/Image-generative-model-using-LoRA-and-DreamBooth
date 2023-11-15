# Image-generative-model-using-LoRA-and-DreamBooth
FishChief’s Customized Artistic Expression: 

Fine-Tuning Diffusion Models with Small Datasets Using DreamBooth and LoRA for Brand Illustration.

<img src="https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/images/messageImage_1699447751593.jpg" width="400" alt="alt text">

These is the guild of how to use stable diffusion 1.5 to train customize models. 
We will train two kinds of models, DreamBooth and LoRA. Both of them only require 20-50 train images(DreamBooth) or paired train data(LoRA).
- DreamBooth checkpoint: a base model for generate images in Stable Diffusion, which is always 2-7 GB. In every image generation, you can only choose one checkpoint model.
- LoRA: which is always 20-100 MB, LoRA cannot be used independently; it needs to be used in conjunction with other checkpoint models. You can choose multiple LoRA models followed by one checkpoint model.

(You can also download checkpoint models or LoRA models from [Hugging Face](https://huggingface.co/models) or [CIVITAI](https://civitai.com/models).)

## Getting start
### Install Stable Diffusion Locally
As a Macbook user with M1/M2 chip, I suggest follow [this Manderine tutorial](https://hossie.notion.site/Stable-Diffusion-MacBook-M1-M2-dda94dc6d59943ea8bc4108897642637) or [this English GitHub](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Installation-on-Apple-Silicon#new-install) for installation on Apple Silicon.

First, open terminal on Mac, check python version:

```python
python3 --version
```

create virtual environment, download right version of python: 3.10.6, I named the virtual environment as SDW:
```python
conda create -n SDW python=3.10.6 -y
```

activate virtual environment SDW
```python
conda activate SDW
```

check SDW python version to check if the version is 3.10.6:
```python
python --version
```

Manually create a folder to clone Stable Diffusion. In my case, I created a folder named SDW on desktop,

after the folder has been created, cd to the folder in termonal:
```python
cd ~/Desktop/SDW
```

Clone github from https://github.com/AUTOMATIC1111/stable-diffusion-webui.git:
```python
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

Move to stable-diffusion-webui in SDW folder:
```python
cd stable-diffusion-webui
```

Install requirments in stable-diffusion-webui file
```python
pip install -r requirements.txt
```

Now go to [Hugging Face](https://huggingface.co/models) or [CIVITAI](https://civitai.com/models) to download checkpoint model then put it into model folder:

path: SDW/stable-diffusion-webui/models/Stable-diffusion

or download LoRA or other kinds of models then put into the right folder. SDW/stable-diffusion-webui/models/LoRA or other path for each kind of model.

(Note: checkpoint model is necessary for generate images, so you need to have at least one)

Now you can run SD model locally
```python
./webui.sh
```

### To terminate Stable Diffusion locally

Run ```control + c``` in your terminal if you want to terminate SD

### To restart Stable Diffusion locally


Activate virtual environment SDW
```python
conda activate SDW
```

Move to stable-diffusion-webui
```python
cd ~/Desktop/SDW/stable-diffusion-webui
```

Run SD model
```python
./webui.sh
```



### Create a vitual environment on RunPod
Or we can use Stable Diffusion via [RunPod](https://www.runpod.io/). Follow [this tutorial](https://www.youtube.com/watch?v=QN1vdGhjcRc&list=PL_pbwdIyffsnDMmNTzopgN6kYDS2KSv-s&ab_channel=SECourses).

On RunPod, we can chose GPU suitable for our project, I chose RTX 4090 since it's much faster, but also cost more money.
In my project, I use local Stable Diffusion Webui to run Stable Diffusion when there's no GPU assigned to my RunPod, which happens very often. (Or when I want to save money💸) 

<img src="https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/images/0%20GPU%20assigned.png" width="400" alt="alt text">

### Run Stable Diffusion via Colab Notebook
You can also run Stable Diffusion via Colab Notebook, there're many tutorials on [YouTube](https://www.youtube.com/watch?v=vpdM9RqkSaM&ab_channel=LauraCarnevali), but I didn't do this since although Colab’s pre-configured system is convenient, its frequent updates that can sometimes disrupt previously functioning codebases due to non-backward-compatible changes.

## Datasets
We will build two kinds of dataset:

- **Dreambooth dataset**: images only (20-50 images)
- **LoRA dataset**: images + .txt prompt file for each image (20-50 paired train data)
### Image dataset
Train images for SD1.5 require 512*512 pixels dimention, [BIRME](https://www.birme.net/) is an useful website to batch process photos. It can resize your images to any specific dimension and crop them proportionately if necessary.

Adobe Photoshop is another way to prepare train image. I used ChatGPT to generate a code for export each layers in .psd file:
https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/231115_export_layers_in_ps.ipynb

### .txt prompt file (only for LoRA training)
There are several ways to generate .txt prompt file for each train image.
- [Comparing image captioning models](https://huggingface.co/spaces/nielsr/comparing-captioning-models) allows users to generate tags using different captioning models at the same time, just drag and drop the image in. However, captions provided by this website don't included too many details.
- [DeepDanbooru](https://github.com/KichangKim/DeepDanbooru) is designed specifically for estimating tags in anime-style girl images. There's also a [live demo site](http://dev.kanotype.net:8003/deepdanbooru/) available where you can test the system with your own images
- [Tagger](https://github.com/picobyte/stable-diffusion-webui-wd14-tagger) is an extension which needs to be installed in Stable Diffusion. It can not only generate booru style tags for single or multiple images but also save those tags into .txt file which is very convenient👍
- GPT4 is also an amazing tool to generate tags for images. With new function allowed users to upload images into communication box, users can ask GPT4 to generate tags for specific field like my oceanic illustrations. But for this method, we need to copy and paste tags into .txt file manually.

Since my images are focusing on topic of marine, most of the caption models are more suitable for portrait or anime-style humen, I chose to use GPT4 to help me generate tags.

Give this instruction to GPT: 
> Please generate 20 booru style tags for this image, format: A, B, C, D......Only one word for each tag

<img src="https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/images/tag%20generation%20instruction%20for%20GPT4.jpg" width="800" alt="alt text">


Following image is how LoRA dataset suppose to look like:
<img src="https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/images/LoRA%20dataset.png" width="800" alt="alt text">

## Train the model
<img src="https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/images/3%20DreamBooth%20Models.jpg" width="800">

Image above are outcomes of DreamBooth generated images trained by different dataset and initial checkpoint.

### DreamBooth training
I followed [this tutorial](https://www.youtube.com/watch?v=g0wXIcRhkJk&t=1070s&ab_channel=SECourses) to train DreamBooth checkpoint on RunPod. Every DreamBooth checkpoint needs an initial checkpoint, since DreamBooth is a kind of fine-tuning technique. You can download initial checkpoint models from [Hugging Face](https://huggingface.co/models) or [CIVITAI](https://civitai.com/models). Initial chackpoint model plays an important role in DreamBooth training. Following image shows different outcomes generated by 3 DreamBooth models (each model was trained by different initial checkpoint) using same prompt.

<img src="https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/images/checkpoin%20outcomes.png" width="800">

### LoRA training
I followed [this tutorial](https://www.youtube.com/watch?v=oksoqMsVpaY&t=5s&ab_channel=Code%26bird) using [this](https://colab.research.google.com/github/Linaqruf/kohya-trainer/blob/main/kohya-LoRA-dreambooth.ipynb) Google Colab Notebook to train LoRA model.
This tutorial simplified the process of training LoRA which is very useful.

I altered the Colab Notebook above into my own version of LoRA model training.
https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/1103_KillerWhale_kohya_LoRA_dreambooth.ipynb

Following is the outcome of LoRA I trained combined with 3 different checkpoint models. It shows that with different checkpoints, the outcome could be very different.

<img src="https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/images/FC%20style%20LoRA.png" width="800">

## Image generation

Here's final outcome of my project, I used DreamBooth checkpoint followed by 2 LoRA models. The platform I use for this image generation is RunPod.

<img src="https://github.com/HanHsunShih/Image-generative-model-using-LoRA-and-DreamBooth/blob/main/images/final%20outcome.png" width="800">





