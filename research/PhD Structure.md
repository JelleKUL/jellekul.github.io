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
	- How are Dynamic scenes created now?
		- 3D scan completion
		- single object completion from differing inputs(images, pointclouds, voxels)
	- A lot of data is scanned, but takes long -> out-of-date and is not intelligent or interactable
	- There is an increase in low-cost fast scanning devices (Hololens, Phones,...) that are worse quality
- **Problem Statement**
	- There is no clear pipeline from partially scanned scenes to dynamic reality model.
		- very labour intensive-> manual reconstructing or Fallback to BIM models, which lack visual fidelity.
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
- **Research Aim**
	- Develop a modular pipeline to transform partially scanned indoor rooms into complete, interactive, dynamic 3D environments.
	- Key Innovation
		- Create a dynamic reality model of an existing scene, which combines the fidelity of a detailed reconstruction with the intractability of a hand-made 3D model (BIM-Model)
	- Create a modular pipeline using a mix of existing and new tools.
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
> Explain the concepts around scene dynamification and the terms

- **Introduction**
	- In this chapter, explain once and for all what scene dynamification is!
- **Dynamic scene requirements**
	- Objects that can be (re)moved from a scene 
	- We need separate objects and scenes
- **==Go over each step and give small background?==**
	- Object completion?
	- scene completion?
- **What kind of input data is used for dynamification?**
- **Current SOTA dynamification works**
- **Conclusion**

## 3. Data Acquisition
> occlusie evaluatie, tussen brol en structuur, door object, in industriële scenes, puntenwolk deficiencies

- **Introduction**
	- We need partially scanned scenes, we get both real and synthetic data
	- We limit our data to indoor single room datasets of furnished and industrial scenes
- **Related work**
	- Capture technologies and real datasets
	- ==explain the limits of real and synthetic here?==
- **Real world data capture**
	- Laserscanners
		- Static, very precise but high amount of occlusion due to limited setups
		- Handheld, Slightly less precise, drift, less occlusions
		- panoramic images
		- E blok opmetingen, werktuigkunde,...
	- XR devices
		- Unprecise, very fast, less object occlusions, but worse range
		- video feed
		- XR data capture app
	- Public Datasets
		- scannet (++), kitty, sunrgbd,...
		- Redwood-data
	- Real Dataset evaluation and shortcomings
		- 
- **Synthetic data creation**
	- Related work
		- limitations of synthetic datasets
		- Existing synthetic datasets
		- ==existing virtual scanners?==
	- Methodology
		- Scanner setup
		- Geometric scanning
		- Image capture and coloring
		- Object Isolation
	- V-scan dataset
		- My own dataset for scene completion evaluation
- **Conclusion**

## 4. Alignment and Updating of the data
> The full alignment and updating pipeline, nothing more, nothing less

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

- **Introduction**
	- If we want to complete the objects and scene separately we need to know which  is which
	- If we want to fill in the missing regions, we need to know what is missing
- **Related work**
	- 3D object detection
	- Instance segmentation
	- Occlusion detection
- **Methodology**
	- Object isolation
		- 3d object detection in the scene
		- ==list different detection frameworks?==
		- Refine detected oriented bounding box of estimated complete object
			- Symmetry detection in partial pcd to determine the center and principal axis of the boundingbox.
			- Use remaining empty scene as limiting surface for boundingbox (not go through walls)
	- Occlusion detection
		- Voxelized raytracing in the scene from scanner perspective on 2 levels
		- Object level
			- calculate the occlusion per object within it's oriented bounding box (oriented is important!)
		- Scene level
			- calculate for the whole scene at once
- **Experiments**
	- Object isolation
	- Occlusion detection
- **Conclusion**

## 6. Scene completion
>The full environment completion pipeline, from partial pointcloud, to textured mesh

- **Introduction**
	- we use the remaining environment and the full scene occlusion detection to fill in the missing parts and convert it to a textured mesh
- **Related Work**
	- plane detection
	- scene segmentation
	- geometry reconstruction
	- Image inpainting
	- object removal
		- matterport's defurnishing: only on pano's
- **Methodology**
	- Plane Instance segmentation
		- RANSAC plane segmentation of pointcloud
		- simple semantic segmentation based on point normals (ceiling, wall, floor)
	- geometry reconstruction
		- plane by plane: calculate the plane intersections of all the planes that are within the scene bounding box.
		- use the scanned points of the plane to determine which parts of the sliced up plane are at least partially scanned.
		- Use the occlusion grid to filter the partial planes because there cannot be new geometry where there were no occlusions
		- Every partial plane of the same original plane is recombined into one.
	- texture inpainting
		- Each plane is mapped to a 2D uv plane along with the original points of that plane.
		- The pano image is remapped to each plane individually using a 3D-to-uv projection.
		- The points are splatted on the image to create an inpainting mask.
		- Inpainting network paints the missing texture on the masked image
		- The inpainted image is used as the texture for each plane.
- **Experiments**
	- evaluation of geometric and textural accuracy of synthetic dataset
- **Conclusion**

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

## 8. Object dynamification 
> Turning the static completed objects into dynamic ones

- **Introduction**
	- ==from textured mesh back to CSDF for dynamification?==
	- the final step in our dynamification process 
- **Methodology**
	- Part segmentation
	- Scaling zone definition
	- Value interpolation
	- Value repeating
- **Experiments**
- **Conclusion**

## 9. Dynamic scene interaction
> Putting it all together, do visual full pipeline comparisons and show potental POC's

- **Introduction**
	- Putting the scene back together
	- Exploring the possible use cases
- **Related Work**
	- existing tools to use virtual environments
- **Evaluating completed scenes**
	- use on different datasets (matterport, scannet, v-scan)
- **Potential Applications**
	- Virtual reorganising
	- Gamification
	- renovations
	- upcycling
	- facility management
- **Conclusions**

## 10. Conclusions & future works

- **Conclusions**
	- Faster data itteration
	- creation of dynamic, complete environments
	- interaction with the digital environment
	- limitations
- **Future work**
	- Increase robustness and expand the scope beyond single room
	- increase the fidelity even more
## 11. Valorisation
- **Introduction**
- **Case study: Digital upcycling**
- **Further valorisation potentials and market position**
- **Conclusions**