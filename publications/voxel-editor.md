# Guided object completion with interactive voxel editing

## Introduction

- Current object completion networks 
- There are no tools to easily complete existing scans
- This work is about improving the object completion using environment cues and user voxel editing
- Environment ques
	- touching planes (walls, floors, ceilings)
	- 

## Background and related work

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
### Environment Aided Completion

[Where Does It End? - Reasoning About Hidden Surfaces by Object Intersection Constraints](https://cosection.is.tue.mpg.de/)
> 2020
> An optimization-based approach to 3D dynamic scene reconstruction, which infers hidden shape information from intersection constraints.
> Uses the scene to limit object completion

[Approximate Symmetry Detection in Partial 3D Meshes](https://github.com/victorvianna/symmetry-detection)
> 2014
> Uses local extrema to find corresponding symmetry points and aligns the partial mesh