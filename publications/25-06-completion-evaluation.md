---
layout: publication
title: "\rVoxel guided object completion evaluation of indoor scenes"
date: 2025-06-01
permalink: /publications/completion-evaluation/
icon: /img/icons/Geometry Generation.png
methodImage: img/publications/ObjectCompletion Methodology.png
doilink: https://kuleuven.limo.libis.be/discovery/fulldisplay?docid=alma9995772055301488&context=L&vid=32KUL_KUL:KULeuven&search_scope=All_Content&tab=all_content_tab&lang=en
githublink: https://github.com/JelleKUL/completiontools-evaluation
objective: 2
type: Thesis
description: A thesis project evaluation the VAE style object completion networks
bibtex:
---
# Voxel-Guided Object Completion for Industrial Indoor Scenes: Evaluation of AutoSDF on Novel Object Classes

**Mauro Decrocq**

Supervisor: Prof. dr. ir. Maarten Vergauwen Co-supervisor: Ir. arch. Jelle Vermandere

KU Leuven, Faculty of Industrial Engineering, Technology Campus Ghent, Belgium

**Keywords:** partially scanned object, neural network, automatic geometry reconstruction, new object classes, model evaluation

---

## Abstract

3D environments often rely on 3D scans as their foundation. However, objects derived from these scans are frequently incomplete due to occlusions and interactions with other objects. With the advent of machine learning and neural networks, it has become possible to probabilistically predict the geometry of incomplete objects. This study examines the geometry completion phase of the Object Completion Pipeline by applying the existing AutoSDF model to three novel object classes from the industrial chemical sector: barrels, rectangular pipe sections, and circular pipe sections. Both scanned and synthetic data were collected for evaluation. Each data type was processed twice: first using the pre-trained AutoSDF model, then after retraining specifically on the new object classes. Results reveal that scanned data performance is disappointing for both the untrained and trained models, primarily due to noise from highly reflective materials. For synthetic data, AutoSDF demonstrates promising performance. The untrained model shows noticeable decline only when the predicted volume exceeds 30–40% of the complete shape, while the trained model maintains performance until 60–70% must be predicted. Per-class evaluation shows that the model predicts volume and surface orientation better for circular objects, while other classes excel in surface positioning. The precision varies across metrics for different object classes.

## 1. Introduction

The demand for 3D environments continues to grow across diverse sectors including gaming, architecture, engineering, construction and operations (AECO), film and animation, virtual reality, 3D printing, computer vision, robotics, healthcare, education, and real estate. Digital models offer advantages in accuracy, efficiency, and collaboration within professional sectors, as well as greater creativity, visualization, and interactivity within cultural and recreational domains.

3D environments are typically based on 3D scans. A laser scanner captures all visible elements from its vantage point, including less relevant information. Consequently, objects in the scan are often incomplete due to occlusions or contact with other objects. To fully leverage the advantages of digital models, objects must have complete geometric form and structure.

Traditional interpolation methods for completing objects lead to loss of accuracy and unrealistic interpretations, loss of topological integrity, and insufficient consideration of contextual information (Vermandere et al., 2025). Thanks to new methods utilizing machine learning and neural networks, it is now possible to probabilistically predict geometry and textures of incomplete objects.

This study applies the AutoSDF model (Mittal et al., 2022) to evaluate performance on three novel object classes not previously tested: barrels, rectangular pipe sections, and circular pipe sections from the industrial chemical sector. Both scanned and synthetic data were collected, with each data type processed first using the original pre-trained model and then after specific retraining.

## 2. Related Work

### 2.1 Geometry Completion Approaches

Existing geometry completion models can be categorized into three classes (Vermandere et al., 2025):

**Database-based models** such as DiffCAD (Gao et al., 2024) and RetrievalFuse (Siddiqui et al., 2021) complete missing parts using segments from available databases. DiffCAD employs three diffusion models working from a single RGB image to select, align, and retrieve CAD models. RetrievalFuse divides scenes into chunks and retrieves the most similar segments from a database using contrastive learning. While these methods work well when matching objects exist in the database, they exhibit low generalizability for complex or unusual objects.

**Point-Voxel-based models** such as Point-Voxel Diffusion (PVD) (Zhou et al., 2021) and IF-Nets (Chibane et al., 2020) convert meshes to normalized point clouds and predict missing geometry through 3D diffusion or implicit features. These models can provide accurate predictions with sufficient information but lose detail in dense areas and struggle with large missing regions.

