---
layout: page
title: Semantic UV Mapping to Improve Texture Inpainting
date: 2024-09-12
permalink: /publications/semantic-uv-mapping/
image: "/img/research/Semantic UVMapping.png"
doilink: "https://doi.org/10.2312/cgvc.20241221"
githublink: "https://github.com/JelleKUL/generationtools"
objective: 2
type: Conference

description: This work aims to improve texture inpainting after clutter removal in scanned indoor meshes. This is achieved with a new UV mapping pre-processing step which leverages semantic information of indoor scenes to more accurately match the UV islands with the 3D representation of distinct structural elements like walls and floors.
---

# Semantic UV Mapping to Improve Texture Inpainting

 <img class="img-fluid  rounded vis-img" src="../../img/research/semanticuv-methodology.pdf" alt="{{page.title}}">

<h3 class="text-primary">
  Matterport3D Results
</h3>

 <img class="img-fluid  rounded vis-img" src="../../img/research/semantic-uvmapping-results.png" alt="{{page.title}}">

## Introduction
Current 2D texture inpainting methods struggle to perform well on reconstructed geometry due to the improper uv-mapping of the scene and its objects
- Context
	- The AECO industry captures more and more data
	- There is a need for empty indoor scenes without any holes.
		- remodelling, renovation, interactivity
	- Current captured scenes are often fully furnished and incomplete, leading to a lot of missing information
	- Meshes are a good way to store indoor scenes, due to efficient 3D representation and detailed textures for detailed parts
- Problem
	- When removing the object in the scenes, large holes are left behind
	- they need to be completed, both geometrically and texturally
	- Geometry reconstruction
		- good results and far along.
	- Texture inpainting:
		- There is a certain disconnect between the geometric and textural data in a mesh.
		- This is a big problem when trying to complete missing parts in a mesh, visually
		- Texture inpainting relies on its surroundings to properly predict the missing parts, when these are not in the same place on the uv plane, it can lead to disjointed maps.
- SOA Solutions / Shortcomings
	- inpaint in a 2D picture of the 3D view and reproject
		- Can't overcome occlusions and will cause double paintovers
	- inpaint only small parts and dynamically reproject
		- difficult to use for big gaps in data and multi-material
	- Use texturefields for full 3D workflow
		- too low resolution for a full scene
- Our Solution
	- Leverage semantic instance segmentation to better divide the scene in distinct parts.
	- unwrap each object separately to minimise distortion
	- Inpaint the new geometry using the semantic mask to limit the surrounding reference to a single object
## Background and related work


