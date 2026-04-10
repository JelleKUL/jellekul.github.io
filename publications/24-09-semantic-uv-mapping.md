---
layout: publication
title: Semantic UV Mapping to Improve Texture Inpainting
date: 2024-09-12
permalink: /publications/semantic-uv-mapping/
icon: "/img/icons/Semantic UVMapping.png"
methodImage: "img/publications/SemanticUVMapping Methodology.png"
resultImage: "img/publications/semantic-uvmapping-results.png"
doilink: "https://doi.org/10.2312/cgvc.20241221"
githublink: "https://github.com/JelleKUL/generationtools"
objective: 2
type: Conference

description: This work aims to improve texture inpainting after clutter removal in scanned indoor meshes. This is achieved with a new UV mapping pre-processing step which leverages semantic information of indoor scenes to more accurately match the UV islands with the 3D representation of distinct structural elements like walls and floors.

bibtex: |
 @inproceedings{10.2312:cgvc.20241221,
 booktitle = {Computer Graphics and Visual Computing (CGVC)},
 editor = {Hunter, David and Slingsby, Aidan},
 title = {{Semantic UV Mapping to Improve Texture Inpainting for 3D Scanned Indoor Scenes}},
 author = {Vermandere, Jelle and Bassier, Maarten and Cuypers, Suzanna and Vergauwen, Maarten},
 year = {2024},
 publisher = {The Eurographics Association},
 ISBN = {978-3-03868-249-3},
 DOI = {10.2312/cgvc.20241221}
 }
        


---
# Semantic UV mapping to improve texture inpainting for 3D scanned indoor scenes

  

**J. Vermandere¹, M. Bassier¹, S. Cuypers¹, M. Vergauwen¹**

  

¹ KU Leuven, Belgium

  

*EG UK Computer Graphics & Visual Computing (2024)*

*A. Slingsby and D. Hunter (Editors)*

*DOI: 10.2312/cgvc.20241221*

  

**CCS Concepts:** Computing methodologies → Mesh geometry models; Texturing;

  

---

  

## Abstract

  

This work aims to improve texture inpainting following clutter removal in scanned indoor meshes. This is achieved through a new UV mapping pre-processing step that leverages semantic information from indoor scenes to more accurately align the UV islands with the 3D representations of distinct structural elements, such as walls and floors. Semantic UV Mapping enhances traditional UV unwrapping algorithms by incorporating not only geometric features but also visual features derived from the existing texture. This segmentation improves UV mapping and simultaneously simplifies the 3D geometric reconstruction of the scene after the removal of loose objects. Each segmented element can then be reconstructed separately, using the boundary conditions of the adjacent elements. Since this is performed as a pre-processing step, other specialized methods for geometric and texture reconstruction can be employed in the future to further enhance the results.

  

## 1. Introduction

  

Empty 3D indoor environments, captured from real locations, are in high demand in the gaming and Architecture, Engineering, Construction, and Operations (AECO) industries [VBV22]. These environments can be used for a wide variety of applications, such as remodeling, renovations, and interactive simulations. These environments can be captured and processed using different methods. One of the more popular methods involves using a 3D scanner to capture a full 3D point cloud accompanied by panoramic images that add more information. For efficient consumption, these models are converted to meshes, which retain much of the geometric detail while also embedding the textural information of the surfaces [BVGW24]. However, these environments are rarely empty when captured. Loose objects present during the capture process can lead to occlusions, either because they are placed against a permanent element or because they block the view of another part of the room. Current semantic instance segmentation methods can automatically detect these objects [DRB\*18], enabling an automated removal process. Removing these objects from the scene reveals occlusions and holes, resulting in an incomplete environment. Therefore, there is a need to complete these missing parts.

  

Holes and missing regions in meshes can be completed in two steps: first geometrically and then texturally. Geometric hole filling has been a field of much research, leading to very robust tools and algorithms [DRB\*18, MCST22, BG14] for filling these holes. Image inpainting has recently gained popularity due to the rise of diffusion models, which dramatically improve inpainting results [LDF\*22]. However, there are still some obstacles to using this method on 3D model textures. The visual appearance of a 3D object is created by using a texture map, which projects the faces onto a 2D plane. This projection is called UV projection. The projection process creates a disconnect between the 3D mesh and the UV texture map [VBV23], as the location of a face in 3D space does not necessarily correspond to the same location on the UV plane. This means adjacent faces do not always remain adjacent in 2D.

  