**SDF-based models** convert meshes to volumetric distance fields and use neural networks to predict missing geometry. AutoSDF (Mittal et al., 2022) encodes SDFs using a VQ-VAE architecture and predicts missing sub-selections. SD Fusion (Cheng et al., 2022) extends this with multi-modal inputs including text prompts. PatchComplete (Rao et al., 2022) splits SDFs into smaller patches for improved generalizability, while DiffComplete (Chu et al., 2023) introduces diffusion-based approaches. SDF-based methods offer superior surface definition retention compared to point clouds but require watertight meshes, which is problematic for scanned data. Unsigned Distance Fields (UDFs) provide a more generalized alternative for real-world incomplete objects.

### 2.2 Material Detection and Texture Completion

The full Object Completion Pipeline consists of three phases (Vermandere et al., 2025): geometry completion, material segmentation and prediction, and texture completion and inpainting. Material detection using models like SAM (Kirillov et al., 2023) helps create better inpainting references. Texture completion approaches include TEXTure (Richardson et al., 2023) for diffusion-based texturing from viewpoints, TUVF (Cheng et al., 2023) for learning generalizable texture UV fields, and IF-Net Texture (Chibane and Pons-Moll, 2020) for completing missing color from partial 3D data.

### 2.3 Evaluation Metrics

Standard evaluation metrics for geometry completion include Intersection over Union (IOU) for volumetric overlap, Chamfer Distance (CD) for average point-to-point distance, F-score for precision-recall balance, Coverage for precision of surface positioning, and Normal Consistency (NC) for surface orientation agreement.

## 3. Methodology

### 3.1 Geometry Completion Pipeline

The methodology follows a structured workflow: preprocessing, voxel selection, AutoSDF completion, and evaluation.

**Preprocessing.** Input meshes undergo conversion to a suitable implicit representation. Meshes are first cleaned and normalized, then converted to Signed Distance Fields discretized at 128³ resolution using a dual octree graph representation (Wang et al., 2022). For non-watertight scanned objects, UDFs are employed as a more generalized representation. The SDF/UDF is stored as a volumetric grid compatible with the AutoSDF network architecture.

**Voxel Selection.** The voxels representing missing regions are selected interactively in Unity. Multiple voxel selections are created per object to test different completion scenarios, ranging from small missing regions to large occlusions. Each selection specifies which voxels the model should predict, enabling systematic evaluation across different levels of incompleteness.

**AutoSDF Completion.** The selected voxels and partial SDF are fed to the AutoSDF model. The Patch-wise VQ-VAE (P-VQ-VAE) encoder compresses patches of the SDF into discrete tokens from a learned codebook. The transformer prior then predicts tokens for the missing regions based on the encoded partial shape. The decoder reconstructs the complete SDF from all tokens. The model generates multiple possible solutions; the best is selected based on highest IOU with the ground truth. The completed SDF is converted back to a mesh using marching cubes.

### 3.2 Evaluation

Five metrics are computed for each completion:

**IOU** measures the ratio of intersection to union volume between the completed and ground truth meshes. Both meshes must be watertight. Values range from 0 to 1, with 1 indicating perfect overlap.

**Chamfer Distance (CD)** computes the average squared distance between nearest-neighbor vertex pairs in both directions:

CD(P, Q) = (1/|P|) Σ_{p∈P} min_{q∈Q} ‖p − q‖² + (1/|Q|) Σ_{q∈Q} min_{p∈P} ‖q − p‖²

**F-score** balances precision and recall using a threshold distance:

F-score = 2 × (precision × recall) / (precision + recall)

**Coverage** equals the precision component, measuring what fraction of predicted surface points lie within the threshold distance of the ground truth.

**Normal Consistency (NC)** compares surface normal orientations by computing the mean absolute dot product between normal vectors at nearest-neighbor pairs, measuring how well the predicted surface orientation matches the ground truth.

### 3.3 Model Training

The training procedure follows the original AutoSDF implementation with three steps: (1) training the P-VQ-VAE encoder-decoder on the new object classes, (2) extracting and caching discrete codes from the dataset, and (3) training the transformer prior on the cached token sequences. Only the transformer is retrained in this study, as the encoder was previously trained on extensive databases including ShapeNet and Matterport.

## 4. Experiments

### 4.1 Data Collection

**Scanned data.** Nine barrels were scanned from an industrial brewery facility at KU Leuven Campus Rabot using terrestrial laser scanning. The barrels feature highly reflective metallic surfaces, which introduce significant noise into the scan data. Individual objects were isolated from the full scene scans.

**Synthetic data.** Three categories of synthetic 3D models were created: barrels (cylindrical vessels), rectangular pipe sections (including straight segments and bends at 30°, 45°, and 90°), and circular pipe sections (with similar geometric variations). Synthetic data provides clean, noise-free geometry with known ground truth, enabling systematic evaluation of the model's capabilities independent of scan quality issues.

### 4.2 Preprocessing

