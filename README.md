# Image-generative-model-using-LoRA-and-DreamBooth
FishChief’s Customized Artistic Expression: Fine-Tuning Diffusion Models with Small Datasets Using DreamBooth and LoRA for Brand Illustration
These is the guild of how to use stable diffusion 1.5 to train cus

## Getting start


## Datasets
We will build two kinds of dataset:

- **Dreambooth dataset**: images only (20-50 images)
- **LoRA dataset**: images + .txt prompt file for each image (20-50 paired train data)
### Image dataset
Train images for SD1.5 require 512*512 pixels dimention, [BIRME](https://www.birme.net/) is an useful website to batch process photos. It can resize your images to any specific dimension and crop them proportionately if necessary.

### .txt prompt file (only for LoRA training)
There are several ways to generate .txt prompt file for each train image.
- [Comparing image captioning models](https://huggingface.co/spaces/nielsr/comparing-captioning-models) allows users to generate tags using different captioning models at the same time, just drag and drop the image in. However, captions provided by this website don't included too many details.
- [DeepDanbooru](https://github.com/KichangKim/DeepDanbooru) is designed specifically for estimating tags in anime-style girl images. There's also a [live demo site](http://dev.kanotype.net:8003/deepdanbooru/) available where you can test the system with your own images
- [Tagger](https://github.com/picobyte/stable-diffusion-webui-wd14-tagger) is an extension which needs to be installed in Stable Diffusion. It can not only generate booru style tags for single or multiple images but also save those tags into .txt file which is very convenient👍
- GPT4 is also an amazing tool to generate tags for images. With new function allowed users to upload images into communication box, users can ask GPT4 to generate tags for specific field like my oceanic illustrations. But for this method, we need to copy and paste tags into .txt file manually.

Since my images are focusing on topic of marine, most of the caption models are more suitable for portrait or anime-style humen, I chose to use GPT4 to help me generate tags.


## Training the model
### DreamBooth
### LoRA

## Image generation




