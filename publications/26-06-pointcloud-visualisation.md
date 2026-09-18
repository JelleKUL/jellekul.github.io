---
layout: publication
title: Pointcloud Visualisation
date: 2026-06-01
permalink: /publications/pointcloud-visualisation/
icon:
methodImage: img/publications/pointcloudVisualisationMethodology.png
doilink: https://lib.is/lbsn9994932389601471/representation?libis=11:1:1&lang=en
githublink: https://github.com/verlindenruben-lang/Walkthrough-Point-Cloud-Unity
objective: 3
type: Thesis
description: A thesis exploring Interactable realtime pointcloud viewing
bibtex:
---


# Realtime Interactive Visualization of 3D Scans: Evaluation of Point-Cloud Rendering in Unity

**Ruben Verlinden**

Supervisor: Prof. dr. ir. Maarten Vergauwen. Co-supervisor: Ir. arch. Jelle Vermandere

KU Leuven, Faculty of Industrial Engineering, Technology Campus Ghent, Belgium

**Keywords:** point cloud, Unity, game engine, PCX, laser scanning, visualization, JSON, runtime loader, surveying

---

## Abstract

Manually modeling realistic 3D environments for games, virtual reality, and professional walkthroughs is time-consuming. Scanning existing spaces makes reality directly available in digital form, but point clouds are normally converted to polygonal meshes first because game engines are designed for mesh-based geometry — a step that costs time and can lose detail or accuracy. This thesis investigates whether point clouds can instead be used directly within real-time, interactive applications built in Unity. Several point-cloud rendering methods were implemented and benchmarked on frame rate, visual quality, and support for large datasets. The PCX plug-in emerged as the clear winner, sustaining strong performance on clouds of up to 200 million points and matching classical mesh rendering in speed. Based on this result, PCX was used to build a practical end product: an application that lets users construct automatic camera walkthroughs through point clouds, with adjustable camera movement, route configuration, and visualization settings stored in JSON files, and with the ability to export the resulting tour as an MP4 video. The application offers two implementations — one using PCX for high-performance demonstration with pre-loaded clouds, and one using a custom-written PLY loader that supports runtime import of new point clouds. The application targets a professional audience such as surveying firms, contractors, and architectural offices, and the study also examines the technical challenges of working with very large point clouds inside Unity.

## 1. Introduction

Current workflows for interactive 3D environments typically convert scanned point clouds into meshes so they work with standard rendering pipelines, physics, and interaction systems. This conversion is time-consuming, requires extensive post-processing and often manual correction, and discards part of the original point data. This raises the question of whether point clouds can be rendered directly, skipping mesh conversion, which could yield a more efficient workflow while fully preserving the original scan information.

This thesis therefore studies to what extent point clouds are suitable for real-time applications and interactive walkthroughs, using Unity as the development environment — chosen for its accessibility and because the KU Leuven Faculty of Industrial Engineering already has the most in-house experience with it. Several point-cloud rendering methods available for Unity are compared on performance, visual quality, and ease of use, and the contribution of techniques such as Level of Detail (LOD) is considered. As a practical end product, an application is developed that lets users visualize, explore, and record scanned environments as interactive walkthroughs, aimed at surveyors, contractors, and architectural firms.

**Central research question:** _Are point clouds suitable for use in games and other industrial applications?_

## 2. Related Work

### 2.1 Point Clouds

A point cloud is a digital 3D representation built from individual points, each defined by XYZ coordinates and optionally additional attributes such as color, intensity, or normals. Point clouds are usually treated as unstructured data, though some formats (e.g. E57) can retain the original measurement structure and metadata.

Point clouds can be generated via **terrestrial laser scanning (TLS)** — static, tripod-mounted scanners offering sub-centimeter accuracy — or **dynamic/SLAM-based scanners** such as the NavVis VLX 2, which are faster to deploy over large areas but less accurate (around 1 cm, with drift limited by "loop closures" performed roughly every 15–30 m). Laser scanning can also suffer from **ghosting** (reflections on glass or highly reflective surfaces creating duplicated structures), refraction artifacts behind glass, and **mixed edges** at sharp boundaries. **Photogrammetry** is a complementary technique using overlapping photos (Structure from Motion / Multi-View Stereo); it is cheaper and captures realistic texture directly but is lighting- and texture-dependent and generally less accurate than TLS.

Point clouds are widely used in AEC (as-built models, BIM integration, progress tracking), industrial reverse engineering, heritage documentation, and increasingly in virtual reality and real-time visualization. In game development, laser scanning is mainly used to capture individual assets rather than entire levels, since levels are continuously redesigned around gameplay.

### 2.2 Visualizing Point Clouds in Game Engines

Game engines are built around mesh data (vertices and faces), so rendering millions of loose points directly is a technical challenge requiring extensions, custom shaders, or specialized techniques. Prior work by Virtanen et al. demonstrated that dense point clouds from TLS, photogrammetry, and handheld scanners can be visualized and interactively used directly in Unity via a custom octree/LOD-based extension, at a performance cost (11 ms frame time vs. 3.9 ms for mesh rendering) that remained acceptable for real-time use and VR.

