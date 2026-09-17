# Neural Network Architectures for 3D Vision

### An Overview for 3D Generation, Detection, Segmentation, Remote Sensing & Computer Graphics

This document maps the fundamental neural network architectures to the concrete implementations that dominate several applied fields. It's meant as a starter reference: enough to orient yourself, understand *why* a given architecture fits a given task, and know which model names to look up next.

---

## 1. The Core Architectures (Quick Recap)

Before the domain-specific breakdown, here are the building blocks that everything else is assembled from.

| Architecture | Core idea | Inductive bias | Role in 3D |
|---|---|---|---|
| **MLP** | Fully-connected layers | None (treats input as flat vector) | Per-point feature learning; the backbone of PointNet; used inside NeRF and as feed-forward blocks everywhere |
| **CNN** | Sliding shared filters over a grid | Locality + translation invariance | Voxel grids, range/BEV image projections, 2D backbones for multi-view methods |
| **Transformer** | Self-attention over all elements | Permutation-equivariant, global context | Point transformers, multi-view reconstruction, detection queries, large 3D generative models |
| **Diffusion** | Iterative denoising (a *framework*, not a layer) | — (uses a CNN/transformer backbone) | State-of-the-art 3D generation |
| **GNN** | Message passing over graph edges | Relational structure | Point clouds treated as k-NN graphs (e.g. DGCNN) |
| **State Space Models (Mamba)** | Linear-complexity sequence modeling | Long-range, ordered dependencies | Emerging alternative to transformers for large point clouds |

A key challenge unifies these fields: **point clouds are unordered, irregular, and variable in density**, unlike the neat grids CNNs were built for. The history of 3D deep learning is largely the story of adapting these architectures to that irregularity.

---

## 2. 3D Segmentation

Segmentation (semantic, instance, or part) assigns a label to every point, voxel, or region. Approaches split into three families based on how they tame the irregular data.

### Projection / Voxel-based (CNN territory)
Convert the 3D data into a regular grid, then apply convolutions.

- **Voxel CNNs / Sparse convolutions** — **MinkowskiNet** and **SparseConvNet** run 3D convolutions only on occupied voxels, avoiding the cubic memory blowup. Still a workhorse for indoor and outdoor scenes.
- **Range-image / BEV projection** — project a LiDAR scan to a 2D image and use a standard 2D CNN (fast, popular in autonomous driving).
- **U-Net** — the encoder–decoder with skip connections is the near-universal template for dense prediction, in both 2D-projection and voxel forms.

*Trade-off:* voxelization is simple and fast but loses fine detail and wastes resolution on empty space.

### Point-based (MLP & GNN territory)
Consume raw points directly, no grid.

- **PointNet** — the foundational method. Shared MLPs process each point independently, then a symmetric max-pooling gives permutation invariance. Lightweight but blind to local neighborhoods.
- **PointNet++** — adds hierarchical grouping so local geometry is captured at multiple scales. Still a common baseline and backbone.
- **DGCNN** — builds a dynamic k-NN graph and applies "EdgeConv"; a GNN-flavored approach that captures local structure well.
- **KPConv** — defines convolution directly on points using kernel points in continuous space; strong on large outdoor/terrain scans.

### Transformer-based (current state of the art)
- **Point Transformer (v1–v3)** — self-attention within local neighborhoods; **Point Transformer V3** is a leading performer on major benchmarks, prioritizing simplicity and scale.
- **Stratified Transformer**, **Swin3D** — hierarchical attention adapted from 2D vision transformers.

### Emerging: State Space Models & Diffusion
- **Point Mamba variants** (Pamba, Serialized Point Mamba) — serialize the point cloud into an ordered sequence and apply Mamba, achieving transformer-level accuracy with **linear** (rather than quadratic) complexity — attractive for very large scenes.
- **Diffusion for segmentation** — models like **PointDiffuse** frame segmentation as a conditional denoising process, an experimental but growing direction.

