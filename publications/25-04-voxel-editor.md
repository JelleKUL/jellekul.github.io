---
layout: publication
title: Guided object completion with interactive voxel editing
date: 2025-04-12
permalink: /publications/voxel-editor/
icon: "img/icons/Voxel Generation.png"
methodImage: "img/publications/voxel-editor-method.png"
doilink: 
githublink: "https://github.com/JelleKUL/geosharpi"
objective: 3
type: Conference

description: "In this work, we aim to improve the completion of objects from partially scanned indoor scenes by leveraging environmental cues to better inform the boundaries of incomplete objects. Furthermore, we introduce an interactive voxel editor that allows users to guide the object completion process toward more accurate results. Our contributions are twofold: (1) a novel boundary-defining and object-alignment method that integrates with existing object completion pipelines, and (2) the development of an interactive voxel editing tool that enhances user control over the completion process. Experimental results demonstrate the effectiveness of our approach in improving object completion in complex, real-world scanned scenes."


---

# Guided object completion with interactive voxel editing

## Introduction

- Context: the need for partial shape completion
	 - indoor environments are captured using lidar or photogrammetry
	 - 3d scans of objects are often incomplete
		 - because of occlusions and contact with other objects
		 - this becomes a problem when the objects are isolated from the environment
	 - When we isolate an object, we need to complete it's occluded parts
- Current object completion networks
	- only perform well on 
- There are no tools to easily complete objects from existing scans
- This work is about improving the object completion using environment cues and user voxel editing
- Environment queues
	- touching planes (walls, floors, ceilings)

## Background and related work

### Object detection

[V-DETR 2023](https://github.com/V-DETR/V-DETR)
> Uses DETR (detection transformer) in 3D with Vertex Relative Position Encoding to improve locality

[VoteNet 2019](https://github.com/facebookresearch/votenet)
> an end-to-end 3D object detection network based on a synergy of deep point set networks and Hough voting.
### Object completion

#### Point based
Uses normalized point clouds as partial inputs to further complete the shape

[Implicit Functions in Feature Space for 3D Shape Reconstruction and Completion](https://github.com/jchibane/if-net)
> IF-NET Implicit feature based point prediction

[3D Shape Generation and Completion through Point-Voxel Diffusionn](https://github.com/alexzhou907/PVD) 
> 2022 (3D-diffuser)

#### Implicit
Uses sdf's to create and a selected zone for completion

[AutoSDF](https://github.com/yccyenchicheng/AutoSDF) & [SDFusion](https://yccyenchicheng.github.io/SDFusion/)
> 2022, 2023
> Uses a 8x8x8 voxel grid as sub-trained elements to predict large missing portions
> (VQ-VAE & transformer)
> Use multiple modalities for object completion

[PatchComplete](https://github.com/yuchenrao/PatchComplete)& [WSSC](https://github.com/ltwu6/WSSC)
> 2022, 2024
> Learning multi resolution priors to complete unseen categories

[ShapeFormer: Transformer-based Shape Completion via Sparse Representation](https://github.com/qheldiv/shapeformer)
> transformer based 

[XCube 2024](https://research.nvidia.com/labs/toronto-ai/xcube/)
> - Nvidia Labs
> - Large-Scale 3D Generative Modeling using Sparse Voxel Hierarchies.
> - a Hierarchical voxel latent diffusion model build on VDB()
> - arbitrary attributes
> Our method trains a hierarchy of latent diffusion models over a hierarchy of sparse voxel grids ={G1,...,GL}. Our model first trains a **sparse structure VAE** which learns a compact latent representation for each level of the hierarchy. We then train a latent diffusion model to generate each level of the hierarchy conditioned on the coarser level above it. At inference time, we simply run each diffusion model in a cascaded fashion from coarse to fine to generate novel shapes and scenes. By leveraging the decoder of the sparse structure VAE at inference time, our generated high-resolution voxel grids additionally contain various attributes, such as normals and semantics, which can be used in down-stream applications.
### Environment Aided Completion

[Co-Section - Where Does It End? - Reasoning About Hidden Surfaces by Object Intersection Constraints](https://cosection.is.tue.mpg.de/)
> 2020
> An optimization-based approach to 3D dynamic scene reconstruction, which infers hidden shape information from intersection constraints.
> Uses the scene to limit object completion

[EM-Fusion: Dynamic Object-Level SLAM With Probabilistic Data Association](https://emfusion.is.tue.mpg.de/)
> 2019
> a novel approach to dynamic SLAM with dense object-level representations. We represent rigid objects in local volumetric signed distance function (SDF) maps, and formulate multi-object tracking as direct alignment of RGB-D images with the SDF representations

[A Variational Taxonomy for Surface Reconstruction from Oriented Points]
> 2014
> a general higher order framework for surface reconstruction. It is based on the idea that position and normal defined by each oriented point can be used to construct an implicit local description of the unknown surface. On the one hand, this allows us to systematically explain and relate several popular methods, for example implicit moving least squares, smooth signed distance surface reconstruction as well as (screened) Poisson surface reconstruction. On the other hand, it allows to derive and discuss a number of new approaches for reconstructing either the signed distance or the indicator function of the sought object.

[Tracking Partially-Occluded Deformable Objects while Enforcing Geometric Constraints]
> 2021
> A better way to handle severe occlusion by using a motion model to regularize the tracking estimate. 
> The formulation of convex geometric constraints, which allow us to prevent self-intersection and penetration into known obstacles via a post-processing step.
### Symmetry Detection

[Approximate Symmetry Detection in Partial 3D Meshes](https://github.com/victorvianna/symmetry-detection)
> 2014
> Partial 3D models
> Uses local extrema to find corresponding symmetry points and aligns the partial mesh

[Partial and Approximate Symmetry Detection for 3D Geometry]
> 2006
> a new algorithm that processes geometric models and efficiently discovers and extracts a compact representation of their Euclidean symmetries.

[SymmetryNet](https://github.com/GodZarathustra/SymmetryNet)
> 2020
>  Single-view RGB
>  given the 3D points present in the RGB-D image, our network outputs for each 3D point its symmetric counterpart corresponding to a specific predicted symmetry.

[PRS-Net: Planar Reflective Symmetry Detection Net for 3D Models]
> 2021
> Complete 3D-models
> a novel learning framework to automatically discover global planar reflective symmetry of a 3D shape. Our framework trains an unsupervised 3D convolutional neural network to extract global model features and then outputs possible global symmetry parameters, where input shapes are represented using voxels.
## Methodology

### Scene Detection

- We preform object detection on a whole scene.
- We perform plane detection
- This results in a list of objects
	- We use the ground plane to define the object bottom boundary
	- We use the walls and other adjacent objects (if any) for sideways boundary
### Object Alignment

- We use a model to predict the principal symmetry axis
	- This is so the objects better fit the voxel grid
	- improves the completion because it better matches the training data
### Voxel guided Object completion

- Use the AutoSDF object completion with the selected voxels
	- The voxels provide a better input mechanism

## Experiments

#### Ablation study
compare the results of the objects when:
- no plane is detected
- the object is not aligned
- no detailed voxel editing

## Conclusion
- the alignment is essential for a proper completed object
- 