Scanned data required extensive preprocessing including noise removal, mesh cleaning, and conversion to watertight SDF representations. The highly reflective material of the barrels caused significant scan artifacts, making clean SDF generation challenging.

Synthetic data preprocessing was more straightforward, involving alignment, normalization, and SDF conversion. The alpha value parameter controlling the SDF boundary definition was carefully tuned: too low produces incomplete surfaces, too high merges separate components.

### 4.3 Voxel Selections

Multiple voxel selections were created per object, systematically varying the proportion of geometry to be predicted from approximately 10% to 90% of the total volume. Ten different orientations were tested for synthetic data, and each selection was applied across all objects within a class.

### 4.4 Results: Untrained Model

**Scanned data.** The untrained AutoSDF model produced generally poor results on scanned barrel data. IOU values ranged from 5.0% to 64.4%, with high variability. The noise in the scanned data prevented the model from finding coherent solutions. Some completions failed to produce watertight meshes, making IOU computation impossible for certain objects.

**Synthetic data.** Performance on synthetic data was substantially better. The model showed a noticeable decline only when the volume to be predicted exceeded 30–40% of the complete shape. Global results showed mean IOU values above 55% for completions with more than 60% original data present, with corresponding Normal Consistency above 80%.

### 4.5 Results: Trained Model

**Scanned data.** Training on the new object classes provided marginal improvement for scanned data. While some metric values improved slightly, the fundamental limitation of noisy input data persisted. The scan quality issue dominated over model capability improvements.

**Synthetic data.** The trained model showed significant improvement. The performance decline threshold shifted from 30–40% (untrained) to 60–70% of volume needing prediction. The model consistently predicted substantial portions of new, accurate geometry, though it lost some detail in scenarios with very high proportions of original data (>70%), where the encoding process slightly altered already-known parts.

**Table 1.** Summary of global performance on synthetic data (trained model, best intervals).

|Metric|Value Range|Assessment|
|---|---|---|
|IOU|55–75%|Satisfactory|
|CD|0.0001–0.009|Satisfactory|
|F-score|0.7–0.9|Satisfactory|
|NC|80–90%|Satisfactory|

### 4.6 Per-Class Analysis

The per-class evaluation revealed distinct performance characteristics:

**Circular pipe sections** achieved better IOU and Normal Consistency scores than other classes, indicating the model predicts volume and surface orientation more effectively for rounded geometries.

**Rectangular pipe sections** and **barrels** showed better Chamfer Distance performance, indicating more accurate surface positioning despite lower volumetric overlap scores.

**Barrels** exhibited the lowest and most consistent Chamfer Distance values across all completion levels, with the critical performance threshold extending further (to the 80–90% interval for the trained model) compared to pipe sections.

Precision (standard deviation) varied across metrics and object classes, with rectangular sections showing higher consistency for F-score and Coverage, while barrels demonstrated more variable IOU and NC results.

## 5. Discussion

### 5.1 Scanned vs. Synthetic Data

The stark performance difference between scanned and synthetic data highlights a fundamental challenge in applying completion models to real-world industrial objects. The highly reflective metallic surfaces of the industrial objects create scan artifacts including noise, ghosting, and incomplete surface capture. These artifacts propagate through the SDF conversion, producing noisy implicit representations that confound the completion network.

This finding aligns with observations in the broader literature that models trained on clean synthetic data struggle with the domain gap to real-world scans. Addressing this gap requires either improved preprocessing to reduce scan noise, training on more realistic noisy data, or developing completion networks more robust to input noise.

### 5.2 Model Capabilities

For clean synthetic data, AutoSDF demonstrates strong generalization to the three novel industrial object classes, despite being originally trained on ShapeNet categories (chairs, tables, etc.). The VQ-VAE architecture successfully learned to represent and complete geometrically simpler industrial objects. The improvement after class-specific training was substantial, extending the usable completion range by approximately 30 percentage points.

### 5.3 Comparison with Literature

The achieved metrics for synthetic data are comparable to those reported for AutoSDF on standard ShapeNet categories. The IOU values of 55–75% and Normal Consistency of 80–90% align with published benchmarks, confirming that the model's capabilities transfer effectively to novel object classes when input quality is adequate.

### 5.4 Limitations

Several limitations should be noted. The evaluation was restricted to geometry completion only, not addressing material or texture prediction. The scanned dataset was limited to nine barrels from a single industrial facility. The voxel selection process, while systematic, was manual. Finally, the 128³ SDF resolution limits the fine detail that can be captured and predicted.

## 6. Conclusion

This study evaluated the AutoSDF geometry completion model on three novel industrial object classes: barrels, rectangular pipe sections, and circular pipe sections. The key findings are:

