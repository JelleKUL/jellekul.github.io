---
layout: publication
title: Automatic alignment and completion of point cloud environments using XR data
date: 2022-07-25
permalink: /publications/automatic-alignment-and-completion/
icon: "/img/icons/Session Completion.png"
resultImage: "img/publications/Completion_turnaround_double.gif"
doilink: "https://doi.org/10.1145/3532719.3543258"
githublink: "https://github.com/JelleKUL/geomapi-completiontools"
objective: 1
type: Poster

description: Large datasets captured through TLS or photogrammetry are often incomplete or out of date. One way to combat these shortcomings is updating these datasets whit faster, less precise data from XR devices. This framework allows for a smart combination of multi-sensory datasets.

bibtex: |
 @inproceedings{10.1145/3532719.3543258,
 author = {Vermandere, Jelle and Bassier, Maarten and Vergauwen, Maarten},
 title = {Automatic alignment and completion of point cloud environments using XR data},
 year = {2022},
 isbn = {9781450393614},
 publisher = {Association for Computing Machinery},
 address = {New York, NY, USA},
 url = {https://doi.org/10.1145/3532719.3543258},
 doi = {10.1145/3532719.3543258},
 booktitle = {ACM SIGGRAPH 2022 Posters},
 articleno = {46},
 numpages = {2},
 location = {Vancouver, BC, Canada},
 series = {SIGGRAPH '22}
 }
---
# Automatic alignment and completion of point cloud environments using XR data

  

**Jelle Vermandere\*, Maarten Bassier\*, Maarten Vergauwen\***

  

Dept. of Civil Engineering, TC Construction - Geomatics, KU Leuven - Faculty of Engineering Technology, Ghent, Belgium

  

jelle.vermandere@kuleuven.be, maarten.bassier@kuleuven.be, maarten.vergauwen@kuleuven.be

  

\* All authors contributed equally to this research.

  

*SIGGRAPH '22 Posters, August 07–11, 2022, Vancouver, BC, Canada*

*https://doi.org/10.1145/3532719.3543258*

  

---

  

## 1. Introduction

  

The AECO industry needs up-to-date and complete geo-spatial data of their facilities, ranging from construction site monitoring to immersive visualisations. Despite an enormous amount of data being captured, this goal remains a great challenge due to several factors. First, the captured data is often incomplete due to occlusions or limited sensor range. Second, captured data quickly becomes outdated, while making numerous consecutive full recordings is very time-consuming and expensive. Finally, the data coming from lower-end, cheaper capture devices is rarely used due to a lack of a proper framework to incorporate them into larger datasets. A way to combat these shortcomings is to make better use of the lower-end data. While these sensors lack range and precision, the increased recording speed and lighter data load is a big advantage over traditional devices. As shown in Figure 1, combining high-end with lower-end datasets, by updating small parts of the full point cloud, could extend their relevance and decrease the total recording investment.

  

Using XR devices like the Hololens 2 as the lower-end sensor can bring additional advantages because they can capture both 2D localised Images and 3D meshes, and get realtime coverage feedback. Allowing users to seek out occlusions and inconsistencies of the previously captured high-end dataset on-site.

  

Existing works rely on using multiple sensors to capture data at different scales and levels of detail, ranging from adding highly detailed scans of small selected objects (Lachat et al. 2016), to filling in large occluded areas using drone footage (Bolkas et al. 2020). These methods, however, rely on a (semi-)manual alignment step, making the quick feedback loop from an XR device not feasible. Merging different datasets in the current state of the art is mostly focused on improving the coverage of the initial scan (Partovi et al. 2021). Updating the initial point cloud with supplementary data captured at a different point in time is a field of little research.

  

The goal of this work is to set up a pipeline to complement and update existing point clouds using automatically aligned XR datasets captured on site. This will keep the dataset relevant over a longer period of time, requiring fewer full-scale recordings and better overall coverage of the site.

  

## 2. Methodology

  

Before 2 datasets can be combined, as explained in 2.2, they need to be aligned. Each recording set is stored as a session, containing all the metadata and raw files. To get feedback on the current state of the scan, the alignment process needs to happen while the user is on-site. This is why an automatic method is proposed in 2.1. This relies on a connection between a python webserver and the XR device to access the reference datasets and increased computational performance.

  

### 2.1 Session Alignment

  

XR devices can record both images and meshes, so multiple pose estimations can be made using 2D and 3D matching techniques. These estimations all have matching parameters like the reprojection error, overlap, inliers and sensor type. These normalized values are used as weights to evaluate the confidence of the proposed pose. The final pose is determined as a weighted average of all the estimations.

  

#### 2.1.1 Image alignment

  

The 2D images are matched with the reference dataset, using the OpenCV framework to generate ORB features and state-of-the-art 2D matching algorithms to estimate a pose for each captured image (Luo et al. 2019).

  

#### 2.1.2 Point cloud alignment

  

The 3D mesh is matched against the point cloud using Open3D FPFH Feature matching, using the Fast global registration algorithm to determine the transformation (Zhou et al. 2016). The point cloud is subdivided into parts of similar size to the mesh to make multiple estimations.

  

#### 2.1.3 Pose Estimation

  

The final pose estimation is the weighted average of the different methods and their confidence factor. The accuracy of this method is precise up to the resolution of the sensor as seen in Table 1 where it was tested on multiple datasets.

  

**Table 1.** The results of the different matching methods, showing the accuracy is up to the resolution of the Hololens sensor.

  

| Method | Pos RMSE (m) | Rot RMSE (deg) | Confidence (%) |

|--------|-------------|----------------|----------------|

| 2D | 0.145 | 0.738 | 27.44 |

| 3D | 0.083 | 0.553 | 70.53 |

| Combined | 0.058 | 0.337 | 49.796 |

  

### 2.2 Session Merging

  

The main challenge is determining which points in the point cloud are still up to date. This includes the removal and addition of the newer XR data onto the existing reference dataset. We propose a 3 step process as outlined in Figure 2.

  

#### 2.2.1 Relevant point selection

  

After the XR dataset is cleaned up, the reference points are sub-selected using the convex hull of the XR dataset to ensure only points within the newly scanned area can be altered. This method is more precise than a bounding box and leads to fewer false positives.

  

#### 2.2.2 Out-of-date point removal

  

To determine which points are out of date, each one must pass at least one of the following checks: a coverage or occlusion check. The coverage is determined with a threshold distance query against the cleaned-up XR mesh. The visibility check serves to maintain points that were not visible to the XR device due to occlusions and is determined by comparing the normals of the n closest faces from the flagged point. If the RANSAC filtered majority of faces are facing away from the point, it is flagged as occluded and fails the check.

  

#### 2.2.3 New point addition

  

The reference point cloud is assumed to have much a higher accuracy than the sampled XR data, so only the not covered XR points are added. To determine these points, an inverse distance query is performed compared to 2.2.2. By default, the XR mesh does not contain any colour information, this is retrieved from the localised XR images by determining the corresponding pixel value through a raycast to the new points.

  

## 3. Discussion

  

This work has shown a method to update large existing point cloud datasets without the need to recapture the whole site. XR devices provide multi-sensory data that can not only be used to locate the user in a reference point cloud to get visual feedback of the site but also to update and complete the existing dataset with new, more focused data.

  

## Acknowledgments

  

This project has received funding from the FWO Postdoc grant (grant agreement: 1251522N) and the Geomatics research group of the Department of Civil Engineering, TC Construction at the KU Leuven in Belgium.

  

## References

  

Bolkas, D., Chiampi, J., Chapman, J., and Pavill, V.F., 2020. Creating a virtual reality environment with a fusion of sUAS and TLS point-clouds. *International Journal of Image and Data Fusion* 11, 2 (2020), 136–161. https://doi.org/10.1080/19479832.2020.1716861

  

Lachat, E., Landes, T., and Grussenmeyer, P., 2016. Combination of TLS point clouds and 3D data from kinect V2 sensor to complete indoor models. *International Archives of the Photogrammetry, Remote Sensing and Spatial Information Sciences - ISPRS Archives* 41 (2016), 659–666. https://doi.org/10.5194/isprsarchives-XLI-B5-659-2016

  

Luo, C., Yang, W., Huang, P., and Zhou, J., 2019. Overview of Image Matching Based on ORB Algorithm. *Journal of Physics: Conference Series* 1237, 3 (2019). https://doi.org/10.1088/1742-6596/1237/3/032020

  

Partovi, T., Dähne, M., Maboudi, M., Krueger, D., and Gerke, M., 2021. Automatic integration of laser scanning and photogrammetric point clouds: From acquisition to co-registration. *International Archives of the Photogrammetry, Remote Sensing and Spatial Information Sciences - ISPRS Archives* 43, B1-2021 (2021), 85–92. https://doi.org/10.5194/isprs-archives-XLIII-B1-2021-85-2021

  

Zhou, Q.Y., Park, J., and Koltun, V., 2016. Fast global registration. *Lecture Notes in Computer Science (including subseries Lecture Notes in Artificial Intelligence and Lecture Notes in Bioinformatics)* 9906 LNCS, August (2016), 766–782. https://doi.org/10.1007/978-3-319-46475-6_47

  

---

  

*© 2022 Copyright held by the owner/author(s).*

*SIGGRAPH '22 Posters, August 07–11, 2022, Vancouver, BC, Canada*