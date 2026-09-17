# Certainty-Modeled 3D Scene Representation — State of the Art

**A survey of the pieces needed to unify measured and generated 3D geometry under a single, continuous, calibrated uncertainty field.**

---

## The core idea

The paradigm being explored here: represent a captured 3D scene (e.g. a LiDAR scan of an indoor environment) not as a set of binary "present/absent" points, but as a **continuous certainty field** over a **volumetric representation**, where:

- Every *measured* point carries an uncertainty reflecting sensor noise, ghosting, and mixed-edge effects — the scanner is already making a decision about where to draw a point, and that decision should be made explicit as a certainty volume.
- Every *generated* point (filling unobserved regions) carries its own certainty, derived from the generative model's confidence.
- Both are expressed in the **same formalism**, so measurement and generation decisions are viewed "in the same light."
- Certainty is **calibrated** against accuracy, so the confidence value actually predicts error.

This document maps the existing literature onto the four threads that make up that idea, and identifies where the genuine open gaps are.

**Bottom line:** No single named framework does all of this yet, but every ingredient exists. One recent paper (Trust3R) implements the key conceptual move — measured and inferred geometry under one probabilistic formalism. The open contribution is a *unified, calibrated, volumetric fusion* of measured and generated geometry, done LiDAR-native indoors.

---

## 1. Continuous per-point certainty from the sensor side

The most mature thread. The scanner's implicit "where to draw the point" decision becomes an explicit covariance ellipsoid per point — which is already a certainty volume.

**Key building block — on-manifold LiDAR noise model (Yuan et al.):** the standard way to attach a covariance to each measured point based on range, bearing, and beam geometry. Later extended to relate point uncertainty to the uncertainty of an estimated plane (i.e. map-level uncertainty). A 2024 refinement (Huang et al.) added **incident angle and surface roughness** — directly relevant to mixed-edge pixels and grazing-angle ghosting.

