---
layout: publication
title: Synthetic Scan Dataset Generation for Non-Static Objects
date: 2026-07-05
permalink: /publications/virtual-scanner/
icon: img/icons/virtualScanner.png
methodImage: img/publications/virtual_scanner_screenshot.png
doilink:
githublink: https://github.com/JelleKUL/VirtualScanner
objective: 1
type: Conference
description: We present a virtual 3D scanning framework developed in Unity for generating realistic synthetic datasets tailored to geometric reconstruction and completion tasks in the architecture, engineering, and construction (AEC) domain. While deep learning approaches for scene and object completion have advanced rapidly, acquiring large-scale, high-quality real-world scan data remains costly and time-intensive. Unlike conventional synthetic datasets created through direct surface sampling or repositories such as ShapeNet, our approach simulates physically plausible scanning processes from virtual viewpoints, faithfully modelling sensor limitations, noise characteristics, and occlusions.
---
# Synthetic Dataset Generation for Partially Observed Indoor Objects

**Jelle Vermandere¹, Maarten Bassier¹, Maarten Vergauwen¹**

¹ KU Leuven, Department of Civil Engineering, Ghent, Belgium (jelle.vermandere, maarten.bassier, maarten.vergauwen)@kuleuven.be

**Keywords:** virtual, scanning, pointcloud, Unity, synthetic

---

## Abstract

Learning-based methods for 3D scene reconstruction and object completion require large datasets containing partial scans paired with complete ground-truth geometry. However, acquiring such datasets using real-world scanning systems is costly and time-consuming, particularly when accurate ground truth for occluded regions is required.

In this work, we present a virtual scanning framework implemented in Unity for generating realistic synthetic 3D scan datasets. The proposed system simulates the behaviour of real-world scanners using configurable parameters such as scan resolution, measurement range, and distance-dependent noise. Instead of directly sampling mesh surfaces, the framework performs ray-based scanning from virtual viewpoints, enabling realistic modelling of sensor visibility and occlusion effects. In addition, panoramic images captured at the scanner location are used to assign colours to the resulting point clouds.

To support scalable dataset creation, the scanner is integrated with a procedural indoor scene generation pipeline that automatically produces diverse room layouts and furniture arrangements. Using this system, we introduce the V-Scan dataset, which contains synthetic indoor scans together with object-level partial point clouds, voxel-based occlusion grids, and complete ground-truth geometry. The resulting dataset provides valuable supervision for training and evaluating learning-based methods for scene reconstruction and object completion.

## 1. Introduction

Predicting missing geometry in partially scanned 3D environments is essential for many applications in the architecture, engineering and construction (AEC) industry. Accurate reconstruction of occluded regions supports tasks such as digital twin generation, progress monitoring, and structural analysis. In recent years, deep learning based scene- and object-completion networks have made significant progress in recovering missing geometry from partial observations (Vermandere et al., 2025). However, training such models requires large amounts of high-quality data containing both partial scans and corresponding ground-truth geometry.

Acquiring such datasets from real-world scans remains time-consuming and expensive. Capturing complete ground-truth geometry typically requires multiple scanning positions or carefully controlled acquisition environments, which limits scalability. As a result, many existing real-world datasets provide only partial observations without accurate ground-truth information for occluded regions, restricting their usefulness for supervised geometric completion tasks.

Synthetic datasets offer a promising alternative. A common approach is to directly sample points on the surfaces of 3D meshes (De Geyter et al., 2022). While efficient, this method produces unrealistic point distributions and often includes surfaces that would be invisible to a real scanner. Consequently, these datasets fail to capture important physical constraints of real scanning systems, such as visibility, range limitations, and measurement noise.

To address these challenges, we propose a virtual scanning framework implemented in Unity that simulates the behaviour of real-world scanning devices. Instead of directly sampling mesh surfaces, the system performs ray-based scanning from configurable virtual viewpoints, allowing realistic modelling of sensor characteristics such as measurement range, angular resolution, occlusion, and distance-dependent noise. In addition, panoramic imagery captured at the scanner location is used to assign colours to the scanned points, producing visually consistent point clouds that reflect the lighting conditions of the virtual environment.

