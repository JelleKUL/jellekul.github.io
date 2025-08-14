# 3D Object Completion from Partial Scans (2021–2025)

3D shape completion aims to reconstruct a full object geometry from an incomplete input (e.g. a partial point cloud or mesh). Modern approaches often use **implicit neural representations** (e.g. signed distance fields, SDFs) or voxel grids as output. SDFs are particularly popular because they can represent fine surface detail at high resolution. Many recent models encode incomplete inputs (points, voxels, partial meshes) and predict a (truncated) SDF volume. We focus on deep learning methods – especially diffusion, transformer, and generative models – that output SDF/TSDF-based shapes.

## Key Methods and Architectures

- **Patch-wise SDF Transformers:** _POC-SLT_ (Hu _et al._, 2024) splits a shape into a grid of small volumetric SDF _patches_, encodes each with a “Patch-VAE”, and treats the latent codes as a sequence. A masked-autoencoder transformer (inspired by MAE) then completes this latent sequence, and the decoder reconstructs the full SDF. This yields high-resolution completions by operating in a compact latent SDF space.
    
- **Autoregressive Latent Priors (AutoSDF):** Mittal _et al._ (CVPR 2022) introduced _AutoSDF_, which discretizes 3D shapes into a low-dimensional symbolic grid of SDF values and learns an autoregressive prior over these symbols. This single model serves multiple tasks (shape completion, single-view 3D reconstruction, text-driven generation) without task-specific retraining. In completion, it conditions on observed voxels of the partial TSDF. (Official code and models are available on the project page.)
    
