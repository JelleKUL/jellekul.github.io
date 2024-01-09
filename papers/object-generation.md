# Object and texture completion of partial 3D scanned objects

## Introduction

 **Structure**
 - Context: the need for partial shape completion
	 	- 3d scans of objects are often incomplete
		 	- because of occlusions, contact with other objects
	 	- the reconstruction is often split in 2 parts, the geometry and the texture
 - Current SOA solutions:
	 	- see background and related work
	 	- prompt based generation/ diffusion models
 - The main problem/ shortcoming
	 	- the texture inpainting is not good enough
	 	- the existing material inpainting techniques either:
	 	- completely fill in the mesh based on a single material prompt
 		- use a 2d image as a base for the texture field prediction
 		- inpaint a very small area on the mesh
 		- use a point based inpainting technique, which limits the resolution of the final result
 - Our proposed solution / the main contribution
 	- introduce an extra abstraction-layer which first predicts the material
 		- use the limited resolution 3d inpaintig techniques to predict the material type
 		- after the material is predicted, we use 2d inpainting technique to paint in the full texture

Completing 3D models of partial 3D scanned objects, both using existing geometry completion networks and finding new texture completion thingy. The main contribution will be using the existing partial texture data to complete the rest of the mesh.
- the geometry can be predicted using implicit SDF's this ensures the object is represented volumetrically and has a clear surface definition (in contrast to point clouds)
- the biggest difference is using 2 steps to predict the final colour
	- 1st predict the material
	- 2nd predict the colour per material based on the texture-map

## Background and Related Work

 **Structure**
 - A lot of research in geometry completion
 - A lot of research in object texturing
 - A lot of research on image inpainting
 - Not a lot on existing texture completion

### Geometry completion

- the generative models need a standardised input, this is why meshes are rarely used directly
	- converted to pointclouds or SDF's

#### Point based
Uses normalized point clouds as partial inputs to further complete the shape

- [Implicit Functions in Feature Space for 3D Shape Reconstruction and Completion](https://github.com/jchibane/if-net)
    > IF-NET Implicit feature based point prediction
- [Point-Voxel Diffusion](https://github.com/alexzhou907/PVD) 
	> 2022 (3D-diffuser)

#### Implicit
Uses sdf's to create and a selected zone for completion

- [AutoSDF](https://github.com/yccyenchicheng/AutoSDF) 
    > 2022 (VQ-VAE & transformer)
- [ShapeFormer: Transformer-based Shape Completion via Sparse Representation](https://github.com/qheldiv/shapeformer)
    > transformer based 

### Object texturing
Uses spatially encoded textures in 3d space to give each point a distinct colour, lower resolution but better spatially aware 
#### Implicit
- [Texture Fields: Learning Texture Representations in Function Space](https://doi.org/10.48550/arXiv.1905.07259)
    > Neural texture representation 

### Image inpainting

- [Stable Diffusion](https://github.com/Stability-AI/generative-models)

### Texture completion

#### Point based
- [Implicit Feature Networks for Texture Completion from Partial 3D Data](https://github.com/jchibane/if-net_texture)
    > IF-NET completing partial scans of humans, both geometry and texture (2020)

- [TSCom-Net: Coarse-to-Fine 3D Textured Shape Completion Network](https://doi.org/10.48550/arXiv.2208.08768)
    > Texture inpainting based on predicted colours

#### 2D based

- [Texture Inpainting for Photogrammetric Models](https://onlinelibrary.wiley.com/doi/10.1111/cgf.14735)
	> 2D inpainting of small selected areas

## Methodology

- make a schema for the full workflow
- steps:
	- convert incomplete mesh to unsigned distance field (not signed because open mesh)
	- detect the different materials in the texture and segment them (using SAM)
	- predict the completed shape using AutoSDF
	- Material prediction using if-Net
	- texture inpainting using (patch-based texture inpainting)

### Implicit surface generation
Convert the incomplete meshes to voxelgrids as input for the networks, this is done with 'mesh2sdf' 
> Note: The object is converted to a USDF (unsigned) because the input data is a partially scanned mesh
> 

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

- compare against ground truth
- check how the sdf changes the existing parts

### Texture reconstruction
- compare against single step completion networks
- compare against ground truth

## Discussion

## Conclusion