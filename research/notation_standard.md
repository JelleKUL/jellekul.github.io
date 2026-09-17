# Mathematical Notation Standard

This document consolidates the notation used across all chapters and papers of this thesis and serves as the authoritative reference for any new writing.

---

## 1. General Conventions

| Convention | Meaning | Example |
|---|---|---|
| Lowercase bold $\mathbf{x}$ | 3D vector or point | $\mathbf{p}_i$, $\mathbf{x}$, $\mathbf{o}$ |
| Uppercase bold $\mathbf{T}$ | Transformation matrix | $\mathbf{T}_{P_s}$, $\mathbf{T}_s$ |
| Uppercase italic $P$, $Q$ | Point set / collection | $P = \{\mathbf{p}_i\}_{i=1}^{N}$ |
| Uppercase calligraphic $\mathcal{G}$ | Named set of structured objects | $\mathcal{G} = \{G_i\}_{i=1}^{N}$ |
| Uppercase $M$, $G$, $V$ | Geometric structures (mesh, voxel grid) | $M = (V, E, F)$ |
| Subscript index $i$, $j$, $k$ | Element index | $\mathbf{p}_i$, $g_{ijk}$ |
| Superscript source identifier | Which dataset/session | $X_r'$, $X_s'$, $\boldsymbol{P'_r}$ |
| $|\cdot|$ | Cardinality of a set | $|P|$, $|A \cap B|$ |
| $\|\cdot\|$ | Euclidean norm | $\|p_i - q_j\|$ |
| $\mathbb{R}^n$ | $n$-dimensional real space | $\mathbf{p}_i \in \mathbb{R}^3$ |
| $\argmin$, $\argmax$ | Argument of the minimum/maximum (operator form) | $\argmin_{\mathbf{T}} \sum \rho(\cdot)$ |

---

## 2. Points, Point Clouds, and Geometry

### Point cloud

A point cloud is a set of $N$ sampled 3D points:
$$P = \{\mathbf{p}_i\}_{i=1}^{N}$$

Each point carries geometric and optionally appearance attributes:
$$\mathbf{p}_i = (x_i,\, y_i,\, z_i,\; n_{x,i},\, n_{y,i},\, n_{z,i},\; r_i,\, g_i,\, b_i)$$

where $(x_i, y_i, z_i) \in \mathbb{R}^3$ is the 3D position, $(n_{x,i}, n_{y,i}, n_{z,i}) \in \mathbb{R}^3$ is the surface normal, and $(r_i, g_i, b_i)$ are colour attributes.

**Convention:** $P$ and $Q$ denote generic point sets. When a reference/session distinction is needed, subscripts $r$ and $s$ are used: $P_r$ for reference data, $P_s$ for session data. Primed versions ($P'$, $\boldsymbol{P'_r}$) denote selected subsets.

**Scanner origin:** $\mathbf{o} \in \mathbb{R}^3$ denotes the scanner position.

### Mesh

$$M = (V, E, F)$$

where:
- $V = \{\mathbf{v}_i\}_{i=1}^{N_v}$, with $\mathbf{v}_i = (x_i, y_i, z_i)$
- $E = \{(\mathbf{v}_i, \mathbf{v}_j)\}$ — edges between neighbouring vertices
- $F = \{(\mathbf{v}_i, \mathbf{v}_j, \mathbf{v}_k)\}$ — triangular faces

### Triangle geometry (mesh cutting, interpolation)

New triangles from face subdivision are written as ordered point triples:
$$f_{i1} = \{p_{ei},\, p_{ej},\, p_{i1}\}$$

Linear interpolation of a new edge point $p_e$ from existing points $p_1$, $p_2$ with 2D coordinates $p_i(x,y)$ and 3D coordinates $p_{i,3D}(x,y,z)$ uses the standard parametric form. Surface normals $\vec{n}_{p_e}$ are determined analogously.

### UV texture coordinates

For a mesh vertex $\mathbf{v}_i$:
$$\mathbf{t}_i = (u_i,\, v_i), \qquad (u_i, v_i) \in [0,1]^2$$

---

## 3. Transformations

### Rigid body transformation

$$\mathbf{T} \in SE(3)$$

