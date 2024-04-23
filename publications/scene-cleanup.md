# Geometry and Texture generation of object-based occlusions in 3D scenes

## Introduction

**Structure**
- Context: the need for hole infilling
- Current SOA solutions
	- 3d pointcloud object


## Background and Related Work

**Structure**
- Object detection
- Hole filling
- Semantic unwrapping
- Texture reconstruction

### Object detection

[V-DETR 2023](https://github.com/V-DETR/V-DETR)
> Uses DETR (detection transformer) in 3D with Vertex Relative Position Encoding to improve locality

[VoteNet 2019](https://github.com/facebookresearch/votenet)
> an end-to-end 3D object detection network based on a synergy of deep point set networks and Hough voting.

### Plane detection

> [Latent RANSAC 2018](https://github.com/rlit/LatentRANSAC)

### Scene completion

[ScanComplete 2018](https://github.com/angeladai/ScanComplete)
> A novel data-driven approach for taking an incomplete 3D scan of a scene as input and predicting a complete 3D model along with per-voxel semantic labels.

[SISNet 2021](https://github.com/yjcaimeow/SISNet)
> a novel framework named Scene-Instance-Scene Network *SISNet*, which takes advantages of both instance and scene level semantic information. Our method is capable of inferring fine-grained shape details as well as nearby objects whose semantic categories are easily mixed-up.

[VIS2Mesh 2021](https://github.com/gdaosu/vis2mesh?tab=readme-ov-file)
> Efficient Mesh Reconstruction from Unstructured Point Clouds of Large Scenes with Learned Virtual View Visibility

### Texture inpainting

[Inpaint anything 2023](https://github.com/geekyutao/inpaint-anything?tab=readme-ov-file#remove-anything-3d)
> Users can select any object in an image by clicking on it. With powerful vision models, e.g., [SAM](https://arxiv.org/abs/2304.02643), [LaMa](https://arxiv.org/abs/2109.07161) and [Stable Diffusion (SD)](https://arxiv.org/abs/2112.10752), **Inpaint Anything** is able to remove the object smoothly (i.e., _Remove Anything_). Further, prompted by user input text, Inpaint Anything can fill the object with any desired content (i.e., _Fill Anything_) or replace the background of it arbitrarily (i.e., _Replace Anything_).

[MAT (Mask Aware Transformer) 2022](https://github.com/fenglinglwb/MAT)
> we present a novel transformer-based model for large hole inpainting, which unifies the merits of transformers and convolutions to efficiently process high-resolution images.

[CM-GAN 2022](https://github.com/htzheng/CM-GAN-Inpainting)
> We introduce a new cascaded modulation design that cascades global modulation with spatial adaptive modulation for better hole filling. We also introduce an object-aware training scheme to facilitate better object removal. CM-GAN significantly improves the existing state-of-the-art methods both qualitatively and quantitatively.

## Methodology

### Object detection

Objects are detected using state of the art object detection methods

### Object removal

Objects are removed by cutting out the bounding boxes

### Hole Filling

Using plane detection of the adjacent existing faces to determine the infill plane

### Semantic unwrapping

In order to properly paint the textures in, we need an undistorted uv projection. We aim to improve this workflow by introducing a semantic based unwrapping algorithm which separates the different parts of the scene into distinct planes

### Texture reconstruction

Texture in the newly generated geometry Using the surrounding faces as reference areas.

## Experiments

### Object detection

Report the accuracy of the object removal pipeline, including false positives and negatives
> Metric
> IOU
### Object removal

Evaluate the resulting holes and there usefulness for the next steps
### Hole filling

Evaluate the filled holed compared to the ground truth

> Metric
> Surface to surface distance
### Texture Reconstruction