To support large-scale dataset generation, the framework is integrated with a procedural scene generation system that automatically creates indoor layouts and populates them with furniture objects. Each generated scene can be scanned automatically using the virtual scanner, producing realistic partial observations of objects and environments. Furthermore, voxel-based occlusion grids are computed at both scene and object level, enabling explicit representation of visibility within the scanned environment.

Using this pipeline, we introduce the V-Scan dataset, a synthetic dataset of indoor scans containing both partial observations and complete ground-truth geometry. Each scene includes a furnished scan, an empty scene scan providing ground truth geometry, object-level partial point clouds, and voxel-based occlusion annotations.

The main contributions of this work are:

- **Virtual Scanner for Unity:** a plug-and-play virtual scanner that simulates real-world scanning behaviour and can be integrated into arbitrary Unity scenes.
- **Procedural Scene Generation:** an automated pipeline for generating indoor layouts and populating them with diverse furniture configurations.
- **Occlusion Voxel Grids:** computation of full-scene and per-object occlusion grids that explicitly represent visibility within the scanned environment.
- **Synthetic Dataset:** V-Scan, a dataset of partially scanned indoor scenes and objects paired with complete ground-truth geometry.

## 2. Background and Related Work

### 2.1 Full-Scene Indoor 3D Scan Datasets

Large-scale indoor 3D datasets have been instrumental in advancing research in scene understanding and reconstruction. Early efforts include SUN RGB-D (Song et al., 2015), which provides over 10,000 RGB-D images with 2D and 3D annotations. Matterport3D (Chang et al., 2017) and ScanNet (Dai et al., 2017) capture detailed indoor scenes using handheld or structured RGB-D scanning pipelines, while Stanford 2D-3D-S (Armeni et al., 2017) provides multi-building semantic annotations. More recent datasets, such as ARKitScenes (Baruch et al., 2022) and ScanNet++ (Yeshwanth et al., 2023), extend these efforts with mobile LiDAR acquisition and improved scene geometry. Despite their scale, most real-world datasets provide only observed geometry, leaving occluded regions unscanned.

### 2.2 Single-Object 3D Scans

Several works focus on high-quality 3D scans of individual objects. The Redwood Object Scan Dataset (Choi et al., 2016) provides over 10,000 real-world object scans, while Pix3D (Sun et al., 2018) pairs images with aligned 3D models. More recent datasets include Google Scanned Objects (Downs et al., 2022) and OmniObject3D (Wu et al., 2023), containing 6,000 real-scanned objects across 190 categories.

### 2.3 Synthetic 3D Scene and Object Datasets

Synthetic datasets address limitations of real-world capture by providing complete geometry and dense annotations. Scene-level synthetic datasets include SUNCG (Song et al., 2016), SceneNet RGB-D (McCormac et al., n.d.), InteriorNet (Li et al., 2018), and the Replica dataset (Straub et al., 2019). At the object level, repositories such as ShapeNet (Chang et al., 2015), ModelNet (Wu et al., 2015), and Amazon Berkeley Objects (ABO) (Collins et al., 2022) provide extensive collections of 3D CAD models or product scans.

### 2.4 Real-World 3D Scanning Systems

Real-world 3D acquisition systems vary widely in accuracy, range, and mobility.

**Table 1.** Comparison of representative 3D scanning devices.

|Device|Type|Range|δ_r|Scan Rate|
|---|---|---|---|---|
|Leica P30|TLS|up to 270 m|1.2mm + 10ppm|1MHz|
|NavVis VLX|Mobile SLAM LiDAR|up to 50 m|6mm/500m²|1.28MHz|
|Leica BLK360|Compact terrestrial LiDAR|up to 45 m|4mm@10m|0.68MHz|
|iPhone Pro LiDAR|Mobile ToF LiDAR|~5 m|~1–2cm|N/A (depth sensing)|

### 2.5 Virtual Scanning and LiDAR Simulation

