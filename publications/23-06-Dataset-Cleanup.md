---
layout: publication
title: "Object detection and removal in 3D scenes with geometry and texture reconstruction"
date: 2023-06-01
permalink: /publications/object-detection-and-removal/
icon: "/img/icons/Dataset Cleanup.png"
methodImage : "img/publications/cleanupSchema.png"
doilink: "https://kuleuven.limo.libis.be/discovery/fulldisplay?docid=alma9993527097301488&context=L&vid=32KUL_KUL:KULeuven&search_scope=All_Content&tab=all_content_tab&lang=en"
githublink: "https://github.com/JelleKUL/dataset-cleanup"
objective: 2
type: Thesis

description: A thesis project researching the technologies and applications behind automatically completing different types of datasets.

---
# Object Detection and Removal in 3D Scenes with Geometry and Texture Reconstruction

  

**Ruben Gillijns, Geert Meeremans**

  

Supervisor: prof. dr. ir. Maarten Vergauwen

Co-supervisor: ir. arch. Jelle Vermandere

  

KU Leuven, Faculty of Industrial Engineering, Technology Campus Ghent, Belgium

  

**Keywords:** 3D mesh, object detection, object removal, structure reconstruction, texture reconstruction

  

---

  

## Abstract

  

Indoor scanning is a fast-growing industry predicted to increase its market share in the coming years. Although indoor scanning has a wide variety of applications, this research focuses on engineering purposes, where indoor scans aid design, identify structural problems, and plan construction projects. While the intended purpose often requires only the geometry of the space, scanned spaces typically contain redundant objects such as furniture. This thesis presents a pipeline to detect and remove objects from 3D mesh datasets and subsequently reconstruct both the geometry and texture of the room, preserving only static elements. The mesh representation is chosen for its organized structure and support for texture operations. Object detection is performed using LabelCloud through manually placed bounding boxes. Object removal leverages PyVista's clipping functions, after which holes are filled using PyMeshFix for three structural scenarios: planar surfaces, two-plane junctions, and three-plane corners. Texture reconstruction is achieved by masking removed object regions in the UV-mapped texture and applying OpenCV's inpainting algorithm. Despite a well-developed method, several challenges complicate full automation, including edge errors from clipping operations, limitations to three geometric scenarios, and loss of UV coordinates during mesh editing. Nevertheless, this pipeline provides a strong foundation for future research in automated indoor scene cleaning.

  

## 1. Introduction

  

Indoor scanning is a fast-growing industry. The global 3D laser scanner market, valued at $3.1 billion in 2022, is expected to reach $5.2 billion by 2030, growing at a compound annual growth rate (CAGR) of 6.9% [1]. Indoor scanning has become indispensable in applications such as scan-to-BIM, interior construction, and renovation planning. For engineering purposes, indoor scanning enables engineers to quickly and accurately capture and visualize building geometry, supporting applications such as renovation design, structural problem identification, and construction project planning.

  

Interior scans of existing, occupied buildings inevitably capture furniture and other objects. While these objects may be valuable for some purposes, engineering applications typically require only the static geometry of the room — walls, floors, ceilings, doors, and similar elements. To date, no fully automated process exists that detects objects in a 3D dataset, removes them, and reconstructs both the geometry and texture while respecting the underlying room structure.

  

This research brings together four sub-processes into a unified pipeline: (1) detecting objects in a 3D dataset, (2) removing detected objects, (3) filling resulting gaps while respecting the geometry, and (4) reconstructing texture on the new geometry of filled holes.

  

## 2. Related Work

  

### 2.1 3D Data Representations

  

Raw 3D data can be represented in several ways, each with distinct advantages. Meshes provide organized and structured representations with support for texture assignment through UV coordinates and are chosen for this research. Point clouds offer simpler representations but lack connectivity information. Voxel grids provide efficient storage but lower resolution. RGB-D images are efficient but lack geometric information. Signed Distance Functions (SDFs) use mathematical equations but require more computational power for complex objects [7,17].

  