### Promptable / open-vocabulary segmentation
- **SAM-family lifting** — methods that take 2D masks from the Segment Anything Model and lift them into 3D. *Note the naming collision:* the 2023 academic paper **"SAM3D: Segment Anything in 3D Scenes"** is a segmentation method (unrelated to Meta's 2025 generation model of nearly the same name — see §4). **SAI3D**, **Gaussian Grouping**, and **SAM 3** (Meta's Nov 2025 open-vocabulary concept segmenter) belong here.

**Common benchmarks:** S3DIS and ScanNet (indoor); SemanticKITTI, nuScenes (outdoor/driving). Metric: **mIoU** (mean Intersection-over-Union).

---

## 3. 3D Object Detection

Detection predicts **3D bounding boxes** (position, size, orientation, class) rather than per-point labels. It's the backbone of autonomous driving perception, and the architectural lineage tracks the same grid-vs-point tension as segmentation.

### Voxel / grid-based (CNN → sparse CNN → transformer)
- **VoxelNet** — the first end-to-end voxel-based detector; learns features per voxel then applies 3D CNNs.
- **SECOND** — introduced **sparse 3D convolutions**, drastically cutting the cost of VoxelNet (only ~1–3% of voxels are actually occupied).
- **PointPillars** — collapses the vertical dimension into "pillars," producing a BEV pseudo-image so a fast **2D CNN** can do the work. A long-standing favorite for real-time deployment; strong on small objects like pedestrians and cyclists.
- **PV-RCNN** — combines voxel-grid structure with point-level refinement for balanced accuracy.
- **Voxel Transformers (VoTr, SST, SWFormer)** — swap parts of the CNN backbone for sparse/windowed attention (drawing on Swin Transformer ideas), pushing accuracy higher.

### Point-based
- Detectors built directly on **PointNet++** backbones (e.g. PointRCNN) that operate on raw points without voxelization — accurate but generally heavier.

### BEV & query-based transformers (the current paradigm, esp. camera-based)
- **DETR3D** — brings the DETR "set prediction with object queries" idea to 3D, removing hand-designed anchors and NMS.
- **BEVFormer** — a spatiotemporal transformer that fuses multi-camera images into a unified **Bird's-Eye-View** feature map via spatial cross-attention and temporal self-attention. It enables strong 3D detection *without LiDAR*, and supports tracking and map segmentation in one network. A landmark for vision-only autonomous driving.

### Multi-modal fusion (camera + LiDAR)
- **TransFusion**, **MSF3DDETR**, **EPNet** — fuse RGB and LiDAR features (often via cross-attention over object queries) for top accuracy, at higher compute cost.

*Trade-off theme:* transformer/two-stage/fusion methods dominate the accuracy leaderboards, while single-stage projection models (PointPillars, PIXOR, SMOKE) stay popular for real-time, low-latency deployment.

**Common benchmarks:** KITTI, Waymo Open Dataset, nuScenes. Metrics: **mAP**, **3D IoU**, and dataset-specific scores (e.g. nuScenes NDS).

---

## 4. 3D Generation

Generation is where **diffusion** and **novel scene representations** have completely reshaped the field. Two axes matter: the *representation* being generated, and the *generative framework* driving it.

### The representations
- **Meshes** — explicit polygons; directly usable in game engines and DCC tools.
- **Point clouds** — the raw generative target of several early diffusion methods.
- **Implicit fields (SDF / occupancy / NeRF)** — a neural network (usually an MLP) represents the surface or radiance continuously.
- **3D Gaussian Splatting (3DGS)** — represents a scene as millions of anisotropic Gaussians. Now **dominant** for many tasks: real-time (60+ FPS) rendering, sharp boundaries, and GPU efficiency that outpaces classic NeRF.