Recent work has explored virtual scanning frameworks to simulate the acquisition process. Early efforts, such as the laser scanner simulator by Blume and Heimann (2006), provide efficient real-time range measurements. Popovas et al. (2021) developed a virtual scanning system for training purposes, while HELIOS++ (Winiwarter et al., 2022) focuses on aerial LiDAR simulation for terrain mapping. While these frameworks provide accurate sensor simulation, they are typically designed for surveying or robotics applications rather than scalable dataset generation for learning-based reconstruction tasks.

**Table 2.** Comparison of virtual scanning frameworks.

|Framework|Indoor Scenes|Object-level GT|
|---|---|---|
|Blume et al.|Yes|No|
|HELIOS++|Limited|No|
|Popovas et al.|Yes|No|
|V-Scan (ours)|Yes|Yes|

### 2.6 Occlusion Modelling and Visibility Analysis

Explicit modelling of occlusions is crucial for reconstruction, completion, and robotic perception tasks. Zhu et al. (2020) demonstrate how integrating occlusion awareness enhances perception and task performance. Incorporating occlusion modelling into virtual scanning frameworks enables the creation of realistic partial scans with ground-truth visibility.

### 2.7 Procedural Room Generation

Procedural and generative methods have become the standard for creating large amounts of variable data efficiently. Infinigen Indoors (Raistrick et al., n.d.) extends a Blender-based procedural framework to photorealistic indoor scenes. RoomPilot (Chen et al., 2025) introduces a multi-modal Indoor Domain-Specific Language (IDSL) that translates textual descriptions or CAD floor plans into structured scene layouts. DiffuScene (Tang et al., 2024) employs a de-noising diffusion model over unordered 3D object attributes, enabling the generation of diverse and physically plausible indoor scenes.

## 3. Virtual Scanner

Our goal is to generate realistic 3D scans of virtual environments that emulate physical scanner behaviour. This is performed by first determining the scan vectors and then ray-casting against the virtual scene. The system is also able to create 3D coloured point clouds of the whole scene, isolate all the non-static objects and calculate the per-object scene occlusion as seen in Figure 1.

### 3.1 Virtual Scanner Setup

The virtual scanner simulates real-world scanning devices using configurable parameters: scan density (mm per 10 m), maximum range (m), vertical field-of-view (degrees), system error (mm), and distance-dependent error (%). Presets emulate commercial devices such as the Leica P30, NavVis VLX, BLK360, and iPhone Pro LiDAR. These parameters determine scan resolution, coverage, and measurement precision (Figure 2).

### 3.2 Scan Vector Generation

Scanning begins by generating a spherical array of vectors representing individual rays. Vectors are distributed uniformly along horizontal disks stacked vertically, starting from a minimum angle to simulate the blind zone beneath the sensor (Figure 3). Equal angular spacing in both vertical and horizontal directions ensures uniform coverage and allows efficient handling of polar regions, while maintaining computational efficiency compared to sequential vertical sweeps.

### 3.3 Ray-casting and Point Cloud Acquisition

Each vector is cast from the scanner origin until it intersects a scene object. Intersection points are recorded with their 3D position and surface normal. Gaussian noise is applied to simulate system error, and a distance-dependent error is included to approximate real measurement inaccuracies. Points beyond the maximum range are discarded. The result is a partial, noise-affected point cloud representing the observed scene geometry.

### 3.4 Point Colouring via Panoramic Capture

To capture realistic appearance, a 360° panoramic image is generated at the scanner's position using a cubemap converted to an equirectangular projection (Figure 4). Each scanned point is assigned a colour by mapping its angular coordinates to the corresponding pixel in the panorama. The resulting coloured point cloud preserves lighting, shading, and material information from the virtual environment, and can be exported with positions, normals, and colours for downstream tasks.

### 3.5 Object-Level Scanning

Each non-structural object in the scene is assigned a ScannedObject script, enabling isolation of points within its oriented bounding box (OBB). The OBB can be slightly expanded to ensure points near the edges are included. This facilitates exporting per-object partial scans along with bounding boxes defined by the eight 3D corner points.

### 3.6 Occlusion Computation

