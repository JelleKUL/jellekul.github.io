# Thesis Outline
> **This thesis proposes an occlusion-aware pipeline for transforming partial indoor scans into complete, interactive dynamic environments.**

## 1. Introduction
> Introduce the thesis and explain the context and research aim

- **General Context**
	- What are Dynamic Scenes and how are they used in AECO?
		- Dynamic reality model explained
		- It needs to be up-to-date
		- Compare to BIM Models?
		- digital twins, smart buildings, or real-time simulations
	- What is their advantage for existing buildings?
	- Why is making DRM models not 
	- A lot of data is scanned, but takes long -> out-of-date and is not intelligent or interactable
	- There is an increase in low-cost fast scanning devices (Hololens, Phones,...) that are worse quality
- **Problem Statement**
	- There is no clear pipeline from partially scanned scenes to dynamic reality model.
		- Either full scene occlusion infilling -> not dynamic
		- or seperate model completion -> not in scene context
		- very labour intensive-> no clear workflow, manual reconstructing on a lot of steps or Fallback to BIM models, which lack visual fidelity.
	- Several obstacles:
		- Data challenges
			- Varying quality of scans from different devices.
			- A lot of occlusions in the data that has to filled in.
				- both because of other object being in the way, but also touching objects with 
		- Technical challenges
			- Difficult alignment of data: (indoor navigation issues)
			- The objects have to be separated from the environment
			- the objects and scene need to be completed and mad dynamic
		- Evaluation challenges
			- The lack of reliable ground truth data for scan completion
- **Research Aim** ==1 page max==
	- Develop a modular pipeline to transform partially scanned indoor rooms into complete, interactive, dynamic 3D environments.
	- Key Innovation
		- Create a dynamic reality model of an existing scene, which combines the fidelity of a detailed reconstruction with the intractability of a hand-made 3D model (BIM-Model)
	- Create a modular pipeline using a mix of existing and new tools. ==enkel output mag vaag blijven, vernieuwende ook bijsteken==
		- Scene Alignment
		- Scene segmentation
		- Scene completion
		- Object completion
		- Object dynamification
		- Scene interaction
	- We want to evaluate it using a novel scanned 3D dataset that has both realistic scans and reliable ground truth data
	- The scope of this is limited to single room furnished indoor environments, captured with both geometry (pcd, mesh) and texture (pano image, textured mesh, colored pointclouds). Aiming to geometrically distinct at least partially observed objects into dynamic ones.
		- This limitation allows us to fucus on densely furnished scenes where occlusions are significant but manageable.
	- This thesis makes the following contributions:
		- Hybrid multi-stage dataset alignment framework
		- Modular pipeline for transforming partial scans into dynamic scenes
		- Synthetic dataset for evaluating dynamic scene reconstruction from partial scans
- **Thesis Outline**
	- Outlining all the chapters

## 2. Scene Dynamification Background and Terminology
> Explain the concepts around scene dynamification and the terms also define scope

- **Introduction**
	- In this chapter, explain once and for all what scene dynamification is!
- **3D representations**
	- The different type of geometry representations and texture representations
	- Explicit
		- pcd, mesh, gaussian splat
		- Store textures at point(vertex/gaussian level) or mapped to an image (face level)
	- implicit
		- SDF, paramteric model (bim), nerf
		- store texture implicitly as a function or basic color and dirived to surface 
- **Dynamic scene requirements**
	- input:
		- DRM relies on partial unstructured pointclouds and image data like panoramic images or localised pinhole camera images
	- output:
		- complete 3D models of the object
		- with a notion of part awareness to enable part-aware scaling that looks more natural rather that uniform scaling
		- Objects that can be (re)moved from a scene 
		- The scene itself must be complete

- **==Go over each step and give small background?==**
	- Object completion?
	- scene completion?
- **Current SOTA dynamification works**
- **Conclusion**

## 3. Data Acquisition
> occlusie evaluatie, tussen brol en structuur, door object, in industriële scenes, puntenwolk deficiencies

- **Introduction**
	- in this chapter:
		- what datasets we used to develop and evaluate our pipeline as a whole and each part 
	- As discussed in chapter 2 the input for our pipeline is a partially scanned scene along with color information (through a pano image or other localised images)
	- Our needs:
		- We need partially scanned indoor single room scenes of industrial and residential styles
		- Multi-temporal and/or multi-sensory for our alignment and updating part
		- object detection ground truth for the object detection part
		- scene level ground truth of the empty scene for our scene completion
		- object level ground truth and isolated partially scanned objects
	- current datasets are either:
		- Scene level:
			- real but have no ground truth completion (they do have (instance)segmentation)
			- synthetic, but are not realistic
		- Object level:
			- Synthetic, large variety and amount, but not scanned data, self intersecting geometry ect...
			- Real, very detailed but not very large dataset
	- so we plan to use a combination of different datasets to cover our needs per step.
		- first from the publically available datasets
		- second we will record multi-temporal datasets at our campus because it is close by and easy to capture, and large variety 
		- last, There is currently no real dataset that has all of the above
			- So We created the V-scan dataset that consists of:
			- scene-level captures, multi temporal and sensory, both with and without objects. Ground truth for all the detectable objects along with the bounding boxes and isolated scans.