Building on that line of work, this thesis evaluates several **free, ready-to-use Unity solutions**:

- **Particle System** — Unity's built-in system for dynamic effects, repurposed for static point display; runs entirely on the CPU, which limits it to small datasets.
- **VFX Graph** — points are read on the CPU but rendered on the GPU in parallel, giving much better performance than the Particle System.
- **FastPoints** — an open-source, no-code plug-in supporting drag-and-drop import of PLY/LAS/LAZ files; builds an out-of-core octree so very large datasets (larger than available memory) can still be visualized interactively, with an initial decimated preview replaced by full resolution once preprocessing completes.
- **PCX** — a GitHub plug-in supporting binary PLY files, offering three internal container types (Mesh, ComputeBuffer, Texture) and two render styles (Point, Disk); automatically splits very large clouds into multiple meshes.
- **Point Cloud Free Viewer (PCFV)** — a free Unity Asset Store package supporting `.off` files, suited to clouds above ten million points but with limited format support.

Unity's three render pipelines (Built-in, URP, HDRP) were also considered; **URP** was judged the best fit for large-scale point-cloud rendering thanks to its balance of performance and hardware compatibility.

### 2.3 Existing Walkthrough Applications

Commercial virtual-tour solutions rarely work directly with raw point clouds. **360° cameras** (e.g. Ricoh Theta) produce panoramic image-based tours that are fast and user-friendly but restrict navigation to fixed viewpoints and carry no real 3D geometry. **Smartphone apps** such as Walkly let users film a walkthrough in one continuous take and auto-generate video content for real-estate marketing, again without true 3D data. **3D-model platforms** such as Matterport automatically reconstruct full interactive 3D environments from scans, offering free navigation but at the cost of some original scan detail and typically at a commercial price. None of these approaches visualize the raw point cloud directly and in real time, which motivates the application developed in this thesis — retaining full scan detail while avoiding costly mesh reconstruction, particularly valuable in surveying, construction monitoring, and inspection contexts where point clouds already exist.

## 3. Methodology: Point-Cloud Rendering Benchmark

### 3.1 Goal

Before building the final application, the different Unity rendering methods were tested and compared on performance, compatibility, visual quality, usability, and support for large point clouds, alongside a classical mesh-rendering baseline, to determine the most suitable method for the practical end product.

### 3.2 Test Data and Benchmark Design

The primary test dataset was a ~39-million-point cloud (372 m²) of a workshop hall at KU Leuven Campus Ghent, captured with a NavVis VLX 2. The point cloud was repeatedly subsampled to create versions with different point counts, each loaded into Unity and rendered with every method. Performance was measured as average frames per second (FPS) along a fixed 30-second, 12 m × 5 m rectangular camera route (capped at 10,000 frames), with the route length chosen because FPS was found to stabilize after about 30 seconds.

### 3.3 Results: Rendering-Method Benchmark

|Method|Format|Max points tested|100k pts|1M pts|10M pts|25M pts|39M pts|
|---|---|---|---|---|---|---|---|
|Particle System|TXT|~1M|10|1|–|–|–|
|VFX Graph|TXT|~25M|478|461|316|126|–|
|PCFV|OFF|~10M|449|440|424|–|–|
|**PCX**|PLY|**~200M**|472|468|469|472|466|
|PLY-loader (custom)|PLY|~200M|490|491|458|182|128|

_Average FPS at increasing point counts._

The CPU-bound Particle System hit its limits almost immediately, while GPU-based methods scaled far better. **PCX** delivered consistently high FPS across all tested sizes — comparable to a classical mesh baseline (462–487 FPS for meshes of 100k–3.5M vertices) — and remained stable even on the full 39-million-point cloud. Looking at the average of the lowest 1% of FPS values (a proxy for stutter/stability), PCX and the custom PLY loader again led the pack, staying above the 30 FPS threshold needed for smooth video output across all tested sizes up to 39 million points.

A further test on a larger, ~190-million-point garage scan (subsampled to 55–190 million points) confirmed PCX's scalability:

|Points|PCX avg FPS|PCX lowest-1% FPS|PLY-loader avg FPS|PLY-loader lowest-1% FPS|
|---|---|---|---|---|
|55M|371|215|91|68|
|70M|304|178|78|56|
|110M|181|107|52|34|
|145M|133|70|41|21|
|190M|100|55|33|17|

PCX stayed comfortably above real-time thresholds even at 190 million points, though loading such a cloud as a Unity asset took over ten minutes. The custom PLY loader remained usable (≥30 FPS on average) up to the full dataset, but its lowest-1% FPS dropped below the 30 FPS video-smoothness threshold somewhere between 110 and 145 million points, whereas PCX did not.

A separate visual comparison (point cloud vs. mesh, both reduced to ~100,000 elements) showed that a mesh retains a recognizable, coherent shape thanks to its connected faces, while an equivalently sized point cloud loses much more spatial readability — meaning point clouds need sufficiently high density to remain visually convincing, and increasing point size can partly compensate at lower resolutions.