AutoSDF performs poorly on scanned data from industrial environments, primarily due to noise from highly reflective materials. Both the untrained and trained models struggle with the quality of real-world scan data, highlighting the need for improved scanning techniques or noise-robust completion methods for industrial applications.

For synthetic data, AutoSDF shows promising generalization to novel object classes. The untrained model maintains performance when up to 30–40% of the geometry must be predicted, extending to 60–70% after class-specific training. The model consistently generates substantial new geometry while maintaining reasonable accuracy.

Per-class performance varies meaningfully: circular objects achieve better volume and surface orientation prediction, while other classes excel in surface positioning accuracy. This suggests that object geometry characteristics influence which aspects of completion the model handles best.

Future work should focus on improving scan quality for reflective industrial objects, developing noise-robust completion networks, extending the evaluation to include material segmentation and texture completion, and testing on a broader range of industrial object classes.

## References

[1] Gao, D., Rozenberszki, D., Leutenegger, S., and Dai, A. (2024). Diffcad: Weakly-supervised probabilistic cad model retrieval and alignment from an rgb image. _ACM Transactions on Graphics_, 43.

[2] Siddiqui, Y., Thies, J., Ma, F., Shan, Q., Nießner, M., and Dai, A. (2021). Retrievalfuse: Neural 3d scene reconstruction with a database. _Proceedings of the IEEE International Conference on Computer Vision_, pp. 12548–12557.

[3] Zhou, L., Du, Y., and Wu, J. (2021). 3d shape generation and completion through point-voxel diffusion.

[4] Chibane, J., Alldieck, T., and Pons-Moll, G. (2020). Implicit functions in feature space for 3d shape reconstruction and completion.

[5] Mittal, P., Cheng, Y.-C., Singh, M., and Tulsiani, S. (2022). Autosdf: Shape priors for 3d completion, reconstruction and generation.

[6] Kirillov, A., Mintun, E., Ravi, N., Mao, H., Rolland, C., Gustafson, L., Xiao, T., Whitehead, S., Berg, A. C., Lo, W.-Y., Dollár, P., and Girshick, R. (2023). Segment anything.

[7] Richardson, E., Metzer, G., Alaluf, Y., Giryes, R., and Cohen-Or, D. (2023). Texture: Text-guided texturing of 3d shapes.

[8] Cheng, A.-C., Li, X., Liu, S., and Wang, X. (2023). Tuvf: Learning generalizable texture uv radiance fields.

[9] Chibane, J. and Pons-Moll, G. (2020). Implicit feature networks for texture completion from partial 3d data.

[10] Chang, A. X., Funkhouser, T., Guibas, L., Hanrahan, P., Huang, Q., Li, Z., Savarese, S., Savva, M., Song, S., Su, H., Xiao, J., Yi, L., and Yu, F. (2015). Shapenet: An information-rich 3d model repository.

[11] Ramakrishnan, S. K., Gokaslan, A., Wijmans, E., Maksymets, O., Clegg, A., Turner, J., Undersander, E., Galuba, W., Westbury, A., Chang, A. X., Savva, M., Zhao, Y., and Batra, D. (2021). Habitat-matterport 3d dataset (hm3d): 1000 large-scale 3d environments for embodied ai.

[12] Vermandere, J., Bassier, M., and Vergauwen, M. (2025). Geometry and texture completion of partially scanned 3d objects through material segmentation. _Proceedings of the 20th International Joint Conference on Computer Vision, Imaging and Computer Graphics Theory and Applications - GRAPP_, pp. 193–202, SciTePress.

[13] Qi, C. R., Su, H., Mo, K., and Guibas, L. J. (2017). Pointnet: Deep learning on point sets for 3d classification and segmentation.

[14] Cheng, Y.-C., Lee, H.-Y., Tulyakov, S., Schwing, A., and Gui, L. (2022). Sdfusion: Multimodal 3d shape completion, reconstruction, and generation.

[15] Rao, Y., Nie, Y., and Dai, A. (2022). Patchcomplete: Learning multi-resolution patch priors for 3d shape completion on unseen categories.

[16] Chu, R., Xie, E., Mo, S., Li, Z., Nießner, M., Fu, C.-W., and Jia, J. (2023). Diffcomplete: Diffusion-based generative 3d shape completion.

[17] Chang, A., Dai, A., Funkhouser, T., Halber, M., Niessner, M., Savva, M., Song, S., Zeng, A., and Zhang, Y. (2017). Matterport3d: Learning from rgb-d data in indoor environments. _International Conference on 3D Vision (3DV)_.

---

_Master thesis, KU Leuven, Faculty of Industrial Engineering, Technology Campus Ghent, 2024–2025._

_Code availability:_

- Completion tools evaluation: https://github.com/JelleKUL/completiontools-evaluation
- AutoSDF: https://github.com/yccyenchicheng/AutoSDF