Meshes consist of vertices, edges, and faces describing object shapes. Each vertex contains 3D coordinates, a normal vector, and either color values or texture coordinates. Texture coordinates (UVs) are two-dimensional coordinates linking pixel positions in a texture image to mesh vertices, enabling projection of 2D images onto 3D surfaces [22].

  

### 2.2 3D Object Detection

  

3D object detection focuses on locating and recognizing objects in scenes by producing oriented bounding boxes. Detection methods can be categorized by input type: point-based (e.g., PointNet [26], PointNet++ [29]), mesh-based (e.g., Mesh R-CNN [31]), and voxel-based (e.g., VoxelNet [32]). VoteNet [25] uses deep Hough voting on point clouds and offers a pre-trained model on ScanNet and SUN RGB-D data. Key challenges include occlusion, scale variation, data sparsity, and complex geometry [10].

  

LabelCloud [3] offers an alternative, domain-independent approach for manually placing rotated bounding boxes on 3D point clouds. Unlike neural network-based methods, it provides a user-friendly interface supporting multiple data formats and sensor types, making it suitable for research prototyping.

  

### 2.3 Hole Filling in Meshes

  

Hole filling methods for meshes are divided into surface-based and volume-based approaches [13]. Surface-based methods (e.g., triangulation) work well for simple holes but struggle with complex topologies. Volume-based methods handle more complex holes including islands and self-intersections but may alter original edge and vertex connectivity.

  

Guo et al. [13] evaluated five hole filling methods on various test models, finding that methods struggle with real-world data due to increased vertex and face counts and that curvature and difficult topology remain challenging. RameshCleaner [15] emerged as one of the better-performing methods, addressing common mesh defects including spikes, foldovers, microtunnels, boundary issues, and small components. PyMeshFix [5], based on Attene's MeshFix [40], provides automatic repair by performing topology reconstruction followed by geometry correction, producing watertight manifold meshes.

  

### 2.4 Texture Inpainting

  

For reconstructing texture in regions where objects have been removed, inpainting techniques fill masked regions using information from adjacent areas [43]. OpenCV provides two algorithms: Telea's Fast Marching Method (FMM) [44], which progressively fills from edges inward using weighted sums of known neighboring pixels, and a Navier-Stokes-based method [45] using partial differential equations from fluid dynamics. Patch-based methods [47,48] synthesize textures from input samples but require training or produce artifacts.

  

## 3. Methodology

  

The pipeline consists of four sequential stages: object detection, object removal, hole filling, and texture reconstruction (Figure 3.1).

  

### 3.1 Object Detection

  

After evaluating VoteNet [25] and LabelCloud [3], LabelCloud was selected for its ease of use in creating and manipulating bounding boxes during the research process. The mesh is first sampled to a point cloud and loaded into LabelCloud, where bounding boxes are manually placed around objects. Each bounding box stores 10 parameters: object class, center position (x, y, z), dimensions (length, width, height), and rotation (roll, pitch, yaw). Parameters are exported in JSON format for use in subsequent processing steps.

  

### 3.2 Object Removal

  

Object removal uses PyVista's clip function in a two-stage process. For each detected object:

  

1. A bounding box enlarged by distance D_s is clipped from the scene, extracting a region surrounding the object.

2. The original-sized bounding box is clipped from this enlarged region, isolating the edge geometry around the object.

  

This two-stage approach preserves the border region around each object, which serves as input for the hole filling algorithm. The edge contains information about the surrounding room geometry (planes, corners) needed to properly reconstruct the missing surfaces.

  

### 3.3 Hole Filling

  

Hole filling is the core contribution of this work. The algorithm handles three geometric scenarios determined by running a RANSAC plane segmentation algorithm [51] on the edge geometry:

  

**Scenario 1 — Single plane.** When only one plane is detected (e.g., a hole in a wall), the edge is directly passed to PyMeshFix for filling. The algorithm triangulates the boundary and fills the hole while respecting the planar geometry.

  