A rigid body transformation consists of a rotation $\mathbf{R} \in SO(3)$ and a translation $\mathbf{t} \in \mathbb{R}^3$. In homogeneous coordinates it is written as the $4 \times 4$ matrix:
$$\mathbf{T} = \begin{bmatrix} \mathbf{R} & \mathbf{t} \\ \mathbf{0}^\top & 1 \end{bmatrix}$$

**Session pose:** $\mathbf{T}_s$ — the transformation of a new captured session into the reference coordinate system.

**Per-image pose:** $\mathbf{T}_{i_s}$ — the pose of a single session image.

**Source-specific variants:** $\mathbf{T}_{P_s}$ (point-cloud-based), $\mathbf{T}_{I}$ (image-based).

**Collection of candidate transforms:** $\boldsymbol{T} = \{\mathbf{T}_P,\, \mathbf{T}_I\}$

---

## 4. Alignment and Registration

### Feature correspondences

- $X_r'$, $X_s'$ — matched 3D feature points in the reference and session datasets respectively.

### FPFH registration objective (Fast Global Registration)

$$\argmin_{\mathbf{T}_{P_s}} \sum_{X_r',\, X_s'} \rho\!\left(\| X_r' - \mathbf{T}_{P_s}\, X_s' \|\right)$$

where $\rho$ is a robust correspondence estimation function.

### Raycasting for 3D--2D correspondences

$$\mathbf{X} = \left\{ p \in P \;\Big|\; l(c,x) \in L :\; p = l(c,x) \cap P \right\}$$

where $L$ is the set of rays from the focal point $c$ through matched pixels, and $P$ is the available geometry.

### Weighted pose vote

$$\mathbf{T}_{s} = \frac{1}{n} \sum_{\boldsymbol{T}} \omega\!\left(\mathbf{T}_{P},\, \mathbf{T}_{I}\right)$$

where $\omega$ is a weight function derived from reprojection error, number of matches, inlier percentage, spatial extent of inliers, and method-specific empirical weights (SUPER4PCS: 1.0; FPFH / SfM two overlapping: 0.8; two separate references: 0.5; one reference with geometry: 0.4).

### Spatial sub-selection for global alignment