- **Existing Datasets**
	- Introduction
		- there are already a number of datasets that are very popular around computer vision tasks
	- useful datasets
		- existing datasets and how they are made
		-  Public manually scanned scene level datasets: Scannet++, Matterport3D, arkit scenes
		- Public object level datasets: Redwood (real), shapenet (synthetic)

- **Real world data capture**
	- Introduction
		- our first set of data is real locally captured data using a mix of sensors
	- Related work
		- laserscanners and the types use
			- Static, very precise but high amount of occlusion due to limited setups
			- Handheld, Slightly less precise, drift, less occlusions
			- panoramic images from laserscanners
		- XR devices
			- Unprecise, very fast, less object occlusions, but worse range
			- video feed
	- Capture methodology
		- capture busy diverse scenes with different scanners, use them like intended, laserscanner, single or dual setups per room, XR device walk around the room and try to capture everything
	- Laserscanned scenes
		- E blok opmetingen, werktuigkunde, living lab... (from 2-step alignment paper)
	- XR devices
		- E blok, living lab
		- XR data capture app -> https://github.com/JelleKUL/XRDataCollection
	- Comparison
		- the xr device vs TLS was quicker to get going, but took longer to scan the full room
		- the laserscanner is much more detailed, but has more occlusions at ground level, but the XR devices have less range, so they struggle to capture high and far parts.

- **Synthetic data creation**
	- intro
		- 2 major parts: creating a virtual scanner and creating a procedural room layout generator
	- Related works
		- limitations of synthetic datasets
		- Existing synthetic datasets
			- Shapenet(synthetic)
		- existing virtual scanners
	- procedural room generators
		- existing works in video games and other synthetic datasets
	- Virtual scanner Methodology
		- Scanner setup
		- Geometric scanning
		- Image capture and coloring
		- Object Isolation
	- Procedural room generator methodology
		- 
	- V-scan dataset
		- My own dataset for scene completion evaluation
		- dataset metrics ook bijzetten (aantal objecten ect....)
- **Datasets comparison and overview**
	- Explanation and Comparison table of all the datasets used in this thesis
		- Manually scanned 3D scenes: E-blok, werktuigkunde, house brugge, livinglab
		- Public manually scanned scene level datasets: Scannet++, Matterport, redwood-data
		- Public object level datasets: Redwood (real), Shapenet(synthetic)
		- My own V-Scan dataset (synthetic, object and scene level)
	- Metrics to compare:
		- GT available
		- multi-temporal
		- multiple- scanners
		- nr. scenes
		- Types of scenes
		- nr of objects
		- average objects per scene
		- average points per scene
		- % of occlusion per scene
- **Conclusion**

## 4. Alignment and Updating of the data
> The full alignment and updating pipeline, based on the 2 first papers

- **Introduction**
	- The need for incremental updating of large datasets
	- Indoor localisation difficulties
	- out-of-date alignment difficulties
- **Related work**
	- Low-cost sensor data alignment
	- 3D-based alignment
	- 2D-based alignment
- **Methodology**
	- global alignment
	- Local alignment
		- 3D alignment
		- 2D alignment
		- Final pose estimation
	- New data combination
		- old point removal
		- new point introduction
- **Experiments**
	- alignment experiments
		- synthetic and real
	- combination experiments
		- with virtual dataset
- **Conclusion**

## 5. Object and Occlusion detection
> This chapter analyses the incomplete scene, first the different objects in the scene, then the per-object and scene wide occlusion.

- **Introduction**
	- If we want to complete the objects and scene separately we need to know which  is which -> object detection
		- object completion methods from chapter "object completion" often rely on nicely aligned and properly bounded boxes, but current detection methods can only bound the present points, not predict where the missing points are to properly fit the bb so it fits the complete object. -> further refinement is needed for better results 
		- so there is also a need for bounding box refinement
	- If we want to fill in the missing regions, we need to know what is missing vs. what just doesn't exist -> occlusion detection
		- we do this both for the whole scene and the objects separately
- **Related work**
	- 3D object detection
		- votenet
	- symmetry detection
	- plane segmentation
	- Occlusion detection