**Scenario 2 — Two-plane junction.** When two planes are detected (e.g., a floor-wall connection), additional geometry must be created before hole filling. The intersection line between the two planes is computed. Six vertices closest to this intersection line are identified (three per plane), and four bridging faces are generated between them. This additional geometry splits the hole into two sub-holes, each lying predominantly in one plane. PyMeshFix then fills each sub-hole separately.

  

**Scenario 3 — Three-plane corner.** When three planes are detected (e.g., a floor-wall-wall corner), the process extends to find three pairwise intersection lines and their common intersection point. For each pair of planes, vertices near the intersection lines are identified, and bridging geometry is created. Additional "star" geometry connects the intersection point to nearby vertices. This splits the complex hole into three simpler sub-holes that PyMeshFix can fill individually.

  

After filling, the new geometry is merged back into the clipped scene to produce the repaired dataset.

  

### 3.4 Texture Reconstruction

  

Texture reconstruction proceeds in three steps:

  

1. **Re-texturing the repaired mesh.** During mesh editing with PyVista, UV coordinates linking the texture to the mesh are lost. As automated UV reassignment proved too complex within the project scope, the texture from the original mesh is baked onto the repaired mesh using Blender's bake function. This projects the original texture onto the new geometry, producing incorrect texture at former object locations but correct texture elsewhere.

  

2. **Masking object regions.** Areas in the texture image corresponding to former object locations are identified and masked in white.

  

3. **Inpainting.** OpenCV's Telea inpainting algorithm [44] fills the masked regions using surrounding texture information. Three inpainting methods were compared: OpenCV Telea (FMM-based), OpenCV Navier-Stokes, and patch-based inpainting. The Telea method provided the best balance of quality and simplicity for this application.

  

## 4. Experiments and Results

  

### 4.1 Dataset Selection

  

Two datasets were evaluated. The ScanNet dataset proved too densely furnished — removing all objects left insufficient geometry for reconstruction (Figure 4.2). The Matterport3D dataset [12] provided a better-suited office room containing moderate furniture density with all three geometric scenarios represented: planar surfaces, floor-wall connections, and floor-wall-wall corners.

  

The selected room (dataset 5ZKStnWn8Zo) contained 6,450 cells and 19,350 points. The dataset was manually extracted from the full building scan and prepared in Blender to ensure watertight mesh properties required by the algorithms.

  

### 4.2 Object Removal Results

  

Objects can be removed individually or as selections. Removing all objects simultaneously from densely furnished rooms creates very large holes that are difficult to fill. A selective approach, removing objects iteratively, produces better results but increases processing time.

  

A key limitation arises from PyVista's clip function: new vertices generated along cut edges are not properly connected to neighboring edges, creating non-manifold geometry that prevents PyMeshFix from closing holes. This requires manual intervention in Blender to merge vertices — a significant impediment to full automation.

  

### 4.3 Hole Filling Results

  

The algorithm successfully fills holes for all three scenarios:

  

- **Planar holes** (e.g., paintings on walls) are filled correctly with geometry matching the surrounding surface.

- **Two-plane junction holes** (e.g., furniture against a wall on a floor) are correctly split and filled, preserving the wall-floor geometry.

- **Three-plane corner holes** (e.g., furniture in a room corner) are properly reconstructed with the corner geometry preserved.

  

However, several challenges were identified:

  

**Proximity of objects.** When objects are closer than distance D_s to each other, parts of neighboring objects contaminate the edge geometry, causing incorrect plane detection by RANSAC and faulty reconstruction. Solutions include reducing D_s (at the cost of less geometric context), manual edge cleanup, or iterative removal starting with the most isolated objects.

  

**Double faces from PyMeshFix.** The filling algorithm sometimes creates faces extending beyond the inner boundary to the outer boundary of the edge, producing overlapping geometry. This prevents correct UV unwrapping and causes errors in subsequent texture reconstruction.

  

**Limited scenarios.** Only three geometric configurations are handled. Anomalous cases (curved surfaces, complex architectural features) cause the algorithm to fail and require manual intervention.

  