Occlusion is computed both at the object and scene level using a voxel-based approach. A cubic voxel grid with a set resolution is defined by encapsulating the entire OBB, and rays are cast from the scanner origin to each voxel centre. Voxels intersected by other geometry are marked as occluded. The resulting grids are exported as sparse voxel representations (Figure 5), providing ground-truth visibility information for each object as well as for the entire scene.

### 3.7 Empty Scene Scanning and Export

To generate complete ground-truth geometry of the empty scene, the scene is rescanned with all non-structural objects removed (Figure 6). This produces a full empty-scene point cloud and panorama, which can be exported alongside the separate object scans. The output of the pipeline includes:

- Full scene scans (with and without objects), including colours and normals.
- Equirectangular panoramic images (with and without objects).
- Full-scene voxel occlusion grids.
- Per-object partial point clouds, oriented bounding boxes, occlusion grids, and ground-truth meshes.

## 4. Dataset

The V-Scan dataset consists of synthetic indoor scenes scanned using the virtual scanning framework. The dataset is generated fully procedurally in Unity, enabling large numbers of diverse scenes to be produced automatically. Each scene contains structural elements such as floors, walls, and ceilings, as well as non-static objects such as furniture and other technical equipment. The dataset is structured to contain ground truth, both per object and per scene.

### 4.1 Procedural Layout Generation

Indoor scenes are generated using a procedural room generation algorithm implemented in Unity. The layout generation begins by sampling the room dimensions from a predefined range, producing rectangular floor plans with widths and lengths between configurable minimum and maximum values. The room is discretized using a regular grid, where each grid cell corresponds to a floor tile of fixed size.

After generating the floor grid, the structural boundaries of the room are constructed by placing walls along the outer edges of the floor layout. For each wall segment, the algorithm probabilistically decides whether to place a standard wall element, a window, or a door based on predefined probabilities. To ensure scene accessibility, at least one doorway is always placed in the generated layout.

Following the wall placement, ceiling elements are instantiated above each floor tile at a fixed height, completing the basic architectural structure of the room. Furniture objects are then added in two stages. First, wall-mounted furniture elements such as shelves or cabinets may be placed along interior walls with a configurable probability. Second, free-standing furniture is placed within the interior of the room. Candidate furniture positions are sampled randomly within the room bounds while ensuring that objects do not intersect with previously placed items. This is achieved using bounding box intersection tests, which enforce collision-free placement.

Each random function is called from a single seed. It is possible to predefine this seed, making the whole generation deterministic. This can be useful in case the scene needs to be regenerated at a later stage.

### 4.2 Asset Styles

To enable the generation of diverse indoor environments, the procedural generation system uses configurable asset collections implemented through Unity ScriptableObjects. Each asset configuration defines a consistent visual style for a room and specifies the prefabricated models used for structural elements and furniture.

A style definition contains references to prefabs for floors, walls, ceilings, doors, and windows, as well as collections of furniture objects that can be placed either against walls or within the interior of the room. This modular design allows different architectural styles or furniture themes to be generated simply by switching the corresponding asset configuration.

### 4.3 Automatic Scan Configuration

After a room layout has been generated, the scanning system is automatically configured to capture the scene. The scanning region is determined by computing the bounding box of the generated room. The virtual scanner is then positioned at the centre of this bounding box and configured to capture the entire scene. This automated setup ensures that each generated room is scanned under consistent conditions without manual intervention.

### 4.4 Dataset Metrics

The dataset consists of approximately 100 furnished virtual scans, all including objective ground truth for both the objects and the scene. Figure 7 shows a sample of 2 different environment styles.

## 5. Conclusion

This paper presented a virtual scanning framework for generating synthetic 3D point cloud datasets of indoor environments. The proposed system simulates realistic scanning behaviour using configurable parameters derived from real-world scanning devices, enabling the generation of point clouds that include realistic measurement noise, visibility constraints, and occlusions. By integrating the scanner simulation within a procedural scene generation pipeline, the framework enables efficient creation of diverse indoor scenes that can be automatically scanned without manual intervention.

