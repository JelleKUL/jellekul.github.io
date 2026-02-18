# Partial 3D Scan Dynamification

## Introduction

### Problem Statement
- Scenes are captured partially and statically
	- 3d pointcloud + pano's/ images -> Sam & geomapi paper
- There is a need for complete and dynamic environments:
	- Gaming environments
	- Renovation planning
	- Safety planning
	- Simulation design
- Current works:
	- complete the scene as a whole, not the space between the objects
	- complete only the objects
	- remove the objects entirely
- Very limited works that create a dynamic scene from a partial scan

### Proposed Workflow

- Input: Partially scanned scene + Images
- 3D Object detection
- Parallel Object and scene completion
	- Object completion using VAE models with point and image guiding
	- Scene completion using geometry based polygon reconstruction
- Object Dyamification using Gaussian splat 27-Slicing / part detection
- Scene reconstruction


## Background and Related Work

**Structure**
- Object detection/ semantic segmentation
- Object completion
- Scene completion
- Full scene completion
- Object Dynamification
### 3D Object detection
[VoteNet 2019](https://github.com/facebookresearch/votenet)
> an end-to-end 3D object detection network based on a synergy of deep point set networks and Hough voting.
> Small improvements: Votenet + & MLCVNet 

[V-DETR 2023](https://github.com/V-DETR/V-DETR)
> Uses DETR (detection transformer) in 3D with Vertex Relative Position Encoding to improve locality

[UniDet3D: Multi-dataset Indoor 3D Object Detection](https://github.com/filaPro/unidet3d)
> a simple yet effective 3D object detection model, which is trained on a mixture of indoor datasets and is capable of working in various indoor environments. By unifying different label spaces, UniDet3D enables learning a strong representation across multiple datasets through a supervised joint training scheme. The proposed network architecture is built upon a vanilla transformer encoder, making it easy to run, customize and extend the prediction pipeline for practical use.
> Extracts point features using a sparse 3D U-Net network. Point features are averaged across superpoints in the superpoint pooling. Aggregated features serve as input queries to a vanilla transformer encoder. Finally, 3D bounding boxes are derived from encoder outputs with a box MLP and class MLP, where box MLP estimates the location of a 3D bounding box w.r.t. the mass center of the superpoint, and class MLP outputs probabilities of object classes in the unified label space.

[AIC3DOD: Advancing Indoor Class-Incremental 3D Object Detection with Point Transformer Architecture and Room Layout Constraints](https://openaccess.thecvf.com/content/WACV2025/papers/Cheng_AIC3DOD_Advancing_Indoor_Class-Incremental_3D_Object_Detection_with_Point_Transformer_WACV_2025_paper.pdf)
> Advancing Indoor Class incremental 3D Object Detection using the point transformer architecture with room layout constraints. Our approach employs a transformer architecture in our detection model and optimizes the class incremental step in the transformer architecture. Besides, AIC3DOD incorporates additional prior information, namely room layout, to impose physical constraints on detected objects, thereby enhancing overall object detection performance.
### 3D Semantic segmentation
[PTv3](https://github.com/Pointcept/Pointcept)
> Point transformer v3

[VOXEL-BASED INDOOR RECONSTRUCTION FROM HOLOLENS TRIANGLE MESHES](https://arxiv.org/pdf/2002.07689)
> we present a novel voxel-based approach for automated indoor reconstruction from unstructured three-dimensional geometries like triangle meshes.

#### Scene Instance segmentation
[SphericalMask 2024](https://github.com/yunshin/SphericalMask?tab=readme-ov-file)
>  we introduce Spherical Mask, a novel coarse-to-fine approach based on spherical representation, overcoming those two limitations with several benefits. Specifically, our coarse detection estimates each instance with a 3D polygon using a center and radial distance predictions, which avoids excessive size estimation of AABB.

[UnScene3D](https://rozdavid.github.io/unscene3d)
>  the first fully unsupervised 3D learning approach for class-agnostic 3D instance segmentation of indoor scans. UnScene3D first generates pseudo masks by leveraging self-supervised color and geometry features to find potential object regions.
#### Plane detection
[Latent RANSAC 2018](https://github.com/rlit/LatentRANSAC)
> a method that can evaluate a RANSAC hypothesis in constant time.
### Object Completion

#### Sparse voxel hierarchies
[XCube 2024](https://research.nvidia.com/labs/toronto-ai/xcube/)
> - Nvidia Labs
> - Large-Scale 3D Generative Modeling using Sparse Voxel Hierarchies.
> - a Hierarchical voxel latent diffusion model build on VDB()
> - arbitrary attributes
> Our method trains a hierarchy of latent diffusion models over a hierarchy of sparse voxel grids ={G1,...,GL}. Our model first trains a **sparse structure VAE** which learns a compact latent representation for each level of the hierarchy. We then train a latent diffusion model to generate each level of the hierarchy conditioned on the coarser level above it. At inference time, we simply run each diffusion model in a cascaded fashion from coarse to fine to generate novel shapes and scenes. By leveraging the decoder of the sparse structure VAE at inference time, our generated high-resolution voxel grids additionally contain various attributes, such as normals and semantics, which can be used in down-stream applications.

[CSVO: Clustered Sparse Voxel Octrees](https://doi.org/10.3390/sym14102114)
> A Hierarchical Data Structure for Geometry Representation of Voxelized 3D Scenes

[High resolution sparse voxel DAGs](https://doi.org/10.1145/2461912.2462024)
> generalising the tree to a directed acyclic graph (DAG). While the SVO allows for efficient encoding of empty regions of space, the DAG additionally allows for efficient encoding of _identical_ regions of space, as nodes are allowed to share pointers to identical subtrees. (only one bit)

[Geometry and attribute compression for voxel scenes](https://doi.org/10.1111/cgf.12841)
> a method to compress _arbitrary_ data, such as colors, normals, or reflectance information. By decoupling geometry and voxel data via a novel mapping scheme, we are able to apply the DAG principle to encode the topology, while using a palette-based compression for the voxel attributes, leading to a drastic memory reduction.

#### CAD Based
[Hybrid 3D Reconstruction of Indoor Scenes Integrating Object Recognition](https://www.researchgate.net/publication/378117522_Hybrid_3D_Reconstruction_of_Indoor_Scenes_Integrating_Object_Recognition)
> a hybrid indoor reconstruction method that segments the room point cloud into internal and external components, and then reconstructs the room shape and the indoor objects in different ways. We segment the room point cloud into internal and external points based on the assumption that the room shapes are composed of some large external planar structures. For the external, we seek for an appropriate combination of intersecting faces to obtain a lightweight polygonal surface model. For the internal, we define a set of features extracted from the internal points and train a classification model based on random forests to recognize and separate indoor objects. Then, the corresponding computer aided design (CAD) models are placed in the target positions of the indoor objects, converting the reconstruction into a model fitting problem. Finally, the indoor objects and room shapes are combined to generate a complete 3D indoor model.
> Uses plane fitting on the environment and Shape matching on the objects, the walls and floors are detected using RANSAC plane detection
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

#### Image based
[SAM3D](https://ai.meta.com/sam3d/)
> - Meta Research
> - needs 32GB VRAM
> - Recreating 3D textured objects from a single image
> - Uses a mask to isolate an object from a scene
> - Optionally allows a pointset input or better object scaling and alignment
> Workings:
> - Input encoding
> 	- DINOV2 on the cropped image + full image for context
> 	- *Optional* Coarse pointmap (pointcloud)
> 	- *shape and layout tokens* learned embeddings initialized at the start of the forward pass (placeholders to transform into final pose and shape)
> - Stage 1: Coarse Geometry and Pose
> 	- Architecture:
> 		- 1.2B Dual latent flow transformer (2 transformers have a shared self-attention layer) https://github.com/facebookresearch/Mixture-of-Transformers
> 		- encodes the features in latent space
> 		- learns generate meaningful data from noise by calculating a continuous flow velocity field (instead of diffusion based linear denoising)
> 	- output:
> 		- creates voxel occupancy grid
> 		- pose in 3D scene
> - Stage 2: Texture and Refinement
> 	- Architecture
> 		- 600M sparse latent flow transformer https://microsoft.github.io/TRELLIS/
> 		- refines fine scale geometry
> 		- generates the texture
> 	- output:
> 		- dense latent 3D representation
> - Stage 3: Decoding
> 	- Mesh Decoder
> 	- Gaussian splat decoder

[Hunyuan3D](https://github.com/Tencent-Hunyuan/Hunyuan3D-Omni)
> - Tensent
> - Needs 10GB VRAM
> - Recreating 3D models from a single image + optional 3D data (pcd, bb, voxels)
> a unified framework for fine-grained, controllable 3D asset generation built on Hunyuan3D 2.1. In addition to images, Hunyuan3D-Omni accepts point clouds, voxels, bounding boxes, and skeletal pose priors as conditioning signals, enabling precise control over geometry, topology, and pose. Instead of separate heads for each modality, our model unifies all signals in a single cross-modal architecture. We train with a progressive, difficulty-aware sampling strategy that selects one control modality per example and biases sampling toward harder signals ( e.g., skeletal pose) while downweighting easier ones ( e.g., point clouds), encouraging robust multi-modal fusion and graceful handling of missing inputs.

[Trellis](https://github.com/microsoft/TRELLIS)
> - Microsoft
> - Needs 16GB VRAM
> - Generates a 3D model from a single Image
> - outputs to: Radiance Fields, 3D Gaussians, and meshes
> a unified Structured LATent (SLAT) representation that allows decoding to different output formats and Rectified Flow Transformers tailored for SLAT as the powerful backbones.

#### Multi object RGB-(D) reconstruction
[SAM3D](https://ai.meta.com/sam3d/)
> Detects all objects in the scene using SAM, uses, the masked object and full image + optional point to reconstruct the object and create a transform to place it in the scene

[CAST: Component-Aligned 3D Scene Reconstruction from an RGB Image](https://github.com/FishWoWater/CAST?tab=readme-ov-file)
> Recovering high-quality 3D scenes from a single RGB image is a challenging task in computer graphics. Current methods often struggle with domain-specific limitations or low-quality object generation. To address these, we propose CAST (Component-Aligned 3D Scene Reconstruction from a Single RGB Image), a novel method for 3D scene reconstruction. CAST starts by extracting object-level 2D segmentation and relative depth information from the input image, followed by using a GPT-based model to analyze inter-object spatial relations. This enables understanding of how objects relate to each other within the scene, ensuring more coherent reconstruction. CAST then employs an occlusion-aware large-scale 3D generation model to independently generate each object's full geometry, using Masked Auto Encoder (MAE) and point cloud conditioning to mitigate the effects of occlusions and partial object information, ensuring accurate alignment with the source image's geometry and texture. To align each object with the scene, the alignment generation model computes the necessary transformations, allowing the generated meshes to be accurately placed and integrated into the scene's point cloud. Finally, CAST applies a physics-aware correction mechanism, which leverages a fine-grained relation graph to generate a constraint graph. This graph guides the optimization of object poses, ensuring physical consistency and spatial coherence. By utilizing Signed Distance Fields (SDF), the model effectively addresses issues such as occlusions, object penetration, and floating objects, ensuring that the generated scene accurately reflects real-world physical interactions.
> This model only Reconstructs the objects in the scene, not the scene itself, also uses physically plausible repositioning

### Full Scene Occlusion Infilling
[Clutter detection and removal](https://weify627.github.io/clutter/)
> Clutter detection network + camera image based depth & image inpainting

[Behind the Veil: Enhanced Indoor 3D Scene Reconstruction with Occluded Surfaces Completion](https://ieeexplore.ieee.org/document/10656875)
> Our method tackles the task of completing the occluded scene surfaces, resulting in a complete 3D scene mesh. The core idea of our method is learning 3D geometry prior from various complete scenes to infer the occluded geometry of an unseen scene from solely depth measurements. We design a coarse-fine hierarchical octree representation coupled with a dual-decoder architecture, i.e., Geo-decoder and 3D Inpainter, which jointly reconstructs the complete 3D scene geometry. The Geo-decoder with detailed representation at fine levels is optimized online for each scene to reconstruct visible surfaces. The 3D Inpainter with abstract representation at coarse levels is trained offline using various scenes to complete occluded surfaces. As a result, while the Geo-decoder is specialized for an individual scene, the 3D Inpainter can be generally applied across different scenes.
> Specifically Completes the occlusions in between objects and the environment.
> No texture or object detection
### Full scene Reconstruction
[3D Semantic Scene Completion: A Survey](https://doi.org/10.1007/s11263-021-01504-5)
> Semantic scene completion (SSC) aims to jointly estimate the complete geometry and semantics of a scene, assuming partial sparse input. In the last years following the multiplication of large-scale 3D datasets, SSC has gained significant momentum in the research community because it holds unresolved challenges.

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

[RfD-Net: Point Scene Understanding by Semantic Instance Reconstruction](https://github.com/GAP-LAB-CUHK-SZ/RfDNet) 
> 2021

[Point Scene Understanding via Disentangled Instance Mesh Reconstruction](https://github.com/ashawkey/dimr) 
> 2022

[SG-NN: Sparse Generative Neural Networks for Self-Supervised Scene Completion of RGB-D Scans](https://github.com/angeladai/sgnn)
> 2020

### Full scene Dynamification
[Gen3DSR: Generalizable 3D Scene Reconstruction via Divide and Conquer from a Single View](https://andreeadogaru.github.io/Gen3DSR/)
> a hybrid method following a divide-and-conquer strategy. We first process the scene holistically, extracting depth and semantic information, and then leverage an object-level method for the detailed reconstruction of individual components. By splitting the problem into simpler tasks, our system is able to generalize to various types of scenes without retraining or fine-tuning.
> Similar workflow, but all in 2D, using depth estimation to get the 3D scene. Also splits the foreground and background. Uses off-the-shelf models and combines them, like I do. Uses depth inpainting for environment reconstruction.
### Object dynamification
#### Scale aware Object scaling
[9-Slicing for Images](https://www.w3.org/TR/css-backgrounds-3/#border-image-slice)
> Define specific zones that can be scaled and need to remain proportional

#### Mesh Deformation
[KeypointDeformer](https://tomasjakab.github.io/KeypointDeformer/)
> automatic keypoint detection and mesh cage deformation

[27-slicing](https://slicer.deftly.games/)
> Same principle as 9 slicing but in 3D

#### Procedural Deformation
[Proc-GS: Procedural Building Generation for City Assembly with 3D Gaussians](https://city-super.github.io/procgs/)
> Dividing gaussian splats of buildings into components and procedurally combining them in new procedural buildings

[Automatic procedural model generation for 3D object variation](https://link.springer.com/article/10.1007/s00371-018-1589-4)
> Automatically detecting 
#### SDF Deformation
[Deformed Implicit Field: Modeling 3D Shapes with Learned Dense Correspondence](https://github.com/microsoft/DIF-Net)
> a 3D shape is represented by a template implicit field shared across the category, together with a 3D deformation field and a correction field dedicated for each shape instance. Shape correspondences can be easily established using their deformation fields. Our neural network, dubbed DIFNet, jointly learns a shape latent space and these fields for 3D objects belonging to a category without using any correspondence or part label

[SALAD: Part-Level Latent Diffusion for 3D Shape Generation and Manipulation](https://salad3d.github.io/)
> a cascaded diffusion model based on a part-level implicit 3D representation. Our model achieves state-of-the-art generation quality and also enables part-level shape editing and manipulation without any additional training in conditional setup.

[DualSDF: Semantic Shape Manipulation using a Two-Level Representation](https://www.cs.cornell.edu/projects/dualsdf/)
> Link primitive based low res SDF to high res to edit it.

[HybridSDF](https://ntalabot.github.io/hybridsdf/)
> compining Impicit shapes with primitives

#### SDF Editing
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
### Pre-processing
- Indoor scenes are captured using TLS'es or MMS's, capturing both unstructured (colored)pointclouds and panoramic images.
- At the same time we also use a synthetic dataset to evaluate the full scene reconstruction numerically. based on the Virtual scanner
- 
- The Pano's have are localised in the same coordinate system as the pointclouds.
### 3D Object detection
- 3d object detection is done using Votenet on the pointcloud directly. Since the pointcloud data has less occlusions and is more complete, we assume we can detect more objects 
- [insert votenet workings]
- We combine the smaller overlapping boxes to reduce the over-detection
- All the detected objects are isolated from the scene, leaving an empty scene, full of holes. We cut out the full box to ensure clean cut-out shapes in the scene.
- After that, we detect the different planes in the scene using a RANSAC plane detector
- The detected planes are used to remove the floors and walls that are wrongly included in the objects bounding box.
### Object completion
- The isolated object partial pointclouds serve as the base for the reconstruction.
- The second part that is needed are images of those objects.
	- We project the bounding box polygons onto the panoramic images and isolate the objects using polar image coordinates in the spherical image. 
	- The object points are used to create a more precise object mask to highlight only the object in the picture.
- The masked image serves as the main input into the completion network, where the points are added for scale and positional reference. [Hunyuan Omni]
- After the geometry is reconstructed, we use the next model [Hunyuan Paint] by using the image again, but this time the texture is reconstructed for the whole object. 
- The resulting object is a textured Mesh.
### Scene completion 
Using geometry based polygon reconstruction
- The holey pointcloud of the remaining scene serves as the input for the scene completion.
- The detected planes are intersected with the bounding boxes to create partial planes that can be inserted into the scene mesh.
- The boundaries between the different RANSAC planes are used to limit the size of the floor/wall/ceiling 
### Object Dyamification 
using Gaussian splat 27-Slicing / part detection
- In order to get a more realistic transformation of the objects, we aim to preserve part-wise proportions of our objects
- We perform a part detection on the object, using convex decomposition to get plausible parts that are not bound to any specific class.
- The part indexes are baked into the vertex colors of the textured mesh.
- For the actual manipulation, the user can divide the object in 3 parts along a chosen axis. After that, the user can stretch the object to a desired position. All parts that are  entirely in the first section do not change, all objects that are entirely in the last section get moved to the end position. All parts that are either fully in the second zone or partially, are stretched to match the new distance.
	- This is done either by just stretching.
	- Or by repeating the parts along the axis and scaled to match the exact size.
### Scene reconstruction
- The objects are placed back into the scene, and can be freely manipulated in the environment.

## Experiments

### Dataset
- Real world datasets of the Campus Rabot
- Synthetic datasets based on Virtual scanner

### Results
- Multi step results of the full pipeline
- Comparisons to SAM3D image only
- Comparison to ground truth synthetic data