- **Methodology**
	- Object isolation
		- 3d object detection in the scene
			- Use votenet
		- Refine detected oriented bounding box of estimated complete object
			- Symmetry detection in partial pcd to determine the center and principal axis of the boundingbox.
			- Use remaining empty scene as limiting surface for boundingbox (not go through walls)
				- use an itterative RANSAC plane detection on the remaining scene to get the large planes and use those to make the oundingboxes smaller, and also keep them symmetrical if a clear symmetry axis is found
	- Occlusion detection
		- Voxelized raymarching in the scene from scanner perspective on 2 levels
		- Object level
			- calculate the occlusion per object within it's oriented bounding box (oriented is important!)
		- Scene level
			- calculate for the whole scene at once
- **Experiments**
	- Object isolation
		- evaluating the accuracy and IOU of the boundingboxes
		- using votenet on: Matterport3D, Scannet and sunrgbd and also V-scan
	- Symmetry detection
		- evaluate on shapenet dataset with partial scans of single objects
		═══ chair (03001627) | n=100 | k=3 ═══
	  Overall mean error : 24.15° ± 27.65°
	     primary axis : mean= 8.01°  found=94.0%  (n=100)
	   secondary axis : mean=33.97°  found=70.0%  (n=100)
	    tertiary axis : mean=30.48°  found=78.0%  (n=100)
	  Visible frac       : 36.5%
	  Score full/partial : 0.355 / 0.297
	- Occlusion detection
		- ground truth is the based on a difference mask between the empty and fully furnished panoramas. check if the coordinates of the occluded voxels are behind the changed zones.
		- ════════════════════════════════════════════════════════════ CLASS SUMMARY (mean ± std over all scenes in class) ════════════════════════════════════════════════════════════ Industrial_1_Leica-P30 n=100 covered= 73.24%±14.99 uncovered= 26.76%±14.99 avg_new_pts= 11894 Office_1_Leica-P30 n= 40 covered= 76.28%±14.74 uncovered= 23.72%±14.74 avg_new_pts= 9631 bedroom_Leica-P30 n= 20 covered= 62.33%±26.32 uncovered= 37.67%±26.32 avg_new_pts= 2211 diningroom_Leica-P30 n= 20 covered= 71.88%±19.19 uncovered= 28.12%±19.19 avg_new_pts= 4093 livingroom_Leica-P30 n= 20 covered= 51.16%±14.78 uncovered= 48.84%±14.78 avg_new_pts= 3215
		- Show results, highlight the shortcomings depending on the voxel resolution
		- ==Compare with V-Scan ground truth mesh vs pointcloud occlusion?==
- **Conclusion**

## 6. Scene completion
>The full environment completion pipeline, from partial pointcloud, to textured mesh

- **Introduction**
	- we use the remaining environment and the full scene occlusion detection to fill in the missing parts and convert it to a textured mesh
- **Related Work**
	- scene segmentation
	- geometry reconstruction
	- Image inpainting
	- object removal scene inpaining
		- matterport's defurnishing: only on pano's ect..
- **Methodology**
	- Plane Instance segmentation
		- Use the RANSAC planes of the previous chapter on the object-removed scene
		- plane boundary detection
			- cut all the planes and filter out the empty planes that do not contain any points
	- geometry reconstruction
		- we want to create a mesh
		- we sample the plane at the unoccupied areas
		- we subdivide the detected plane to the voxel grid density
		- we project each vertex in the perpendicular direction to the closest scanned point of that plane, except for the edges, these are kept to ensure a tight fit to the other planes.
		- The rooms are created as full walls, any openings like windows and door are handled later in chapter 8 to make them dynamic
	- texture inpainting
		- Each plane is mapped to a 2D uv plane with the up axis and orientation towards the scanner position kept so the inpainting can have a logical layout
		- On the pano image, we generate 2 depthmaps, one of the reconstructed planes and on of the captured scene( if not already available) then we compare depths to both images and all the points that are closer than the planes are converted to an image mask. (this can also be checked with the occlusion mask if the resolution is high enough)
		- The pano image and mask are remapped to each plane individually using a 3D-to-uv projection.
		- Inpainting network paints the missing texture on the masked image for each plane indivudually
		- The inpainted textures are remapped to the reconstructed scene
- **Experiments**
	- evaluation of geometric and textural accuracy of synthetic dataset on V-scan (see table)
	- Qualitative evaluation on the matterport dataset (I will add this)
- **Conclusion**

V-scan experiment results:

