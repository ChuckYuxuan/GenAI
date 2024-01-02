### Acknowledgments

Utilization of the Hotshot-XL Model on the OPV2V Dataset

In our recent endeavors, we have rigorously applied and evaluated the capabilities of the Hotshot-XL models on the OPV2V dataset. We extend our sincere gratitude to the creators of both the Hotshot-XL model and the OPV2V dataset for enabling this research. The original project can be found here: [Hotshot-XL](https://github.com/hotshotco/hotshot-xl). [OPV2V](https://github.com/DerrickXuNu/OpenCOOD)

 
### Additional Changes:
We have meticulously extracted front camera images from the OPV2V dataset and subsequently amalgamated them into frames, thereby creating a concise video that illustrates traffic scenarios within the CARLA simulator. Further, we employed fine_tune.py and inference.py scripts to rigorously test the capabilities of HotShot-XL in generating extended videos, with durations varying between 2 to 5 seconds per video. This process was aimed at evaluating HotShot-XL's proficiency in handling longer sequences and more complex dynamic environments.


### Introduction:
About OPV2V:
OPV2V represents the forefront of large-scale Open Datasets for Perception with Vehicle-to-Vehicle (V2V) communication. It was meticulously assembled using a cooperative driving co-simulation framework, OpenCDA, alongside the CARLA simulator. This dataset encompasses a wide array of diverse scenarios, featuring various connected vehicles aimed at addressing complex driving challenges, particularly those involving severe occlusions.

Key Features of OPV2V:

Aggregated Sensor Data: Utilizes comprehensive sensor data collected from multiple connected automated vehicles.
Extensive Scenario Coverage: Includes 73 diverse scenes across 6 road types and 9 cities.
Rich Data Collection: Features 12K frames of LiDAR point clouds and RGB camera images, coupled with 230K annotated 3D bounding boxes.
Comprehensive Benchmarks: Provides an extensive benchmark encompassing 4 LiDAR detectors and 4 distinct fusion strategies, cumulatively presenting 16 models.

Hotshot-XL is an AI text-to-GIF model trained to work alongside [Stable Diffusion XL](https://stability.ai/stable-diffusion). 

Hotshot-XL can generate GIFs with any fine-tuned SDXL model. This means two things:
1. You’ll be able to make GIFs with any existing or newly fine-tuned SDXL model you may want to use.
2. If you'd like to make GIFs of personalized subjects, you can load your own SDXL based LORAs, and not have to worry about fine-tuning Hotshot-XL. This is awesome because it’s usually much easier to find suitable images for training data than it is to find videos. It also hopefully fits into everyone's existing LORA usage/workflows :) See more [here](#text-to-gif-with-personalized-loras).

Hotshot-XL is compatible with SDXL ControlNet to make GIFs in the composition/layout you’d like. See the [ControlNet](#text-to-gif-with-controlnet) section below.

Hotshot-XL was trained to generate 1 second GIFs at 8 FPS.

Hotshot-XL was trained on various aspect ratios. For best results with the base Hotshot-XL model, we recommend using it with an SDXL model that has been fine-tuned with 512x512 images. You can find an SDXL model we fine-tuned for 512x512 resolutions [here](https://huggingface.co/hotshotco/SDXL-512).

# 🌐 Try It

Try Hotshot-XL yourself here: https://www.hotshot.co

Or, if you'd like to run Hotshot-XL yourself locally, continue on to the sections below.

If you’re running Hotshot-XL yourself, you are going to be able to have a lot more flexibility/control with the model. As a very simple example, you’ll be able to change the sampler. We’ve seen best results with Euler-A so far, but you may find interesting results with some other ones.

# 🔧 Setup

### Environment Setup
```
pip install virtualenv --upgrade
virtualenv -p $(which python3) venv
source venv/bin/activate
pip install -r requirements.txt
```
### Download the Training Dataset
Our training dataset is organized into multiple subfolders, each containing hundreds of frames and a text prompt that describes the scene. Follow these steps to download the dataset:
1. **Access the Dataset:**
   - Access all the data from the [Training Dataset Folder](https://drive.google.com/drive/folders/1xZTRrTwdB12A3XsEDtY_Z53aFORrTIlL) on Google Drive.
2. **Download Individual Subfolders:**
   - Inside the main folder, you will find subfolders for each scene. If you have a good internet connection, you can download these subfolders individually.
3. **Handling Large Files:**
   - In case you encounter issues with downloading large files, consider downloading smaller portions of the dataset.
4. **Merging Segments (if applicable):**
   - If the dataset is split into parts, merge them using the command line:
     ```bash
     cat scene_folder_name.zip.part* > scene_folder_name.zip
     ```
   - Replace `scene_folder_name` with the actual name of your scene's subfolder.
   - Unzip the merged file with:
     ```bash
     unzip scene_folder_name.zip
     ```
5. **Post-Download:**
   - After downloading and extracting, check that each scene folder contains all the frames and the associated text prompt.

For any issues or assistance, please contact our support team.

### Download the Hotshot-XL Weights

```
# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone https://huggingface.co/hotshotco/Hotshot-XL
```

or visit [https://huggingface.co/hotshotco/Hotshot-XL](https://huggingface.co/hotshotco/Hotshot-XL)

### Download our fine-tuned SDXL model (or BYOSDXL)

- *Note*: To maximize data and training efficiency, Hotshot-XL was trained at various aspect ratios around 512x512 resolution. For best results with the base Hotshot-XL model, we recommend using it with an SDXL model that has been fine-tuned with images around the 512x512 resolution. You can download an SDXL model we trained with images at 512x512 resolution below, or bring your own SDXL base model.
  
```
# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
git clone https://huggingface.co/hotshotco/SDXL-512
```

or visit [https://huggingface.co/hotshotco/SDXL-512](https://huggingface.co/hotshotco/SDXL-512)

# 🔮 Inference

### Text-to-GIF
```
python inference.py \
  --prompt="a bulldog in the captains chair of a spaceship, hd, high quality" \
  --output="output.gif" 
```


### Text-to-GIF with personalized LORAs

```
python inference.py \
  --prompt="a bulldog in the captains chair of a spaceship, hd, high quality" \
  --output="output.gif" \
  --spatial_unet_base="path/to/stabilityai/stable-diffusion-xl-base-1.0/unet" \
  --lora="path/to/lora"
```


### Text-to-GIF with ControlNet
```
python inference.py \
  --prompt="a girl jumping up and down and pumping her fist, hd, high quality" \
  --output="output.gif" \
  --control_type="depth" \
  --gif="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExbXNneXJicG1mOHJ2dzQ2Y2JteDY1ZWlrdjNjMjl3ZWxyeWFxY2EzdyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/YOTAoXBgMCmFeQQzuZ/giphy.gif"
```

By default, Hotshot-XL will create key frames from your source gif using 8 equally spaced frames and crop the keyframes to the default aspect ratio. For finer grained control, learn how to [vary aspect ratios](#varying-aspect-ratios) and [vary frame rates/lengths](#varying-frame-rates--lengths-experimental).

Hotshot-XL currently supports the use of one ControlNet model at a time; supporting Multi-ControlNet would be [exciting](#-further-work).

*What to Expect:*
| **Prompt** | Car driving in a rainy day, HD, high resolution. | Car driving in a foggy day, HD, high resolution. | Car driving in a rainy day, HD, high resolution, multiple cars passing by | A green Car driving in the center of the camera in a rainy day, HD, high resolution, multiple cars passing by |
|-----------|----------|----------|----------|----------|
| **Output** | <img src="https://storage.googleapis.com/genaioutputgifs-fall2023/output.gif"/>         | <img src="https://storage.googleapis.com/genaioutputgifs-fall2023/output_207_v3.gif"/>          |  <img src="https://storage.googleapis.com/genaioutputgifs-fall2023/output_207_v5.gif">       |  <img src="https://storage.googleapis.com/genaioutputgifs-fall2023/output_207_v6.gif"/>        |
| **Control** |<img src="https://storage.googleapis.com/genaioutputgifs-fall2023/Oiginal.gif"  /> | <img src="https://storage.googleapis.com/genaioutputgifs-fall2023/Oiginal.gif"  /> | <img src="https://storage.googleapis.com/genaioutputgifs-fall2023/Oiginal.gif"  /> | <img src="https://storage.googleapis.com/genaioutputgifs-fall2023/Oiginal.gif"  /> |


### Varying Aspect Ratios

- *Note*: The base SDXL model is trained to best create images around 1024x1024 resolution. To maximize data and training efficiency, Hotshot-XL was trained at aspect ratios around 512x512 resolution. Please see [Additional Notes](#supported-aspect-ratios) for a list of aspect ratios the base Hotshot-XL model was trained with.

Like SDXL, Hotshot-XL was trained at various aspect ratios with aspect ratio bucketing, and includes support for SDXL parameters like target-size and original-size. This means you can create GIFs at several different aspect ratios and resolutions, just with the base Hotshot-XL model. 

```
python inference.py \
  --prompt="a bulldog in the captains chair of a spaceship, hd, high quality" \
  --output="output.gif" \
  --width=<WIDTH> \
  --height=<HEIGHT>
```


### Varying frame rates & lengths (*Experimental*)
By default, Hotshot-XL is trained to generate GIFs that are 1 second long with 8FPS. If you'd like to play with generating GIFs with varying frame rates and time lengths, you can try out the parameters `video_length` and `video_duration`.

`video_length` sets the number of frames. The default value is 8.

`video_duration` sets the runtime of the output gif in milliseconds. The default value is 1000.

Please note that you should expect unstable/"jittery" results when modifying these parameters as the model was only trained with 1s videos @ 8fps. You'll be able to improve the stability of results for different time lengths and frame rates by [fine-tuning Hotshot-XL](#-fine-tuning). Please let us know if you do!

```
python inference.py \
  --prompt="a bulldog in the captains chair of a spaceship, hd, high quality" \
  --output="output.gif" \
  --video_length=16 \
  --video_duration=2000
```

### Spatial Layers Only
Hotshot-XL is trained to generate GIFs alongside SDXL. If you'd like to generate just an image, you can simply set `video_length=1` in your inference call and the Hotshot-XL temporal layers will be ignored, as you'd expect.

```
python inference.py \
  --prompt="a bulldog in the captains chair of a spaceship, hd, high quality" \
  --output="output.jpg" \
  --video_length=1 
```

### Additional Notes

#### Supported Aspect Ratios
Hotshot-XL was trained at the following aspect ratios; to reliably generate GIFs outside the range of these aspect ratios, you will want to fine-tune Hotshot-XL with videos at the resolution of your desired aspect ratio.

| Aspect Ratio | Size |
|--------------|------|
| 0.42         |320 x 768|
| 0.57         |384 x 672|
| 0.68         |416 x 608|
| 1.00         |512 x 512|
| 1.46         |608 x 416|
| 1.75         |672 x 384|
| 2.40         |768 x 320|


# 💪 Fine-Tuning
The following section relates to fine-tuning the Hotshot-XL temporal model with additional text/video pairs. If you're trying to generate GIFs of personalized concepts/subjects, we'd recommend not fine-tuning Hotshot-XL, but instead training your own SDXL based LORAs and [just loading those](#text-to-gif-with-personalized-loras).

### Fine-Tuning Hotshot-XL

#### Dataset Preparation

The `fine_tune.py` script expects your samples to be structured like this:

```
fine_tune_dataset
├── sample_001
│  ├── 0.jpg
│  ├── 1.jpg
│  ├── 2.jpg
...
...
│  ├── n.jpg
│  └── prompt.txt
```

Each sample directory should contain your **n key frames** and a `prompt.txt` file which contains the prompt.
The final checkpoint will be saved to `output_dir`.
We've found it useful to send validation GIFs to [Weights & Biases](www.wandb.ai) every so often. If you choose to use validation with Weights & Biases, you can set how often this runs with the `validate_every_steps` parameter.

```
accelerate launch fine_tune.py \
    --output_dir="<OUTPUT_DIR>" \
    --data_dir="fine_tune_dataset" \
    --report_to="wandb" \
    --resolution=512 \
    --mixed_precision=fp16 \
    --train_batch_size=4 \
    --learning_rate=1.25e-05 \
    --lr_scheduler="constant" \
    --lr_warmup_steps=0 \
    --max_train_steps=1000 \
    --save_n_steps=20 \
    --vae_b16 \
    --gradient_checkpointing \
    --noise_offset=0.05 \
    --snr_gamma \
```

# 📝 Further work
There are lots of ways we are excited about improving Hotshot-XL. For example:

- [ ] Fine-Tuning Hotshot-XL at larger frame rates to create longer/higher frame-rate GIFs
- [ ] Fine-Tuning Hotshot-XL at larger resolutions to create higher resolution GIFs
- [ ] Training temporal layers for a latent upscaler to produce higher resolution GIFs
- [ ] Training an image conditioned "frame prediction" model for more coherent, longer GIFs
- [ ] Training temporal layers for a VAE to mitigate flickering/dithering in outputs
- [ ] Supporting Multi-ControlNet for greater control over GIF generation
- [ ] Training & integrating different ControlNet models for further control over GIF generation (finer facial expression control would be very cool)
- [ ] Moving Hotshot-XL into [AITemplate](https://github.com/facebookincubator/AITemplate) for faster inference times

We 💗 contributions from the open-source community! Please let us know in the issues or PRs if you're interested in working on these improvements or anything else!


### Copyright & Contact:
The OPV2V dataset is a proprietary creation of UCLA Mobility Lab and is fully protected under copyright laws. All rights are reserved by the institution. For further inquiries or detailed information, the creators can be contacted at jiaqima@ucla.edu.

As we continue to explore and expand upon the capabilities of the Hotshot-XL models using the OPV2V dataset, we are committed to adhering to all ethical guidelines, giving due credit, and upholding the principles of academic integrity.

Text-to-Video models are improving quickly and the development of Hotshot-XL has been greatly inspired by the following amazing works and teams:

- [SDXL](https://stability.ai/stable-diffusion)
- [Align Your Latents](https://research.nvidia.com/labs/toronto-ai/VideoLDM/)
- [Make-A-Video](https://makeavideo.studio/)
- [AnimateDiff](https://animatediff.github.io/)
- [Imagen Video](https://imagen.research.google/video/)

We hope that releasing this model/codebase helps the community to continue pushing these creative tools forward in an open and responsible way.

### Original Work Details
Hotshot-XL Project:

Authors: John Mullan, Duncan Crawbuck, Aakash Sastry
Version: 1.0.0, October 2023
License: Apache-2.0 (Details below)
URL: https://github.com/hotshotco/hotshot-xl

OPV2V Dataset:

Authors: Runsheng Xu, Hao Xiang, Xin Xia, Xu Han, Jinlong Li, Jiaqi Ma
Published: 2022
In: 2022 IEEE International Conference on Robotics and Automation (ICRA)
Details: OPV2V represents a comprehensive Open Benchmark Dataset and Fusion Pipeline for Perception with Vehicle-to-Vehicle Communication.
URL: https://github.com/DerrickXuNu/OpenCOOD

### Citation
Please cite:
```
@software{Hotshot-XL_2023,
  title = {{Hotshot-XL}},
  author = {Mullan, John and Crawbuck, Duncan and Sastry, Aakash},
  month = oct,
  year = {2023},
  version = {1.0.0},
  license = {Apache-2.0},
  url = {https://github.com/hotshotco/hotshot-xl}
}

@inproceedings{xu2022opencood,
  title = {OPV2V: An Open Benchmark Dataset and Fusion Pipeline for Perception with Vehicle-to-Vehicle Communication},
  author = {Xu, Runsheng and Xiang, Hao and Xia, Xin and Han, Xu and Li, Jinlong and Ma, Jiaqi},
  booktitle = {2022 IEEE International Conference on Robotics and Automation (ICRA)},
  year = {2022}
}
```
## About the Maintainer
- **Name:** Yuxuan Zhao, Yuxiao Zheng
- **Contact:** yz4397@columbia.edu, yz4364@columbia.edu
- **Affiliation/Organization:** Columbia University