### Object detection
[V-DETR 2023](https://github.com/V-DETR/V-DETR)
> Uses DETR (detection transformer) in 3D with Vertex Relative Position Encoding to improve locality

[VoteNet 2019](https://github.com/facebookresearch/votenet)
> an end-to-end 3D object detection network based on a synergy of deep point set networks and Hough voting.
### Semantic segmentation
[PTv3](https://github.com/Pointcept/Pointcept)
> Point transformer v3

### Scene Instance segmentation
[SphericalMask 2024](https://github.com/yunshin/SphericalMask?tab=readme-ov-file)
>  we introduce Spherical Mask, a novel coarse-to-fine approach based on spherical representation, overcoming those two limitations with several benefits. Specifically, our coarse detection estimates each instance with a 3D polygon using a center and radial distance predictions, which avoids excessive size estimation of AABB.

[UnScene3D](https://rozdavid.github.io/unscene3d)
>  the first fully unsupervised 3D learning approach for class-agnostic 3D instance segmentation of indoor scans. UnScene3D first generates pseudo masks by leveraging self-supervised color and geometry features to find potential object regions.

[Sai3d](https://arxiv.org/html/2312.11557v2)
> a novel zero-shot 3D instance segmentation approach that synergistically leverages geometric priors and semantic cues derived from Segment Anything Model (SAM).

[Texture-based mesh refinement 2023](https://isprs-annals.copernicus.org/articles/X-1-W1-2023/479/2023/)
> Detect different materials on the 2D texture plane to refine the mesh separation.
### Scene completion
[3D Semantic Scene Completion: A Survey](https://doi.org/10.1007/s11263-021-01504-5)
> Semantic scene completion (SSC) aims to jointly estimate the complete geometry and semantics of a scene, assuming partial sparse input. In the last years following the multiplication of large-scale 3D datasets, SSC has gained significant momentum in the research community because it holds unresolved challenges.

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

### Primitive fitting
[Supervised Fitting of Geometric Primitives to 3D Point Clouds]()
> Supervised Primitive Fitting Network (SPFN), an end-to-end neural network that can robustly detect a varying number of primitives at different scales without any user control.

[RANSAC]()
> primitive detection
### Semantic unwrapping
[GraphSeam 2020](https://www.researchgate.net/publication/346475280_GraphSeam_Supervised_Graph_Learning_Framework_for_Semantic_UV_Mapping)
> we use the power of supervised GNNs for the first time to propose a fully automated UV mapping framework that enables users to replicate their desired seam styles while reducing distortion and seam length.

[AUV-Net](https://research.nvidia.com/labs/toronto-ai/AUV-NET/)
> AUV-Net learns **aligned UV maps** for a set of 3D shapes, enabling us to easily transfer textures to provided shapes by copying texture images.

[Nuvo: Neural UV Mapping for Unruly 3D Representations](https://pratulsrinivasan.github.io/nuvo/)
> a UV mapping method designed to operate on geometry produced by 3D reconstruction and generation techniques. Instead of computing a mapping defined on a mesh's vertices, our method Nuvo uses a neural field to represent a continuous UV mapping, and optimizes it to be a valid and well-behaved mapping for just the set of visible points, i.e. only points that affect the scene's appearance.

[Flatten Anything: Unsupervised Neural Surface Parameterization](https://arxiv.org/abs/2405.14633)
>  an unsupervised neural architecture to achieve global free-boundary surface parameterization via learning point-wise mappings between 3D points on the target geometric surface and adaptively-deformed UV coordinates within the 2D parameter domain.
### Texture inpainting

[Gaussian texture inpainting](https://github.com/Ozeuth/2D-Texture-Inpainter?tab=readme-ov-file)
> In this paper proposes a stochastic inpainting methods, dealing with the case of Gaussian textures. First a simple procedure is proposed for estimating a Gaussian texture model based on a masked exemplar, which, although quite naive, gives sufficient results for our inpainting purpose. Next, the conditional sampling step is solved with the traditional algorithm for Gaussian conditional simulation.

[Inpaint anything 2023](https://github.com/geekyutao/inpaint-anything?tab=readme-ov-file#remove-anything-3d)
> Users can select any object in an image by clicking on it. With powerful vision models, e.g., [SAM](https://arxiv.org/abs/2304.02643), [LaMa](https://arxiv.org/abs/2109.07161) and [Stable Diffusion (SD)](https://arxiv.org/abs/2112.10752), **Inpaint Anything** is able to remove the object smoothly (i.e., _Remove Anything_). Further, prompted by user input text, Inpaint Anything can fill the object with any desired content (i.e., _Fill Anything_) or replace the background of it arbitrarily (i.e., _Replace Anything_).

[MAT (Mask Aware Transformer) 2022](https://github.com/fenglinglwb/MAT)
> we present a novel transformer-based model for large hole inpainting, which unifies the merits of transformers and convolutions to efficiently process high-resolution images.

[CM-GAN 2022](https://github.com/htzheng/CM-GAN-Inpainting)
>We introduce a new cascaded modulation design that cascades global modulation with spatial adaptive modulation for better hole filling. We also introduce an object-aware training scheme to facilitate better object removal. CM-GAN significantly improves the existing state-of-the-art methods both qualitatively and quantitatively.

[Free-Form Surface Texture Inpainting Using Graph Neural Networks](https://github.com/johnpeterflynn/surface-texture-inpainting-net)
>We present the Surface Texture Inpainting Network (STINet), a graph neural network-based model that generates complete surface texture for partially textured 3D meshes. In contrast to 2D image inpainting which focuses on predicting missing pixel values on a fixed regular grid, STINet aims to inpaint color information on mesh surfaces of varying geometry and topology. STINet learns from spatial information such as vertex positions and normals as well as mesh connectivity to effectively predict vertex color.

[Free-form 3D Scene Inpainting with Dual-stream GAN](https://github.com/ruby2332ruby/Free-form-3D-Scene-Inpainting?tab=readme-ov-file)
> a tailored dual-stream GAN method is proposed. First, our dual-stream generator, fusing both geometry and color information, produces distinct semantic boundaries and solves the interpolation issue. To further enhance the details, our lightweight dual-stream discriminator regularizes the geometry and color edges of the predicted scenes to be realistic and sharp.

[Automatic Defurnishing of indoor panoramas 2024](https://matterport.github.io/automatic-defurnishing-of-indoor-panoramas/)
>A pipeline that leverages Stable Diffusion to improve inpainting results in the context of defurnishing---the removal of furniture items from indoor panorama images. Specifically, we illustrate how increased context, domain-specific model fine-tuning, and improved image blending can produce high-fidelity inpaints that are geometrically plausible without needing to rely on room layout estimation. We demonstrate qualitative and quantitative improvements over other furniture removal techniques.

[PanoDR: Spherical Panorama Diminished Reality for Indoor Scenes](https://github.com/VCL3D/PanoDR)
>a model that initially predicts the structure of an indoor scene and then uses it to guide the reconstruction of an empty -- background only -- representation of the same scene.
## Methodology
### Scene segmentation
static and dynamic objects are detected using a semantic instance segmentation. The dynamic objects are removed from the scene this results in large holes in the remaining static elements.-
- Use "PointCept PTV3" to segment the scene, the segmentation serves 2 purposes: 
	- first to distinguish the loose objects from the rest of the scene
	- second, the find the different structural elements
		- walls, floors, ceilings
		- use ransac to find the different planes
		- Use the detected windows and doors to preserve holes
	- segment the scene into the 20 semantic labels.
		- then separate the different clusters using DB clustering algorithm to define the loose objects
		- create aligned bounding boxes per loose object 
### Geometric reconstruction
The loose objects are removed from the scene, resulting in large holes
- A bounding box is created for each object, this ensures a clean cut between the meshes and highlights the boundary edges for easier completion
- The different structural elements are completed geometrically using plane detection to isolate each plane of the object and calculate intersection edges. (this works because the structural elements are mostly flat objects(walls and floors))
- Delauny trianglulation is used to fill the holes based on the new edges and point at the boundaries of the object
### Semantic unwrapping
In order to properly paint the textures in, we need an undistorted uv projection. We aim to improve this workflow by introducing a semantic based unwrapping algorithm which separates the different parts of the scene into distinct planes
- once the separate objects are filled in to complete parts, we need to texture them
- each segmented object is presumed to be one inpaintable material
	- if this is not the case we use the texture-based mesh refinement to separate them further.
- Each structural element is also projected with z/y up to ensure texture continuity in the next part
- we ensure there is no distortion per object plane, essential for accurate 2D inpainting
### Texture reconstruction
Texture in the newly generated geometry is inpainted using the surrounding faces of the same instance as reference areas.
- Each object is mapped separately
- the reconstructed regions are marked for inpainting
- each segmented label serves as a reference mask for the inpainting algorithm
## Experiments
### Datasets Used
Use the scannet and matterport dataset, they contain semantically labeled indoor scenes that can be used for 

### Comparisons
Which methods try to do the same thing?
- Free-form 3D scene inpainting
- view based inpainting
	- dependant on lighting conditions
	- difficult to align with view and get coverage

### Object detection and removal
The object in the scene are detected and removed, leading to large holes in the scenes.
> show examples of the before and after
### Semantic unwrapping
The scenes are unwrapped per object
> show the different uv maps, compared to the standard unwrapping algorithms of different scenes
### Texture Reconstruction
Use inpainting to paint in the walls and floors
> compare with other bad uv mapped objects to get worse results

## Conclusion
The uv mapping creates the seams at more logical places, resulting in better object aware 