Using this system, we introduced the V-Scan dataset, which contains synthetic indoor scans together with rich ground-truth information. The dataset includes furnished and empty scene scans, object-level partial point clouds, oriented bounding boxes, and voxel-based occlusion annotations. This combination of observed geometry and ground-truth information provides valuable supervision signals for tasks such as object completion, occlusion reasoning, and scene reconstruction.

The results demonstrate that virtual scanning can provide a scalable alternative to real-world data acquisition while maintaining realistic scanning characteristics. Future work will focus on extending the framework to support mobile scanning trajectories and increasing the diversity of procedurally generated environments. These extensions will further improve the realism and applicability of synthetic datasets for learning-based 3D perception methods.

## References

Abdel-Majeed, H. M., Shaker, I. F., Abdel-Wahab, A., Awad, A. A. D. I., 2024. Indoor mapping accuracy comparison between the apple devices' LiDAR sensor and terrestrial laser Scanner. _HBRC Journal_, 20(1), 915–931.

Armeni, I., Sax, S., Zamir, A. R., Savarese, S., 2017. Joint 2D-3D-Semantic Data for Indoor Scene Understanding. arXiv:1702.01105.

Baruch, G., Chen, Z., Dehghan, A., Dimry, T., Feigin, Y., Fu, P., Gebauer, T., Joffe, B., Kurz, D., Schwartz, A., Shulman, E., 2022. ARKitScenes: A Diverse Real-World Dataset For 3D Indoor Scene Understanding Using Mobile RGB-D Data. arXiv:2111.08897.

Blume, H., Heimann, B., 2006. SIMULATING A LASER RANGE SCANNER UNDER APPLICATION CONDITIONS. _IFAC Proceedings Volumes_, 39(15), 419–424.

Chang, A., Dai, A., Funkhouser, T., Halber, M., Niessner, M., Savva, M., Song, S., Zeng, A., Zhang, Y., 2017. Matterport3D: Learning from RGB-D Data in Indoor Environments. _International Conference on 3D Vision (3DV)_.

Chang, A. X., Funkhouser, T., Guibas, L., Hanrahan, P., Huang, Q., Li, Z., Savarese, S., Savva, M., Song, S., Su, H., Xiao, J., Yi, L., Yu, F., 2015. ShapeNet: An Information-Rich 3D Model Repository. Technical report, Stanford University — Princeton University — Toyota Technological Institute at Chicago. arXiv:1512.03012.

Chen, W., Zhang, S., Zhang, Y., Zhou, T., Li, R., 2025. RoomPilot: Controllable Synthesis of Interactive Indoor Environments via Multimodal Semantic Parsing. arXiv:2512.11234.

Choi, S., Zhou, Q.-Y., Miller, S., Koltun, V., 2016. A Large Dataset of Object Scans. arXiv:1602.02481.

Collins, J., Goel, S., Deng, K., Luthra, A., Xu, L., Gundogdu, E., Zhang, X., Vicente, T. F. Y., Dideriksen, T., Arora, H., Guillaumin, M., Malik, J., 2022. ABO: Dataset and Benchmarks for Real-World 3D Object Understanding. _2022 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)_, IEEE, New Orleans, LA, USA, 21094–21104.

Dai, A., Chang, A. X., Savva, M., Halber, M., Funkhouser, T., Nießner, M., 2017. ScanNet: Richly-annotated 3D Reconstructions of Indoor Scenes. arXiv:1702.04405.

De Geyter, S., Bassier, M., Vergauwen, M., 2022. AUTOMATED TRAINING DATA CREATION FOR SEMANTIC SEGMENTATION OF 3D POINT CLOUDS. _The International Archives of the Photogrammetry, Remote Sensing and Spatial Information Sciences_, XLVI-5/W1-2022, 59–67.

Downs, L., Francis, A., Koenig, N., Kinman, B., Hickman, R., Reymann, K., McHugh, T. B., Vanhoucke, V., 2022. Google Scanned Objects: A High-Quality Dataset of 3D Scanned Household Items. arXiv:2204.11918.