- [Safety-Critical LiDAR-Inertial Odometry with On-Manifold Deterministic Protection Level](https://arxiv.org/pdf/2605.09383) — good entry point; reviews the Yuan et al. lineage and the incident-angle/roughness extensions.
- [Semantic Point Cloud Mapping of LiDAR Based on Probabilistic Uncertainty Modeling for Autonomous Driving](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7588973/) — explicit probability models for LiDAR points, used for semantic registration and class disambiguation across scans.
- [Uncertainty assessment and probabilistic change detection using terrestrial and airborne LiDAR](https://www.researchgate.net/publication/261723452_Uncertainty_assessment_and_probabilistic_change_detection_using_terrestrial_and_airborne_LiDAR) — Bayesian uncertainty maps from point clouds; argues explicitly against a single global accuracy number.

**Metrology / high-precision side — spatial error fields:**

- [Uncertainty Evaluation of Dense Point Clouds Using Regional Gaussian Random Fields](https://papers.ssrn.com/sol3/Delivery.cfm/eabe6a24-122d-4560-9aff-99554b323723-MECA.pdf?abstractid=4403084&mirid=1) — fits Gaussian Random Fields to model measurement error *spatially* rather than globally; the regional variant makes this tractable at full-scan density.

---

## 2. Certainty for *generated* points

Thinner and less settled — this is where a contribution is most likely to sit. Three distinct approaches:

**(a) Explicit "uncertain region" prediction in completion** — the most on-the-nose:

- [Shape Completion with Prediction of Uncertain Regions](https://arxiv.org/html/2308.00377v2) — flags which generated regions are ambiguous; explicitly notes that prior work targets *known* objects with no UQ under shape variation.

**(b) Generative completion as posterior sampling** — the spread across multiple plausible completions is itself an epistemic uncertainty estimate:

- [Multimodal Shape Completion via IMLE](https://arxiv.org/pdf/2106.16237) — generates diverse plausible completions for a missing region.
- [DualGenerator: Information Interaction-based Generative Network for Point Cloud Completion](https://arxiv.org/pdf/2305.09132) — dual adversarial + variational paths for completing unknown regions.
- [P2C: Self-Supervised Point Cloud Completion from Single Partial Clouds](https://openaccess.thecvf.com/content/ICCV2023/papers/Cui_P2C_Self-Supervised_Point_Cloud_Completion_from_Single_Partial_Clouds_ICCV_2023_paper.pdf) — separates observed vs. unseen regions in the loss, relevant to weighting generated points differently.
- [Self-Supervised Point Cloud Completion via Inpainting](https://arxiv.org/pdf/2111.10701) — inpainting-based completion trained from partial clouds only.

**(c) Completion-by-correction** — philosophically closest to the "same light" idea: measurement and generation as one pipeline, with observation *correcting* a generative substrate:

- [Rethinking Multimodal Point Cloud Completion: A Completion-by-Correction Perspective](https://arxiv.org/html/2511.12170) (Nov 2025) — starts from a complete generative prior and grounds/corrects it in the partial observation, rather than inpainting holes.

**Related — uncertainty in registration/overlap:**

- [UTOPIC: Uncertainty-aware Overlap Prediction Network for Partial Point Cloud Registration](https://arxiv.org/pdf/2208.02712) — per-point overlap uncertainty; useful for down-weighting uncertain regions in fusion.

---

## 3. The unifying move — one formalism for measured *and* inferred points

The intellectual core. The closest existing realization is **evidential deep learning (EDL)**, which gives every point a full predictive distribution with a principled split into two uncertainty types that map cleanly onto measured-vs-generated:

- **Aleatoric** = observational noise / inherent ambiguity → a noisy measured edge point.
- **Epistemic** = weak evidence / limited coverage → a generated point in an unobserved region.

Both are expressed in the same units — this *is* the move from binary to a continuous field. EDL also gives closed-form decomposition from a single forward pass (no Monte Carlo at inference), and compact per-point evidential parameters that propagate through alignment, fusion, and filtering.

- [Trust It or Not: Evidential Uncertainty for Feed-Forward 3D Reconstruction with Trust3R](https://arxiv.org/pdf/2605.19539) — **the closest conceptual bridge.** Each reconstructed point is a full predictive distribution; principled aleatoric/epistemic decomposition; ambiguity attributed either to intrinsic scene properties (textureless regions) or to limited training coverage.
- [Uncertainty Estimation for 3D Object Detection via Evidential Learning](https://arxiv.org/html/2410.23910v1) — EDL head on a BEV representation; per-cell uncertainty; out-of-distribution scene detection.
- [Uncertainty Estimation for Deep Reconstruction in Disaster Scenarios](https://arxiv.org/html/2604.06387) — makes the actionable point that **only epistemic uncertainty is reducible by collecting more data** (i.e. tells you where to scan next); unified observation-model framing; analyses epistemic-uncertainty↔error correlation.

**Foundational EDL references** (Amini et al. evidential regression; Sensoy et al. evidential classification) are cited throughout the above and are the theoretical backbone.

---

## 4. Volumetric representation + correlating certainty with accuracy

### The volumetric target

Two existing forms of "measurement as a certainty volume":

- **Per-point covariance ellipsoids** from the LiDAR noise models in §1.
- **Spatial Uncertainty Field (SUF)** — an explicit continuous field of geometric uncertainty across a scene, from the Gaussian-splatting line. Note that a 3D Gaussian with an opacity/confidence is *already* a volumetric certainty primitive.

Gaussian-splatting uncertainty work (mostly image/photometric-driven — porting to LiDAR-native is open ground):

- [Uncertainty-aware Normal-Guided Gaussian Splatting (UNG-GS)](https://arxiv.org/html/2503.11172v1) — introduces the explicit Spatial Uncertainty Field.
- [UncertainGS: Uncertainty-aware indoor reconstruction via Gaussian splatting](https://www.sciencedirect.com/science/article/abs/pii/S0925231225028085) — **indoor-specific**; predicts uncertainty of geometric prior cues to guide optimization.
- [UA-GS: Uncertainty-Aware Gaussian Splatting with View-Dependent Regularization](https://diglib.eg.org/items/2396ac8a-1d30-40a8-8e51-c03cc4961c46) — view-dependent uncertainty for resolving multi-view inconsistency.
- [USplat4D: Uncertainty Matters in Dynamic Gaussian Splatting](https://arxiv.org/abs/2510.12768) — per-Gaussian, time-varying uncertainty; well-observed Gaussians act as reliable anchors.

### The accuracy ↔ certainty correlation

A well-defined, measurable question. The tooling is standard **calibration**: does predicted uncertainty actually track realized error?

- **Reliability diagram / Y=X calibration plot** — a well-calibrated estimator has estimation errors falling within the predicted uncertainty threshold at the predicted frequency.
- **ECE** (Expected Calibration Error) for classification-style confidence.
- **ENCE** (Expected Normalized Calibration Error) — the regression variant; the right metric for continuous per-point uncertainty.

References:

- [Multi-view 3D Object Reconstruction and Uncertainty Modelling with Neural Shape Prior](https://arxiv.org/pdf/2306.11739) — spells out the calibration-plot property (errors within predicted threshold; Y=X line); propagates 3D uncertainty into view via differentiable rendering.
- [MUSE: Multimodal Uncertainty Quantification of State Estimation](https://arxiv.org/pdf/2605.17421) — uses ENCE for regression calibration; clear metric definitions.
- [Query2Uncertainty: Robust UQ and Calibration for 3D Object Detection under Distribution Shift](https://arxiv.org/pdf/2605.05328) — D-ECE (detection-adapted ECE) under distribution shift.
- [Conformalized Generative Bayesian Imaging](https://arxiv.org/pdf/2504.07696) — aleatoric via generative posterior sampling, epistemic via Bayesian nets; useful cross-domain framing for calibrated generative confidence.

---

## Where the open gap actually is

Putting it together:

| Thread | Maturity |
|---|---|
| Sensor-side per-point uncertainty (§1) | Mature |
| Calibration machinery (§4) | Standard, off-the-shelf |
| Unifying evidential formalism (§3) | Recently available (Trust3R, 2026) |
| Volumetric certainty primitives (§4) | Exists (covariances, SUF, Gaussians) |
| **Calibrated generative confidence for completion** | **Open** |
| **Single field fusing measured + generated uncertainty** | **Open** |
| **LiDAR-native indoor version of the above** | **Open** |

The genuinely open pieces:

1. **A single representation** where a measured point's aleatoric covariance and a generated point's epistemic evidence live in the *same* field and fuse consistently. Most work does one or the other.
2. **Calibrated generative confidence** for scene completion, validated against real held-out geometry. Completion papers mostly report Chamfer distance, not calibration.
3. **LiDAR-native, indoors** rather than image-native.

A defensible framing not currently claimed by anyone found here:

> *Calibrated, evidential, volumetric fusion of measured and generated geometry under a unified uncertainty field.*

**Two questions sitting right on the novelty boundary, worth resolving before committing:**

- Can Trust3R's evidential head be **conditioned on an explicit sensor noise prior** (rather than learning aleatoric purely from data)?
- Can the **completion-by-correction** framing be made **evidential**?

---

## Quick reference — all papers by thread

**Sensor uncertainty:** on-manifold LiDAR odometry ([2605.09383](https://arxiv.org/pdf/2605.09383)) · semantic probabilistic mapping ([PMC7588973](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7588973/)) · probabilistic change detection ([RG 261723452](https://www.researchgate.net/publication/261723452_Uncertainty_assessment_and_probabilistic_change_detection_using_terrestrial_and_airborne_LiDAR)) · regional GRF ([SSRN 4403084](https://papers.ssrn.com/sol3/Delivery.cfm/eabe6a24-122d-4560-9aff-99554b323723-MECA.pdf?abstractid=4403084&mirid=1))

**Generative point certainty:** uncertain regions ([2308.00377](https://arxiv.org/html/2308.00377v2)) · IMLE multimodal ([2106.16237](https://arxiv.org/pdf/2106.16237)) · DualGenerator ([2305.09132](https://arxiv.org/pdf/2305.09132)) · P2C ([ICCV2023](https://openaccess.thecvf.com/content/ICCV2023/papers/Cui_P2C_Self-Supervised_Point_Cloud_Completion_from_Single_Partial_Clouds_ICCV_2023_paper.pdf)) · inpainting ([2111.10701](https://arxiv.org/pdf/2111.10701)) · completion-by-correction ([2511.12170](https://arxiv.org/html/2511.12170)) · UTOPIC ([2208.02712](https://arxiv.org/pdf/2208.02712))

**Unified evidential formalism:** Trust3R ([2605.19539](https://arxiv.org/pdf/2605.19539)) · EDL 3D detection ([2410.23910](https://arxiv.org/html/2410.23910v1)) · disaster reconstruction ([2604.06387](https://arxiv.org/html/2604.06387))

**Volumetric + calibration:** UNG-GS ([2503.11172](https://arxiv.org/html/2503.11172v1)) · UncertainGS ([S0925231225028085](https://www.sciencedirect.com/science/article/abs/pii/S0925231225028085)) · UA-GS ([EG](https://diglib.eg.org/items/2396ac8a-1d30-40a8-8e51-c03cc4961c46)) · USplat4D ([2510.12768](https://arxiv.org/abs/2510.12768)) · neural shape prior calibration ([2306.11739](https://arxiv.org/pdf/2306.11739)) · MUSE ([2605.17421](https://arxiv.org/pdf/2605.17421)) · Query2Uncertainty ([2605.05328](https://arxiv.org/pdf/2605.05328)) · conformalized generative imaging ([2504.07696](https://arxiv.org/pdf/2504.07696))

---

*Note: some arXiv IDs above (26xx.xxxxx) correspond to 2026 preprints; verify current versions and check for newer follow-up work, as this is a fast-moving area.*