### 3.4 Conclusion of the Benchmark

**PCX** was selected as the visualization method for the rest of the thesis, based on its excellent performance, stable support for very large point clouds, and high visual quality relative to the alternatives. The custom PLY loader is a solid runtime-capable alternative, usable both in the Editor's Scene View and at runtime in Play Mode, though less performant than PCX at extreme scales.

## 4. Application

### 4.1 Goal

Building on the PCX result, an application was developed for surveyors and other professionals who scan buildings or construction sites. It lets users open a point cloud and automatically generate a camera walkthrough, with adjustable camera movement and visualization settings, and export the tour as a video file — useful for real-estate presentation without a physical visit, or for construction progress tracking and communication.

### 4.2 Design and Implementation

The application was built and tested on a ~190-million-point scan of a car garage. It offers **two complementary implementations**:

- A **PCX-based** implementation, used as a high-performance demonstration but limited to a pre-loaded library of point clouds, since PCX cannot import new clouds at runtime.
- A **custom PLY-loader** implementation supporting runtime import of arbitrary point clouds, better suited to real-world practical use despite being somewhat less performant on extremely large datasets.

The application is driven by two main scripts: `SmoothRoute`, which computes the camera path (supporting four interpolation methods) and drives the camera along it, and `Tourbuilder`, which handles all user interaction — placing waypoints, managing building floors, starting the tour, and camera control.

A notable technical issue addressed was **coordinate precision**: point clouds georeferenced in the Belgian Lambert 72 system have coordinates up to nearly 300,000 m, and Unity's internal 32-bit floats lose precision at that scale. A Python preprocessing script converts coordinates from 64-bit double to 32-bit float and re-centers the cloud on its centroid (shifting it to the origin) while preserving relative point positions, so the cloud displays correctly without loss of visual fidelity.

The application also supports multi-floor buildings: users can define named floors with heights, and the camera's near/far clipping planes are automatically adjusted so only the points of the selected floor remain visible during that segment of the tour. Settings (camera movement, route configuration, visualization parameters) are saved and loaded via JSON, and completed walkthroughs can be recorded and converted to MP4 video.

### 4.3 Evaluation

The evaluation focused on two questions: (1) can a point cloud alone visually convey a building's interior and exterior well enough for practical surveying use, avoiding a time-consuming conversion to mesh or BIM, and (2) how many points can the application handle before performance degrades?

**Visual aspect:** Both implementations produce clear, usable walkthroughs of full building scans, adequate for professional presentation and exploration. However, because point clouds have no closed surfaces, cameras moving close to walls or objects can "see through" the point structure — an effect that is more pronounced in small or densely furnished rooms (where the camera is often close to surfaces) and less noticeable in large, open spaces. Increasing point size partially mitigates this at typical viewing distances.

**Point-count limits:** Consistent with the Chapter 3 benchmark, PCX remained performant up to 200 million points, and the custom runtime loader handled the same scale without issues.

### 4.4 Conclusion

Point clouds proved to be a suitable interactive-visualization method for professional applications where the main goal is presenting a scanned environment quickly — such as client presentations, progress tracking, or internal review — since a cloud can be used almost immediately after scanning and cleanup, without mesh reconstruction. Quality of the result still depends heavily on the quality of the original scan. Because of the "see-through" limitation inherent to unconnected points, the approach is **less suited** to compact interior spaces requiring a fully closed, realistic look (e.g. residential real estate) and **well suited** to larger, more open environments such as industrial buildings, warehouses, production halls, and construction sites in shell/rough-build phase.

## 5. Conclusion

This thesis examined whether point clouds can be used directly for interactive visualization in Unity, without prior mesh conversion. A systematic benchmark of five rendering methods (Particle System, VFX Graph, Point Cloud Free Viewer, PCX, and a custom PLY loader) against a mesh baseline showed that CPU-bound rendering (Particle System) is unsuitable for large, dense clouds, while GPU-based methods scale far better. **PCX** was the clear winner, rendering point clouds at speeds comparable to classical meshes even at hundreds of millions of points — demonstrating that direct point-cloud visualization in Unity is technically viable.

Using PCX, a practical **walkthrough application** was built with two implementations (pre-loaded PCX clouds for maximum performance, and a custom runtime PLY loader for flexibility), allowing users to explore a full scan and automatically generate video visualizations. This is a clear asset for professional workflows valuing speed and direct use of scan results, though the approach suits large, open environments (industrial buildings, warehouses, construction sites) better than compact interiors, due to the inherent "see-through" limitation of unconnected point geometry, and remains fundamentally bounded by the completeness and quality of the original scan.

The application has been made publicly available on GitHub. A concrete direction for future work is making point size adapt dynamically to camera distance, so nearby points render larger than distant ones, further improving perceived visual density during the walkthrough.

**Code availability:** https://github.com/verlindenruben-lang/Walkthrough-Point-Cloud-Unity

---

_Master thesis, KU Leuven, Faculty of Industrial Engineering, Technology Campus Ghent, 2025–2026._