Leica BLK360 | 3D Laser Scanner, n.d.

Leica ScanStation P40 / P30 - High-Definition 3D Laser Scanning Solution | Leica Geosystems, n.d.

Li, W., Saeedi, S., McCormac, J., Clark, R., Tzoumanikas, D., Ye, Q., Huang, Y., Tang, R., Leutenegger, S., 2018. InteriorNet: Mega-scale Multi-sensor Photo-realistic Indoor Scenes Dataset. arXiv:1809.00716.

McCormac, J., Handa, A., Leutenegger, S., Davison, A. J., n.d. SceneNet RGB-D: 5M Photorealistic Images of Synthetic Indoor Trajectories with Ground Truth.

NavVis VLX 3 Specifications, n.d.

Popovas, D., Chizhova, M., Gorkovchuk, D., Gorkovchuk, J., Hess, M., Luhmann, T., 2021. VIRTUAL TERRESTRIAL LASER SCANNER SIMULATOR IN DIGITAL TWIN ENVIRONMENT. _Proceedings ARQUEOLÓGICA 2.0 - 9th International Congress & 3rd GEORES_, Editorial Universitat Politècnica de València.

Raistrick, A., Mei, L., Kayan, K., Yan, D., Zuo, Y., Han, B., Wen, H., Parakh, M., Alexandropoulos, S., Lipson, L., Ma, Z., Deng, J., n.d. Infinigen Indoors: Photorealistic Indoor Scenes using Procedural Generation.

Song, S., Lichtenberg, S. P., Xiao, J., 2015. SUN RGB-D: A RGB-D scene understanding benchmark suite. _2015 IEEE Conference on Computer Vision and Pattern Recognition (CVPR)_, IEEE, Boston, MA, USA, 567–576.

Song, S., Yu, F., Zeng, A., Chang, A. X., Savva, M., Funkhouser, T., 2016. Semantic Scene Completion from a Single Depth Image. arXiv:1611.08974.

Straub, J., Whelan, T., Ma, L., Chen, Y., Wijmans, E., Green, S., Engel, J. J., Mur-Artal, R., Ren, C., Verma, S., Clarkson, A., Yan, M., Budge, B., Yan, Y., Pan, X., Yon, J., Zou, Y., Leon, K., Carter, N., Briales, J., Gillingham, T., Mueggler, E., Pesqueira, L., Savva, M., Batra, D., Strasdat, H. M., Nardi, R. D., Goesele, M., Lovegrove, S., Newcombe, R., 2019. The Replica Dataset: A Digital Replica of Indoor Spaces. arXiv:1906.05797.

Sun, X., Wu, J., Zhang, X., Zhang, Z., Zhang, C., Xue, T., Tenenbaum, J. B., Freeman, W. T., 2018. Pix3D: Dataset and Methods for Single-Image 3D Shape Modeling. arXiv:1804.04610.

Tang, J., Nie, Y., Markhasin, L., Dai, A., Thies, J., Nießner, M., 2024. DiffuScene: Denoising Diffusion Models for Generative Indoor Scene Synthesis. arXiv:2303.14207.

Vermandere, J., Bassier, M., Vergauwen, M., 2025. Geometry and Texture Completion of Partially Scanned 3D Objects Through Material Segmentation. _Proceedings of the 20th International Joint Conference on Computer Vision, Imaging and Computer Graphics Theory and Applications_, SCITEPRESS, Porto, Portugal, 193–202.

Winiwarter, L., Esmorís Pena, A. M., Weiser, H., Anders, K., Martínez Sánchez, J., Searle, M., Höfle, B., 2022. Virtual laser scanning with HELIOS++: A novel take on ray tracing-based simulation of topographic full-waveform 3D laser scanning. _Remote Sensing of Environment_, 269, 112772.

Wu, T., Zhang, J., Fu, X., Wang, Y., Ren, J., Pan, L., Wu, W., Yang, L., Wang, J., Qian, C., Lin, D., Liu, Z., 2023. OmniObject3D: Large-Vocabulary 3D Object Dataset for Realistic Perception, Reconstruction and Generation. arXiv:2301.07525.

