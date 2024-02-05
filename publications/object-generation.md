# Geometry and texture completion of partially scanned 3d objects

## Introduction

 **Structure**
 - Context: the need for partial shape completion
	 - indoor environments are captured using lidar or photogrammetry
	 - 3d scans of objects are often incomplete
		 - because of occlusions and contact with other objects
		 - this becomes a problem when the objects are isolated from the environment
	 - When we isolate an object, we need to complete it's occluded parts
	 - our goal is completing those partial objects both for the geometry as the texture
 - Current SOA solutions:
	 - the reconstruction is often split in 2 parts, the geometry and the texture
	 - Geometry
		 - point based completion
		 - SDF based completion
	 - Texture
		 - point/voxel based
		 - 2d image based
	 - prompt based generation/ diffusion models
		 - Using partial 3d data as 2d input is difficult
 - The main problem/ shortcoming
	 - the texture inpainting is not good enough
	 - the existing material inpainting techniques either:
		 - completely fill in the mesh based on a single material prompt
		 - use a 2d image as a base for the texture field prediction
		 - inpaint a very small area on the mesh
		 - use a point based inpainting technique, which limits the resolution of the final result
 - Our proposed solution / the main contribution
	 - Use the existing partial texture as input for the completion
	 - introduce an extra abstraction-layer which first predicts the material
		 - the geometry can be predicted using implicit SDF's this ensures the object is represented volumetrically and has a clear surface definition (in contrast to point clouds)
		 - use the limited resolution 3d inpainting techniques to predict the material type
		 - after the material is predicted, we use 2d inpainting technique to paint in the full texture

## Background and Related Work

 **Structure**
 - Object generation
 - Geometry completion
 - Object texturing
 - Texture inpainting
 - Texture completion?

### Object generation

[TM-NET: Deep Generative Networks for Textured Meshes](http://arxiv.org/abs/2010.06217)
> 3d Mesh and texture generation

[Zero-1-to-3: Zero-shot One Image to 3D Object](http://arxiv.org/abs/2303.11328)
> Nerf creation from single rgb image

### Geometry completion
the generative models need a standardised input, this is why meshes are rarely used directly
	- converted to pointclouds or SDF's

#### Point based
Uses normalized point clouds as partial inputs to further complete the shape

[Implicit Functions in Feature Space for 3D Shape Reconstruction and Completion](https://github.com/jchibane/if-net)
> IF-NET Implicit feature based point prediction

[3D Shape Generation and Completion through Point-Voxel Diffusionn](https://github.com/alexzhou907/PVD) 
> 2022 (3D-diffuser)

#### Implicit
Uses sdf's to create and a selected zone for completion

[AutoSDF](https://github.com/yccyenchicheng/AutoSDF) 
> 2022 (VQ-VAE & transformer)

[ShapeFormer: Transformer-based Shape Completion via Sparse Representation](https://github.com/qheldiv/shapeformer)
> transformer based 

### Object texturing
Uses spatially encoded textures in 3d space to give each point a distinct colour, lower resolution but better spatially aware 

#### Implicit
[Texture Fields: Learning Texture Representations in Function Space](https://doi.org/10.48550/arXiv.1905.07259)
> Neural texture representation 

### Image inpainting

[Stable Diffusion](https://github.com/Stability-AI/generative-models)
> generative texture inpainting

### Texture inpainting

[Texture Inpainting for Photogrammetric Models](https://onlinelibrary.wiley.com/doi/10.1111/cgf.14735)
> 2D inpainting of small selected areas

[Texture Inpainting Using Efficient Gaussian Conditional Simulation](https://doi.org/10.1137/16M1109047)
> gaussian texture inpainting

### Texture generation

#### Implicit

[Texture Fields: Learning Texture Representations in Function Space](http://arxiv.org/abs/1905.07259)
> creating a shape independent representation for the texture 

#### UV based

[TUVF: LEARNING GENERALIZABLE TEXTURE UV RADIANCE FIELDS](https://doi.org/10.48550/arXiv.2305.03040)
> Generalizing the uv map to generate a full texture

### Texture completion

#### Point based
- [Implicit Feature Networks for Texture Completion from Partial 3D Data](https://github.com/jchibane/if-net_texture)
    > IF-NET completing partial scans of humans, both geometry and texture (2020)

- [TSCom-Net: Coarse-to-Fine 3D Textured Shape Completion Network](https://doi.org/10.48550/arXiv.2208.08768)
    > Texture inpainting based on predicted colours



## Methodology

**Structure**
- convert incomplete mesh to unsigned distance field (not signed because open mesh)
	- MESH2SDF
- detect the different materials in the texture and segment them
	- SAM
- predict the completed shape
	- AUTOSDF
- Material prediction
	- if-Net Texture
- texture inpainting
	- patch-based texture inpainting / Stable diffusion

Create a big schema showing all the steps

### Implicit surface generation
Convert the incomplete meshes to voxelgrids containing distaces as input for the networks, this is done with 'mesh2sdf' 
> Note: The object is converted to a USDF (unsigned) because the input data is a partially scanned mesh
> SDF's are more precise than voxelgrids, leading to better results

### Material segmentation
Use material detection to segment the uv-map of the object into distinct materials, for this we use Segment anything model, while the embedding is pretty slow, it returns a reliable segmentationmask.
- The segmentation creates a different patch for each uv island.
- in a next step we need to find patches of the same material. this is done with efficientnet image features

#### Patch grouping

The different patches of material can be grouped by comparing their similarity. 
- objects are grouped based on a similarity threshold value and grouped in an arbitrary amount of groups. 

### Geometry prediction
Geometry can be predicted using autosdf's VAE network, by highlighting selected voxels, the network can predict the shape in those voxels
- The network needs a reliable sdf, not just an occupancy grid
- the network needs a selection of voxels to ignore these can be determined based on the holes in the incomplete input mesh
- The objects completion works best if the object is positioned straight and 
- 

#### Occlusion detection
to decide which voxels we need to fill in, we predict the grounding plane, symmetry axes, and the center of mass, using this we can accurately predict the geometry

#### Surface meshing
The generated sdf is then converted back to a mesh. this is done using the marching cubes algorithm

### Texture inpainting

#### UV unwrapping

The newly created meshes need to be unwrapped in order to be inpainted, for the inpainting to work we need big patches of single material data. for this we look at some existing methods that preserve aspects as much as possible
- take the directionality into account of the uv layout
- take semantics into account?

#### Inpainting

The inpainting is performed on the uv map, where each material is painted in separately.
The existing part of the texture is used as a reference for the inpainting for the other parts

## Experiments

- the experiments should focus on comparisons against the ground truth
- compare the texture field inpainting method against 

### Datasets

- [ShapeNet](https://shapenet.org/)
- [PartNet](https://partnet.cs.stanford.edu/)

#### Data processing

- we used the shapenet dataset for both the geometry & material completion
- geometry completion, used pretrained models from autosdf
- texture: retrained the model to learn how to predict materials.
	- the meshes are first sampled by their material, then holes are cut out


### Geometry reconstruction
This part is not really anything new, apart from validating the conversion between the incomplete meshes to SDF's and how they change compared to the ground truth.

- compare against ground truth
- check how the sdf changes the existing parts 

### Texture reconstruction

- compare against single step completion networks
	- if-net_texture trained on the shape-net dataset with direct colour prediction
- compare against ground truth

## Discussion
- Are texturefields too much linked to a similar structure eg: cars, humans,... to be used for something as wide as generic indoor objects?
- the extra step allows for a more precise inpainting process with clear material separation
- it does not improve non uniform graphic elements 
- Increases processing-time
## Conclusion