### 4.4 Texture Reconstruction Results

  

After manually removing duplicate faces and vertices in Blender, the UV unwrapping and texture baking produce correct results. The inpainting algorithm successfully reconstructs texture for removed objects including cabinets, seats, desks, and wall paintings. Results are best when masked areas are relatively small and surrounding textures are uniform. Larger masked areas produce less accurate inpainting with visible artifacts.

  

The fully cleaned scene (Figure 4.31) demonstrates that the pipeline can produce a room dataset containing only static elements with reconstructed geometry and texture, though manual corrections remain necessary at several stages.

  

## 5. Conclusion

  

This thesis presents a pipeline for detecting and removing objects from 3D scanned indoor environments with subsequent geometry and texture reconstruction. The pipeline integrates LabelCloud for object detection, PyVista for object removal, a custom RANSAC-based algorithm with PyMeshFix for hole filling across three geometric scenarios, and OpenCV's Telea inpainting for texture reconstruction.

  

The approach successfully removes objects and reconstructs room geometry for planar surfaces, two-plane junctions, and three-plane corners. The texture inpainting produces reasonable results for moderate-sized masked regions. However, several limitations prevent full automation: PyVista's clip function generates non-manifold vertices requiring manual repair, UV coordinates are lost during mesh editing necessitating external texture baking, the hole filling is limited to three scenarios, and densely furnished rooms pose challenges due to object proximity effects.

  

### Future Work

  

Several improvements could advance this pipeline toward full automation. First, an alternative object removal method could determine which vertices lie inside bounding boxes and remove them with their adjacent faces, avoiding the edge intersection problems caused by PyVista's clip function. Second, the geometric scenarios for hole filling could be extended to handle curved surfaces, more complex junctions, and architectural features. Third, an automated process for reassigning UV coordinates to vertices after mesh editing would eliminate the dependency on Blender for texture baking. Finally, integrating automated object detection (e.g., VoteNet) would remove the manual bounding box placement step.

  

The code for this project is available at: https://github.com/JelleKUL/dataset-cleanup

  

## Acknowledgments

  

This project was supervised by prof. dr. ir. Maarten Vergauwen and co-supervised by ir. arch. Jelle Vermandere at the Department of Civil Engineering, TC Construction — Geomatics, KU Leuven, Technology Campus Ghent, Belgium.

  

## References

  

[1] R. and Markets, "3D Laser Scanners Global Market Report 2023," 2023.

  

[2] V. V. Lehtola, S. Nikoohemat, and A. Nüchter, *Indoor 3D: Overview on scanning and reconstruction methods*, 2021.

  

[3] C. Sager, P. Zschech, and N. Kühl, "labelCloud: A Lightweight Labeling Tool for Domain-Agnostic 3D Object Detection in Point Clouds," *Computer-Aided Design and Applications*, vol. 19, no. 6, pp. 1191–1206, 2022.

  

[4] PyVista, "Extract Edges."

  

[5] PyMeshFix, "Partial Fill Holes."

  

[7] A. Toisoul, "3D Data representations," 2021.

  

[8] C.-K. Shene, "Mesh Basics," pp. 1–45, 2010.

  

[9] W. Zhao, S. Gao, and H. Lin, "A robust hole-filling algorithm for triangular mesh," *Visual Computer*, vol. 23, no. 12, pp. 987–997, 2007.

  

[10] Y. Guo, H. Wang, Q. Hu, H. Liu, L. Liu, and M. Bennamoun, "Deep Learning for 3D Point Clouds: A Survey," *IEEE Transactions on Pattern Analysis and Machine Intelligence*, vol. 43, no. 12, pp. 4338–4364, 2021.

  

[13] X. Guo, J. Xiao, and Y. Wang, "A survey on algorithms of hole filling in 3D surface reconstruction," *Visual Computer*, vol. 34, no. 1, pp. 93–103, 2018.

  

[14] Y. Jun, "A piecewise hole filling algorithm in reverse engineering," *CAD Computer Aided Design*, vol. 37, no. 2, pp. 263–270, 2005.

  

