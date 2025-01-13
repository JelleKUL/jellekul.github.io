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

[PhysFiT: Physical-aware 3D Shape Understanding for Finishing Incomplete Assembly]
> Part based completion
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
- [ScanNet](http://www.scan-net.org/)
- [PartNet](https://partnet.cs.stanford.edu/)
- [3DObjTex.v1](https://cvi2.uni.lu/sharp2022/challenge1/)
- [Matterport3d](https://niessner.github.io/Matterport/)
- [Real World Textured Things (RWTT)](https://texturedmesh.isti.cnr.it/)

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



## Reviews

### Reviewer 1
  
 **Description**
> The paper presents a three-step pipeline for reconstructing 3D objects. Initially, the method focuses on geometry reconstruction, utilizing an implicit representation and a standard method to establish a foundational structure. Subsequently, the mesh undergoes segmentation into distinct areas, with each area assigned a specific material. Any areas lacking initial material assignment are predicted. Finally, a 2D inpainting process enhances the texture on the UV map, refining visual details. The pipeline leverages state-of-the-art technology at each step.

 **Clarity of Exposition**
> The paper provides a thorough explanation overall, but it lacks detail on how the UV mapping is derived for each material patch. This is an important aspect, as the quality of UV mapping can significantly impact the final visual result. Including this information would enhance the completeness of the paper's explanation.

 **Quality of References**
> seems ok to me.

 **Technical Correctness and Reproducibility**
> There are some technical details that are not convincing. For example, the use of a 64^3 resolution seems quite small for state-of-the-art scanned objects. It is unclear if the proposed technology can effectively handle real data rather than just toy examples.

 **Validation**
> The paper only showcases a few examples, all focused on a single object (a sofa). This limited scope may not be sufficient to convincingly demonstrate the effectiveness of the proposed method across a broader range of objects or scenarios.

 **Ethics**
> no issue

 **Explanation of Rating**
> While the paper introduces an interesting pipeline, I struggle to identify its main contribution. The pipeline utilizes a sequence of steps with well-known technologies to construct a complete framework. However, each individual step of the pipeline does not appear to be novel in the field. The paper asserts that it outperforms classical inpainting methods, yet the examples provided are not convincing, as they consist only of a few toy examples. Typically, when a paper assembles a pipeline of well-known steps into a new method, it should demonstrate the full potential, stability, and generality of the approach across a wide range of scenarios. In this case, I do not see evidence of this, which leaves me unconvinced about the paper's contribution to the field.  
> One aspect that I find somewhat interesting is the use of block decomposition for materials. However, the examples do not clearly demonstrate an advantage over other methods, nor do they show how the approach performs on complex datasets.

 **Explanation of Conference vs Journal Recommendation
> I believe that the paper does not meet the standards for the conference.


### Reviewer 2

 **Description**
> The paper presents a system to complete partially scanned objects, as a sequence of steps each being the application of an existing techniques to tackle a given subproblem, most of them employing a data driven, machine learning approach. Specifically, the Objects are first realigned, turned into a volumetric distance function, fed to a network for the actual completion of this function, and reconverted back in polygonal format; on the texture front, textures are then fed to a material classifier, they are re-UV mapped, and their empty parts are re-impainted according to their material id with another CNN.

 **Clarity of Exposition**
> The text can be followed.  
>   
> The process described in Lines 397 to 420 is difficult to follow sentence by sentence, but overall what it's being done is very standard and expected. I would rather summarize the objective on an higher level, something like "We bake the boundary between texture materials as new edges in the mesh, and duplicate the involved vertices, so that each face shares the same material in its three vertices".  
>   
> Notation: use more distinguishable symbols for elements and set names (this applies both to set S and P)  
>   
> Minor (style):  
>   
> 
> - Change the color coding of figures 3 and 4 (right) to make the wireframe (and possibly the shading) visible on the polygonized mesh.  
>       
>     
> - "A big challenge" => "A challenge"  
>       
>     
> - Line 274: "rendered" => "that can be rendered"  
>       
>     
> - The acronym "SDF" is introduced three times (lines 144, 153, 280).  
>       
>     
> - Line 304 and 520: replace "x" with "\times" in "64x64x64"  
>       
>     
> - Line 305: "Following," => "Next," or "Then," etc.  
>       
>     
> - Line 366: "In parallel to" implies a meaning that I don't think is what is intended here. Use, maybe, "In addition to".  
>       
>     
> - I would avoid the initial sentences of Sec 2, 3, 4, 5 that announce what the section is about to discuss. The section title is sufficient.  
>       
>     
> - In Section 5 (discussion), avoid summarizing again what has been described in the paper, e.g. "Disjoint patches in the UV map are linked using cosine similarity." or ", which is necessary because object regions next to each other in 3D can be far from each other on the 2D UV map."

 **Quality of References**
> I could not spot any missing reference. Positioning with respect to previous work is fair (as far as I know).

 **Technical Correctness and Reproducibility**
> There are parts that aren't described, such as, which technique is used to construct the optimized UV layout (sec 3.2).  
>   
> The paper should also clarify what is the expected input: I didn't understand until very late that it comes with its own initial UV map (how is that constructed? it is not something that is natural to assume on intermediate scanned data. The paper should discuss the motivation of this assumption).  
>   
> (More in general, it would make a stronger case for this paper if the system could claim to work on "raw scanned data"; but a UV-mapped mesh -- even an incomplete and crudely UV-mapped mesh -- is in turn the result of a long process, involving a sequence of steps, performed on captured data. This hinders somewhat reproducibility, because much of the outcome of this system depends on that initial processing, left undiscussed, and diminishes usefulness.)  
>   
> The part about dealing with open meshes resorting unsigned distance field has a weak motivation, being seemingly unaware of the existence of modern ways to address that specific limitation (see [1]). This is a rather central choice at the core of this contribution that seems unwarranted.  
>   
> Minor:  
>   
> "naked edges" (in line 305): do you mean "open edges"?  
>   
> Captions of Fig 3, 4: don't call a mesh (extracted from a SDF) a SDF. Be more precise  
>   
> Aboud SDF, in the state of the art…  
>   
> 
> - line 157: "An SDF _can be_ voxalized" (they aren't necessarily, and often they are sampled on a hierarchical structure such as a octree rather than a regular voxel grid)  
>       
>     
> - Later: "SDFs can only be generated from watertight meshes." This is, as discussed above, inaccurate  
>       
>     
> 
> [1] Robust inside-outside segmentation using generalized winding numbers  
> A Jacobson, L Kavan, O Sorkine-Hornung - TOG, 2013

 **Validation**
> The paper shows the final results of their system over some four incomplete scanned objects.  
> Like typical in methods based on CNN, there is an additional validation consisting in measuring the learning rate during the training.

 **Ethics**
> I'm not aware of any ethical concern for this work

  
 **Explanation of Rating
> I consider this to be a system paper (which, to be clear, is not a negative thing): rather than proposing a novel technique to address a given problem, the paper puts together and adapts a number of existing solution, and dataset to train/retrain networks, to provide a complete system going from (processed, scanned data) of incomplete meshes to more complete (and watertight-by-construction) meshes.  
>   
> With this premise, my concern is that this work is not nearly validated enough for a system paper.  
> I also have some issue with a few technical aspects, as discussed in the relative section.  
>   
> Still, I'm somewhat undecided because this could serve as an useful case-study and a point being made on our collective current capabilities in the field of object completion.

  
 ****Explanation of Conference vs Journal Recommendation † ✓****
> My point is exactly "Conference Papers [..] can be less complete or comprehensive (e.g. have less extensive validation or formal verification)", so this is a good fit.

### Reviewer 3

 **Description**
> This paper introduces a geometry and texture completion system for partially scanned objects.  
> Given a partial shape, it utilizes a geometry prediction network to produce a complete shape without texture.  
> For the texture part, the complex mesh is first unwrapped to the UV space with scanned texture filled.  
> Before inpainting textures in the UV space, a SAM model predicts the semantic information for pixels on the UV texture.  
> The overall pipeline is clear but misses many details.  
> The contribution of the paper mainly lies at the system level as most modules already exist.

 **Clarity of Exposition**
> The pipeline of the method is clear but misses many details in each step.  
>   
> 
> - USDF->UDF. In previous literature, the unsigned distance field is shortened to UDF instead of USDF.  
>       
>     
> - The original input of the AutoSDF is an SDF voxel grid. Will there be problems if you feed a UDF?  
>       
>     
> - I think replacing `material` with `semantic label` would be easier to understand.  
>     In computer graphics, `Material` refers to `texture`.  
>       
>     
> - L369-370, I am not sure if this claim is correct. I believe traditional texture parameterization aims at minimum distortion too.  
>       
>     
> - L374-376, how do you realize minimum distortion while preserving topology?  
>       
>     
> - L387 and equation 2. How is `t_c` determined in the grouping process?  
>       
>     
> - L465-469, why bother to have two UV layouts? Would it be simpler to unwrap the mesh after completion and computer segmentation masks on the unwrapped texture? The scanned texture can be transferred to the completed mesh with little effort as the completed mesh is close to the input.  
>       
>     
> - Since the method involves multiple steps, I am wondering if the method is sensitive to each step's accuracy.  
>     For example, if a step(geometry completion, segmentation, or texture completion) fails, will the whole pipeline still produce reasonable results?

 **Quality of References**
> The reference is sparse and only contains 30 papers.  
> I believe some retrieval-based methods should taken into discussion.  
>   
> DiffCAD: Weakly-Supervised Probabilistic CAD Model Retrieval and Alignment from an RGB Image  
> ROCA: Robust CAD Model Retrieval and Alignment from a Single Image  
> RetrievalFuse: Neural 3D Scene Reconstruction with a Database

 **Technical Correctness and Reproducibility**
> Since the method involves multiple modules and each is complex enough, it is not likely to reproduce without code release.

 **Validation**
> For the validation part, the paper only presents a few qualitative results and does not include any quantitative results.  
> In addition, comparisons with baseline methods are also missing.  
> I believe at least some retrieval-based methods are comparable.

 **Ethics**
> None

 **Explanation of Rating
> I am slightly leaning toward rejection because the paper misses important implementation details, quantitative results, and comparisons with baseline methods.  
> In addition, the paper's contribution is limited since it basically combines multiple methods together.

 ****Explanation of Conference vs Journal Recommendation † ✓****
> Considering the limited contribution and evaluation, the conference track should be more suitable.

### Reviewer 4

 **Description**
> This paper proposes a geometry and texture completion process for incomplete textured mesh models. The geometry and texture completions are divided into two stages. The geometry completion first establishes the unsigned distance field to voxelise the mesh geometry, which is followed by a modified AutoSDF to predict several possible completion results. The texture completion first adopts SAM to segment the partial texture image and clusters the material patches based on image feature vectors. The proposed method also refines the edges of the 3D mesh according to the 2D boundaries of the material patches. Finally, it utilizes the extended IF-Net for geometry and appearance encoding, and leverages patch-based inpainting to achieve texture completion. In summary, this method combines some existing methods (or their extended versions) to achieve geometry and texture completion of the incomplete mesh segmented from some large-scale scanned scenes. Although this combination can produce some results, it has not brought enough technical insight.

 **Clarity of Exposition**
> The format of the submission is ok.  
> Overall, the paper provides a clear description of the entire process of the method. However, as the proposed method involves many stages, some details need to be further elaborated.  
> For example, in equation (1), the definition of the symbol ‘k’ has not been given and there is no detailed explanation provided for the entire equation. Although the equation comes from existing work, AutoSDF, necessary explanations are still needed to ensure the self-contained and completeness of the paper.  
> Another point I did not fully understand lies in section 3.3, lines 447-452, about how to apply the IF-Net to generate the material. What is the definition of material here, and does it refer to PBR material information? What color is given for each material patch? How to combine the three color channels? Why there are a maximum of 8 materials?

 **Quality of References**
> As far as I know, the references are ok to me.

 **Technical Correctness and Reproducibility**
> The technical content is correct. It is suggested to add an algorithm pseudocode for further elaboration. The work can be reproduced with the information provided in the paper, but as the process has several parts which makes the process a bit complicated, so a code release is recommended.

 **Validation**
> Validation is a major issue of this paper. Although the paper presents some completion results in Figure 12, the quantity is not sufficient, with only four examples, and the objects are mostly chairs without examples of other categories. The authors mention in section 4.1 that there are a total of 90 scenes, and each scene can be segmented into 15-30 objects, so there should be a lot of objects available to demonstrate the effectiveness of the proposed method. Another point is the lack of comparison with other methods. There is no way to evaluate the advantages of this method. Some existing methods can be applied to a similar task with some small modifications (such as Texture Fields). Or some baseline methods can be used in the comparisons.  
> On the other hand, the experiment section lacks ablation experiments to verify the role of each module in the proposed method, especially the material segmentation module. Considering that the proposed method involves multiple modules, ablation experiments are essential.  
> Last, the presentation format of the results should also be more diverse. What is most needed is to show the rendering results of other views that are different from the input view. Furthermore, if possible, a supplementary video should be provided to showcase the completed model by rotating the model.

 **Ethics**
> None

 ****Explanation of Rating † ✓****
> The biggest issue with this paper is the limited technical contribution. The authors combine many existing methods (or their extended versions), and the results of this combination are predictable, so this leans more towards engineering work and brings less technical insight. Although the material prediction part seems to be new, it also combines existing segmentation and clustering methods, and there is no relevant ablation experiment to verify its effectiveness.  
> And there is a lack of many other necessary experiments, as mentioned in the previous ‘Validation’ part.  
> Although the authors claim in the abstract that the method can have clear material boundaries, according to the results of the teaser, some areas are still affected by adjacent regions.  
> So, based on the above points, I tend to reject this paper.

  
 ****Explanation of Conference vs Journal Recommendation † ✓****
> Due to the lack of technical insights and sufficient experimental evidence to demonstrate the effectiveness and superiority of the proposed method, I do not recommend its acceptance.

### Reviewer 5

 **Description**
> The paper proposes a pipeline to complete partially scanned objects in indoor environments. The proposed pipeline consists of three stages for geometry prediction, material segmentation, and texture completion. The geometry is predicted using AutoSDF adapted to take unsigned distance field of partial objects as input, and applying marching cubes to extract an watertight mesh from the output SDF. To segment the material, SAM is applied on unwrapped texture images to obtain a set of patches which are clustered using patch features into materials (3D mesh vertices and edges are added at the material boundaries as needed). In the last stage of texture completion, IF-Net is used to predict the material index of the completed geometry and then each material is completed using patch-based inpainting. Models from ShapeNet are used for training and the approach used to complete four partially scanned objects extracted from Matterport3D scans.  
>   
> The main contribution of the work is the proposed pipeline, with the main novelty being the material prediction step that allows for more consistent texturing based on material.  
> As there is limited work on textured shape completion, the proposed pipeline (despite its relative simplicity), can be of potential interest to the community, as it is reasonable combination of existing shape completion and texturing methods.

 **Clarity of Exposition**
> The paper is clear and well organized.  
>   
> Figure captions can be improved to include more detail. For instance, it would be helpful if the Figure 2 caption explained the details of each component (as depicted in the figure).

 **Quality of References**
> References are reasonably complete.  
>   
> Some recent work on shape completion that can be included:  
>   
> 
> - PatchComplete: Learning Multi-Resolution Patch Priors for 3D Shape Completion on Unseen Categories [Rao et al. NeurIPS 2022]
> - DiffComplete: Diffusion-based Generative 3D Shape Completion [Chu et al. NeurIPS 2023]
> 
> The paper is lacking discussion on prior work that do textured shape completion. For instance, it was not mentioned in the Geometry Completion section that there is an extension to the IF-Net work for completing textured shapes [6].

 **Technical Correctness and Reproducibility**
> An appropriate amount of details is provided for the method and data processing. There are some details that are unclear:
> 
> - How exactly was ShapeNet used in training and validation? Was it used mainly for training the IF-Net as pretrained models were used for AutoSDF and SAM?

 **Validation**
> The main weakness of this work is that very little validation that is presented. There are very limited qualitative examples (just a handful), and there are no quantitative evaluation. In addition, there are no comparisons to any alternative design choices. What is presented is more a proof-of-concept pipeline, with components taken from prior work, and no evaluation how much the material prediction mattered.

 **Ethics**
> None

 ****Explanation of Rating † ✓****
> There is not enough experiments to validate the approach that is proposed. In particular, there should be comparisons again of using material prediction and mask vs not.  
>   
> In section 5, the paper claims to "thoroughly examine the strengths and limitations of key techniques employed". While there is some discussion of this, there are not quantitative or qualitative results comparing the proposed method to alternatives.

 ****Explanation of Conference vs Journal Recommendation † ✓****
> Work needs to be improved (more experiments and comparisons added) before it is ready for publication.