| Category       | Chamfer ↓ (m)   | F-score @5cm ↑  | PSNR ↑ (dB)   | SSIM ↑          | LPIPS ↓         |
| -------------- | --------------- | --------------- | ------------- | --------------- | --------------- |
| **Bedroom**    | 0.0161 ± 0.0022 | 0.9756 ± 0.0109 | 59.25 ± 11.91 | 0.9902 ± 0.0049 | 0.0121 ± 0.0054 |
| **Diningroom** | 0.0152 ± 0.0013 | 0.9807 ± 0.0074 | 51.25 ± 10.70 | 0.9833 ± 0.0077 | 0.0231 ± 0.0114 |
| **Industrial** | 0.0183 ± 0.0016 | 0.9823 ± 0.0064 | 44.62 ± 5.47  | 0.9345 ± 0.0215 | 0.0497 ± 0.0121 |
| **Livingroom** | 0.0153 ± 0.0013 | 0.9798 ± 0.0074 | 48.30 ± 12.36 | 0.9822 ± 0.0094 | 0.0208 ± 0.0143 |
| **Office**     | 0.0182 ± 0.0013 | 0.9842 ± 0.0037 | 42.79 ± 3.56  | 0.9246 ± 0.0356 | 0.0449 ± 0.0081 |

## 7. Object completion
> Compare the 2 proposed object completion models, this will be the biggest chapter for sure

- **Introduction**
	- We use the isolated points from the detected objects, the boundingbox and the determined occlusion-grid, along with the pano image from the scene
- **Related Work**
	- 3D object completion networks
	- Evaluation criteria
- **Voxel based object completion**
	- Methodology
		- convert the scanned points to a CSDF (colored signed distance field)
		- input the partial SDF and the occluded voxels into the AutoSDF network
		- Unwrap the meshed object and project the partial objects color onto it
		- Inpaint in 3D using TEXTure
	- Experiments
- **Image based completion**
	- Methodology
		- Create a cropped view of the object in the pano by reprojecting the image into a pinhole camera, framing the objects boundingbox.
		- Use the cropped image with the partial scan in the Hunyuan Completion network.
		- Color the completed object using the cropped image.
	- Experiments
- **Method Comparison**
	- ==1 joined experiment section??==
	- Evaluation metrics
		- chamfer distance, IOU,...
	- method 1 vs 2
- **Conclusion**

## 8. Scene dynamification 
> Turning the static completed objects into dynamic ones

- **Introduction**
	- the final step in our dynamification process 
	- make the scene (the walls, floor and ceilings scaleable and the doors and windows also moveable and scalable) and the objects (part-aware scalable)
	- Putting the scene back together as one dynamic representation
- **Related Work**
	- part segmentation
	- surface deformation
	- scaling
- **Methodology**
	- Scene dynamification
		- see notebook
	- object dynamification
		- see paper
- **Experiments**
	- showcase of dynamic environments of different datasets
	- object focus: empirical comparison of the scaled meshes with the different methods
- **Conclusion**

## 9. Conclusions & future works

- **Conclusions**
	- Faster data iteration
	- creation of dynamic, complete environments
	- interaction with the digital environment
	- limitations
	- the gap towards the construction industry is still present, because of the huge variety of stuff 
- **Future work**
	- Increase robustness and expand the scope beyond single room
	- increase the fidelity even more
## 10. Valorisation
- **Introduction**
	- use this technology for faster digitisation of reality
- github repos ook beschrijven
- **Potential Applications**
	- Virtual reorganising
	- Gamification
	- renovations
	- upcycling
	- facility management
- **Conclusions**
- **Case study: Digital upcycling**
	- explain the IOF business plan
- **Further valorisation potentials and market position**
- **Conclusions**
## 10. Valorisation

10.1 Introduction
     - Goal of valorisation
     - Explain the core value proposition: faster digitisation of reality
     - Overview of the chapter

10.2 Open-Source Software Repositories
     - Describe the main repo's:
	     - Geomapi/geosharpi
		     - data capture and standardisation
		 - https://github.com/JelleKUL/VirtualScanner
			 - procedural dataset generation
	     - https://github.com/JelleKUL/DRM
		     - the full pipeline compatible with modular external repos for the complex tasks
     - Address: community, maintenance, infrastructure, validation
	     - it's on github and public under MIT

10.3 Public Dataset: V-Scan
     - What it contains, 
	     - (see chapter 3 for what it contains)
     - why it's valuable
	     - (see chapter 3)
     - how it's shared
	     - Available on github: as an output from the Virtualscanner repo

10.4 Proof-of-Concept Applications
- Room digitisation
	- Reorganisation planning, renovations, gamification, real estate 
- Digital Upcycling

10.5 Valorisation Potential and Market Position
     10.5.1 Target Markets and End Users
            - AEC/Arts, Manufacturing, AR/VR
            - Name concrete end users and early adopters
     10.5.2 Added Value and Differentiation
            - Time savings analysis
            - How this differs from existing solutions
     10.5.3 Intellectual Property and Freedom-to-Operate
            - Background IP (GEOMAPI MIT, publications as prior art)
            - FTO screening results
            - Foreground IP strategy
     10.5.4 Case Study: IOF Valorisation Trajectory
            - Sprint structure, consortium, submission targets

10.6 Dissemination
     - Publications (6 peer-reviewed papers)
     - Conference presentations
     - Open-source community engagement

10.7 Conclusion