Wu, Z., Song, S., Khosla, A., Yu, F., Zhang, L., Tang, X., Xiao, J., 2015. 3D ShapeNets: A Deep Representation for Volumetric Shapes. arXiv:1406.5670.

Yeshwanth, C., Liu, Y.-C., Nießner, M., Dai, A., 2023. ScanNet++: A High-Fidelity Dataset of 3D Indoor Scenes. http://arxiv.org/abs/2308.11417.

Zhu, L., Menon, M., Santillo, M., Linkowski, G., 2020. Occlusion Handling for Industrial Robots. _2020 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)_, IEEE, Las Vegas, NV, USA, 10663–10668.
# Related Works

## Full scene indoor 3D Scan Datasets w/o GT

### Song et al. (2015) - SUN RGB-D Dataset
Introduced **SUN RGB-D**, a large-scale benchmark for indoor scene understanding built from RGB-D images captured with multiple depth sensors. The dataset contains over 10,000 images with dense annotations, including 2D polygons, 3D bounding boxes with object orientations, and room layout information, enabling training and evaluation of algorithms for indoor scene understanding using meaningful 3D metrics.
### Chang et al. (2017) - Matterport3D
Presented **Matterport3D**, a large-scale dataset of indoor environments captured using RGB-D cameras in a structured scanning pipeline. It provides highly detailed indoor reconstructions and panoramic views, enabling research in scene understanding, navigation, and 3D reconstruction.
###  Dai et al. (2017) - ScanNet
**ScanNet** is one of the most widely used indoor 3D datasets, consisting of RGB-D video sequences captured with handheld sensors and reconstructed into 3D scenes. It contains **2.5 million frames across 1,513 scans** with camera poses, surface reconstructions, and semantic annotations used for scene understanding and reconstruction tasks.
### Armeni et al. (2017) - Stanford 2D-3D-S
The **Stanford 2D-3D-Semantic dataset** contains large-scale scans of university buildings with detailed semantic annotations. It includes RGB images, depth maps, surface normals, and 3D meshes across multiple buildings covering over **6000 m² of indoor space**.
### Apple (2021) - ARKitScenes
A dataset of indoor scans captured using **mobile devices equipped with LiDAR sensors**. It provides RGB-D sequences and reconstructed scenes designed for augmented reality applications and mobile 3D scene understanding.
### Yeshwanth et al. (2023) - ScanNet++
Introduced **ScanNet++**, an updated large-scale indoor dataset that extends previous RGB-D scanning efforts with higher-quality geometry and improved capture pipelines. The dataset provides richly annotated indoor scenes for tasks such as semantic segmentation, scene reconstruction, and 3D perception.
## Single object 3D scans

### Choi et al. (2016) — Redwood Object Scan Dataset
Introduced the **Redwood Object Scan Dataset**, a collection of over 10,000 real-world 3D object scans captured using consumer-grade mobile scanning setups. The dataset was created by recruiting non-expert operators to scan objects in natural environments, resulting in a diverse set of object geometries useful for evaluating 3D reconstruction and object scanning methods.
### Sun et al. (2018) — Pix3D
Introduced **Pix3D**, a dataset of real-world images paired with aligned 3D models of individual objects, primarily furniture. It enables research on single-view 3D reconstruction and pose estimation by providing accurate image-to-model correspondences for over 10,000 images and 395 object shapes.
### Downs et al. (2022) — Google Scanned Objects
Introduced **Google Scanned Objects**, a dataset of over 1,000 high-quality 3D scans of household items, released under a Creative Commons license. The models are preprocessed for robotics simulators like Ignition Gazebo and Bullet, supporting research in interactive simulation, synthetic perception, and robotic learning with diverse, photo-realistic objects.
### Wu et al. (2023) — OmniObject3D
Introduced **OmniObject3D**, a large-scale real-scanned 3D object dataset with **6,000 objects across 190 everyday categories**. Each object includes textured meshes, point clouds, multiview images, and real-captured videos, supporting research in 3D perception, novel-view synthesis, neural surface reconstruction, and 3D object generation with high-quality, realistic scans.