- **Latent SDF Diffusion (SDFusion):** Cheng _et al._ (CVPR 2023) propose _SDFusion_, a diffusion model built on an SDF autoencoder. First, shapes are encoded into a low-dimensional latent; then a diffusion process is learned in latent space. SDFusion can handle multiple input modalities (partial TSDF, images, text) via task-specific encoders and cross-attention. Because diffusion occurs in a compact latent, it scales to 643 or even 1283 SDF outputs. SDFusion achieves state-of-the-art performance on shape completion and 3D reconstruction benchmarks (e.g. Pix3D), outperforming prior autoencoder and voxel methods. (_Code:_ the authors’ [GitHub repo](https://github.com/yccyenchicheng/SDFusion).)
    
- **Diffusion of Neural SDF (Diffusion-SDF):** Chou _et al._ (ICCV 2023) model shapes as **neural SDFs** (small MLP networks) and train a diffusion model on those parameters. This implicitly learns a distribution over SDF functions. At inference, conditioning on a partial scan (or image) steers the diffusion to complete the neural SDF. The published code is available (_Diffusion-SDF_ repository).
    
- **Latent TSDF Diffusion (SC-Diff):** Galvis _et al._ (ECCV 2024) introduce _SC-Diff_, a 3D latent diffusion specifically for **Truncated SDFs**. A volumetric encoder compresses a partial TSDF into a latent vector; a diffusion model then generates the complete latent; finally a decoder outputs a TSDF grid. SC-Diff can process any object class in one model and optionally use an input image via cross-attention. On standard benchmarks it matches or exceeds SOTA accuracy at higher effective resolution (thanks to the latent TSDF representation).
    
- **Point-Voxel Diffusion (PVD):** Zhou _et al._ (2021) propose _PVD_, an early diffusion-based shape generator. PVD uses a **hybrid point+voxel** representation: it diffuses noise added to both a point cloud and a voxel grid of a shape. This unified model can do _unconditional generation_ of shapes and _conditional completion_ of partial point clouds. The paper shows PVD generates high-fidelity shapes and diverse completions from single-view depth scans. (While predating our 2021 cut-off, PVD set a foundation for later diffusion work.)
    
- **Conditional Diffusion (DiffComplete):** Chu _et al._ (NeurIPS 2023) present _DiffComplete_, which treats completion as a conditional diffusion problem. It injects partial scan features via a hierarchical fusion mechanism and an “occupancy-aware” strategy that can combine multiple partial inputs (e.g. scans from different views). DiffComplete significantly outperforms previous methods – achieving about **40% lower L1 reconstruction error** on standard shape-completion benchmarks – while producing realistic, high-fidelity results. Importantly, it generalizes to unseen object categories and real scans without retraining.
    
- **Patch-Based Priors (PatchComplete):** Cai _et al._ (NeurIPS 2022) introduce _PatchComplete_, which learns multi-scale SDF “patch priors”. The idea is to learn a dictionary of local SDF patches (at several resolutions) from training shapes, and use attention to match input partial patches to these learned priors. An attention-weighted fusion of the multi-resolution patch codes yields the completed SDF. This allows generalization to unseen categories, since local structures (e.g. legs, edges) recur across shapes. On benchmarks (synthetic and real) PatchComplete dramatically improves over prior methods – e.g. it reports ~19% reduction in Chamfer distance relative to earlier approaches.
    
- **Transformer and Autoencoder Models:** Some works adapt general transformers to 3D completion. _PoinTr_ (Yu _et al._, ICCV 2021) reformulates point-cloud completion as a “set-to-set” problem: it groups points into unordered sets with positional embeddings, forming a sequence of tokens, and uses a transformer encoder-decoder to predict missing points. This outperforms earlier 3D-CNNs on point-cloud benchmarks. A more recent example is _NeuSDFusion_ (Cui _et al._ 2024): it maps SDFs into 3 orthogonal tri-plane features, compresses them via a spatial transformer autoencoder, and then diffuses in that latent space. Results show NeuSDFusion generates high-quality surfaces (Figure 4) and handles multi-modal completion (text, images) with 3D spatial attention (although code is not yet public).
    
- **Multi-Resolution Spectral Networks:** Deka _et al._ (WACV 2025) propose a novel CNN architecture for TSDF completion. It uses multi-resolution 3D convolutional encoders combined with a **Spectral Module** (an FFT-based convolution between fast-Fourier and inverse-FFT) to capture global context without losing locality. An attention-based decoder fuses partial scans with pre-encoded priors. They report state-of-art results on synthetic (ShapeNet) and real scanned object tests, showing their multi-res+spectral design yields more complete and detailed reconstructions.
    
- **Weakly-Supervised Prior Models:** Wu _et al._ (2024) tackle generalization via _Weakly-Supervised Shape Completion (WSSC)_. They build a **“prior bank”** of representative shapes from seen categories and learn multi-scale pattern correlations between the input scan and these priors.  A self-supervised refinement network uses a _voxel-based partial-matching loss_ (which only requires the partial input during training). This method outperforms supervised baselines on unseen categories by a wide margin, despite using no complete ground truth for them. (Code is publicly released: [github.com/ltwu6/WSSC](https://github.com/ltwu6/WSSC).)
    

## Datasets

Researchers use a variety of synthetic and real datasets, often focusing on indoor-object categories:

- **ShapeNet** (Chang _et al._ 2015) – A large CAD model repository (chairs, tables, etc.). Almost every method uses ShapeNet for training and synthetic completion tests.
    
- **BuildingNet** – A dataset of 3D building interiors. For completion, Cheng _et al._ use 128^3 SDFs (vs. 64^3 for ShapeNet) to capture fine architectural detail.
    
- **Pix3D** (Xiang _et al._ 2014) – Real images of objects (aligned with 3D models). Used for image-conditioned completion; e.g. SDFusion reports Chamfer/F-score on Pix3D and surpasses ResNet baselines.
    
- **PartNet** – Detailed object-part annotations (e.g. for chairs). ShapeMorph and others evaluate part-level completion on PartNet–Mobility (Figure 4 shows superior F-scores for ShapeMorph on PartNet).
    
- **ScanObjectNN** (Uy _et al._ 2019) – Real scanned indoor objects (partial, cluttered). It includes ground-truth segments; some works use its classes or scenes to test robustness.
    
- **MultiScan / PARIS** – Real indoor scene scans with articulated furniture. (E.g. PARIS two-part object dataset references MultiScan data.)
    
- **SUNCG / 3D-FRONT / 3D-FUTURE** – Synthetic scene datasets (rooms with furniture). Portions of these have been used to sample objects or synthesize scans.
    
- **Stanford 3D Scanning (Redwood)** – Scans of ~150 real objects with holes. Early shape completion papers used Redwood; modern generative models sometimes fine-tune on it.
    
- **ScanNet / Matterport3D** – Entire indoor scenes. While these are usually for scene completion, some studies extract object-level scans (via segmentation) for single-object completion.
    
- **ABC Dataset** (Koch _et al._ 2019) – A massive collection of CAD models (mechanical parts). POC-SLT fine-tuned on ~100K ABC meshes to learn symmetric completions.
    
- **ModelNet40** – (Pre-2021) common for classification, occasionally used for synthetic shape tasks.
    

## Quantitative Results and Benchmarks

Completion methods are evaluated with metrics such as Chamfer Distance (CD), Hausdorff Distance (HD/UHD), F-Score (fraction of points within a threshold), Intersection-over-Union (IoU) of voxels, or L1 error on SDF grids.

- **Fidelity vs Diversity:** Cheng _et al._ report SDFusion’s performance on completion: it achieves both **higher fidelity and diversity** than baselines. For example, on ShapeNet completion SDFusion attains UHD=0.0557 vs MPC’s 0.0627, and TMD=0.0855 vs AutoSDF’s 0.0341 (lower UHD and higher TMD are better). On single-view (Pix3D), SDFusion’s reconstructions have lower CD (1.852 vs 2.267) and higher F-Score (0.432 vs 0.415) than the next-best (AutoSDF).
    
- **AutoSDF:** In bottom-half completion tasks on ShapeNet, AutoSDF achieves UHD≈0.0567, outperforming MPC’s 0.0627. This indicates more accurate completions (smaller distance). AutoSDF also matches or beats a point-based PoinTr in diversity (its TMD score 0.0341 vs 0 for PoinTr, since PoinTr produces a single output).
    
- **DiffComplete:** Reports ~40% reduction in L1 error compared to prior methods. Its samples look more realistic and closer to ground truth than deterministic models.
    
- **PatchComplete:** Outperforms earlier unseen-category methods by large margins. Cai _et al._ report that their completion errors (CD) are ~19% lower than the previous best on synthetic scans, and ~9% lower on real scans.
    
- **Others:** Many papers include category-by-category results. For example, ShapeMorph’s WACV 2025 paper shows higher F-Scores than SFormer, PVD, etc., on PartNet chair and table splits (see their Table).
    

A summary comparison is in **Table 1** below. Note that reported performance depends on the exact task setup, so numbers are illustrative:

|Model (Ref)|Input|Output|Key Idea|Datasets|
|---|---|---|---|---|
|**PoinTr** (ICCV ’21)|Partial points|Point cloud|Set-to-set transformer|ShapeNet (55→3 cats)|
|**AutoSDF** (CVPR ’22)|Partial TSDF voxels|SDF grid|Autoregressive discrete SDF|ShapeNet|
|**SDFusion** (CVPR ’23)|Partial TSDF, image, text|Latent SDF|Latent diffusion on SDF|ShapeNet, BuildingNet, Pix3D|
|**Diffusion-SDF** (ICCV ’23)|Partial TSDF, image|Neural SDF|Diffusion over SDF network|ShapeNet, real scans|
|**SC-Diff** (ECCV ’24)|Partial TSDF, image|Latent TSDF|3D latent diffusion (single model)|Multi-class ShapeNet|
|**DiffComplete** (NeurIPS ’23)|Partial scans|Occupancy|Conditional diffusion w/ fusion|Synthetic + ScanNet|
|**PVD** (arXiv ’21)|Partial points + voxels|Points/voxels|Point-voxel diffusion|ShapeNet|
|**POC-SLT** (arXiv ’24)|Partial SDF patches|SDF grid|Latent SDF MAE transformer|ShapeNet, ABC|
|**PatchComplete** (NeurIPS ’22)|Partial SDF patches|SDF grid|Multi-res patch priors (attention)|ShapeNet|
|**NeuSDFusion** (2024)|Points → Tri-plane|SDF (tri-plane)|Spatial-aware transformer+diffusion|ShapeNet|
|**Spectral-3D** (WACV ’25)|Partial TSDF|TSDF grid|Spectral FFT module + attention|ShapeNet, real scans|
|**WSSC** (ICCV ’24)|Partial voxels|Voxel grid|Weakly-supervised shape priors|Seen / unseen ShapeNet|

_Table 1: Recent 3D completion methods (2021–2025). “Input”/“Output” indicate the representation (point cloud, TSDF, etc.). Key features and example datasets are shown. References are given for further details._

## Datasets

Common benchmarks include synthetic and scanned indoor objects: ShapeNet and BuildingNet for synthetic CAD models, Pix3D for image-guided reconstructions, PartNet for articulated furniture, and ScanObjectNN or MultiScan for real partial scans. Many papers report completing the bottom-half or random holes of ShapeNet shapes (see Cheng _et al._, Chen _et al._). Real-world tests use segmented objects from ScanNet/Matterport or the PARIS/Redwood scans. The ABC dataset of CAD parts is also used (e.g. POC-SLT fine-tunes on 100K ABC models). Despite these efforts, there is no single unified benchmark – results depend on category splits, resolution, and input type (e.g. depth vs. point clouds).

## Limitations and Open Challenges

- **Generalization & Data Gaps:** Most models train on a fixed set of categories and see ground-truth complete shapes. In practice, “supervised” completion fails on unseen categories. Some recent works (PatchComplete, WSSC) explicitly address this, but broader generalization remains hard. Moreover, real scans often have noise and missing regions unlike synthetic data, and large real datasets with paired partial/full objects are rare.
    
- **Resolution & Detail:** High detail requires fine grids. However, many 3D generators are limited to coarse voxels (e.g. 32^3) or few points by GPU memory. Approaches like SDFusion mitigate this by encoding shapes to latent, but very high resolution (millions of voxels) is still infeasible. Thin structures and fine geometry can be lost at low resolution.
    
- **Compute & Efficiency:** Diffusion and transformer models incur heavy computation. Inference may require hundreds of diffusion steps or large attention maps. This can be too slow for real-time use. Autoencoding or latent diffusion (SDFusion, SC-Diff) help, but faster algorithms are needed.
    
- **Diversity vs. Accuracy:** Generative models (diffusion, GANs) can produce multiple plausible completions, but selecting the “best” one is non-trivial. Many methods default to a single mean prediction or don’t quantify uncertainty. Balancing fidelity (accuracy) with diversity (multiple modes) is an open problem.
    
- **Multi-Modal Fusion:** Some works (SDFusion, NeuSDFusion) begin to combine partial 3D scans with images or text. Jointly leveraging 2D and 3D information is promising but complex. Guiding completion with RGB or semantics is still early-stage.
    
- **Scene-Level Completion:** Nearly all reviewed methods handle one object at a time. Extending completion to cluttered scenes (multiple interacting objects, room-scale layouts) is significantly harder and largely unexplored in deep SDF models.
    

In summary, recent methods have greatly advanced single-object completion – especially via implicit SDF representations, latent diffusion, and transformers. However, challenges of real-data robustness, high resolution, and efficiency remain active research areas. Ongoing work is likely to leverage large pre-trained 3D models and multi-modal cues to push 3D completion closer to practical real-world use.

**Sources:** Key methods and results are documented in the cited literature, and in linked official papers and code repositories.