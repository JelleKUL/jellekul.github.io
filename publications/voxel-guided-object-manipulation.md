# Voxel Guided Object Manipulation

IGARSS 2025
## Introduction

- Dynamic objects in indoor scenes are needed for industry
	- Indoor renovations in AECO
	- Simulations (light, smoke,...)
	- Gaming environments
- Objects captured in environments are static and cannot be scaled without unwanted distortions
- There is a need for selective and proportion-retaining scaling by only scaling certain parts of the object
-  Current part aware object scaling is used in the games industry for procedural large scale object generation on meshes, but require a database of existing modular parts.
- A lot of the object detection and completion pipelines use SDF's as outputs
	- These are implicit definitions and define an object boundary
	- The texture can be made implicit (texture fields) as well creating a direct link between the distance to the surface and the colour at the surface at any point in space.
- Our method
	- Divide the completed object from the scene into parts
	- interpolates the sdf and colour values between scaled boundaries
	- Introduce a repeating parts feature for large deformations of small parts

## Background

### Datasets
[Partnet]([GitHub - daerduoCarey/partnet_dataset: PartNet Dataset Official Release Repo](https://github.com/daerduoCarey/partnet_dataset))
> a consistent, large-scale dataset of 3D objects annotated with fine-grained, instance-level, and hierarchical 3D part information.

### Scale aware Object scaling
[9-Slicing for Images](https://www.w3.org/TR/css-backgrounds-3/#border-image-slice)
> Define specific zones that can be scaled and need to remain proportional

### Mesh Deformation
[KeypointDeformer](https://tomasjakab.github.io/KeypointDeformer/)
> automatic keypoint detection and mesh cage deformation

[27-slicing](https://slicer.deftly.games/)
> Same principle as 9 slicing but in 3D

### Procedural Deformation
[Proc-GS: Procedural Building Generation for City Assembly with 3D Gaussians](https://city-super.github.io/procgs/)
> Dividing gaussian splats of buildings into components and procedurally combining them in new procedural buildings

[Automatic procedural model generation for 3D object variation](https://link.springer.com/article/10.1007/s00371-018-1589-4)
> Automatically detecting 
### SDF Deformation
[Deformed Implicit Field: Modeling 3D Shapes with Learned Dense Correspondence](https://github.com/microsoft/DIF-Net)
> a 3D shape is represented by a template implicit field shared across the category, together with a 3D deformation field and a correction field dedicated for each shape instance. Shape correspondences can be easily established using their deformation fields. Our neural network, dubbed DIFNet, jointly learns a shape latent space and these fields for 3D objects belonging to a category without using any correspondence or part label

[SALAD: Part-Level Latent Diffusion for 3D Shape Generation and Manipulation](https://salad3d.github.io/)
> a cascaded diffusion model based on a part-level implicit 3D representation. Our model achieves state-of-the-art generation quality and also enables part-level shape editing and manipulation without any additional training in conditional setup.

[DualSDF: Semantic Shape Manipulation using a Two-Level Representation](https://www.cs.cornell.edu/projects/dualsdf/)
> Link primitive based low res SDF to high res to edit it.

[HybridSDF](https://ntalabot.github.io/hybridsdf/)
> compining Impicit shapes with primitives

### SDF Editing
[XCube 2024](https://research.nvidia.com/labs/toronto-ai/xcube/)
> - Nvidia Labs
> - Large-Scale 3D Generative Modeling using Sparse Voxel Hierarchies.
> - a Hierarchical voxel latent diffusion model build on VDB()
> - arbitrary attributes
> Our method trains a hierarchy of latent diffusion models over a hierarchy of sparse voxel grids ={G1,...,GL}. Our model first trains a **sparse structure VAE** which learns a compact latent representation for each level of the hierarchy. We then train a latent diffusion model to generate each level of the hierarchy conditioned on the coarser level above it. At inference time, we simply run each diffusion model in a cascaded fashion from coarse to fine to generate novel shapes and scenes. By leveraging the decoder of the sparse structure VAE at inference time, our generated high-resolution voxel grids additionally contain various attributes, such as normals and semantics, which can be used in down-stream applications.

[SPAGHETTI: Editing Implicit Shapes through Part Aware Generation](https://amirhertz.github.io/spaghetti)
> manipulation of implicit shapes by means of transforming, interpolating and combining shape segments together, without requiring explicit part supervision. SPAGHETTI disentangles shape part representation into extrinsic and intrinsic geometric information. This characteristic enables a generative framework with part-level control.
- Combines parts of different object to create new objects
- Part-wise affine transformation, like moving and rotating, the rest of the object conforms to the new conditions
- Does not allow free scaling at a sub-part level

[SENS: Part-Aware Sketch-based Implicit Neural Shape Modeling](https://github.com/AlexandreBinninger/SENS)
> continuation of spaghetti
> edit SDFs based on hand drawn sketches


## Methodology
- Complete the object from a scene
- Segment the object into its distinct parts
- selecting the parts that need to be moved
- Interpolating the SDF, color and part index of the parts in the selection zone
### Object Completion
We use the state-of the art method (AUTO SDF) to complete the different partially scanned objects in the scene.
The objects are completed as SDF's and the textures are completed on the sampled version of the SDF using "IF-Net-Texture"
- The SDF is discretised into a $128x128x128$ voxel grid
- The colours of the object are also discretized into a voxel array matching the SDF dimensions

### Part Detection
- We use a state-of-the-art method to detect the parts of the object (Cross shape attention (CSN))
- The model performs a pointwise segmentation on the sampled SDF, where each point is a voxel-grid coordinate. The resulting labels are a semantic label indicating the part of the model.
- The each seperate instance of the semantic labels is given a unique index by grouping all the points using a DBSCAN clustering algorithm.
- The whole object is now defined by 3 values at each voxel coordinate:
	- Distance to the surface
	- Color at the nearest surface
	- Part index at the nearest surface

### Scaling zone definition
- Next is defining which parts of the object should be scaled/moved
- The selection is made by defining 2 parallel planes to 
- A planar selection is made of the desired scaling zone
- This is done by creating 2 boundary planes for a given axis, the starting plane, and the ending plane. All the objects before the starting plane will remain in place, the objects in between the 2 planes are scaled proportionally. the objects behind the end plane are moved to the desired end position

### SDF, Colour and Part Index Interpolation
- The last plane is moved to a new location
- The parts beyond the last plane are moved to the end location
- The parts in between the 2 planes are isolated as a selection.
- The parts before the first plane remain in place
- The Selection is isolated and a new empty array is created that matches the new size of the new distance between the 2 planes.
- There are 2 scaling modes
	- Scale: The SDF, color and index values in the selection are linearly interpolated along the move axis to match the new size
	- Repeat: The selection is repeated an $n$ amount of times depending on the length of the scaling factor. The repeating section is also scaled up/down to ensure the $n$ number of parts fit precisely in the new length. That scaling is also performed as a linear interpolation.

## Experiments
- We tested our workflow on objects from the matterport dataset.
- The objects are isolated and completed, resulting 
- We compared our smart scaling to regular object scaling
	- We see clear distortion in parts that should not be scaled 

- show results of texture and SDF repeating parts

## Conclusion
- our method allows for object dynamification with context aware scaling. 
- Strengths
	- The one to one connection between the texturegrid and SDF enables the dynamic scaling of object while retaining visual continuity
	- The SDF encoding allows for a smoother interpolation of the stretched parts without losing geometric detail
	- The patterned texture scaling ensures the patterns do not get distorted beyond an acceptable level
- Weaknesses
	- Only scaling along the main grid axis is supported, however, the 3 axis can be chosen on an object-by-object basis to find the orientation for the desired transformation.
	- Struggles with angled shapes because the stretching does not retain the angle of incidence.
	- For very large deformations, the textures can become repetitive if the original selection size is not very big