### Optimization-based (2D diffusion → 3D)
- **DreamFusion** — introduced Score Distillation Sampling (SDS): use a pretrained 2D text-to-image diffusion model to sculpt a NeRF. The seed of the modern text-to-3D explosion.
- **ProlificDreamer**, **LucidDreamer** — refine the distillation loss for higher fidelity and diversity.
- **DreamGaussian**, **GaussianDreamer** — swap the NeRF backbone for 3D Gaussian Splatting, cutting generation time from hours to minutes.

### Multi-view diffusion
- **MVDream**, **Zero-1-to-3** — generate multiple consistent views of an object, then lift them to 3D. Solves the "Janus problem" (multi-faced artifacts) of pure SDS.

### Feed-forward reconstruction & production models (transformer / DiT-based)
This is the fast, single-pass branch — no per-scene optimization — and where the two models you asked about live.

- **Large Reconstruction Models (LGM, GS-LRM, FlexRM/Flex3D)** — transformers that patchify posed images and decode 3D (often Gaussian) parameters in one forward pass.
- **Hunyuan3D (Tencent)** — a leading **open-weight** production system for text-to-3D and image-to-3D. Two-stage architecture: geometry via **Hunyuan3D-DiT** (a flow-based **Diffusion Transformer**) and texture via **Hunyuan3D-Paint** (which produces PBR materials). Version lineage moves fast: 1.0 (2024) → 2.0 → 2.1 (production-ready PBR) → **2.5** (April 2025, scaled from 1B to 10B parameters, geometry resolution raised to 1024), plus a **Hunyuan3D-Omni** variant for controllable generation. A great concrete example of the "DiT applied natively to 3D assets" trend.
- **SAM 3D (Meta, Nov 2025)** — a **single-image 3D reconstruction** system ("3D-fying"), released alongside SAM 3. It's generative and uses strong learned priors, so it works on cluttered, occluded, in-the-wild photos rather than needing many calibrated views like classic photogrammetry. Ships as **SAM 3D Objects** (general objects/scenes) and **SAM 3D Body** (human shape + pose). It's designed to **chain off segmentation**: *SAM 3 segments → SAM 3D reconstructs*, aligning the 3D output to the object's appearance and position in the photo. Bridges the Segmentation and Generation worlds. (Don't confuse it with the 2023 "SAM3D" segmentation paper — see §2.)

### Native 3D diffusion
- Models that run diffusion directly in a 3D latent space (triplanes, point latents, structured Gaussians) — e.g. **DirectTriGS**, **GaussianAnything**, **Atlas Gaussians** — avoiding the 2D-lifting bottleneck entirely. Hunyuan3D-DiT's geometry stage also belongs to this philosophy.

**GANs & VAEs** — historically important (3D-GAN for voxel shapes; VAEs for latent shape spaces) and still used as *components* (e.g. a VAE compressing shapes into the latent space a diffusion model operates on), but largely superseded as standalone generators.

---

## 5. Remote Sensing

Remote sensing (satellite, airborne LiDAR, radar, hyperspectral) adds its own constraints: enormous scale (dozens of km²), multi-modality, variable point density, and scarce labels. The architectures are largely borrowed from segmentation/detection, then adapted.

### Point cloud classification (airborne/mobile LiDAR)
- **PointNet / PointNet++** — widely adapted with minimal modification; lightweight enough for large surveys.
- **DGCNN**, **ConvPoint**, **KPConv** — strong performers on benchmarks like **Toronto3D** and **ModelNet40**; KPConv variants excel on complex terrain (ground-point extraction, DEM/DTM generation).
- **U-Net / LU-Net** — projection-based pipelines for efficient large-area labeling.

### Multi-modal fusion (the defining remote-sensing problem)
Combining optical, radar (SAR), and LiDAR streams:
- **CNNs** for spatial feature extraction per modality.
- **RNNs / temporal models** for time-series (crop monitoring, change detection).
- **GANs & Autoencoders** for dimensionality reduction, denoising, and data harmonization.
- **Fusion levels:** pixel-level, feature-level, or decision-level combination.