Current SOTA works approach this problem in different ways. Works like [GSZ\*21, SGC\*24, WFR23] use a 2D inpainting approach to paint on the camera views and reproject them onto the mesh. While this works well for objects close to walls or distant from the camera, these methods struggle with large occlusions due to the complex room geometry and multiple objects covering the views. Other works [FN22, OMN\*19] aim to directly predict the color in 3D space. However, these models are limited in resolution due to the use of vertex colors or texture fields.

  

The main goal of this work is to improve the UV projection of the scene by leveraging semantic instance segmentation to separate loose parts from the scene, as well as distinct structural elements like walls and floors. Using these masks, the loose objects can be removed from the scene, and the resulting missing geometry can be reconstructed element by element. Furthermore, the segmented structural elements also allow for better UV mapping, ensuring the resulting UV map more closely matches the 3D mesh, minimizing distortion and keeping adjacent faces together. This new UV map will improve the texture inpainting process, leading to a more visually appealing mesh.

  

## 2. Background and related work

  

### 2.1. Texture inpainting

  

Restoring missing parts of an image has evolved from algorithm-based methods like Gaussian inpainting [GL17] to machine learning-based approaches like Inpaint Anything [YFF\*23] and Mask-Based Inpainting [LLZ\*22]. These approaches have the advantage of being able to predict the missing pixels based on the surrounding data instead of solely extrapolating a pattern from the image.

  

The shift towards diffusion-based inpainting has enabled works like No Shadow Left Behind [ZMBKC21] to remove masked objects completely from a picture, including the object's shadows. PanoDR [GSZ\*21] and the work that builds on it [SGC\*24] take this a step further by training the diffusion model on spherical panoramic images to enable direct object removal on 360° images. Because these models operate purely in 2D, they do not contain any 3D representation of the scene.

  

Diffusion-based inpainting has also been used to remove objects from a scene in 3D. Clutter Detection and Removal [WFR23] inpaints both RGB and depth images from multiple viewpoints of a single object and reconstructs the 3D mesh in those missing parts. NeRFiller [WHJ\*23] uses a similar approach but creates a Neural Radiance Field (NeRF) instead. These view-based models are limited by what is visible to the camera in a single view. Instead of using a camera view of the missing region, Texture Inpainting for Photographic Models [MCT23] uses dynamic UV mapping to ensure the missing region is always centered and surrounded by reference pixels to perform the inpainting, but it is limited to small areas.

  

The missing color can also be predicted directly on the mesh. STINet [FN22] directly predicts the vertex colors of the missing regions, while Texture Fields [CYF22] creates an implicit neural field to generate the missing regions. However, these methods are limited by the resolution of the geometry and struggle to generate fine details.

  

### 2.2. Scene Segmentation

  

When trying to segment a scene, the different objects need to be detected. Works like Votenet [DHN20] and V-DETR [SGY\*23] use a point-transformer model [WJW\*23] to create bounding boxes for each distinct object. While these work well, they only detect objects.

  

Full scene instance segmentation takes this a step further by labeling every face. Works like Unscene3D [RLD23] can perform class-agnostic segmentation completely unsupervised. Sai3D [YLX\*23] also enables CLIP-based embedding to search for specific objects in the scene.

  

### 2.3. UV mapping

  

The biggest obstacle in using 2D inpainting methods on 3D meshes is the lack of a UV map that is both efficient and retains the face-adjacent relationships of objects in a scene. Graphseam [TRC\*20] uses a Graph Neural Network (GNN) to automate the UV mapping process while retaining semantic seams, while Flatten Anything [ZHWH24] uses point-wise mappings between the 3D points and UV coordinates. However, these methods are difficult to generalize to a large scene. Nuvo [SGV\*23] aims to address this by optimizing the UV layout for the visible parts using a neural field. This largely overcomes the challenges posed by the complex geometry of reconstructed scenes.

  

## 3. Methodology

  

The proposed method as shown in Figure 1 consists of multiple steps: First the input mesh is segmented, and the segmentation masks are used for both element separation and UV seam creation. Second, the loose objects are removed and the segmented structural elements are all completed geometrically. Third, the UV map is unwrapped following the semantic seams. Fourth, the texture is inpainted in the newly created geometry. Finally, the texture is reprojected on the empty mesh.

  

### 3.1. Scene segmentation

  