[15] M. Centin and A. Signoroni, "Rameshcleaner: Conservative fixing of triangular meshes," *Italian Chapter Conference 2015 - Smart Tools and Apps in Computer Graphics, STAG 2015*, pp. 129–138, 2015.

  

[17] Alan Zucconi, "Volumetric Rendering: Signed Distance Functions," 2016.

  

[22] B. Pluralsight, "Understanding UVs: Love or Hate Them, They're Essential," 2022.

  

[25] C. R. Qi, O. Litany, K. He, and L. Guibas, "Deep hough voting for 3D object detection in point clouds," *Proceedings of the IEEE International Conference on Computer Vision*, vol. 2019-Octob, pp. 9276–9285, 2019.

  

[26] C. R. Qi, H. Su, K. Mo, and L. J. Guibas, "PointNet: Deep learning on point sets for 3D classification and segmentation," *Proceedings - 30th IEEE Conference on Computer Vision and Pattern Recognition, CVPR 2017*, pp. 77–85, 2017.

  

[29] C. R. Qi, L. Yi, H. Su, and L. J. Guibas, "PointNet++: Deep hierarchical feature learning on point sets in a metric space," *Advances in Neural Information Processing Systems*, vol. 2017-Decem, pp. 5100–5109, 2017.

  

[31] G. Gkioxari, J. Johnson, and J. Malik, "Mesh R-CNN," *Proceedings of the IEEE International Conference on Computer Vision*, vol. 2019-Octob, pp. 9784–9794, 2019.

  

[32] Y. Zhou and O. Tuzel, "VoxelNet: End-to-End Learning for Point Cloud Based 3D Object Detection," *Proceedings of the IEEE Computer Society Conference on Computer Vision and Pattern Recognition*, pp. 4490–4499, 2018.

  

[36] C. Sullivan and A. Kaszynski, "PyVista: 3D plotting and mesh analysis through a streamlined interface for the Visualization Toolkit (VTK)," *Journal of Open Source Software*, vol. 4, no. 37, p. 1450, 2019.

  

[38] M. Attene, M. Campen, and L. Kobbelt, "Polygon mesh repairing: An application perspective," *ACM Computing Surveys*, vol. 45, no. 2, 2013.

  

[40] M. Attene, "A lightweight approach to repairing digitized polygon meshes," *Visual Computer*, vol. 26, no. 11, pp. 1393–1406, 2010.

  

[41] P. Liepa, "Filling Holes in Meshes," *Eurographics*, pp. 200–205, 2003.

  

[43] B. Galerne and A. Leclaire, "Texture inpainting using efficient Gaussian conditional simulation," *SIAM Journal on Imaging Sciences*, vol. 10, no. 3, pp. 1446–1474, 2017.

  

[44] A. Telea, "An Image Inpainting Technique Based on the Fast Marching Method," *Journal of Graphics Tools*, vol. 9, no. 1, pp. 23–34, 2004.

  

[45] M. Bertalmío, A. L. Bertozzi, and G. Sapiro, "Navier-Stokes, fluid dynamics, and image and video inpainting," *Proceedings of the IEEE Computer Society Conference on Computer Vision and Pattern Recognition*, vol. 1, 2001.

  

[47] L. Liang, C. Liu, Y. Q. Xu, B. Guo, and H. Y. Shum, "Real-time texture synthesis by patch-based sampling," *ACM Transactions on Graphics*, vol. 20, no. 3, pp. 127–150, 2001.

  

[48] A. A. Efros and W. T. Freeman, "Image quilting for texture synthesis and transfer," *Proceedings of the 28th Annual Conference on Computer Graphics and Interactive Techniques, SIGGRAPH 2001*, pp. 341–346, 2001.

  

[51] Leonardo Mariga, "pyRANSAC-3D."

  

---

  

*Master thesis, KU Leuven, Faculty of Industrial Engineering, Technology Campus Ghent, 2022–2023.*