$$\boldsymbol{P'_r} = \left\{ P_r \in \boldsymbol{P_r} \;\Big|\; P_r \cap \left[P_{s,\min} - \sigma_g;\; P_{s,\max} + \sigma_g\right] \right\}$$
$$I'_r = \left\{ i_r \in I_r \;\Big|\; i_s \in I_s :\; \|i_r(c) - i_s(c)\| \leq t_d + \sigma_g \right\}$$

where $\sigma_g$ is the global positioning uncertainty and $t_d$ is a distance threshold. $i_r(c)$ and $i_s(c)$ denote the focal centre of a reference and session image respectively.

---

## 5. Dataset Updating

### Coverage check

$$P' = \left\{ p_i \;\Big|\; \forall p_i \in P,\; q_j \in Q :\; \argmin_{q_j} \|p_i - q_j\| \geq t_d \right\}$$

Points in $P'$ have no nearby observation in the new session and are considered uncovered. $t_d$ is the distance threshold.

---

## 6. Voxel Grids and Spatial Structures

### Voxel grid

$$G = \{g_{ijk}\}, \qquad i, j, k \in \{1, \dots, n\}$$

Binary occupancy: $g_{ijk} \in \{0, 1\}$.

SDF/UDF-encoded voxels: $g_{ijk} = f(x_i, y_j, z_k)$.

### Octree subdivision

A node at depth $d$ representing region $\Omega_d$ subdivides as:
$$\Omega_d \rightarrow \{\Omega_{d+1}^1,\, \dots,\, \Omega_{d+1}^8\}$$

### k-d tree partitioning

$$P = P^{-} \cup P^{+}$$

where $P^-$ and $P^+$ are the subsets below and above the splitting plane.

---

## 7. Occlusion Detection (Ray Marching)

**Scanner position:** $\mathbf{o}$

**Input point set:** $P = \{\mathbf{p}_i\}_{i=1}^{N}$

**Voxel categories** (after traversal using a 3D Bresenham variant):
- **Observed empty**: voxel lies along the ray before the first occupied voxel.
- **Occupied**: voxel contains one or more scanned points $\mathbf{p}_i$.
- **Occluded**: voxel lies behind an occupied voxel along the ray direction.

When multiple scanner positions are used, classifications are accumulated: a voxel that is occluded from one position but observed empty from another is classified as empty.

---

## 8. Implicit Representations

### Signed Distance Function (SDF)

$$f(\mathbf{x}) < 0 \quad \text{inside the object}$$
$$f(\mathbf{x}) = 0 \quad \text{on the surface}$$
$$f(\mathbf{x}) > 0 \quad \text{outside the object}$$

The surface is the zero level set of $f$.

### Neural implicit representation

A neural network with parameters $\theta$ defines:
$$f_\theta : \mathbb{R}^3 \rightarrow \mathbb{R}^m$$

### Neural Radiance Field (NeRF)

$$F_\theta(\mathbf{x}, \mathbf{d}) = (\sigma,\, \mathbf{c})$$

where $\mathbf{x} = (x, y, z)$ is the spatial coordinate, $\mathbf{d}$ is the viewing direction, $\sigma$ is volumetric density, and $\mathbf{c} = (r, g, b)$ is view-dependent radiance.

### 3D Gaussian Splatting

$$\mathcal{G} = \{G_i\}_{i=1}^{N}, \qquad G_i = (\boldsymbol{\mu}_i,\, \Sigma_i,\, \alpha_i,\, \mathbf{s}_i)$$

where $\boldsymbol{\mu}_i \in \mathbb{R}^3$ is the centre, $\Sigma_i \in \mathbb{R}^{3 \times 3}$ the covariance, $\alpha_i \in [0,1]$ the opacity, and $\mathbf{s}_i$ the spherical harmonic coefficients.

---

## 9. Evaluation Metrics

### Chamfer Distance (CD)

$$\text{CD}(P, Q) = \frac{1}{|P|} \sum_{p \in P} \min_{q \in Q} \|p - q\|^2 \;+\; \frac{1}{|Q|} \sum_{q \in Q} \min_{p \in P} \|q - p\|^2$$

Lower is better ($\downarrow$). Used for geometric evaluation of completed objects and scene elements.

### Intersection over Union (IoU)

$$\text{IoU}(A, B) = \frac{|A \cap B|}{|A \cup B|}$$

Range: $[0, 1]$, higher is better ($\uparrow$). Applied to voxel grids for volumetric comparison and to oriented bounding boxes for detection evaluation (threshold commonly 0.25 or 0.5).

### Precision and Recall

$$\text{Precision} = \frac{\text{TP}}{\text{TP} + \text{FP}}, \qquad \text{Recall} = \frac{\text{TP}}{\text{TP} + \text{FN}}$$

Range: $[0, 1]$, higher is better ($\uparrow$). Used for 3D object detection.

### Peak Signal-to-Noise Ratio (PSNR)

$$\text{PSNR}(\hat{I}, I) = 10 \cdot \log_{10}\!\left(\frac{L^2}{\text{MSE}(\hat{I}, I)}\right)$$

where $\text{MSE}(\hat{I}, I) = \frac{1}{N}\sum_{i=1}^{N}(\hat{I}_i - I_i)^2$ and $L$ is the maximum pixel value (255 for 8-bit images). Higher is better ($\uparrow$). Used for texture and scene completion.

### Structural Similarity Index Measure (SSIM)

$$\text{SSIM}(x, y) = \frac{(2\mu_x \mu_y + c_1)(2\sigma_{xy} + c_2)}{(\mu_x^2 + \mu_y^2 + c_1)(\sigma_x^2 + \sigma_y^2 + c_2)}$$

where $\mu_x$, $\mu_y$ are local means, $\sigma_x^2$, $\sigma_y^2$ local variances, $\sigma_{xy}$ the cross-covariance, and $c_1$, $c_2$ stabilisation constants. Range: $[-1, 1]$, higher is better ($\uparrow$).

### Learned Perceptual Image Patch Similarity (LPIPS)

$$\text{LPIPS}(\hat{I}, I) = \sum_l \frac{1}{H_l W_l} \sum_{h,w} \| w_l \odot (\hat{\phi}_l^{hw} - \phi_l^{hw}) \|^2$$

where $\phi_l^{hw}$ and $\hat{\phi}_l^{hw}$ are unit-normalised feature activations at spatial position $(h,w)$ in layer $l$, and $w_l$ are learned per-channel weights. Lower is better ($\downarrow$).

### Texture Similarity (TexSim)

$$\text{TexSim}(T_1, T_2) = \frac{f(T_1) \cdot f(T_2)}{\|f(T_1)\| \cdot \|f(T_2)\|}$$

where $f(\cdot)$ is a pretrained EfficientNet feature extractor. Range: $[-1, 1]$, higher is better ($\uparrow$). Used for texture completion evaluation.

---

## 10. Summary Table

| Symbol | Type | Meaning |
|---|---|---|
| $\mathbf{p}_i$ | vector | A 3D point |
| $P$, $Q$ | set | Point cloud (unordered set of $\mathbf{p}_i$) |
| $P_r$, $P_s$ | set | Reference / session point cloud |
| $P'$, $\boldsymbol{P'_r}$ | set | Selected subset of a point cloud |
| $\mathbf{o}$ | vector | Scanner origin position |
| $\mathbf{T}$ | matrix | Rigid body transformation ($SE(3)$) |
| $\mathbf{T}_s$, $\mathbf{T}_{P_s}$, $\mathbf{T}_I$ | matrix | Session / point-cloud / image-based transformation |
| $\mathbf{R}$ | matrix | Rotation component of $\mathbf{T}$ |
| $\mathbf{t}$ | vector | Translation component of $\mathbf{T}$ |
| $\sigma_g$ | scalar | Global positioning uncertainty |
| $t_d$ | scalar | Distance threshold |
| $\omega$ | scalar | Alignment method weight |
| $\rho$ | function | Robust loss / correspondence weighting function |
| $M = (V, E, F)$ | struct | Triangle mesh |
| $G = \{g_{ijk}\}$ | grid | Voxel grid |
| $\Omega_d$ | region | Octree node at depth $d$ |
| $f(\mathbf{x})$ | scalar field | SDF / UDF value at position $\mathbf{x}$ |
| $f_\theta$ | network | Neural implicit function with parameters $\theta$ |
| $G_i = (\boldsymbol{\mu}_i, \Sigma_i, \alpha_i, \mathbf{s}_i)$ | struct | 3D Gaussian primitive |
| $i_r$, $i_s$ | image | Reference / session image |
| $i_r(c)$ | vector | Focal centre of image $i_r$ |
| $L$ | set | Set of camera rays |
| $\mathbf{t}_i = (u_i, v_i)$ | vector | UV texture coordinate |
| $\hat{I}$, $I$ | image | Predicted and reference image |
| $\mu_x$, $\sigma_x^2$ | scalars | Local mean and variance (SSIM) |
| $\phi_l^{hw}$ | vector | Feature activation at layer $l$, position $(h,w)$ |
| $f(\cdot)$ (TexSim) | function | EfficientNet feature extractor |
| TP, FP, FN | count | True positive, false positive, false negative |

---

## 11. Formatting Rules

- All vectors and matrices are typeset in **bold**: $\mathbf{p}$, $\mathbf{T}$, $\mathbf{R}$.
- Scalars and scalar fields are typeset in *italic*: $t_d$, $\sigma_g$, $f(\mathbf{x})$.
- Sets are typeset in uppercase italic: $P$, $Q$, $L$.
- Named sets of structured objects use calligraphic: $\mathcal{G}$.
- Operators $\argmin$ and $\argmax$ are typeset as math operators (not italic text).
- All equations are numbered using the chapter-prefixed label convention `eq:CH_name` (e.g., `eq:4_coverage`).
- Inline equations use `$...$`; display equations use the `equation` or `equation + gathered/split` environments.
- The `\Big|` separator is used inside set-builder notation for clarity.
- Threshold and distance parameters are lowercase italic scalars ($t_d$, $\sigma_g$).
- Do not use `---` (em-dashes) in prose; use commas, colons or restructure the sentence.