The first step is segmenting the full scene as seen in Figure 1a. Before the segmentation is performed, due to the limited resolution of the mesh, we cannot guarantee that each face is exclusive to a single object. This is why we first perform a Geometry refinement step [VBV23] to split the faces according to their texture. Both the loose objects and the structural elements are detected using UnScene3D [RLD23], which uses geometric and colour features to generate pseudo masks, these masks are then refined using a self-trained model. Since the model is optimised for object detection, structural elements like walls can sometimes remain clustered. We also perform a RANSAC plane segmentation [KL18] to refine the walls.

  

### 3.2. Geometric Reconstruction

  

The loose objects, detected in the previous step, are removed from the scene as illustrated in Figure 1b. This results in large holes that need to be reconstructed. Before each segmented structural element is reconstructed one by one, the RANSAC planes, detected in the previous step, are used to determine the intersection edges between the elements. These edges form the boundary conditions for the Delaunay reconstruction [BG14].

  

### 3.3. Semantic UV Mapping

  

After the geometry has been reconstructed, the newly generated faces are given the same label as the original element. The boundaries between the different segmentation labels are marked as UV seams. These seams serve as the basis for the semantic UV mapping as seen in Figure 1c. Since the end goal is to inpaint the missing areas, we optimize the UV map to minimize distortion by using Least Squares Conformal Maps [LPRM02], while still aiming to keep as many faces as possible from the same label joined. This is largely possible due to the flat nature of the structural elements in indoor scenes. Furthermore, when inpainting textures the orientation of the image is also relevant. By introducing a Y/Z up consistency in our unwrapping method, each element is oriented consistently, where vertical elements like walls and beams are always oriented with the up-direction facing up on the image, and flat elements like floors and ceilings have their forward direction facing up.

  

### 3.4. Texture reconstruction

  

The final step after the mesh has been semantically UV mapped is painting in the missing regions. This is performed on the 2D texture of each element. The newly generated geometry serves as the inpainting mask, this ensures that only the new parts are altered. The rest of the UV island serves as a reference for the diffusion-based inpainting [LDF\*22]. Because each element is inpainted separately, there is no confusion from other adjacent materials possible. After the texture has been inpainted completely, the texture is reprojected on the 3D mesh. Because the UV map was optimized for inpainting, and not for efficiency, the resulting UV maps can be very large. This is why, for a final step we repack the UV layout for optimized space efficiency while keeping the semantic islands intact.

  

## 4. Experiments

  

### 4.1. Dataset

  

For our experiments, we used the ScanNet++ [YLND23] and Matterport 3D [CDF\*17] Datasets as seen in Figure 2. The ScanNet++ dataset contains 460 high-resolution 3D reconstructions of indoor scenes with dense semantic and instance annotations. The Matterport 3D Dataset is a scanned dataset that consists of 90 fully textured building-scale scenes, including semantic labels of the whole dataset. We focused on single-room scenes with moderately dense furniture, pre-labelled. These labels serve as the baseline for both the loose object removal and the semantic UV mapping.

  

### 4.2. Object detection and removal

  

For our experiments, the instance masks from the Matterport3D dataset are used to separate the mesh as seen in Figure 3. The labels do not always align perfectly with the objects, this is why we removed all the faces inside the bounding box of the objects. This ensured a clean-cut line. The RANSAC plane segmentation performed well on the walls and floors but had difficulty with more complex geometry.

  

The experiments have shown that the geometry completion performs better when each object is removed sequentially, rather than in parallel. Furthermore, overlapping objects can disrupt the planar detection, so they should be removed together, while this leads to a higher amount of existing data that is removed, the reconstruction results, illustrated in Figure 4, will be better.

  

### 4.3. Semantic UV Mapping

  

The semantic segmentation has created the UV seams at not just geometrically distinct edges, but also texturally (Figure 3). Due to the simple geometry of the structural elements the UV maps can be created without too much distortion using the segmentation seams as seen in Figure 5. We do see, however, that due to the orientation constraint, the UV maps are laid out separately for each object. This means the texture size is larger than the original texture map from the dataset.

  

### 4.4. Texture reconstruction

  

The texture inpainting process is able to use the existing texture as an example which creates mostly indistinguishable textures for the more basic surfaces (Figure 5). The faces of the new geometry provide a clear bounding mask, allowing the inpainting to only affect the required area. The final result can be seen in Figure 6.

  

## 5. Discussion

  

