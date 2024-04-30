# Geometry and Texture generation of object-based occlusions in 3D scenes

## Introduction

**Structure**
- Context: the need for hole filling
	- After scans we need clean structural models for further processing
	- For remodeling and renovations, we also need a textural representation
- Current SOA solutions
	- Full workflows
		- Object detection and inpainting based on 2d image
			- focussed on removing elements against walls, not in the middle of the room
		- generate an sdf
			- no textural reconstruction
	- Geometry completion
		- ScanComplete Geometry + semantics
		- Texture inpainting
	- Scene Segmentation
		- instance segmentation 
	- Texture Inpainting
- The key shortcomings
	- requires images for 2d inpainting
	- the inpainting either happens in 3d using voxels or vertex colours, resulting in a low resolution, or in 2d from a projected view, difficult due to occlusions
- Our solution
	- use a state of the art object detection and segmentation
	- keep the detected segmented instances to also create uv maps of the remaining environment
	- perform the texture inpainting on the partial mesh hole using the surrounding faces as reference
	- use it on the uv plane to eliminate the need for extra images and prevent occlusions covering the important texture parts


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

### Semantic segmentation

[PTv3](https://github.com/Pointcept/Pointcept)
> Point transformer v3


### Plane detection

[Latent RANSAC 2018](https://github.com/rlit/LatentRANSAC)
> a method that can evaluate a RANSAC hypothesis in constant time.

### Scene completion

[Clutter detection and removal](https://weify627.github.io/clutter/)
> Clutter detection network + camera image based depth & image inpainting

[ScanComplete 2018](https://github.com/angeladai/ScanComplete)
> A novel data-driven approach for taking an incomplete 3D scan of a scene as input and predicting a complete 3D model along with per-voxel semantic labels.

[SISNet 2021](https://github.com/yjcaimeow/SISNet)
> a novel framework named Scene-Instance-Scene Network *SISNet*, which takes advantages of both instance and scene level semantic information. Our method is capable of inferring fine-grained shape details as well as nearby objects whose semantic categories are easily mixed-up.

[CIRCLE 2021](https://github.com/otakuxiang/circle)
> CIRCLE (Convolutional Implicit Reconstruction and Completion for Large-scale Indoor Scene) is a framework for large-scale scene completion and geometric refinement based on local implicit signed distance function.

[SPSG 2021](https://github.com/angeladai/spsg)
> SPSG presents a self-supervised approach to generate high-quality, colored 3D models of scenes from RGB-D scan observations by learning to infer unobserved scene geometry and color. Rather than relying on 3D reconstruction losses to inform our 3D geometry and color reconstruction, we propose adversarial and perceptual losses operating on 2D renderings in order to achieve high-resolution, high-quality colored reconstructions of scenes.

[VIS2Mesh 2021](https://github.com/gdaosu/vis2mesh?tab=readme-ov-file)
> Efficient Mesh Reconstruction from Unstructured Point Clouds of Large Scenes with Learned Virtual View Visibility

[NeRFiller 2023](https://github.com/ethanweber/nerfiller)
> NeRFiller completes missing regions in 3D scenes via an off-the-shelf 2D generative inpainter.

[No Shadow Left Behind](https://arxiv.org/abs/2012.10565)
> 2d object removal and inpainting

### Semantic unwrapping

[GraphSeam 2020](https://www.researchgate.net/publication/346475280_GraphSeam_Supervised_Graph_Learning_Framework_for_Semantic_UV_Mapping)
> we use the power of supervised GNNs for the first time to propose a fully automated UV mapping framework that enables users to replicate their desired seam styles while reducing distortion and seam length

### Texture inpainting

[Gaussian texture inpainting](https://github.com/Ozeuth/2D-Texture-Inpainter?tab=readme-ov-file)
> In this paper proposes a stochastic inpainting methods, dealing with the case of Gaussian textures. First a simple procedure is proposed for estimating a Gaussian texture model based on a masked exemplar, which, although quite naive, gives sufficient results for our inpainting purpose. Next, the conditional sampling step is solved with the traditional algorithm for Gaussian conditional simulation.

[Inpaint anything 2023](https://github.com/geekyutao/inpaint-anything?tab=readme-ov-file#remove-anything-3d)
> Users can select any object in an image by clicking on it. With powerful vision models, e.g., [SAM](https://arxiv.org/abs/2304.02643), [LaMa](https://arxiv.org/abs/2109.07161) and [Stable Diffusion (SD)](https://arxiv.org/abs/2112.10752), **Inpaint Anything** is able to remove the object smoothly (i.e., _Remove Anything_). Further, prompted by user input text, Inpaint Anything can fill the object with any desired content (i.e., _Fill Anything_) or replace the background of it arbitrarily (i.e., _Replace Anything_).

[MAT (Mask Aware Transformer) 2022](https://github.com/fenglinglwb/MAT)
> we present a novel transformer-based model for large hole inpainting, which unifies the merits of transformers and convolutions to efficiently process high-resolution images.

[CM-GAN 2022](https://github.com/htzheng/CM-GAN-Inpainting)
> We introduce a new cascaded modulation design that cascades global modulation with spatial adaptive modulation for better hole filling. We also introduce an object-aware training scheme to facilitate better object removal. CM-GAN significantly improves the existing state-of-the-art methods both qualitatively and quantitatively.

[Surface texture inpainting](https://github.com/johnpeterflynn/surface-texture-inpainting-net)
> We present the Surface Texture Inpainting Network (STINet), a graph neural network-based model that generates complete surface texture for partially textured 3D meshes. In contrast to 2D image inpainting which focuses on predicting missing pixel values on a fixed regular grid, STINet aims to inpaint color information on mesh surfaces of varying geometry and topology. STINet learns from spatial information such as vertex positions and normals as well as mesh connectivity to effectively predict vertex color.

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




