<h1 align="center"> StreamMOS: Streaming Moving Object Segmentation with Multi-View Perception and Dual-Span Memory </h1>

<div align="center">

**Zhiheng Li**, Yubo Cui, Jiexi Zhong, Zheng Fang*

[![arXiv](https://img.shields.io/badge/arXiv-2407.17905-b31b1b.svg)](https://arxiv.org/abs/2407.17905)

</div>

![](https://github.com/NEU-REAL/StreamMOS/blob/main/picture/SemanticKITTI.png)
## Absract

Moving object segmentation based on LiDAR is a crucial and challenging task for autonomous driving and mobile robotics. Most approaches explore spatio-temporal information from LiDAR sequences to predict moving objects in the current frame. However, they often focus on transferring temporal cues in a single inference and regard every prediction as independent of others. This may cause inconsistent segmentation results for the same object in different frames. To overcome this issue, **we propose a streaming network with a memory mechanism, called StreamMOS**, to build the association of features and predictions among multiple inferences. Specifically, we utilize a **short-term memory** to convey historical features, which can be regarded as spatial prior of moving objects and adopted to enhance current inference by temporal fusion. Meanwhile, we build a **long-term memory** to store previous predictions and exploit them to refine the present forecast at voxel and instance levels through voting. Besides, we present **multi-view encoder** with cascade projection and asymmetric convolution to extract motion feature of objects in different representations. Extensive experiments validate that our algorithm gets competitive performance on SemanticKITTI and Sipailou Campus datasets.

## Overview
<p align="center">
    <img src="picture/StreamMOS.png" width="100%">
</p>

**The overall architecture of StreamMOS.** (a) *Feature encoder* adopts a point-wise encoder to extract point features and project them into BEV. Then, the multi-view encoder with cascaded structure and asymmetric convolution is applied to encode motion features from different views. (b) *Temporal fusion* utilizes an attention module to propagate memory feature to the current inference. (c) *Segmentation decoder* with parameter-free upsampling exploits multi-scale features to predict class labels. (d) *Voting mechanism* leverages memory predictions to optimize the motion state of each 3D voxel and instance.

<!--## Challenging Scenes
![](https://github.com/NEU-REAL/StreamMOS/blob/main/picture/SemanticKITTI.png)
![](https://github.com/NEU-REAL/StreamMOS/blob/main/picture/Sipailou_Campus.png)-->

## Quickstart

### 0. Data Download
Download [SemanticKITTI](http://www.semantic-kitti.org/dataset.html#overview) dataset to the folder `SemanticKITTI`. The data structure is as follows:        
```
├──StreamMOS 
  ├──SemanticKITTI
    ├──dataset
      ├──sequences
        ├── 00         
        │   ├── velodyne
        |   |	├── 000000.bin
        |   |	├── 000001.bin
        |   |	└── ...
        │   └── labels 
        |       ├── 000000.label
        |       ├── 000001.label
        |       └── ...
        ├── 08 # for validation
```
Download the [object bank](https://drive.google.com/file/d/1QdSpkMLixvKQL6QPircbDI_0-GlGwsdj/view?usp=sharing) of SemanticKITTI to the folder `object_bank_semkitti`. The data structure is as follows:   
```
├──StreamMOS 
  ├──object_bank_semkitti
    ├── bicycle
    ├── bicyclist
    ├── car
    ├── motorcycle
    ├── motorcyclist
    ├── other-vehicle
    ├── person
    ├── truck
```


### 1. Environment Setup
Our code is implemented on Python 3.8 with Pytorch 2.1.0 and CUDA 11.8. To reproduce and use our environment, you can use the following command:

a. Clone the repository to local
```
git clone https://github.com/NEU-REAL/StreamMOS.git
cd StreamMOS
```               
b. Set up a new environment with Anaconda
```
conda create -n stream python=3.8
conda activate stream
```                       
c. Install common dependices and pytorch
```
pip install torch==2.1.0 torchvision==0.16.0 torchaudio==2.1.0 --index-url https://download.pytorch.org/whl/cu118
pip install -r requirements.txt
```

### 3. Model Training
Running the following command to start the first stage of training.
```
python -m torch.distributed.launch --nproc_per_node=2 train_StreamMOS.py --config config/StreamMOS.py
```

### 4. Model Evaluation
Running the following command to start evaluation.
```
python3 -m torch.distributed.launch --nproc_per_node=2 val_StreamMOS.py --config config/StreamMOS.py
```

## Acknowledgement

This repo is based on [CPGNet](https://github.com/GangZhang842/CPGNet) and [SMVF](https://github.com/GangZhang842/SMVF), we are very grateful for their excellent work.                     

## Citation

If you find our repository useful, please consider citing us as
```
@ARTICLE{10804055,
  author={Li, Zhiheng and Cui, Yubo and Zhong, Jiexi and Fang, Zheng},
  journal={IEEE Robotics and Automation Letters}, 
  title={StreamMOS: Streaming Moving Object Segmentation With Multi-View Perception and Dual-Span Memory}, 
  year={2025},
  volume={10},
  number={2},
  pages={1146-1153},
  keywords={Laser radar;Three-dimensional displays;Motion segmentation;Feature extraction;Point cloud compression;Object segmentation;Heuristic algorithms;Convolution;Dynamics;Decoding;Computer vision for transportation;deep learning methods;semantic scene understanding},
  doi={10.1109/LRA.2024.3518844}}
```