The resulting empty scenes after the loose objects have been removed show believable results. This is helped by the fact that the structural elements in indoor scenes are generally straightforward. By reducing the geometric reconstruction to a planar triangulation, the problem becomes much less complex and manageable. This is however not possible for all types of structural elements. More organic shapes require a more complex reconstruction like AutoSDF [MCST22]. The advantage of our pre-processing pipeline is that the methods are interchangeable, while still retaining the advantages of the semantic segmentation. The inpainted textures show very good results for repetitive and basic materials. However, more graphic elements that are not properly segmented can lead to artifacting in the final results. The effectiveness of this method is however difficult to quantify due to the lack of real ground truth data. This is why we visually evaluated each scene, checking for visual consistency and believability.

  

## 6. Conclusion

  

This paper introduced a novel pre-processing step in the object removal pipeline for indoor scanned environments. By semantically labelling the different elements in the scene, both the geometry completion and texture reconstruction can be improved due to clearer boundaries between the different elements. The holes resulting from the removal of the detected loose objects can be better completed element-wise, rather than for the whole scene. Using the predicted intersection lines between the different elements, we can clearly define the boundary conditions for the geometric reconstruction. The semantic UV mapping also ensures each element is mapped as close as possible to its 3D representation, making the inpainting process much more straightforward. The existing, textured parts of the elements serve as a reference for the newly created geometry.

  

## References

  

[BG14] Boissonnat, J. D., Ghosh, A.: Manifold reconstruction using tangential delaunay complexes. *Discrete and Computational Geometry* 51 (2014), 221–267. https://doi.org/10.1007/s00454-013-9557-2

  

[BVGW24] Bassier, M., Vermandere, J., Geyter, S. D., Winter, H. D.: Geomapi: Processing close-range sensing data of construction scenes with semantic web technologies. *Automation in Construction* 164 (2024), 105454. https://doi.org/10.1016/j.autcon.2024.105454

  

[CDF\*17] Chang, A., Dai, A., Funkhouser, T., Halber, M., Niessner, M., Savva, M., Song, S., Zeng, A., Zhang, Y.: Matterport3d: Learning from rgb-d data in indoor environments. *International Conference on 3D Vision (3DV)* (2017).

  

[CYF22] Chen, Z., Yin, K., Fidler, S.: Auv-net: Learning aligned uv maps for texture transfer and synthesis, 2022. https://nv-tlabs.github.io/AUV-NET

  

[DHN20] Ding, Z., Han, X., Niethammer, M.: Votenet+: An improved deep learning label fusion method for multi-atlas segmentation. *Proceedings - International Symposium on Biomedical Imaging* 2020-April (2020), 363–367. https://doi.org/10.1109/ISBI45749.2020.9098493

  

[DRB\*18] Dai, A., Ritchie, D., Bokeloh, M., Reed, S., Sturm, J., Niebner, M.: Scancomplete: Large-scale scene completion and semantic segmentation for 3d scans. *Proceedings of the IEEE Computer Society Conference on Computer Vision and Pattern Recognition* (2018), 4578–4587. https://doi.org/10.1109/CVPR.2018.00481

  

[FN22] Flynn, J. P., Niessner, M.: Free-form surface texture inpainting using graph neural networks. *Visual Computing Group*.

  

[GL17] Galerne, B., Leclaire, A.: Texture inpainting using efficient gaussian conditional simulation. *SIAM Journal on Imaging Sciences* 10 (2017), 1446–1474. https://doi.org/10.1137/16M1109047

  

[GSZ\*21] Gkitsas, V., Sterzentsenko, V., Zioulis, N., Albanis, G., Zarpalas, D.: Panodr: Spherical panorama diminished reality for indoor scenes. http://arxiv.org/abs/2106.00446

  

[KL18] Korman, S., Litman, R.: Latent ransac. http://arxiv.org/abs/1802.07045

  

[LDF\*22] Lugmayr, A., Danelljan, M., Fisher, A. R., Radu, Y., Luc, T., Gool, V.: Repaint: Inpainting using denoising diffusion probabilistic models, 2022.

  

[LLZ\*22] Li, W., Lin, Z., Zhou, K., Qi, L., Wang, Y., Jia, J.: Mat: Mask-aware transformer for large hole image inpainting, 2022. https://github.com/fenglinglwb/MAT

  

[LPRM02] Lévy, B., Petitjean, S., Ray, N., Maillot, J.: Least squares conformal maps for automatic texture atlas generation, 2002.

  