---
## Synthetic 3D Scene Datasets
### Song et al. (2017) — Semantic Scene Completion Network (SSCNet) / SUNCG
Proposed **SSCNet**, an end-to-end 3D convolutional network that takes a single depth image and simultaneously predicts **volumetric occupancy and semantic labels** for all voxels in the camera frustum. To train the network, they introduced **SUNCG**, a large-scale synthetic indoor scene dataset with dense volumetric annotations, enabling joint learning of scene completion and semantic labeling from synthetic data.
### McCormac et al. (2017) — SceneNet RGB-D
Presented **SceneNet RGB-D**, a dataset of millions of synthetic RGB-D frames generated from procedurally created indoor environments. Each frame includes perfect depth, camera poses, and object annotations, enabling training of models for SLAM, semantic segmentation, and depth estimation.
### Li et al. (2018) — InteriorNet
Developed **InteriorNet**, a synthetic dataset with high-fidelity indoor scenes rendered from CAD furniture and realistic room layouts. It provides millions of RGB-D frames, motion trajectories, and photorealistic lighting, supporting robotics perception, navigation, and dynamic scene understanding.
### Straub et al. (2019) — Replica Dataset
Introduced **Replica**, a dataset of 18 highly photo-realistic 3D indoor scene reconstructions at room and building scale. Each scene includes dense meshes, high-resolution HDR textures, per-primitive semantic and instance labels, and reflective surfaces, enabling research in 3D perception, semantic segmentation, geometric inference, and training embodied agents in realistic simulated environments. (very detailed reconstruction, nut no occlusion infilling inbetween objects)
## Synthetic 3D Object Datasets

### Chang et al. (2015) — ShapeNet
Proposed **ShapeNet**, a large-scale repository of annotated 3D CAD models covering a wide range of object categories. It is widely used for generating synthetic datasets by sampling surface points from meshes, enabling research in shape analysis, reconstruction, and deep learning on 3D data.
### ModelNet (3D ShapeNets) (Wu et al., 2015)
Introduced **ModelNet**, a large-scale collection of 3D CAD models of common objects, organized into 10 (ModelNet10) or 40 (ModelNet40) categories. The dataset provides standardized splits for tasks like 3D object classification, retrieval, and shape analysis, making it a widely used benchmark in deep learning for 3D data.
### Wang et al. (2020) — Amazon Berkeley Objects (ABO) Dataset
Introduced the **ABO dataset**, a large-scale collection of real-world product 3D models with high-quality geometry, textures, and metadata. It includes over **8,000 object models** spanning consumer products, enabling research in single-object reconstruction, rendering, and vision-to-3D learning tasks.

---

## Virtual Scanning and LiDAR Simulation
### Blume & Heimann (2006) — Simulating a Laser Range Scanner Under Application Conditions
Presented a **virtual laser range scanner simulator** for wheeled mobile robots, designed to emulate real-time range measurements in grid-based artificial environments. The simulator allows development and testing of navigation and obstacle avoidance methods, efficiently modeling sensor behavior with low computational cost, and interfaces identically to a real scanner for seamless method validation.
### Popovas et al. (2021) — Virtual Laser Scanning System

Developed a **virtual laser scanning framework** intended primarily for training and educational purposes. The system simulates terrestrial laser scanning equipment in a virtual environment, allowing users to practice scanning procedures and understand sensor behavior.

### Winiwarter et al. (2022) — Virtual LiDAR for Aerial Scanning

Introduced a **virtual scanning framework for aerial LiDAR simulation**, focusing on modelling airborne scanning workflows. The system aims to reproduce realistic flight paths and scanning behavior for applications such as forestry analysis and terrain mapping.

---

## Occlusion Modelling and Visibility Analysis

### Zhu et al. (2020) — Occlusion-Aware Perception in Robotics

Investigated **occlusion reasoning methods for robotic perception**, focusing on identifying regions that are not visible from a sensor viewpoint. The work demonstrates how explicit modelling of occlusions improves perception and planning in cluttered environments.