### Transformers & Foundation Models (the current frontier)
- **Vision Transformers** adapted to multispectral/hyperspectral inputs.
- **Remote-sensing foundation models** — large models pre-trained on diverse modalities (self-supervised), then fine-tuned per task. These directly address the field's chronic **labeled-data scarcity** by improving generalization and transfer.

**Common applications:** land-cover classification, DEM/DSM/DTM generation, forestry & tree-species mapping, building/change detection, disaster monitoring.

---

## 6. Computer Graphics

Graphics overlaps heavily with generation but emphasizes *rendering*, *editing*, and *real-time performance* over pure asset synthesis.

### Neural rendering & scene representation
- **NeRF and its accelerated successors** — **Instant-NGP** (hash-grid encoding, ~1000× faster training) and **ZipNeRF** made neural rendering practical on consumer hardware.
- **3D Gaussian Splatting** — the current dominant representation for real-time, high-fidelity novel-view synthesis; explicit, editable, and fast. A large ecosystem now exists for **segmenting, editing, and animating** Gaussian scenes (e.g. GaussianEditor).

### Generative & procedural content
- **Diffusion-backed text-to-3D** (the DreamFusion → DreamGaussian → Hunyuan3D lineage above) is increasingly integrated into DCC tools and engines, letting artists generate editable meshes/splats from prompts.
- **GANs** — still used for texture synthesis, style transfer, and image-space effects.

### Underlying building blocks
- **MLPs** — the literal representation inside NeRF and many implicit-surface methods (an MLP *is* the scene).
- **CNNs (U-Net)** — super-resolution, denoising renders, image-to-image translation, and as the diffusion backbone.
- **GNNs** — mesh processing, physics/simulation, and character rigging where connectivity matters.
- **Transformers** — mesh generation and large reconstruction models.

---

## 7. Cross-Cutting Summary

| Field | Dominant today | Rising | Legacy / component role |
|---|---|---|---|
| **Segmentation** | Point Transformer V3, sparse-conv U-Nets | Point Mamba (SSMs), diffusion & SAM-lifting | PointNet/++, DGCNN, KPConv |
| **Detection** | BEVFormer, query-based transformers, sparse-voxel | Multi-modal fusion, occupancy prediction | VoxelNet, PointPillars, SECOND |
| **Generation** | Diffusion + 3D Gaussian Splatting; DiT (Hunyuan3D) | Feed-forward LRMs, single-image recon (SAM 3D), native 3D diffusion | GANs, VAEs (now components) |
| **Remote Sensing** | CNN/transformer fusion, foundation models | Self-supervised pretraining | PointNet++, RNNs, autoencoders |
| **Graphics** | 3D Gaussian Splatting, Instant-NGP | Generative/editable splats | Classic NeRF, GANs |

### Big-picture trends
1. **Convergence on Gaussian Splatting** as a shared representation across generation and graphics — explicit, fast, editable.
2. **Diffusion as the default generative engine**, increasingly via a **Diffusion Transformer (DiT)** backbone (Hunyuan3D) and increasingly running natively in 3D rather than lifting from 2D.
3. **Transformers everywhere in perception** — object queries (DETR3D) and BEV attention (BEVFormer) reshaped detection the way Point Transformer reshaped segmentation.
4. **The efficiency race** — transformers gave global context but quadratic cost; State Space Models (Mamba), sparse convolutions, and feed-forward reconstruction models are the response, chasing linear scaling and single-pass inference for ever-larger 3D data.
5. **Segmentation and generation are merging** — SAM 3 → SAM 3D is the clearest signal: segment an object in a photo, then reconstruct it in 3D, in one pipeline.

---

*Notes on scope:* Model names reflect widely-cited, actively-used implementations as of early–mid 2026. This field moves fast, especially in generation and detection — treat the "rising" columns as leading edges rather than settled conclusions, and verify current benchmark leaders (e.g. on Papers-with-Code or recent survey papers) before committing to a specific model.