[MCST22] Mittal, P., Cheng, Y.-C., Singh, M., Tulsiani, S.: Autosdf: Shape priors for 3d completion, reconstruction and generation. http://arxiv.org/abs/2203.09516

  

[MCT23] Maggiordomo, A., Cignoni, P., Tarini, M.: Texture inpainting for photogrammetric models. *Computer Graphics Forum* 42 (9 2023). https://doi.org/10.1111/cgf.14735

  

[OMN\*19] Oechsle, M., Mescheder, L., Niemeyer, M., Strauss, T., Geiger, A.: Texture fields: Learning texture representations in function space. http://arxiv.org/abs/1905.07259

  

[RLD23] Rozenberszki, D., Litany, O., Dai, A.: Unscene3d: Unsupervised 3d instance segmentation for indoor scenes. http://arxiv.org/abs/2303.14541

  

[SGC\*24] Slavcheva, M., Gausebeck, D., Chen, K., Buchhofer, D., Sabik, A., Ma, C., Dhillon, S., Brandt, O., Dolhasz, A.: An empty room is all we want: Automatic defurnishing of indoor panoramas. http://arxiv.org/abs/2405.03682

  

[SGV\*23] Srinivasan, P. P., Garbin, S. J., Verbin, D., Barron, J. T., Mildenhall, B.: Nuvo: Neural uv mapping for unruly 3d representations. http://arxiv.org/abs/2312.05283

  

[SGY\*23] Shen, Y., Geng, Z., Yuan, Y., Lin, Y., Liu, Z., Wang, C., Hu, H., Zheng, N., Guo, B.: V-detr: Detr with vertex relative position encoding for 3d object detection. http://arxiv.org/abs/2308.04409

  

[TRC\*20] Teimury, F., Roy, B., Casallas, J. S., MacDonald, D., Coates, M.: Graphseam: Supervised graph learning framework for semantic uv mapping. http://arxiv.org/abs/2011.13748

  

[VBV22] Vermandere, J., Bassier, M., Vergauwen, M.: Automatic alignment and completion of point cloud environments using xr data. In *Proceedings - SIGGRAPH 2022 Posters* (2022). https://doi.org/10.1145/3532719.3543258

  

[VBV23] Vermandere, J., Bassier, M., Vergauwen, M.: Texture-based separation to refine building meshes. *ISPRS Annals of the Photogrammetry, Remote Sensing and Spatial Information Sciences* X-1/W1-2023 (12 2023), 479–485. https://doi.org/10.5194/isprs-annals-x-1-w1-2023-479-2023

  

[WFR23] Wei, F., Funkhouser, T., Rusinkiewicz, S.: Clutter detection and removal in 3d scenes with view-consistent inpainting. http://arxiv.org/abs/2304.03763

  

[WHJ\*23] Weber, E., Hołyński, A., Jampani, V., Saxena, S., Snavely, N., Kar, A., Kanazawa, A.: Nerfiller: Completing scenes via generative 3d inpainting. http://arxiv.org/abs/2312.04560

  

[WJW\*23] Wu, X., Jiang, L., Wang, P.-S., Liu, Z., Liu, X., Qiao, Y., Ouyang, W., He, T., Zhao, H.: Point transformer v3: Simpler, faster, stronger. http://arxiv.org/abs/2312.10035

  

[YFF\*23] Yu, T., Feng, R., Feng, R., Liu, J., Jin, X., Zeng, W., Chen, Z.: Inpaint anything: Segment anything meets image inpainting. http://arxiv.org/abs/2304.06790

  

[YLND23] Yeshwanth, C., Liu, Y.-C., Niessner, M., Dai, A.: Scannet++: A high-fidelity dataset of 3d indoor scenes. http://arxiv.org/abs/2308.11417

  

[YLX\*23] Yin, Y., Liu, Y., Xiao, Y., Cohen-Or, D., Huang, J., Chen, B.: Sai3d: Segment any instance in 3d scenes. http://arxiv.org/abs/2312.11557

  

[ZHWH24] Zhang, Q., Hou, J., Wang, W., He, Y.: Flatten anything: Unsupervised neural surface parameterization. http://arxiv.org/abs/2405.14633

  

[ZMBKC21] Zhang, E., Martin-Brualla, R., Kontkanen, J., Curless, B.: No shadow left behind: Removing objects and their shadows using approximate lighting and geometry, 2021.

  

---

  

*© 2024 The Authors.*

*Proceedings published by Eurographics - The European Association for Computer Graphics.*

*CC BY License.*

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
