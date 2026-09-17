%=================================================================
\documentclass[remotesensing,article,submit,moreauthors,pdftex]{Definitions/mdpi} 

%=================================================================
\firstpage{1} 
\makeatletter 
\setcounter{page}{\@firstpage} 
\makeatother
\pubvolume{xx}
\issuenum{1}
\articlenumber{5}
\pubyear{2021}
\copyrightyear{2021}
%\externaleditor{Academic Editor: name}
\history{Received: date; Accepted: date; Published: date}
%\updates{yes} % If there is an update available, un-comment this line

\usepackage{graphicx}
\usepackage{amssymb}
\usepackage{pdfpages}
\usepackage{lineno}
\usepackage{hyperref}
\usepackage{mathtools}
\usepackage{booktabs}
\usepackage{kpfonts}
\usepackage{algpseudocode}
\usepackage{algorithm}
\usepackage{gensymb}
\usepackage{subcaption}
\usepackage{todonotes}
\usepackage{soul}
\usepackage{booktabs}

\usepackage{placeins}
\usepackage{setspace}
\usepackage{geometry} % added 27-02-2014 Markus Englich
\usepackage{epstopdf}
\usepackage{breqn}
\usepackage{dblfloatfix}
\usepackage{url}
\usepackage{multirow}
\usepackage{textgreek}
\usepackage[T1]{fontenc}
\usepackage{lmodern}
\usepackage{lscape}

\usepackage{pgfplots}
\usepackage{rotating}
% \usepackage{makecell}
\usepackage{tabu}

% \usepackage{multirow}
% \usepackage[utf8]{inputenc}
% \newcolumntype{C}[1]{>{\centering\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}


%\DeclareMathOperator*{\min}{min} 
%\DeclareMathOperator*{\max}{max} 
\DeclareMathOperator*{\argmin}{argmin} 
\DeclareMathOperator*{\argmax}{argmax} 

\newcolumntype{L}[1]{>{\raggedright\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}
\newcolumntype{C}[1]{>{\centering\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}
\newcolumntype{R}[1]{>{\raggedleft\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}


%=================================================================
% Full title of the paper (Capitalized)
\Title{Point cloud validation: On the impact of laser scanning technologies on the semantic segmentation for BIM modeling and evaluation}

% Author Orchid ID: enter ID or remove command
\newcommand{\orcidauthorA}{0000-0002-5231-2853} % Add \orcidA{} behind the author's name
\newcommand{\orcidauthorB}{0000-0002-7809-9798} % Add \orcidA{} behind the author's name
\newcommand{\orcidauthorC}{0000-0003-4894-6965} % Add \orcidA{} behind the author's name
\newcommand{\orcidauthorD}{0000-0001-8526-8847} % Add \orcidB{} behind the author's name
\newcommand{\orcidauthorE}{0000-0003-3465-9033} % Add \orcidB{} behind the author's name
\newcommand{\orcidauthorF}{0000-0001-6368-4399} % Add \orcidB{} behind the author's name

% Authors, for the paper (add full first names)
\Author{Sam De Geyter$^{1,2}$\orcidA{}, Jelle Vermandere$^{1}$\orcidB{}, Heinder De Winter$^{1}$\orcidC{}, Maarten Bassier $^{1,*}$\orcidD{} and Maarten Vergauwen $^{1}$\orcidE{}}

% Authors, for metadata in PDF
\AuthorNames{Sam De Geyter, Jelle Vermandere, Heinder De Winter, Maarten Bassier, Maarten Vergauwen}

% Affiliations / Addresses (Add [1] after \address if there is only one affiliation.)
\address{
$^{1}$ \quad Dept. of Civil Engineering, TC Construction - Geomatics, KU Leuven - Faculty of Engineering Technology, Ghent, Belgium \\ (sam.degeyter, jelle.vermandere, heinder.dewinter, maarten.bassier, maarten.vergauwen)@kuleuven.be\\

$^{2}$ \quad MEET HET BV, Mariakerke, Belgium\\
}
% Contact information of the corresponding author
\corres{Correspondence: maarten.bassier@kuleuven.be}

% Current address and/or shared authorship
%\firstnote{Current address: Affiliation 3} 
\secondnote{The authors contributed equally to this work.}

\abstract{Creating Building Information models from laser scanning inputs are becoming increasingly commonplace, but the automation of the modeling and evaluation is still a subject of ongoing research. Current advancements mainly target the data interpretation steps i.e. the instance and semantic segmentation by developing advanced deep learning models. However, these steps are highly influenced by the characteristics of the laser scanning technologies themselves which also impact the reconstruction/evaluation potential. In this work, the impact of different data acquisition techniques and technologies on these procedures is studied. More specifically, we quantify the capacity of static, trolley, backpack and head-worn mapping solutions and their semantic segmentation results such as for BIM modeling and analyses procedures. For the analysis, international standards and specifications are used wherever possible. From the experiments, the suitability of each platform is established along with the pros and cons of each system. Overall, this work provides a much needed update on point cloud validation that is needed to further fuel BIM automation.}

% Keywords
\keyword{Data acquisition; Semantic segmentation; Lidar; BIM; Point clouds}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\begin{document}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Introduction}
\label{Introduction}
The demand of Building Information Modeling (BIM) databases of existing buildings is rapidly increasing as the BIM adaption in the construction industry is expanding~\cite{McKinseyGlobalInstitute2017}. BIM models are requested for early design stages for refurbishment or demolition~\cite{Volk2014}, as-is models are requested for facility management and digital twinning~\cite{Patraucean2015}, as-built models are requested for project delivery and quality control, and so on~\cite{Wang2019d}. This fast growing industry relies on surveyors and modelers to produce accurate and reliable Building Information Modeling objects as well as evaluate already modeled objects with the as-built situation.

For these procedures to be successful, it is important that all the components needed to model/evaluate the structure can be extracted. To this end, Lidar-based point cloud data is captured of the existing structure. This point cloud data is then interpreted by experts or automated procedures that model and evaluate all the visible BIM objects that are part of the scope. As such, proper point cloud data is the key to successful BIM processes and any defects to this input will have severe consequences for the quality, completeness and reliability of the final model/evaluation~\cite{Mellado2020}. 

Currently, BIM modeling/evaluation methods limit their scope to idealized data and assume that the semantic and instance segmentation have operated perfectly. However, this is very much not the case as sensor specifications, temporal variations, object reflectivity characteristics and so on have a massive impact on the resulting point cloud which in turn affect the segmentation process. In this research, we will therefore study the discrepancies between point cloud inputs and evaluate their processing results at key stages (Fig.~\ref{fig:Related_work}). Concretely, we will evaluate the impact of different types of point cloud data on the semantic segmentation step. Additionally, we will analyse which modeling/evaluation information can be reliably extracted from the various point clouds. In summary, the works main contributions are:

\input{Tables/Fig2_Related_work}

\begin{enumerate}
	\item A detailed literature study on point cloud processing from the static and mobile Lidar data acquisition to the semantic segmentation
	\item A capacity study of four state-of-the-art static and Lidar mobile mapping solutions
	\item An empirical study of the impact on the semantic segmentation step based on international specifications
	\item An in-depth overview of the BIM information that can be reliably extracted from each system for modeling/evaluation
\end{enumerate} 

The remainder of this work is structured as follows. The background and related work is presented in Section~\ref{sec:Background_Related work}. In Section~\ref{sec:Sensors}, the sensors used in this study are presented. Following is the methodology for the capacity and semantic segmentation suitability study in section~\ref{sec:Methodology}. In section~\ref{sec:Testcases}, the test sites are introduced along with their corresponding results in section~\ref{sec:Experimental results}. The test results are discussed in Section~\ref{sec:Discussion}. Finally, the conclusions are presented in Section~\ref{sec:Conclusions}.

% %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Background \& Related work}
\label{sec:Background_Related work}
In this section, the related work for the key aspects of this research are discussed: (1) the suitability of prominent indoor photogrammetric and Lidar data acquisition approaches for as-built modeling and analyses, (2) an overview of the data impact on semantic segmentation processes, (3) the inputs for scan-to-BIM methods and (4) validation methods and specifications. 

\subsection{Data acquisition}

Photogrammetry and Lidar-based geometry production are the most common in BIM modeling/evaluation procedures. Terrestrial, oblique and aerial photogrammetry are among the most versatile measurement techniques. These systems are low-cost, can be mounted on nearly any platform and produce high-quality and dense texture information at an unparalleled rate. However, extensive processing of the imagery is required to produce suitable point clouds or polygon meshes. While current software such as Agisoft Metashape\cite{Agisoft2018}, Pix4Dmapper~\cite{Pix4D2021} and RealityCapture~\cite{CapturingReality2017} are capable of matching thousands of unordered images in a matter of minutes, the subsequent dense point/mesh reconstruction can take several days for large-scale projects without guaranteed success. Also, photogrammetric routines can underperform in indoor environments due to extreme lighting variations and low-texture variance on smooth objects. Drift and scaling also remain troublesome bottlenecks for photogrammetry-based routines and thus require the use of labor-intensive control networks to achieve suitable accuracy\cite{Remondino2010}.

Alternatively, static and mobile Lidar-based systems have a higher applicability as they can operate in poorly lit environments due to their active remote sensing. These systems also directly produce the highly accurate point cloud data without the needs for extensive post-processing. Static Terrestrial Laser Scanners (TLS) currently are the most popular systems for any type of building documentation. These systems generate millimeter range errors and accumulate little to no drift throughout consecutive scans due to the high quality of the data~\cite{Bassier2015}. High-end TLS are among the few systems that can effectively operate without the need for control networks, which reduces the time on site by an average of 39\% \cite{Bassier2015}. However, significant post-processing time can halt these systems as the registration of the consecutive scans still requires significant human interaction.

On the other hand, Indoor Mobile Mapping systems (iMMs) benefit from continuous data acquisition which greatly improves the coverage while also lowering the data acquisition time~\cite{Laguela2018}. The post-processing is also significantly less labor-intensive as the registration is tied to the sensor localization which is mostly performed unsupervised. Most iMMs leverage both Lidar- and photogrammetric techniques to ensure robust localization of the system~\cite{Thomson2013}. However, iMMs can generate significant drift in low-texture or low geometric variance areas and thus still rely on control networks to maintain accuracy~\cite{Hubner2020}. Several researchers have compared both low-end and high-end iMMs including Matterport, SLAMMER, NavVis, Pegasus and so on~\cite{Chen2018a,Sammartano2018}. Depending on the Simultaneous Localization and Mapping (SLAM) algorithms for the localization, high-end systems in 2021 can maintain 20mm accurate tracking for small-scale projects\cite{Tucci2018a,Chen2018a}. Overall, TLS is still the most popular technique due to its robustness and accuracy but iMMs are rapidly closing the gap with their faster data acquisition and increasingly more accurate localization~\cite{Lehtola2017}. 


% \subsection{Geometry structuration}
% \input{Tables/Fig2_Related_work}
% There are three highly suitable geometric representations that can serve as the input layer for machine learning architectures. Each representation can be directly or indirectly extracted from the above inputs and offers distinct advantages/disadvantages.

% \begin{enumerate}
% 	\item Depth maps: The simplest approach are depth maps. This 2D rasterized structure is directly obtained from RGB-D cameras and TLS or can easily be generated from virtual setups in iMMS that use Lidar. It has a fixed 2D size which is highly similar to the more popular image processing. It size makes it efficient and its similarity to image processing leads to potential reuse of image convolutions/kernels. However, it is not a holistic approach as each depth map only sees a small portion of the scene, thus making it error prone to objects at the edges. Also, is bound by Line-of-Sight, making it harder to detect volumetric objects. To properly exploit this technique, a secondary reasoning framework is needed that combines the individual frames into a 3D semantic segmentation. This will theoretically underperform compared to true 3D semantic segmentation as not all available information is used at once.
% 	\item Voxel octrees: This technique rasterizes a 3D space into a hierarchy (octree or kd) of voxels that each house a subset of the point cloud. It is a highly efficient structure that is excellently suited for nearest neighborhood searches and so on. However, complex calculations quickly become computationally demanding due to its $(O)^3$ dimensionality. In terms of input structuration, it is a step up from the 2D rasters and requires 3D kernels/convulutions to be designed, making it slightly less explored than RGB-D processing. However, the approach is quite similar in that a kernel is ran against the entire matrix and convoluted with a fixed size, making it generelizable. It is a true 3D semantic segmentation approach that is excellently suited for segment 3D volumes and is not bound by LoS. However, the extend of the kernels is limited due to the computational complexity.
% 	\item Polygonal meshes: The third approach further processes the point clouds to polygonal (triangular or quad) meshes. It is a 3D surface-based approach that represents the scene with a variable number of 3D faces, each with its own texture coordinates. In contrast to the above, the texture and geometry representations are treated separately which allows for efficient scene representation. It is among the most widely adopted formats in computer graphics and is far more memory-efficient than point cloud data (i.e. circa 95\% less vertices). In theory, the semantic segmentation should outperform both above formats since faces are more distinct and its memory-efficiency lead to more discriminative features. 	However, its dynamic texture/geometry structure makes its ill-compatible with the above fixed 2D/3D kernels and thus requires again the development of novel convolutions which are less explored. So far, 3D polygonal meshes are among the least explored in semantic segmentation despite its potential advantages [].  
% \end{enumerate} 	
	
\subsection{Data processing}
Data processing steps aim to process initial geometric data so BIM objects can be modeled/evaluated based on the points as unsupervised as possible. Prominent steps include data structuration, primitive segmentation, semantic and instance segmentation. The key step is the semantic and instance segmentation that assigns class labels to a subset of geometric inputs e.g. the assignment of a column class to a section of the point cloud. This step is very impactful but there currently is a gap in the literature how different geometry inputs affect the semantic and instance segmentation. 

Deep learning currently is by far the most popular method to conduct semantic segmentation and instance segmentation. A plethora of Convolutional Neural Network (CNN) architectures have spawned that are fueled by increasingly larger datasets such as ScanNet, Rio, S3DIS~\cite{Armeni2017}, SEMANTIC3D~\cite{Hackel2017a}, ISPRS, etc.~\cite{Garcia-Garcia2017}. Several of these networks are Open-Source which are continuously innovated and can easily be adapted for other tasks. However, the inputs generally are fixed. The dominant geometry inputs are 2D rasters from structured data sensors, 3D voxels structured in octrees or kd-trees that can be generated from any point cloud, the raw point cloud and finally also polygonal meshes~\cite{Xie2020}. 

Popular 2D rasterized multiview CNN (MVCNN) are SnapNet~\cite{Boulch2018},  MVDepthNet~\cite{Wang2018d} and 3DMV~\cite{Dai2018}. These methods closely align with image semantic segmentation networks and thus can benefit from their advancements. However, through the reduction to 2D rasters from the sensor's vantage point, a significant portion of geometric features is ignored e.g. the coplanarity of opposite wall faces. Similarly, rasterized 2D slices from 3D point clouds only allow for a partial interpretation of the scene with limited features~\cite{Jahrestagung2020}. This downside is compensated with an unparalleled speed, with MVCNNs performing near real-time~\cite{Jiang2018}.

Popular voxel-based networks are VoxNet~\cite{Maturana2015}, SegCloud~\cite{Tchapmi2018}, OctNet~\cite{Riegler2017}, O-CNN~\cite{Wang2017c} and VV-NET~\cite{Meng2019}. These approaches are known for their speed and holistic features. However, voxel-based methods reduce the spatial resolution and thus can underform near edges and details. 

Popular point-based networks are PointNet~\cite{Qi2016}, PointNet++ ~\cite{Qi2017}, PointCNN~\cite{Li2018b}, PointSIFT~\cite{Jiang2018}, SAN~\cite{Cai2019} and RandLA-Net~\cite{Hu2020}. These network compute individual point features in addition to conventional global features, leading to a performant semantic segmentation without a reduction in spatial resolution. Because point-based classification best reflects the impact of input variations, these methods are ideally suited for the input validation. Of specific interest is RandLA-Net, which is one of the most performant recent networks that was also trained on the Stanford 2D-3D-Semantics Dataset (S3DIS), which closely aligns to typical indoor environments. 

Polygonal meshes will be a serious contender with point cloud methods as it significantly reduces the data size (up to 99\% reduction) while preserving geometric detailing. However, the work on polygonal meshes is still very much a subject of ongoing research and thus is not yet as performant as the above methods~\cite{Hu2021}.

\input{Tables/Table_specifications}

\subsection{Reconstruction methods and inputs}
Once the inputs are processed to a set of observations that each represent a single object instance, the data is fed to class-specific reconstruction algorithms that attempt to retrieve the object's class definition and parameter values. Reconstruction algorithms vary wildly depending on the class of the object (walls vs ceilings or doors) and even within a single class there are a plethora of methods such as discussed in our previous work~\cite{Bassier2020Scan2BIM}. A key difference between methods is the type of object geometry that is pursued. For instance, there are the boundary-based representations such as in CityGML that solely model the exterior of an object, typically in an explicit manner such as with polygonal meshes. In contrast, volumetric object representations such as in BIM require knowledge about the internal buildup of an object and are more frequently modelled in an implicit manner i.e. based on parametric design. For a typical class such as walls this difference is very pronounced. Boundary-based walls will have their wall faces reconstructed individually and are solely linked through semantics. This makes it very easy for both 2D and 3D reconstruction methods that have to fit the best fitting surface on each visible surface of the wall and correctly draw the niches, protrusions and openings in that surface~\cite{Yang2019a}. In contrast, volumetric walls will be reconstructed by accurately estimating the wall object parameters such as the hearth line, the height and the thickness along with additional parameters for each opening, niche and protrusion that each also have their parameters~\cite{Nikoohemat2020}. The semantic segmentation plays a vital roll in whether a reconstruction method will achieve success since it lies at the basis of the geometry assumptions of a class. For instance, the assignment of a column class to a section of the point cloud decides that a parameter extraction algorithm or a modeler will fit a column to that section, regardless of whether that is correct. Also, if part of that column is mislabelled, it is very likely to upset any parameter estimation of the final object's geometry. Finally, the topology between objects will also be severely distorted by rogue objects that are being created because of misclassifications~\cite{Tran2019a}. Any interpretation error therefore directly and exponentially propagates the error in the reconstruction  and the typology configuration step and must be avoided.  

\subsection{validation methods and specifications}
Concerning the impact of data acquisition on the data processing, few comparisons are currently available. However, there are several researchers that formulate validation criteria for the point cloud and the semantic segmentation with relation to BIM.

For the point cloud validation, most researchers only perform an accuracy analysis on the point cloud data~\cite{Tucci2018a,Hubner2020,Lehtola2017}. The closest related works for a more holistic validation are those of Rebolj et al.~\cite{Rebolj2017} and Wang et al.~\cite{Wang2019d}, in which the quality criteria of point cloud data for Scan-to-BIM and Scan-vs-BIM are established which we translate to LOA and LOD requirements in Table~\ref{tab:BIM_requirements}. Aside from the accuracy, they determine parameters for the completeness and density of the point cloud that are required to model various building elements. For the accuracy, researchers either report deviations on benchmark datasets directly, or refer to international specifications such as the Level of Accuracy (LOA)~\cite{U.S.InstituteofBuildingDocumentation2016}, the Level of Development (LOD)~\cite{BIMFORUM2016} or the Level of Detail~\cite{U.S.GeneralServicesAdministration2009}. An interesting work is that of Bonduel et al.~\cite{Bonduel2017} who take into consideration the occlusions of the objects when computing the accuracy. 

For the data processing, researchers typically report cross-validation or testing rates on the above benchmark datasets. Popular metrics include recall and precision, F1-scores and Intersection over Union (IoU) of the ground-truth data and the prediction of the network. These metrics give a good overall overview of the network's performance. For a more in-depth study, call-outs of specific objects are typically gathered and subjected to a visual inspection. In this work, both methods will be used to evaluate the impact of the data acquisition differences on the semantic segmentation.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Sensors}\label{sec:Sensors}
In this section, the sensors used for indoor mapping are presented. The specifications per sensor are discussed as well their advantages and shortcomings. Concretely, we compare a high-end terrestrial laser scanner with two state-of-the-art iMMs and one low-end mapping sensor. For the experiments, the TLS data and a manually created as-built BIM model are taken as the baseline to compare the impact of the sensors on the data capture and processing. 

%\input{Tables/Fig_Sensors}
\input{Tables/Table_Sensors}

\subsection{Leica Scanstation P30}\label{sec:Leica P30}
Static Terrestrial Laser Scanning (TLS) (Fig.~\ref{tab:sensors} a) is the most conventional and accurate Lidar-based data acquisition solution on the market. Current direct and indirect Time-of-Flight sensors can capture full-done scams in only a couple of minutes with scanrates up to 1-2Mhz. Furthermore, the images takes by the sensor can now also be used in structure-from-motion pipelines to automatically register consecutive point clouds. The raw outcome for indoor environments typical is a structured point grid of 10 to 40Mp , similar to a spherical depth map and with a transformation matrix for each setup. The typical single point accuracy is <5mm/50m which corresponds to LOA40~\cite{U.S.InstituteofBuildingDocumentation2016}. Additionally, less than 1mm of error propagation can be expected throughout consecutive setups~\cite{Bassier2016TLS}.  


\subsection{NavVis M6}\label{sec:NavVis M6}
Cart-based indoor Mobile Mapping systems (Fig.~\ref{tab:sensors} b) are the most stable of the indoor mobile solutions. Theoretically, these are also the most accurate mobile solutions as they can pack more high-end (and heavier) Lidar sensors and only require 4 Degree-of-Freedom (DoF) SLAM in most cases. These iMMs' Lidar sensors yield unordered point clouds with uneven point distributions and thus are ideally suited for voxel-based semantic segmentation, although other representations are also possible. Overall, iMMs point clouds are less dense than static scans but have better coverage and are captured up to three times faster~\cite{Lehtola2017}. The typical global accuracy in 2021 is <2cm (LOA20-LOA30) along the trajectory of the system and local point accuracy is similar to TLS (LOA40). The stable localization is mainly due to the camera-based SLAM, that together with the Lidar and the motion sensor provides a well-rounded tracking mechanism. 

\subsection{NavVis VLX}\label{sec:NavVis VLX}
Backpack-based indoor Mobile Mapping systems (Fig.~\ref{tab:sensors} c)  are a more dynamic iMMs variant that still pack high-end Lidar sensors but have increased accessibility. Their random trajectory is typically tracked through 6 DoF SLAM, which theoretically is more error prone in low-texture or low geometric variance areas depending on localization sensors. However, 4 DoF SLAM is also affected for these zones, and thus iMMs performances are more tied to the environment and sensors than the concrete setup. As such, similar accuracies of LOA20-LOA30 for the global point accuracy and LOA40 for the local point accuracy are reported for these systems. Analogue to the M6, the combination of Lidar-and camera-based SLAM yields superior results. An important feature however is the software that ships with each system. With NavVis, the software inherently allows for the inclusion of ground control points in an automated manner which is not the case with low-end sensors. These control points are actively used during post-processing and function as fixed constraints in the bundle adjustment of the scan network.

\subsection{Microsoft Hololens 2}\label{sec:Microsoft Hololens 2}
A recent addition to capturing devices are Mixed Reality systems (Fig.~\ref{tab:sensors} d). These portable data acquisition solutions can be considered iMMs but do not have the same large-scale mapping capabilities. They are part of this research' scope as these systems will play a major role in digital built environment interaction which inherently includes mapping. They operate with 6 DoF SLAM but can only be deployed for smaller scenes due to poor field-of-view, range and low-end sensor specifications. Single-point accuracies of 1cm/m for the Lidar sensors are not unusual with a high error propagation, restricting them to close-range applications (LOA10-LOA20). Specifically for the Hololens 2, the spacial mapping is designed to run as a background task, using minimal resources to keep the device performant. Being a head-mounted device, the mobility leaves no restrictions for the user. the capabilities are focused on real-time mapping and less on accuracy. This results in on-device-computed meshes that can be used directly in the processing. These are generally lower in resolution so they can be stored on the device, but are also usable for mapping. However, the restricted range of only circa 3-4m is a significant downside for BIM modeling/evaluation.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\section{Methodology}\label{sec:Methodology}
In this section, the methodology is formulated to determine the impact of data acquisition and the subsequent semantic segmentation on the point cloud suitability for BIM modeling/evaluation. Concretely, we first conduct a sensor capacity test, where each sensor is tested to produce proper point cloud data. Next, we evaluate the point cloud suitability for BIM modeling/evaluation in a detailed study where each system's data is semantically segmented and evaluated whether the results can be used for as-built BIM modeling or analyses. We use international specifications such as the LOA ~\cite{U.S.InstituteofBuildingDocumentation2016} and common literature metrics wherever possible to provide a clear comparison of the results.

\input{Tables/Fig_QualityMap}

\subsection{Sensor capacity}
First, a quantitative analysis on the point clouds is conducted to analyze the raw performance of each system (Fig.~\ref{fig:quality_maps}). As is common in the literature, we evaluate the sensor accuracy and error propagation along the trajectory by computing the Euclidean cloud-to-cloud (c2c) distances between the sensor's point cloud and the ground truth data. For these tests, the TLS data, which is confirmed to comply to LOA40 $[2\sigma<0.005m]$ by total station measurements, is considered as the ground truth. To compare the point clouds, all datasets are referenced to the same coordinate system and only the overlapping areas are evaluated. For every point cloud, the cumulative percentage of c2c-distances are computed at 68\% and 95\% inliers as reported by \cite{NavVisGmbH2019,NavVisGmbH2020}. Furthermore, the metrics will be compared to the LOA specifications as stated in~\cite{U.S.InstituteofBuildingDocumentation2016}. The overlapping point clouds are reported cumulatively for each bracket i.e. LOA30 $[2\sigma<0.015m]$, LOA20 $[2\sigma<0.05m]$ and LOA10 which is user-defined and set to $[2\sigma<0.1m]$ conform common building tolerances~\cite{Wang2019d}. 

To asses the capacity of each system, the need for loop closures and control points needed to obtain accurate results, are evaluated. This knowledge is vital to obtain a maximal efficiency on site with a minimal amount of time needed to capture a scene. These characteristics are tested by mapping the same datasets shown in Figure~\ref{fig:quality_maps}, with and without control points and loop closures. First, a long hallway is mapped without possibility for loop closures to test the raw tracking cabailities of each device. The narrow pathway and scene repetitiveness pose key challenges for iMMs and thus significant drift can be expected this subideal D-hall dataset (Fig.~\ref{fig:quality_maps} a). 

Analogue, the influence of loop closures is measured by mapping the same loop with and without loop closures (Fig.~\ref{fig:quality_maps} b). To this end, a square shaped corridor is mapped with the different systems. The P30 again is used as a reference and is validated with control points on each corner of the square. Important in this dataset is the passage between the starting and end point of the loop. To ensure an unbiased evaluation, the passage was closed off so the raw drift of each system could be measured directly by observing both sides of the passage. When loop closures were to be applied, the passage was left open so the processing software could align the starting and end zone. 

\subsection{Point cloud suitability}
The second analysis evaluates each system's capabilities for BIM modeling and evaluation. As described in the related work, the key stages that influence this process are the initial geometry production and the semantic segmentation. In this test, we evaluate both aspects for each system on 3 distinct target areas (see Section~\ref{sec:Testcases}). We test 5 common structure classes that are part of most unsupervised BIM reconstruction/analyses methods as they form the observable core of each structure. i.e. ceilings, floors, walls, beams and columns. For these experiments, a manual as-built BIM is conceived as accurately as possible by expert modellers. Analogue to the capacity tests, the P30 data is used as the main repository as it is proven to comply with LOA40. However, in areas where the P30 did not capture any data, the Leica VLX was used in combination with control points and loop closure which is also highly accurate and has the best coverage.  

From the literature, the BIM information extraction requirements for these classes are established. Table~\ref{tab:BIM_requirements} depicts the expected Level of Accuracy and Level of Development that are commonly reported for the structure classes~\cite{Patraucean2015,Volk2014,Czerniawski2020}. 

\input{Tables/Table_BIM_requirements}

The point cloud suitability of the initial geometry production is established by translating the above requirements to point cloud parameters for quality, completeness and detailing as reported by Wang et al.~\cite{Wang2019d} and Rebolj et al.~\cite{Rebolj2017}. We report each parameter per class, based on its semantic segmentation. As such, we can asses how well the different classes correspond to the expected requirements.

% The accuracy of the captured data is reported as the inliers for LOA30 $[2\sigma<0.015m]$, LOA20 $[2\sigma<0.05m]$ and LOA10 $[2\sigma<0.1m]$ as discussed in the methodology. To evaluate the Scan-to-BIM suitability, the represented accuracy is evaluated rather than the documentation accuracy in the above experiments. As such, the BIM is used as the reference for each dataset's distance evaluation, which also allows us to evaluate the suitability of the TLS.

It is important to notice that significantly lower inliers are expected for the represented accuracy due to modeling abstractions. For instance, none of the sensors achieve LOA30 in any of the testcases as abstract BIM objects are used to represent the geometry (which is custom in industry), that do not take into consideration the detailing and imperfections of the real environment. Furthermore, while the filtering algorithm for the classes (see Methodology) achieves a proper segmentation, some noise and clutter can be expected that will negatively impact the number of inliers. However, we can still evaluate the relative performance between the sensors to establish the point cloud suitability.

The quality is established by the point accuracy similar to the capacity tests with the c2c-distance being evaluated. However, the LOA represented accuracy (point cloud vs model) is evaluated rather than the documentation accuracy (point cloud vs point cloud) in the above tests (Eq.~\ref{eq1}). As such, the BIM is uniformly sampled ($P_{\text{synth}}$) and used as the reference for each dataset's distance evaluation, which also allows us to evaluate the suitability of the TLS. To ensure a balanced quality measure, we uniformly sample the input point clouds $P$ up to $0.01m$, which does not compromise the number of LOA30 inliers. Similar to the above tests, the distance threshold $t_d$ is capped at 0.1m to not include outlier points.

\begin{equation}
\begin{split}
\label{eq1}
% establish spatial subsampling
& D=\left\{d\Big|q\in Q, p \in P_{\text{synth}}: \min\limits_{q}\|p-q\| <t_d\right\} 
\end{split}
\end{equation}

As the point accuracy is not normally distributed, we report the inliers for the cumulative LOA30 $[2\sigma<0.015m]$, LOA20 $[2\sigma<0.05m]$ and LOA10 $[2\sigma<0.1m]$ brackets (Eq.~\ref{eq2}). 

\begin{equation}
\label{eq2}
% LOA calculation
Quality=
\begin{cases}
\  \text{LOA30:} & \frac{|D<0.015m|}{|D|} \\
\  \text{LOA20:} & \frac{|D<0.05m|}{|D|}\\
\  \text{LOA10:} & \frac{|D<t_d|}{|D|}
\end{cases}
\end{equation}

The completeness of the point clouds is established by determining the coverage per class. Wang et al.~\cite{Wang2019d} define this as the ratio between covered area and total area, but they correctly state that this is an ambiguous measure as it does not account for the clutter that occludes significant portions of the object. We therefore perform an initial filtering on $P$ for each class based on $t_d$. 
Also, we evaluate the normal similarity between the observed $\overrightarrow{n(q)}$ and the reference  $\overrightarrow{n(p)}$ normals in both datasets~\cite{Bassier2020ICPconstruction} (Eq.~\ref{eq3}). 

\begin{equation}
\begin{split}
\label{eq3}
% establish segmented point cloud P_w per class component surface
& \text{Coverage}= \frac{1}{|P_{\text{synth}}|}  \left|\left\{p\in P_{\text{synth}} \Big| q\in Q: \argmin\limits_{p}\|p-q\| < t_d  \text{    for which   } \left|\overrightarrow{n(p)} \cdot \overrightarrow{n(q)}\right|>t_{\|} \right\} \right| \\
\end{split}
\end{equation}

Finally, the density of each point cloud is established by the average spatial resolution of $P$. To this end, $n$ samples are extracted from $P$ for which the Euclidean distance to its nearest neighbor is computed (Eq.~\ref{eq4}).

\begin{equation}
\begin{split}
\label{eq4}
% establish spatial density
& \text{Density}=\frac{1}{n} \sum_{i=1}^{n} \left\{d \Bigg| p_i,p_j\in P: \min\limits_{p_j}\|p_i-p_j\| \right\} \\
\end{split}
\end{equation}

The subsequent semantic segmentation capacity is tested by processing each system's dataset with the same state-of-the-art CNN. Concretely, we adapt RandLA-Net which was pretrained on Area 5 of the S3DIS stanford dataset~\cite{Armeni2017}. The model was trained according to the specifics discussed in ~\cite{Hu2020} and achieved on average 88\% of the structure classes of S3DIS Areas 1-6 which are representative indoor scenes captured by a mobile RGBD scanner. From S3DIS, we solely retain the ceilings, floors, walls, beams and columns classes and store the remainder in a clutter class. Except, the classes windows, doors and boards, which are stored in the wall class because these object classes are contained within the wall structure class and are modeled once the structure is completed. 

As a baseline for the model expectations, we inherit the Hu et al.~\cite{Hu2020} mIoU values of each class given a 6-fold cross-validation (see table~\ref{tab:BIM_requirements}). Additionally, we report the IoU values of an idealized synthetic point cloud of our datasets that are generated from the as-built model. Given the IoU values of each class, we will conduct a quantitative and visual analysis of the results. 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Test setups}\label{sec:Testcases}

The experiments are conducted in different areas of our university technology campus in Ghent, Belgium. In total, 5 test zones are developed for the sensor capacity and point cloud suitability tests (Fig.~\ref{tab:dataset_overview}). 

\input{Tables/Table_DatasetOverview}

For the sensor capacity test, two drift-sensitive areas are selected. The first is the D-hall (Fig.~\ref{tab:dataset_overview} row 1) and the E-hall (Fig.~\ref{tab:dataset_overview} row 2). Both hallways do not contain distinct features and form a monotonous scene. The D-hall trajectory (90m) is specifically chosen to evaluate the drift of the different iMMs. The E-hall trajectory (120m) is chosen to evaluate the effect of loop closure on each point cloud. In each zone, a control network was established with total station measurements and georeferenced in the Belgian Lambert 72 (L72) coordinate system. In the capacity tests, the Leica Scanstation P30 is used as the ground truth due to its superior accuracy which was established in previous work~\cite{Bonduel2017}. The alignment of the iMMs datasets with and without control points was performed with control points outside of the targeted survey area. Fig.~\ref{fig:quality_maps} shows the trajectory of M6 and the VLX as well as the location of the control points. While the NavVis software allows a seamless integration with control and uses this in post-processing, this is not the case for the Hololens 2. Therefore, the Hololens 2 data was divided into chunks that were manually registered to the control. In table \ref{tab:survey-time}, an overview of the survey is presented including the time needed to map the zones and the manual intervention time in the post-processing that was required. The time for measuring end materializing the control points and time to prepare the sensors before data capture was not included as all methods employ this data. 

\input{Tables/Table_survey-time}

For the point cloud suitability test, three industrial laboratories were selected. Lab 1 is used for concrete processing and contains hydraulic presses, aggregate storage, a classroom and several experiment setups (Fig.~\ref{tab:dataset_overview} row 3). Lab 2 has facilities for road construction research including several environmental cabins and asphalt processing units (Fig.~\ref{tab:dataset_overview} row 4). It also has several free standing desks, desktop computers and a separate office space. Both these labs are located in a refurbished factory space where the concrete beams and columns are still visible. Finally, lab 3 is a building physics lab in an adjacent masonry building which has visible steal beams and contains several experimental setups i.e. a blowerdoor test, insulation setup and ventilation experiment (Fig.~\ref{tab:dataset_overview} row 5). These environments are chosen for their representation of industrial environments and the presence of all target classes. The column class however still is underrepresented as the columns are located inside the walls, which will also negatively impact their coverage and subsequent semantic segmentation. However, the pretrained S3DIS model also suffers from similar issues as columns and beams are commonly occluded. Similar to the capacity test, each zone was mapped with the four sensors and georeferenced with TS measurements (Table~\ref{tab:survey-time}). In this test, the synthetic point cloud generated from the as-built BIM is used as the ground truth for the quality, completeness and semantic segmentation comparison. 

% \subsection{Dataset office}\label{sec:Dataset offices}

% The last dataset exists out of a three storey office building. This multi storey, multi room building that was in use at the time of the mappings represents best real surveying situations. The building is only accessible from the outside on the front because it has adjacent office buildings on all three the other sides. Inside the building as a typical layout. On the ground floor there is a large garage/storage space and the first and second floor exist out of offices.

% \input{Tables/Fig_comparison_stairs}

% The scanning with the NavVis M6 was conducted in 3 separate sets. This because the sensor cannot map multiple storeys in one take, so for every storey a separate set was created. On each floor more then two ground targets where materialized and included in the TS network and measured with the M6. This made it possible to merge the three sets in one pointcloud. The mapping of the building with the M6 trolley took approximately 2 hours. The processing of the data was done with the NavVis software overnight and required 20 minutes of manual intervention of uploading/selecting the data and set the desired specifications. While mapping with the M6, some minor disadvantages where noticed. The trolley is difficult to maneuver in small spaces e.g. in toilets or small storage spaces. The staircases also could not be properly scanned as you can only scan some parts from the top or the bottom of the staircase but this is not sufficient to properly document the stairs as can be seen in figure \ref{fig:comparison stairs}, the figure also shows the coverage of the same staircase with the VLX and the P30.

% The offices where also captured using the VLX frontpack mobile scanner. In comparison with the M6 this sensor only needed two datasets to capture the complete building, including the roof which was not accessible with the M6 trolley. With this sensor the building was split in separate datasets because of the recommended time per dataset was past. The NavVis solutions recommend to map only 40 minutes in one take, as after this point the post-processing time of increases significantly. In contrast to the M6, overlap between both datasets was possible. The capturing of the building (including the roof) was done by one person and took about 1,5 hours. Also all the targets in the building where measured with the sensor, both ground targets as wall targets. This targets are imputed in the NavVis software which processed the data, resulting in two pointclouds in L72 which where thereafter combined. Just like with the M6 the processing only took about 20 minutes of manual interference. 

% The static laser scanning to serve as reference, was conducted using the Leica P30 scan station and was manually post-processed in the Leica CYCLONE software package. The survey using the TLS took 72 setups and was done in one day or approximate 8 hours with one person. Resulting in a registered pointcloud in L72 with a Mean Absolute Error of 0.002m as reported by CYCLONE. The post processing of this survey took an additional 8 hours.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Experimental results}\label{sec:Experimental results}
In this section, the results of the experiments to evaluate the sensor capacity and the point cloud suitability are discussed.

\subsection{Sensor capacity}
\label{sec:Experimental results macro}

\subsubsection{Impact of control points}
As discussed in section~\ref{sec:Testcases}, accumulative drift along the repetitive hallway is expected. Fig.~\ref{tab:hall-d} reports the visual deviations and the inliers for each LOA bracket for the D-hall with and without the support of a control network. Graph~\ref{fig:graphs} a shows the percentual inliers with respect to $t_d=0.1m$, conform common industry tolerances. Overall, the VLX and M6 iMMs maximum deviations fall within the threshold, showing less than 0.01mm/10m drift without control which is in line with the NavVis reports~\cite{Vlx2021}. As expected, the low-end Hololens 2 reports much higher drift, with a maximum of 0.86m deviation at 90m. With the inclusion of control, the LOA30, 20 and 10 inliers for the M6 and the VLX respectively improve by 44.8\% and 48.5\% while for the Hololens 2 this is only 21\%. This is mainly caused by the poor close-range data quality of the Hololens 2, which prevents it from achieving LOA20 or 30 even without drift accumulation. 

\input{Tables/Table_D-hall_results} 
\input{Tables/Graphs}
\input{Tables/Table_E-hall_results}

When compared to each other, the impact of the different sensors of each system on the drift can be observed. The experiments clearly show that the range errors of each system (Hololens 2: $0.01m/1m$, M6: $0.01m/10m$, VLX: $0.01/50m$) play a major role in the drift accumulation. For low-end sensors, the close-range noise negatively impacts the data-driven SLAM which leads to increased drift. However, there is a clear drop-off of this affect when the range-noise drops below a certain point. I.e. the NavVis VLX Velodyne lidar sensors produce more qualitative data, and yet the drift accumulation is similar to the M6. This is because of the sideways configuration of the Lidar sensors, which thus only capture data that is less than 10m away. As such, the distribution of the data and the quality of the motion sensor are more important in close-range scenes than further increasing the quality of the lidar sensor.

From the experiments, it can be concluded that without control, the M6 and VLX can be used small-scale projects up to LOA10 $[2\sigma<0.10m]$ and that the Hololens 2 is unsuitable for BIM modeling/analyses with these settings. With control, the M6 and VLX easily achieve LOA20 $[2\sigma<0.05m]$ close to LOA30 $[2\sigma<0.015m]$ when control is added circa every 25m. Analogue, the Hololens 2 can achieve LOA10 close to LOA20 when control is added every 10m. However, as discussed in section~\ref{sec:Testcases}, the Hololens 2 processing software does not yet have a functionality to import control points and thus no control can be used during the adjustment of the Hololens 2 setups during processing. This is highly advised since control is one of the most effective ways to reduce drift accumulation for iMMs.

\subsubsection{Impact of loop closure}
The results of the loop closure experiments are shown in Fig.~\ref{tab:hall-e} and Graph~\ref{fig:graphs} b. Analogue to the control point evaluation, the accumulative error within the loop as well as the inliers for the consecutive LOA brackets are reported. First of all, all values are lower than in the E-hall. This is expected since the path is significantly longer (160m opposed to 120m) than in D-hall. As a result, maximum deviation of each system respectively is 0.06m for the M6, 0.2m for the VLX and 0.85m for the Hololens2. Upon closer inspection, this error is both in X, Y and Z direction which can be explained by the similar point distributions in these directions for E-hall. In terms of drift patterns, Fig.~\ref{tab:hall-e} shows that the VLX slightly underperforms compared to the D-hall due to the increased length of the trajectory. Additionally, since the registration of the system has 6 DoF instead of 4 (see section\ref{sec:NavVis VLX}), an increased error can be expected from the randomized trajectory of the sensor. Similarly, the M6 loop closure optimization does not improve the results since the 120m loop is too large for loop closure to compensate and thus fails to create a meaningful difference between the LOA20 and 30 inliers. This effect is more expressed for the Hololens 2, where the optimization of already properly aligned areas further improves, but poorly aligned areas do not shift towards a more accurate solution. 





From D-hall and E-hall combined, the following conclusions are presented. In terms of control points, it is stated that the high-end iMMs can achieve LOA20 for circa 50m of trajectory without the support of control points or loop closures. The low-end sensor can solely achieve LOA10 close to LOA20 when control is added every 10m. In terms of loop closures, it is stated that the high-end iMMs can achieve LOA20 for circa 100m of trajectory without the support of loop closures. The low-end sensor can solely achieve LOA10 close to LOA20 when loops are made every 30-40m. While simultaneously applying control and loop closures doesn't significantly improve the result, there is a massive gain by using both techniques complementary. For instance, control networks only need to be established along the main trajectory of a building, while any short side-trajectories (e.g. seperate rooms) can be accurately mapped solely using loop closures. 

% \subsubsection{Office}\label{sec:Results Office}
% The last dataset discussed in this paper, which is also the closest to real world surveys, including regular  loop closures and ground control points. When examining the results in figure \ref{fig:Result Office} there is no distinct difference between both iMMs. This is besides the fact that the M6 was not capable to capture the roof of the building, which also resulted in less coverage of the exterior walls. The gray area on the garage door, suggesting an error above 10cm is caused by the fact that the door was closed during the capturing of the reference scans but was open during the mapping to allow  loop closures. When looking at the c2c-distances, in figure \ref{graph:Office cumulative percentage of inliers}, no distinct difference can be seen between both sensors. Due to the realistic conditions of this dataset, it allows to compare the time needed for the surveying campaign with the three different sensors, including the M6, the VLX and the Leica P30 TLS. The time needed to capture and process the data and the number of datasets needed per sensor are given in table \ref{tab:survey-time}. From this overview of the time needed to capture and post-process the building with one person, it can be clearly seen that the iMM sensors have a large time advantage against the TLS. One distinction that can be made between the two iMMs presented, is the usability in the situation of a small office building. During the mapping the VLX was easier to use in the small spaces inside the building. With the M6 it is not easy to maneuver in those small spaces, such as toilets, or stock rooms. Also the VLX allowed the mapping of the staircases as well, resulting in an additional link between both dataset, this would allow us to capture the building without control. As discussed in section \ref{sec:Dataset offices} the M6 is not able to capture these staircases, resulting in an insufficient covering of the higher stairs as shown in figure \ref{fig:comparison stairs}. This also introduces the need to split the building in datasets maximal covering only one floor. When these floors have to be linked to each other, at least 3 control points are needed in a common coordinate system, making a TS-measurement necessary.
% \input{Tables/Graph_Office}
% \input{Tables/Fig_Office_results}
\input{Tables/Table_Lab1_results}
\input{Tables/Table_Lab2_results}
\input{Tables/Table_Lab3_results}

\subsection{Point cloud suitability}
\label{EXP:Scan-to-BIM suitability}
The quantitative results of the point cloud suitability experiments are shown in Table~\ref{tab:lab-1},\ref{tab:lab-2} and \ref{tab:lab-3}. Each table includes the data acquisition parameters to evaluate the quality, completeness and detailing as described in Section~\ref{sec:Methodology}. The IoU percentages for the semantic segmentation of RandLA-Net are also reported per class and an overview of each classification is shown in the tables. Aside from the quantitative results, figures ~\ref{fig:callouts6} to ~\ref{fig:callouts5} show detailed call-outs to evaluate the point cloud's suitability to be processed to individual objects. Each aspect is discussed below.




\subsubsection{Impact of quality}
The accuracy of the captured data is reported as the LOA30, 20 and 10 inliers of the represented accuracy as discussed in the methodology. For the evaluation, it is important to notice that significantly lower inliers are expected for the represented accuracy due to modeling abstractions. For instance, none of the sensors achieve LOA30 in any of the testcases since abstract BIM objects (i.e. IfcWallStandardCase) are used to represent the geometry as is custom in the industry. Furthermore, while the filtering algorithm for the classes (see Methodology) achieves a proper segmentation, some noise and clutter can be expected that will negatively impact the number of inliers. However, this dataset is the preferred reference to evaluate the relative performance between the sensors to establish the point cloud suitability.

Overall, from each sensor LOA20 results (avg. 88\%) could be reliably produced given the abstractions and clutter save for the Hololens 2, that on average has 10\% less inliers. For objects that fit well with the abstract object definitions, the a BIM reconstruction from these point clouds will achieve LOA30 for the high-end sensors and LOA20 for the Hololens 2. Locally, this can improve to LOA40 for the high-end sensors and LOA30 for the Hololens 2. However, there are significant differences between each class. For instance, the average accuracies for the walls is significantly lower than for other classes. This is due to the increased abstractions. For instance, Lab 3 consists of ornamented masonry walls, leading to circa 35\% lower inliers for the represented accuracy. For the ceilings, all iMMs surprisingly yield similar inliers while this is not the case for the floors. Aside from abstractions, this is due to the sensor's range noise, that misaligned the floor and the ceiling. When looking at the joint inliers, this is confirmed with the M6 (1cm/10m) and Hololens 2 (1cm/m) on average showing 20\% and 30\% less inliers. Furthermore, when looked at in more detail, a major error is detected in the floor data of the Hololens 2 (Fig.~\ref{fig:callouts6}). Due to its horizontality assumption, it does not properly detect the 0.1m height difference in adjacent spaces in lab 1. For the columns, the statistics show that each sensor achieves good LOA20 and even LOA30 results which is due to the positioning of these elements. Especially on square or rectangular elements, the number of inliers is very high. For the beams, the Hololens 2 again underperforms due to the height of the beams. However, specifically for beams and columns,  statistics alone are very ambiguous since high Euclidean distance inliers do not guaranty proper point cloud processing. For instance, the increased noise on a beam section would make detailed modeling from the M6 data challenging and impossible from the Hololens 2.  

\subsubsection{Impact of completeness}
In addition to the quality, the completeness is essential to create as-built models. Some occlusions are inevitable i.e. with ceilings and floors and this also reflects in the BIM LOD requirements that are lower for these categories. 

\input{Tables/Table_Results_Callouts6}
\input{Tables/Table_Results_Callouts2}
\input{Tables/Table_Results_Callouts3}
\input{Tables/Table_Results_Callouts4}

The sensors achieved the following results. Overall, the VLX (60\%) scores extremely high given the clutter and systematic occlusions, followed by M6 (52\%), the P30 (41\%) and the Hololens 2 (37\%). This is due to the increased accessibility of the iMMs, where the backpack system scores circa 10\% better than the cart-based system. Surprisingly, the highly mobile Hololens 2 underperforms, mainly due to its range limitations. This especially affects the capture of industrial sites where ceiling heights typically are more than 3-4m. Also for higher and complex walls the range of the Hololens 2 is insufficient resulting in large parts of walls that remain unrecorded as can be seen in Fig.~\ref{fig:callouts2}. In contrast, the P30 underperforms due to its limited setups which leads to large gaping occlusions despite its range. E.g., Fig.~\ref{fig:callouts3} shows that with the P30, massive occlusions exist on the ceiling when a beam is attached near the ceiling. 

Overall, the coverage of floors with any sensor is much lower (on average <30\%). The accessibility to the different floor parts is the driving factor for a proper point cloud. Especially in densely occupied spaces, every sensor struggles to achieve sufficient coverage to model anything more complex than a simplistic floor. However, the distribution of the occlusions varies widely between the sensors. Especially the M6 and the P30 have large occlusions as they have restricted accessibility. With the other devices, however, it is possible to walk between obstacles (Fig.~\ref{fig:callouts4}). For the walls, the P30 and the Hololens 2 score on average 25\% lower than the two other iMMs. However, their occlusions are very different with the P30 mainly struggling with clutter and the Hololens 2 missing large portions of the upper part of the walls due to its limited range. It is argued that the Hololens 2 occlusions are less impactful as these occlusions do not interfere with the proper modeling of the walls.

Analogue to the quality estimation, the coverage statistics for beams and columns are ambiguous as the impact drastically varies with respect to the beam/column type and the location of the occlusions. For instance, an I-profile for which the section is partially occluded prohibits proper modeling. For the exposed beams, the high-end sensors have an average coverage of 89\% while the Hololens 2 only achieves 58\%. Furthermore, due to the height of the beams, a large portion of the section is occluded for the Hololens 2. For the largely occluded columns, all sensors achieve similar coverage as with the walls. 

\subsubsection{Impact of detailing}
The impact of the detailing of the point clouds varies wildly depending on the class and the complexity of the objects. For generic walls, ceilings and floor, even an extremely sparse point cloud suffices to properly model or analyze a LOD350 as-built BIM. In these cases, it is argued that the density of the P30, NavVis M6, and the NavVis VLX are complete overkill if their registration algorithms were not also data-driven. For the beams and columns, the opposite is true e.g. for a modeler/algorithm to determine the proper beamtype, the resolution should be less than the flange thickness. For the high-end sensors, the density surpasses the flange thicknesses of most beam/column types so it is stated that LOD350 can be achieved with these sensors. The Hololens 2 data on the other hand does not allow the recognition of the proper beamtype as can be seen in Fig.~\ref{fig:callouts5}. Therefore, the Hololens 2 can only be used for object classes with details larger than 5cm. With the high-end scanners, the detailing and resolution is sufficient, on the other hand with the Hololens 2, it must be possible to go close enough to the object in order to get good detailing. 
\input{Tables/Table_Results_Callouts1}
\input{Tables/Table_Results_Callouts5}

\subsubsection{Impact of semantic segmentation}
For the RandLA-Net processing, every dataset was processed as an unorganized point set with the BIM mesh being sampled up to 0.01m analogue to the synthetic data. As discussed in the methodology, RandLA-Net was pretrained on Area 6 of the Stanford S3DIS dataset, which consists of a single-storey indoor office/school environment. It is therefore important to notice that lower IoU scores are expected for classes and objects that are not part of this dataset. A very clear indication of this limitation is the poor performance on the synthetic data, which is supposed to resemble a near perfect environment. The generic color, combined with the lack of clutter and unexpected data introduces confusion in the semantic segmentation that was solely trained on real data.

\input{Tables/Table_Results_Callouts7}

Overall, every dataset was able to be processed by the network at a similar speed as reported by Hu et al.~\cite{Hu2020}. For the ceilings and floors, all sensors achieve reasonable results (avg. 81\%) which is expected given the cross-validation of 93\% and 96\% of respectively the ceilings and floors of S3DIS Area 6. However, while in each sensor's data the main ceiling is easily found, there is significant confusion with lower ceilings and central pieces of the floors. These missclassifications appear in similar locations in all datasets due to training shortcomings of S3DIS. Surprisingly, the lower completeness and quality of the Hololens 2 data  do not significantly affect the ceiling or floor classification (Fig~\ref{fig:callouts7} a). 

For the walls, there is a statistical IoU difference between the Hololens 2 and the M6 (avg. 73\%), the P30 (65\%) and the VLX (58\%). However, the results should be nuanced. First of all, the driving factor in the wall IoU is the confusion with clutter observations, especially near wall detailing. The VLX has the highest completeness and thus documents more detailing than other sensors, which in turn leads to higher confusion rates. Second, the wall classification heavily prioritizes precision over recall. As such, relevant parts are found on nearly all wall surfaces without significant false positives for the different sensors which is preferred for general BIM reconstructions (Fig~\ref{fig:callouts7} b). 

For the beams and columns, the results are dramatic, which is expected due to the low cross-validation (beams: 62\% and columns: 48\%) on Area 6 as a result of limited observations of these classes. For instance, the I-profile in lab 3 was not found in any sensor data due to a lack of training data (Fig~\ref{fig:callouts7} c). However, there is an important difference in the classification of each sensor's data. In lab 2, the Hololens 2 and P30 only found 2 out of 9 beams while the M6 and VLX found 5 (Fig~\ref{fig:callouts7} d). While the Hololens 2 underperforms due to range limitations, the P30 had systematic occlusion gaps which significantly lowered the detection rate. For the columns, it is observed that in the synthetic data, erroneous columns were frequently found near the side faces of the walls where data was also sampled in occluded areas. 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Discussion}\label{sec:Discussion}
From the experiments, the relation between the point cloud characteristics and the subsequent semantic segmentation can be described. Overall, a high completeness is the most impactful parameter for a successful segmentation, followed by the quality and finally the detailing. For instance, the P30 data has systematic gaps that negatively impact the semantic segmentation of beam parts. However, the impact of all three parameters on the semantic segmentation is rather limited. E.g. in the Hololens 2 data, which in some regions only contains small patches of ceilings, the majority of isolated patches are still properly classified. Similarly, systematic occlusions on floors or walls do not necessarily lead to a worse semantic segmentation. This is also confirmed by other classes and the low discrepancy between IoU statistics in the experiments. If anything, the training data of the model is significantly more impactful. E.g. errors are found near the centre of ceilings and floors due to inappropriately trained network weights. 

Interestingly, this lack of discrepancy between the classification of different sensor data is extremely beneficiary for developing generalized deep learning models. 

For instance, the training data from different sensors can be combined to train a network without having to fear it will not properly train the weights. Also, models that are trained on data from one specific sensor can be used on other sensory data without a significant drop in performance. It is therefore very likely that synthetic traing data can be added to the already existing training data and that the model will be further improved. As such, we can make training datasets more balanced and include scenes that would otherwise be very rare in real-world datasets.

\input{Tables/Table_BIM_achievements}

Given the above experiments, the points cloud suitability for the BIM structure classes can be compiled for each sensor (Table~\ref{tab:BIM_achievements}). To this end, expert modellers visually inspected the point clouds and tested whether object class instances their parameters could be reliably set for the different point clouds. It is important to notice that a proper reconstruction requires both suitable point cloud characteristics and that the relevant portions of each object are properly semantically segmented. Overall, the NavVis VLX shows the best results for LOD200-300 reconstructions. This type of portable system is the ideal setup to achieve the highest possible accessibility and have an efficient data acquisition without the need to sacrifice sensor quality due to the weight restrictions. The experiments show that both abstract wall, ceiling and floor classes as well as details and beam/column types can be reliably extracted up to LOD350 from the classified point clouds conform LOA20 and even LOA30. These systems do need to be supported by total station measurements but the speed of the data acquisition is sufficiently high to merit this approach. In contrast, TLS is still the most qualitative approach and allows modeling up to LOA40. However, this technique struggles to achieve sufficient coverage for LOD350 modeling which also negatively affects the semantic segmentation. Furthermore, TLS generates massive amounts of redundant data in overlapping zones and has disproportionately high detailing compared to its suboptimal coverage. TLS is among the slowest techniques but its efficiency can be increased when used as a standalone solution which is a suitable approach for mid-scale projects~\cite{Bassier2015}. The NavVis M6 and cart-based systems in general are equally fast as portable systems and offer similar benefits in terms of speed, accuracy and semantic segmentation. As such, they are outperformed by backpack-based systems that have increased accessibility and thus coverage. Also backpack-based systems allow to map stairs and so connect different datasets, where this is not supported by most cart-based solutions. Overall, these systems in 2021 are a suitable solution for LOD200-350 modeling up to LOA20 and LOA30 if properly supported by TS. 

Finally, the Hololens 2 and other head-worn or hand-held devices offer a low-cost alternative to the above high-end systems. Their coverage rivals that of backpack-based systems although the range is restrictive. Also, despite their low data quality, their semantic segmentation is surprisingly good. As such, these systems are capable of LOD200 modeling/analyses up to LOA10 and even LOA20 if properly supported by control measurements or in small areas. Currently, the spatial resolution and the accuracy are the main obstacles to reliably produce LOD300-350 of exact beam/column profiles, accurate wall thickness, etc. Also, the speed is surprisingly low (3x slower than high-end mobile mappers) due to the limited field of view and range that both result in much longer pathing. Overall, these systems are best suited to be used on conjunction with other mapping systems that also provide proper alignment i.e. standalone TLS or iMMs plus TS. 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Conclusions}\label{sec:Conclusions}
In this paper, the relation between the point cloud data acquisition and the unsupervised data interpretation for as-built BIM modeling/analyses is analysed to advance the state of the art in point cloud technologies. More specifically, the impact of point cloud data acquisition technologies on the semantic segmentation for structure classes is analysed. In a first step, the sensor capacity of state-of-the-art data acquisition systems is evaluated for the production of detailed and accurate point cloud data. In a second step, the input point clouds are analysed for their modeling/analyses suitability conform the LOD and LOA specifications. To this end, the input point clouds are processed by a pretrained RandLA-Net so that for the first time, the impact of point cloud characteristics on the semantic segmentation are quantified and related to their information extraction suitability.  

% experiments
In the experiments, four types of sensors are tested: static TLS (Leica P30), cart-based iMMs (NavVis M6), backpack-based iMMs (NavVis VLX) and head-worn iMMs (Hololens 2). The point cloud suitability is quantified by the quality, completeness, detailing of the sensor data and the IoU of the semantic segmentation. Overall, it is concluded that the high-end sensors can be used to model/evaluate geometries up to LOD300-350 by LOA20 and LOA30 if properly supported. Specifically, the NavVis VLX shows the best results for unsupervised point cloud processing automation due to its high coverage and accuracy. Other high-end systems achieve similar results but can struggle with occlusions due lower mobility or slower data acquisition. The low-end Hololens 2 is better suited for close-range LOA10 and LOA20 applications due to its limited range. An important conclusion is that the semantic segmentation is not significantly impacted by the large discrepancies in point cloud characteristics. The accuracy and detailing have little to no impact and while occlusions do impact the results locally, deep learning networks can overcome this lack of information even for the low-end Hololens 2 data. Overall, we can state that point-based semantic segmentation models do not significantly suffer from differences in the input point clouds. What differences remain can easily be overcome by adding more suitable training data to the network. This poses a great opportunity for 3D data interpretation since inputs from multiple sensors can be combined to provide much needed deep learning models. As such, the currently scarce and heterogeneous point cloud benchmark datasets can be jointly leveraged. It is important to notice that this sensor invariance also opens the door to synthetic and automatically labeled training data which underexplored for 3D scene interpretation.  

% what comes from this work + future work
This work provides crucial information for researchers and software developers to take into consideration the combined impact of the initial data acquisition and the semantic segmentation and unsupervised point cloud processing for BIM reconstruction/evaluation. Specifically, this work will serve as the basis for future work to build deep learning networks for 3D semantic segmentation that are sufficiently robust for market adoption. The next steps is to generate more (synthetic) data for these networks and investigate whether texture or imagery can offer complementary information to improve the interpretation and reconstruction of heavily occluded building environments.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\authorcontributions{Sam De Geyter, Jelle Vermandere, Maarten Bassier, Heinder De Winter and Maarten Vergauwen contributed equally to the work.}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\funding{This project has received funding from the VLAIO BAEKELAND programme (grant agreement HBC.2020.2819) together with MEET HET BV, the VLAIO COOCK project (grant agreement HBC.2019.2509), the FWO Postdoc grant (grant agreement: 1251522N) and the Geomatics research group of the Department of Civil Engineering, TC Construction at the KU Leuven in Belgium.}

\conflictsofinterest{There are no conflicts of interest to report.}

%=====================================
% References, variant A: internal bibliography
%=====================================
\reftitle{References}
\bibliography{bibliography}

\end{document}


%=================================================================
\documentclass[remotesensing,article,accept,moreauthors,pdftex]{Definitions/mdpi} 
%\graphicspath{{Figures/}}

%=================================================================
\firstpage{1} 
\makeatletter 
\setcounter{page}{\@firstpage} 
\makeatother
\pubvolume{1}
\issuenum{1}
\articlenumber{0}
\pubyear{2022}
\copyrightyear{2022}
\externaleditor{{Academic Editor: Mohammad Awrangjeb} 
} % For journal Automation, please change Academic Editor to "Communicated by"
\datereceived{15 April 2022} 
\dateaccepted{30 May 2022} 
\datepublished{} 
%\datecorrected{} % Corrected papers include a "Corrected: XXX" date in the original paper.
%\dateretracted{} % Corrected papers include a "Retracted: XXX" date in the original paper.
\hreflink{https://doi.org/} % If needed use \linebreak
%\updates{yes} % If there is an update available, un-comment this line
\usepackage{caption}
\usepackage[labelformat=simple]{subcaption}
\renewcommand\thesubfigure{\alph{subfigure}}
\DeclareCaptionLabelFormat{subcaptionlabel}{\normalfont\hspace{60pt}(\textbf{#2}\normalfont)}
\captionsetup[subfigure]{labelformat=subcaptionlabel}

%\usepackage{graphicx}
%\usepackage{amssymb}
%\usepackage{pdfpages}
%\usepackage{lineno}
%\usepackage{hyperref}
%\usepackage{mathtools}
%\usepackage{booktabs}
%\usepackage{kpfonts}
%\usepackage{algpseudocode}
%\usepackage{algorithm}
%\usepackage{gensymb}
%\usepackage{subcaption}
%\usepackage{todonotes}
%\usepackage{soul,color}
%\usepackage{booktabs}

%\usepackage{placeins}
%\usepackage{setspace}
%\usepackage{geometry} % added 27-02-2014 Markus Englich
%\usepackage{epstopdf}
%\usepackage{breqn}
%\usepackage{dblfloatfix}
%\usepackage{url}
	%\usepackage{multirow}
%\usepackage{textgreek}
%\usepackage[T1]{fontenc}
%\usepackage{lmodern}
%\usepackage{lscape}

%\usepackage{graphicx}
%\usepackage{amssymb}
%\usepackage{pdfpages}
%\usepackage{lineno}
%\usepackage{hyperref}
%\usepackage{mathtools}
%\usepackage{booktabs}
%\usepackage{kpfonts}
%\usepackage{algpseudocode}
%\usepackage{algorithm}
%\usepackage{gensymb}
\usepackage{subcaption}
%\usepackage{amsmath}

%\usepackage{pgfplots}
%\usepackage{rotating}
% \usepackage{makecell}
%\usepackage{tabu}

% \usepackage{multirow}
% \usepackage[utf8]{inputenc}
% \newcolumntype{C}[1]{>{\centering\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}
%\usepackage{float}


%\DeclareMathOperator*{\min}{min} 
%\DeclareMathOperator*{\max}{max} 
\DeclareMathOperator*{\argmin}{argmin} 
\DeclareMathOperator*{\argmax}{argmax} 

\newcolumntype{L}[1]{>{\raggedright\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}
\newcolumntype{C}[1]{>{\centering\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}
\newcolumntype{R}[1]{>{\raggedleft\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}


%=================================================================
% Full title of the paper (Capitalized)
\Title{Two-Step Alignment of Mixed Reality Devices to Existing Building Data}
%MDPI:
%1. The initial layout for your manuscript was done by our layout team. Please do not change the layout, otherwise we cannot proceed to the next step.
%2. Please do not delete our comments.
%3. Please revise and answer all questions that we proposed. Such as: “It should be italic”; “I confirm”; “I have checked and revised all.”
%4. Please directly correct on this version. 
%5. Please make sure that all the symbols in the paper are of the same format.
%6. Please confirm and provide a graphic abstract.




\TitleCitation{Two-Step Alignment of Mixed Reality Devices to Existing Building Data}
% Author Orchid ID: enter ID or remove command
\newcommand{\orcidauthorA}{0000-0002-7809-9798} % Add \orcidA{} behind the author's name
\newcommand{\orcidauthorB}{0000-0001-8526-8847} % Add \orcidB{} behind the author's name
\newcommand{\orcidauthorC}{0000-0003-3465-9033} % Add \orcidC{} behind the author's name

% Authors, for the paper (add full first names)
\Author{Jelle Vermandere †\orcidA{}, Maarten Bassier *†\orcidB{} and Maarten Vergauwen †\orcidC{}}
%MDPI:  Please carefully check the accuracy of names and affiliations.
 %everything is correct
 
\AuthorCitation{Vermandere, J.; Bassier, M.; Vergauwen, M.}
%MDPI:  Please check all author names carefully.
% correct

% Authors, for metadata in PDF
\AuthorNames{Jelle Vermandere, Maarten Bassier, Maarten Vergauwen}



% Affiliations / Addresses (Add [1] after \address if there is only one affiliation.)
\address[1]{Department of Civil Engineering, TC Construction---Geomatics,Faculty of Engineering Technology, KU Leuven,9000 Ghent, Belgium;  jelle.vermandere@kuleuven.be (J.V.); maarten.vergauwen@kuleuven.be (M.V.)}
%MDPI:
%1. please consider this suggested change of Dept. into full spelling
%2. KU Leuven seem to be a university, we adjusted the order and punctation of '' KU Leuven - Faculty of Engineering Technology '', please confirm
%3. Please check that the address information is complete correct. The provided information should be arranged from subordinate to superior. 
%4. Please check if the information provided presents more than one address. If so, please separate the addresses into different affiliations.
%MDPI: 5. please add post code
% okey



% Contact information of the corresponding author
\corres{\hangafter=1 \hangindent=1.05em \hspace{-0.82em} Correspondence: maarten.bassier@kuleuven.be}

% Current address and/or shared authorship
\firstnote{\hangafter=1 \hangindent=1.05em \hspace{-0.82em} These authors contributed equally to this work.} 
 %MDPI:  please add a dagger symbol after author name as a citation of this note; or please remove this firstnote
% symbol added


%\secondnote{The authors contributed equally to this work.}

\abstract{With the emergence of XR technologies, the demand for new time- and cost-saving applications in the AEC industry based on these new technologies is rapidly increasing. Their real-time feedback and digital interaction in the field makes these systems very well suited for construction site monitoring, maintenance, project planning, and so on. However, the continuously changing environments of construction sites and facilities requires extraordinary robust and dynamic data acquisition technologies to capture and update the built environment. New XR devices already have the hardware to accomplish these tasks, but the framework to document and geolocate multi-temporal mappings of a changing environment is still very much the subject of ongoing research. The goal of this research is, therefore, to study whether Lidar and photogrammetric technologies can be adapted to process XR sensory data and align multiple time series in the same coordinate system. Given the sometimes drastic changes on sites, we do not only use the sensory data but also any preexisting remote sensing data and as-is or as-designed BIM to aid the registration. In this work, we specifically study the low-resolution geometry and image matching of the Hololens 2 during consecutive stages of a construction. During the experiments, multiple time series of constructions are captured and registered. The experiments show that XR-captured data can be reliably registered to preexisting datasets with an accuracy that matches or exceeds the resolution of the sensory data. These results indicate that this method is an excellent way to align generic XR devices to a wide variety of existing reference data.}

% Keywords
\keyword{XR; BIM; point cloud; structure-from-motion; AECO; construction site monitoring}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\begin{document}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Introduction}\label{sec:Introduction}
With the increasing digitisation of the built environment, innovative technologies are needed to visualise and interact with this digital information in the field~\cite{PerkinsCoieLLP2020}. This is where extended reality (XR) devices can provide a solution. XR devices, whether they are handheld, head-worn, or otherwise, strive to integrate the digital environment with the real world~\cite{Alizadehsalehi2020a}. In the architectural, engineering, and construction (AEC) industry, XR technologies can be leveraged for a range of different domains, i.e., property visualisation in real estate, conceptualization in architecture, digital overlays of design schemes in construction and maintenance, and so on~\cite{Zhang2020a}. Moreover, XR devices contain a number of mapping sensors that can aid in the tracking of construction or fabrication errors, improve worker efficiency, and even improve safety by highlighting needed/dangerous objects. Overall, XR technologies benefit immensely from an increased digital built environment and vice versa. 

The key bottleneck to linking the digital to the built environment is the alignment of both environments. Concretely, this implies that the remote sensing data captured by XR devices including depth maps, polygonal meshes, RGB imagery, and so on, must be aligned with the same coordinate system as the virtual data. Multiple works have been dedicated to solving this problem, but, up until now, have had glaring weaknesses that prevent XR technologies from being deployed for extended periods in industrial environments. A major factor is the changing nature of construction sites and facilities where we look to deploy these systems. Current registration algorithms do not cope with partially changed environments and are prone to misalignment. Additionally, the lack of Global Navigation Satellite System (GNSS) availability remains a major obstacle and only the images or the geometries separately are used for the registration which easily falter in the challenging measurement conditions of construction sites and facilities. 

Therefore, the goal of this research is to develop a registration framework that deals with the above obstacles. Concretely, we look to create a pipeline which can create an accurate global pose and orientation estimation of a sensor, given its sensory data, by matching the data with existing geolocated reference data. As such, our method can process any predated Lidar or photogrammetric point clouds and 2D images of the facility. Additionally, the Building Information Modelling (BIM) model that is present of the site is also used as a reference for the positioning of the XR device as robustly and accurately as possible. The main contributions of this work are as follows: 

\begin{enumerate}
    \item  A novel multi-source approach that computes a more robust and accurate pose and orientation estimation within pre-documented facilities; 
     \item  A novel multi-temporal framework that processes the data of consecutive changed environments using semantic web technologies;
     \item  \textls[15]{An empirical study of the framework during the consecutive stages of a real constructions};
     \item  An extensive literature study on XR registration technologies on construction sites and facilities.  
\end{enumerate}

The remainder of this work is structured as follows. The background and related work is presented in Section~\ref{sec:Background_Related work}. In Section~\ref{sec:Sensors}, the sensors used in this study are presented. Following is the methodology for the capacity and semantic segmentation suitability study in Section~\ref{sec:Methodology}. In Section~\ref{sec:Test Data}, the test sites are introduced along with their corresponding results in Section~\ref{sec:Experimental results}. The test results are discussed in Section~\ref{sec:Discussion}. Finally, the conclusions are presented in Section~\ref{sec:Conclusions}.

% %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Background and Related work}\label{sec:Background_Related work}
In this section, the related work for the key aspects of this research are discussed: (1) XR data and applications for construction execution and monitoring; (2) XR-based Lidar-and photogrammetric registration techniques; and (3) the multi-temporal Linked Data management of construction and facility data. 

\subsection{XR in the AEC Industry}
XR applications are a combination of virtual (VR), augmented (AR), and mixed (MR) reality. In the case of the AEC industry, each component has its unique uses. For instance, VR application excel at simulations and the design phase of constructions. VR applications are created to digitally test as-designed facilities for user friendliness, to simulate evacuation plans, communicating with clients and so on~\cite{Wu2021,Du2018a}. The as-designed BIM model plays a pivotal role that simultaneously is both the virtual reality environment and the project design database. As such, VR applications directly extend architects and engineers capabilities to better plan a project through XR-driven design, simulations, and so on~\cite{PourRahimian2019,Boton2018}. AR applications are created once an asset is constructed or an existing facility needs to be renovated or maintained~\cite{Coupry2021}. In this case, the XR technologies bring the BIM to the field to better execute the project, i.e., by visualising objects on site, overlaying plan information, such as electrical grids, and so on~\cite{Chu2018}. MR applications incorporate aspects of both AR and VR, and typically blend digital information with real objects. For instance, MR applications have been designed to highlight connectivity of electrical grids in existing buildings~\cite{Chalhoub2018a}, or a BIM-based facility management platform that guided workers to repair highlighted components~\cite{Chen2019c}, and many others~\cite{Diao2019}. Some experiments also have been performed to use MR for construction monitoring to detect defects~\cite{Park2013,Al-Sabbag2022} which is pf  particular interest to this work since it requires an extensive processing of the XR data.


In terms of data, current XR devices nearly always have on-board cameras and optional RGBD cameras, Lidar sensors, and Inertial Measurement Units (IMU). One of the more recent examples is the Hololens 2, which is equipped with a 8~Mp RGB camera and four gray-scale cameras, a holographic processing unit (HPU), and a 1~Mp Time-of-Flight (ToF) sensor. The resulting data are a polygonal mesh generated from the point clouds of the ToF sensor and a series of images that are locally registered using structure-from-motion photogrammetry and the IMU measurements in a performant SLAM algorithm. Currently, the meshes are not colourised and by default are sub-sampled to a resolution of (0.08 m$^3$). 
Other devices on the market have similar sensors, but usually lack certain aspects. That is why the Hololens 2 is chosen for this study, since it provides a wide array of data. Current generation smartphones also have XR capabilities by using ArKit and ArCore for IOS and Android devices, respectively, but most of these devices lack ToF sensors. However, in order to validate the usability of the proposed framework, localised imaging data from these device will also be taken into account. 

Aside from the XR data itself, one should also consider the preexisting data repositories that will be used for the alignment in this work. Most facilities are captured using Terrestrial Lasers Scanners (TLS). These are Lidar-based systems that can capture up to 2 million points per second of their surroundings. Indoor Mobile Mappings systems, such NavVis M6 and VLX, are also employed but these are typically supported by a total station~\cite{Vlx2021}. The resulting point cloud is among the most accurate geospatial data with single point accuracies of <5~mm for high-end systems~\cite{Bassier2016TLS}. However, TLS can suffer from occlusions due to the limited number of setups of the scanner used to capture a facility. A second data repository are images taken in and around the facility by handheld cameras, smartphones, Unmanned Aerial Vehicles (UAVs), surveillance cameras, and so on. These images can be geolocated through photogrammetric routines similar to procedures we use in this work (Figure~\ref{fig:reference_data}). Geolocated imagery are also generated by TLS themselves in the form of panoramic imagery or cuboid images. Finally, there are also the BIM databases themselves to consider. As-built or even as-designed BIM models have somewhat abstract geometries of the main objects in the facility, including the structure, windows, doors, and perhaps also fixed furniture and mechanical, electrical, and plumbing (MEP) elements. Overall, each asset has some preexisting data that can be used as a reference for the registration. However, it is important to notice that significant parts of the preexisting data are outdated due to construction progression or changes, refurbishment, or interior changes. Aside from the physical changes, the lighting conditions and weather conditions can drastically alter the appearance of facilities which is particularly true for construction sites.

\begin{figure}[H]
    \includegraphics[width=\textwidth]{Figures/TestvsRef.png}
    \caption{Examples of a reference dataset and a local measurement session of the same structure, taken at different times with different sensors. The localised images in relation to the geometry are displayed in red.}
    \label{fig:reference_data}
\end{figure}


\subsection{XR Registration Techniques}
% XR advancements 
The XR pose estimation is split into a local and global estimation. First, an XR system should keep track of its own location within a measurement session. To this end, Simultaneous Localisation And Mapping (SLAM) algorithms are proposed that use the sensor's IMU, GNSS if available, image and geometric data to continuously estimate the sensor's pose and orientation within the local coordinate system. Most SLAM methods are solely based on 2D or 3D and are supported by an IMU, with visual Slam being the most popular choice~\cite{Taketomi2017}. For instance, the Hololens 2 combined with the Microsoft Mixed Reality API relies on ORB-SLAM~\cite{Cyrus2019}. 

Aside from the matching between consecutive sensor setups, loop closure is a key feature in SLAM approaches. If the sensor revisits a known location in the local coordinate system, the error of the path in between both encounters can be adjusted to compensate for drift. To this end, a bundle adjustment is computed for all observations in the loop which drastically reduces the error. Any Indoor Mobile Mapping System (iMMs) mapping is, therefore, encouraged to make as many loops as possible and also to capture control points along the trajectory to keep the error propagation in check.  

Overall, the combined geometry and visual SLAM work well both in indoor and outdoor environments. From accuracy tests in our previous work, we found that entire spaces can be mapped up to LOA20~\cite{U.S.InstituteofBuildingDocumentation2016} [2$\sigma\leq$ 0.5 m] %MDPI: we removed italics of unit, please confirm % confirm
 given sufficient control and loop closures ~\cite{DeGeyter2022}. The sensor trajectory in itself is more accurate since the inaccuracy of the Hololens 2 Lidar sensor (0.01 m/10 m) and the sub-sampling must be considered. 

Second, the XR device must be positioned within a preexisting coordinate system, which is the focus of this work. This global alignment is achieved either directly by measuring GNSS signals or by retrieving the correspondences between the local measurements and a global reference dataset. In this work, where we target both indoor and outdoor environments within existing facilities or facilities that are under construction, we will not consider the direct alignment methods as a GNSS-hemisphere only provides sufficient accuracy in a wide open outdoor space. Instead, we discuss the related work to retrieve correspondences between a local and a reference dataset through exact, approximate, and indirect correspondences.

\subsubsection{Exact Correspondences} These are spatial anchors with accurate coordinates, e.g., targets established by total station. These correspondences serve as control points and can be used to align the local measurements using a rigid body transformation or even can be used within the SLAM processing to improve the results~\cite{Marchand2016}. These correspondences can be used in any environment but can be quite costly or impractical to establish. Exact correspondences can also directly stem from preexisting Lidar or image datasets. If a repository of referenced images and/or scans exist of the facility, image feature matching or geometric feature matching can be used to yield accurate spatial correspondences. To this end, conventional computer vision or Lidar registration techniques can be used. For instance, Liu et al.~\cite{Liu2018b} initialize their SLAM in outdoor environments by estimating the relative pose of the sensor from a set of localised panoramic images. Multi-view object detection and localization is also proposed, which uses feature matching to the global database, point triangulation and registration~\cite{Ventura2014}. Convolutional Neural Networks (CNN) are also proposed for the feature extraction. For instance, Brachmann et al.~\cite{Brachmann2019} extract CNN features and apply Expert Sample Consensus (ESAC) to deal with scene outliers. A serious challenge for these reference-based methods are the changes to the environment between the reference and the newly collected data and dynamic scene elements. To compensate for this, derivative features are proposed, such as vanishing lines or geometry line features, that are less likely to belong to temporal objects~\cite{Zollmann2020}. Additionally, as reference datasets can become quite large, real-time processing is problematic. Finally, the repetitivity of the target facility might confuse the pose estimation, e.g., by finding matches in the wrong room.


\subsubsection{Approximate Correspondences} These are spatial anchors that do not have exact coordinates but are linked to a certain location within the facility, e.g., a specific room. Typical examples of these anchors are markers, Bluetooth Low Energy (BLE) i.e., XBee or ZigBee, VHF, Wi-Fi access points and so on~\cite{Liu2020}. XR devices can detect these correspondences which narrows the pose estimation task to the localisation within a single room. A second-step fine-alignment is then used to estimate the exact pose of the sensor, which is analogue to the exact correspondences. The final positioning of the XR-device is then determined by one of the above methods. This method is very well suited for existing buildings but it is rather costly because of the number of beacons needed and is challenging to apply on construction sites or facilities that do not have a room-based layout. 

The major advantage of approximate correspondences is that placing these beacons is much less labor intensive than the above defined accurate spatial anchors. However, this method is mostly restricted to existing facilities with fixed room-based layout. Additionally, non-visual approximate correspondences can be error prone as there can be confusion about the exact room since the beacon with the highest signal strength is not necessarily the same room due to multi-pathing and ambiguous wall materials.

\subsubsection{Indirect Correspondences} This technique uses the signalling beacons to calculate the sensor's position, typically by means of triangulation. To this end, the same beacons as described above are strategically spread out across the structure and their coordinates are accurately determined. The XR-device then measures the signal intensity to the closest beacons and triangulates the sensor's position based on the signal strength of at least three beacons. This approach works well in open spaces and yields an exact pose estimation. However, in indoor spaces, the distance calculation is extremely ambiguous due to multi-pathing of the signals, the unknown materials and objects that the signal passes through, and so on. In practice, this technology also only presents a coarse pose estimation and a more accurate second registration step is needed to properly align the XR device in the common coordinate system. 

Overall, reference datasets are considered the most complete option to robustly initialise the pose of XR devices. If the geometric or visual feature estimation can be made less ambiguous to the structure's repetitivity and less computationally demanding, this technique can be used throughout consecutive building stages, from early construction to facility management and, finally, demolition. 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%this first section will talk about how the project is handled
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Methodology}\label{sec:Methodology}

This section explains the overall structure of the method. Concretely, we discuss \linebreak (1) the data preprocessing for the BIM, image, and geometry reference data and consecutive XR-data captures of a site; and (2) the global XR pose estimation per session based on visual and geometric features. It is important to notice that the continuous local pose estimation within a session by the Microsoft SLAM API (Scene understanding SDK), in combination with the Hololens 2, is left unaltered as it is proven to yield reliable and accurate results for small-scale scenes \cite{DeGeyter2022}. Instead, we pursue the localisation of the entire data acquisition session with respect to the facilities' coordinate system in the cadre of interfacing with and updating the facilities' digital twin (Figure~\ref{fig:method}). 

\begin{figure}[H]
    \includegraphics[width=\textwidth]{Figures/Methodology.png}
    \caption{\textls[15]{Overview of the proposed two-step alignment process based on both image and \linebreak geometric features.}}
    \label{fig:method}
\end{figure}

\subsection{Data Preprocessing}\label{sec:Sensors}
When handling a wide variety of datasets across different time periods, there must be a joint framework to link and jointly process the data. In this work, we will utilize the geospatial component of the remote sensing and BIM data to link the different datasets. When a surveillance is made, the captured data are generally stored with respect to a common reference point. This collection of data are referred to as a session. Sessions can contain images, meshes, point clouds, and even BIM models, all with their own relative transformation. All the data need to be geolocated, and since there are a number of different standards, it is important to always include which coordinate system is being used. The three most used in Belgium are: WSG, Lambert72, and Lambert2008. For this section, the reference and test data are handled separately. However, it is important to notice that each new session can be used as a reference for future test data and, thus, both data structures need to be standardized. To this end, semantic web technologies are leveraged to manage the spatial and temporal metadata of each reference dataset. Concretely, a set of light weight RDF graphs are constructed, which are updated with each new XR data capture. The geospatial data processing, metadata extraction, RDF graph conceptualization, and implementation of each dataset is discussed below.

\subsubsection{Reference Data}\label{sec:ref data}
The following reference data are considered for the global pose estimation (Figure~\ref{fig:features}): the BIM digital twin in a preset coordinate system (preferably geolocated), Lidar data, localised image data, and XR data captured from previous sessions. These repositories are preprocessed and geolocated as follows.

\vspace{-8pt}
\begin{figure}[H]
	\resizebox{1.0\textwidth}{!}{%
		\begin{tabular}{C{8cm} C{8cm}} 
			\hspace{-10pt}\includegraphics[height=5.8cm]{Figures/FeaturesExample.png} & \hspace{12pt}\includegraphics[height=5.5cm]{Figures/3D featureExample.png}\\ %\\[2pt]
			%
			(\textbf{a}) \textbf{2D Image Features (ORB)} &(\textbf{b}) \textbf{3D Mesh Features (FPFH)} \\
			typical amount: 10,000  & typical amount: 10,000  \\
			typical resolution: 4 MP        & typical resolution: 5 cm$^3$  \\
			%
			\includegraphics[width=6cm]{Figures/Pointcloud Feature Example.png}& \includegraphics[height=6cm]{Figures/experimental results/BIM Feature example.png} \\%[2pt]
			%
			(\textbf{c}) \textbf{3D Point Cloud Features (FPFH)} & (\textbf{d}) \textbf{BIM Features (SUPER4PCS)}\\
			typical amount: 10,000 &  typical amount: 1000\\
			typical resolution: 5 cm$^3$ & typical resolution: 5 cm$^3$\\
		\end{tabular}
	}
	\caption{Overview of the different types of data with their respective features for geolocalisation.}
	\label{fig:features}%MDPI: Please check if subfigure explanations could be moved into Figure caption. % the information is too dense to fit everything in the caption
\end{figure}



\paragraph{Imagery}%MDPI: we removed bold of the level 4 headings, please confirm ´% confirm
{The} preexisting imagery of a facility is one of the most promising methods to align newly captured XR data (Figure~\ref{fig:features}a). There are iMMs and TLS images to consider, as well as panoramic imagery, images taken by smartphones, handheld cameras, and UAVs. The iMMs and TLS imagery are already localised during the Lidar registration processing. Other imagery is processed by structure-from-motion (SfM) software, such as MetaShape or RealityCapture, to estimate the camera's interior and exterior orientation parameters, including the focal length, position, rotation, and so on. During this process, control points, either from Lidar, total station, or GNSS measurements, need to be manually added to the image collections to properly reference and scale the imagery. The result is a set of images accompanied with an RDF graph that contains the location and orientation of each image along with its camera parameters and timestamp. Additionally, the Oriented FAST and Rotated BRIEF (ORB) features extracted in the SfM pipeline are stored per image, so no additional preprocessing is needed in the pose estimation step.

\paragraph{{Point Clouds}}
Facilities are increasingly scanned at key stages of their life-cycle, such as during construction, renovations, and so on. These captured data, either with static TLS or iMMs, generate a collection of Cartesian coordinates with optional colour and intensity values per setup or trajectory (Figure~\ref{fig:features}c). During the post-processing of these datasets, control points established with GNSS or total station are added to geolocate the resources. The geolocated points clouds in an E57 format are the starting point of our method. As our codebase operates in Open3D~\cite{Zhou2018a}, each point cloud is converted to the PCD format. There are both ordered and unordered point cloud datasets to consider. For instance, .e57 %MDPI: please check if the dot is correct % this is correct, it is an extension
 point cloud files containing a collection of per setup captured structured point clouds are stored as separate PCD files. The e57xmldump tool~\cite{Zwierzycki2016} is used to first extract the E57 metadata information which is than parsed using the RDFlib API~\cite{Carl2018}. The resulting metadata is stored as triples in an RDF graph \textit{pcdGraph.tll} %MDPI: is the italics necessary? % the italics are to distinguish a file name
 which is serialized using the turtle syntax. During this operation, a heavily downsampled voxel octree and a set of Fast Point Feature Histograms (FPFH) geometric features is extracted from the Lidar data that will serve as reference for the pose estimation~\cite{Rusu2009b}. The octree and features are also stored in the RDF graph so they can be reused throughout consecutive pose estimations without the need to load the original point cloud data. 

\paragraph{{Polygonal Meshes}}
Polygonal meshes can both stem from remote sensing or from the Building Information Model (discussed below) (Figure~\ref{fig:features}b). The former is a direct product of XR device data captures, such as the Hololens 2 or the SfM pipelines as discussed above, that generate textured mesh geometry of the facility. Additionally, Lidar point clouds can be processed to polygonal mesh geometries using various meshing techniques, such as Poisson meshing variants~\cite{Wiemann2015}. The features that are extracted from the polygonal meshes are the same as those extracted from the point cloud data. To this end, point clouds are sampled on the mesh surfaces and subjected to the same feature extractors, as described above. Analogue to the point cloud processing, the features, bounding box, centroid, and so on are stored in an RDF graph.

\paragraph{{Building Information Model}}
\textls[-15]{The geospatial representation of BIM elements can be either defined by BREP or polygonal mesh representations (Figure~\ref{fig:features}d). As such, they are compatible with the same code as for the polygonal mesh geometry processing. However, using the point cloud features on an abstract BIM model are likely to fail due to modeling abstractions and sparsity of the BIM. Therefore, plane-based descriptors are extracted from the BIM geometries and stored in an RDF graph bimGraph.ttl. Specifically, we extract Super4PCS features, as described in~\cite{Mellado2014}. The descriptors are stored using their absolute coordinates, which also includes the translation and rotation of the BIM project with respect to the global coordinate system.}

The main goal of the alignment is to position the XR device's data in the world. For that, the reference data needs to be geolocated. There are a number of international standards for geolocating data, so each RDF resource is enriched with the coordinates system information that is being used. Each resource is also given an accuracy parameter which will play an important role in the pose estimation reliability. This accuracy metric is either directly obtained from the processing of the remote sensing data, i.e., the network error in Lidar networks or the mean error on the control points in a SfM pipeline. For the BIM geometries, a default 0.05 m accuracy is chosen as conform LOA20~\cite{U.S.InstituteofBuildingDocumentation2016}, which is a safe option considering the common abstractions of BIM models.

\subsubsection{XR Data Capture}\label{sec:xr data capture}
This work focuses on cross platform compatibility, so we try to capture and link as much data as possible. Therefore, the codebase, which is developed in Unity3D, accepts common inputs from various XR devices. Specifically in this work, we build our framework against the Hololens 2 and Android smartphone inputs to showcase the multi-sensor inputs. Analogue to the reference data, the XR data are organised in periodic sessions. Each session contains a global reference point, a number of images and meshes. Since these data are captured (near) real time, the fidelity and file size is relatively small. This lowers the time to transfer files across devices and also the computation time. Note that it is not required to have both 2D and 3D data available in a session, as not all devices contain the necessary sensors to capture both. The pose estimation is specifically designed to deal with very limited data and provides different methods depending on the input.

\paragraph{Two-Dimensional Capture}
Images are captured using the on-board device cameras. As previously mentioned, We rely on the XR SLAM capabilities to track the subsequent sensor poses within the session. As such, the relative location and orientation of the imagery is directly adopted into the RDF graph which is identical to the imageGraph proposed above. once a new image is captured by the device, it is send to a server that automatically extracts the relevant metadata and features and stores this information in the session's RDF graph.

\paragraph{Three-Dimensional Capture}
Some XR devices are equipped with special sensors that can capture depth, such as the ToF on the Hololens 2, or RGBD sensors of some Android devices. Using these data, the XR SLAM can create a real-time mesh of the environment. Specifically for the Mixed Reality API of the Hololens 2, the mesh is dynamically built from consecutive blocks of by default 8 m$^3$. By default, the spatial resolution of the mesh is kept rather low to save computational resources, but this can be changed for a more detailed mapping. Because the generated mesh is spatially sub-sampled, the distance to the environment is largely irrelevant as long as the structure remains within range of the sensor. Once a number of cells is captured, the mesh is sent to the server where it can commence the 3D pose estimation process. To this end, an RDF Graph similar to the meshGraph defined above is generated from the raw mesh and serialized in a .ttl file.  

\subsubsection{RDF Schema}
There is a clear need for standardisation, due to the fact that a lot of the reference data will come from diverse sources and different periods throughout the building's life-cycle. Properties such as an id, position, and rotation already have strict schemes built out, so it is imperative that we use the same standards. Currently, we implement RDF, RDFS for the general concepts. GEO is used overall to represent the geospatial information of the resources, while EXIF is specifically used early on to extract the metadata from the images. We rely on OMG for the geometry definitions and the pathing of each session. For the sensory metadata, including the position, centroid, bounding box, number of points, vertices, faces, and so on, we use the OpenLabel which is extensively used for mobile mapping and navigation data. Finally, the Image, Mesh, and Point Cloud classes are designed on top of our V4Design ontology and have a series of relationships that govern exchange of information between the classes~\cite{Bassier2020a}. A feature relation is also defined to store the resources' 2D and 3D features along with the description of the feature type (ORB, SIFT, etc.). The Arpenteur \cite{Ellefi2018} ontology is also used that already defines a number of relationships for SfM processes and fits well with this framework.

\subsection{Pose Estimation}\label{sec:pose estimation}

% Survey and evaluation of monocular visual-inertial SLAM algorithms for augmented reality => math on VSLAM
% Instant SLAM initialization for outdoor omnidirectional augmented reality => math on 2D 3D correspondences
% Pose Estimation for Augmented Reality: A Hands-On Survey
% Envslam: Combining slam systems and neural networks to improve the environment fusion in ar applications
In this section, the pose estimation of the session is presented. The alignment is divided into two consecutive steps. First, an approximate global pose request is processed by the XR operator's android device to narrow the search area for the pose estimation. In a second step, an exact pose estimation is calculated using the XR captured data and the above described reference datasets. In the following sections, the global pose estimation and the subsequent 2D and 3D pose estimations are discussed in detail. 

% s(P_s,I_s,G_s)= session with point clouds, images and graphs
% S = all sessions
% r(P_s,I_s,G_s) = reference session with point clouds, images and graphs
% \boldsymbol{P_s} = all point clouds in a session
% I_s = all images in a session
% G_{P_s}  = point cloud graph of the session
% X_s = distinct 3D points in a point cloud of a session
% F_{3D} = features of 3D points in a point cloud of a session
% G_{i_s} = image graph of the session
% F_{2D} = features of 2D pixels in an image

$s\in S$ is a session that contains some point clouds $P_s \in \boldsymbol{P_s}$ %MDPI: please check if the all the bold of variable in the text is necessary % the bold symbols indicate the collection of the elements
(either from meshes, depth imagery or structured point clouds) and images $i_s \in I_s$. From the preprocessing, every $P_s$ has an RDF graph $G_{P_s}$ that contains its metadata, a set of distinct 3D points $X_s$ and 3D feature vectors $F_{3D}$. Analogue, every $i_s$ is stored in an RDF graph $G_{I}$ that contains the metadata, a set of distinct 2D pixels $x_s \in \boldsymbol{x_s}$ and 2D feature vectors $F_{2D}$.

$R$ are all the reference datasets that each contain some point clouds $P_r \in \boldsymbol{P_r}$ (either from meshes, depth imagery, structured point clouds or the Building Information Model) and images $i_r \in I_r$. From the preprocessing, all $P_r$ are stored in an RDF pcdGraph $G_{P}$ that contains the metadata, a set of distinct 3D points $X_r$ and 3D feature vectors $F_{3D}$ of each resource. Analogue, the $i_r$ are stored in and RDF imageGraph $G_{I}$ that contains the metadata, a set of distinct 2D pixels $x_r \in \boldsymbol{x_r}$ and 2D feature vectors $F_{2D}$ of the images.

\subsubsection{Global Alignment and Reference Data Selection}
As already mentioned, the bulk of the calculation will be performed on a server in order to ensure a smooth operation of the device and give access to all the reference data. The server is build in python, using the Flask framework. It is critical that the server has access to the reference data and has enough computing power too compute the tasks. The data captured on the XR device is organised in a session and send as a whole to the server, where it can be prepared for the pose estimation.

The first step of the alignment process is the sub-selection of reference data. Due to the large amount of reference data, it is not feasible to use every session in the pose estimation. This selection is performed by using the global pose retrieved from the XR device or another device in the general vicinity based on the HTML Geolocation API~\cite{W3Schools2022}. A positioning query is formulated on OpenStreetMap data using the Overpass API which generates a HTTP GET request, and receives a response in XML format. In the GNSS thread, a query is executed at the system start up, using the initial user position and a threshold radius. After this, a new query is executed when the user has moved significantly from the starting location given a distance threshold with respect to the last executed query. Since the device can be indoors or lack a GPS, the retrieved geolocation is not necessarily very accurate, with a error radius of circa 20 m. However, this is sufficient to narrow down the available reference data to reduce the computational effort of the precise localization algorithm. 

The result of this query is the initial session pose $[p_s \pm \sigma_g ]$ with the positioning accuracy as determined by the Wi-Fi, radio, and GNSS availability near the receiver. Given the pose, the relevant subsets of $\boldsymbol{P_r}$ and $ I_r$ are extracted. To this end, the Euclidean distance is evaluated between the focal point of each session image $i_{s}(c)$ and the focal point of each reference image $i_r(c)$. Analogue, when a session point cloud $P_{s}$ falls within the boundaries of a reference point cloud $P_r$ considering $\sigma_g$, the cloud is withheld as a valid reference (Equation~\eqref{eq1}) (Figure %MDPI: Figures should be cited in numercial order,  therefore, Figure 5 should be cited after Figure 4. Please confrim and add Figure 4 citation in the text, and make sure all figures first citation in numerical order. %changed
~\ref{fig:method_1}).

\begin{equation}
\begin{split}
\label{eq1}
% establish subselection of point clouds and images
& \boldsymbol{P'_r}=\left\{ P_r \in \boldsymbol{P_r} \Big|P_{r} \cap \left[P_{s,min}-\sigma_g;P_{s,max}+\sigma_g\right]\right\}  \\
& I'_r=\left\{i_r \in I_r\Big| i_{s} \in I_{s}: \|i_r(c)-i_{s}(c)\|\leq t_d+\sigma_g\right\}  \\
\end{split}
\end{equation}
where threshold $t_d$ serves as the distance threshold to limit the number of selected images. From the subsets $\boldsymbol{P'_r}$ and $I'_r$, the relevant graphs $G_{P}$ and $G_{I}$ are retrieved along with the 2D and 3D features. Overall, this selection step significantly lowers the computational complexity of the matching if a descent pose estimation accuracy is achieved. Moreover, the selection itself is also extremely efficient as the input variables are directly taken from the metadata graphs instead of having to transfer and evaluate the actual imagery and point cloud data.

\vspace{-6pt}
\begin{figure}[H]
    \includegraphics[width=0.98\textwidth]{Figures/global Estimation schema.png}
    \caption{Overview of the global alignment process between the reference datasets (red) and test dataset (blue) to establish a sub-selection of reference data with center $\boldsymbol{p_{s}}$ and error radius $\boldsymbol{\sigma_g}$.}
    %MDPI: Please add note for the pink  area at the bottom of the figure % confirm
    \label{fig:method_1}
\end{figure}

\begin{figure}[H]
	 \begin{subfigure}[]{0.99\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/Good Image Match.png}     
        % \label{fig}
    \end{subfigure}\\[0.5ex]
    \centering(\textbf{a}) \\
  \begin{subfigure}[]{0.99\textwidth}
    \centering
        \includegraphics[width=0.69\textwidth]{Figures/Good 3D match.png}
        % \label{fig}
    \end{subfigure}\\[0.5ex]
      \centering(\textbf{b})\\
	\caption{Overview %MDPI: this figure has not been refered to in the text. Please add Figure 4 citation in the text. % confirm
	of the feature matches between different data types. (\textbf{a}) %MDPI: we  moved subfigure caption here, please confirm % confirm
 Example of the epipolar lines of the ORB feature matches between two images with (green) successful matches and (red) erroneous matches. (\textbf{b}) Example of the successful FPFH %MDPI: Please make sure that permission has been obtained and there is no copyright issue. % these are our own created images
~\cite{Rusu2008} feature matches (lines) between points clouds captured in consecutive XR measurements.}
	\label{fig:feature_matches}
\end{figure}	


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\vspace{-9pt}
\subsubsection{Three-Dimensional Pose Estimation}\label{sec:3D pose}
% B-SHOT: A binary feature descriptor for fast and efficient keypoint matching on 3D point clouds

% Theiler2013: registration k4PCS

% Detecting and segmenting objects for mobile manipulation
\paragraph{Point Clouds and Meshes}
Given the 3D FPFH features of every reference point cloud and the session point cloud, a rigid body transformation is computed. To this end, a Fast global registration is applied as proposed by Zhou et al.~\cite{Zhou2016}. We specifically do not use ICP variants as it would require the transfer of all reference clouds to the server. Additionally, the method of Zhou et al. foregoes computationally demanding RANSAC variants for the correspondence matching and instead propose a correspondence estimation function. Concretely, the distances between correspondences $X_s'\in X_s$ and $X_r'\in X_r$ are minimized while simultaneously the correspondence outliers are neglected by the correspondence estimation function $\rho$ (Equation~\eqref{eq2}) (Figure \ref{fig:feature_matches}).
%  je moet wel de voxelised point cloud overbrengen
\begin{equation}
\begin{split}
\label{eq2}
% find T_r for which eucl distance between matches is minimal
& \argmin\limits_{\boldsymbol{T}_{P_s}} \sum_{X_r',X_s'} \rho \left(\| X_r'- \boldsymbol{T}_{P_s} X_s'\|\right)
\end{split}
\end{equation}
where the target rigid body transformation $\boldsymbol{T}_{P_s}$ between a reference cloud and the session cloud is found by minimizing the distance between correspondences in $X_s$ and $X_r$. Both for the feature descriptors and the rigid body transformation estimation, the Open3D framework is implemented based on the work of Zhou et al.~\cite{Zhou2016}. As previously mentioned, only the feature graphs are transferred to the server and, thus, only the transformation estimation is calculated in runtime which frees up computational resources. The resulting pose, as well as the RMSE, the number of inlier correspondences, and the bounding box of the inlier correspondences are stored with a relation to the reference point cloud. These metrics will be later used in the final pose estimation.

\paragraph{BIM Alignment}
The same logic is applied to the BIM model. Given the sampled meshes, SUPER4PCS is used as described by Mellado et al.~\cite{Mellado2014}. The approach relies on approximately congruent 4-point sets from a 3D point cloud that can be related by rigid body tranformations. A key innovation over the established 4-Points Congruent Sets (4PCS) algorithms is the computational dimensionality reduction from $O(n^2+k)$ to $O(n+k)$, where $k$ %MDPI: please check if k should be unified into italic fomat % confirm
 is the number of reported sets, of the pairing problem and a smart indexing scheme to filter all the redundant pairs in the second stage. As the verification steps remains the same as the above procedure (using $k$ sets instead of $X$), Equation~\eqref{eq2} is also valid for the BIM transformation assessment. Additionally, the same metrics are stored in the RDF graph including the RMSE, the number of inlier correspondences and the bounding box of the inlier correspondences.

\subsubsection{Two-Dimensional Pose Estimation}\label{sec:2D pose}
% is each image session registered seperately?
% Structure-from-Motion Revisited
% Survey and evaluation of monocular visual-inertial SLAM algorithms for augmented reality

% I_s = all session images
% I_r = all reference images
% i_s = session image 
% i_r = reference image
% \boldsymbol{T}_{i_s} = ( \boldsymbol{R}_{i_s} , \boldsymbol{t}_{i_s} ) => camera motion state with the rotation matrix and camera position of image i_s
% x_s = 2D distinct pixel of i_s
% x_r = 2D distinct pixel of i_r
% x_{r,s} = matched 2D point from x_s and x_r 
% \boldsymbol{x}_{r,s} = matched 2D points from x_s and x_r 
% X_{r,s} = matched 3D point from x_s and x_r 
% \boldsymbol{X}_{r,s} = matched 3D points from x_s and x_r 

Given the 2D ORB features of every reference and session image, a transformation matrices can be computed for every image in the session using state-of-the-art SfM methods. The camera pose of a session image $\boldsymbol{T}_{i_s}$ in relation to the global coordinate system is given by the rotation matrix $\boldsymbol{R}_{i_s}$ and the camera position of image $\boldsymbol{t}_{i_s}$. The relation between the matched 2D pixels $ x \in \boldsymbol{x}$ of an image $i_s$ and their 3D projections $ X\in \boldsymbol{X}$ can then be defined as follows (Equation~\eqref{eq2}).
\begin{equation}
\begin{split}
\label{eq3}
% define 3D-2D projection relation of a single image over all correspondences
& x= \pi(\boldsymbol{T}_{i_s},X)= \boldsymbol{K} \begin{bmatrix} \boldsymbol{R}_{i_s}^T & -\boldsymbol{R}_{i_s}^T \boldsymbol{t}_{i_s} \\0 & 1 \end{bmatrix} X
\end{split}
\end{equation}
where both $X$ and $x$ are represented by their homogeneous coordinates. $\boldsymbol{K}$ is the camera intrinsic parameters matrix. To estimate the camera poses of all $i_s\in I_s$, the following energy function can be minimized through bundle adjustment~\cite{Jinyu2019} (Equation~\eqref{eq4}).
\begin{equation}
\begin{split}
\label{eq4}
% retrieve T_s by optimizing over correspondences and cameras
& \argmin\limits_{\boldsymbol{T}_{i_1}...\boldsymbol{T}_{i_s}} \sum_{I_s,I_r}\sum_{\boldsymbol{X}} \rho \left(\|\pi(\boldsymbol{T_{i_s}},\boldsymbol{X})-\boldsymbol{x}\|^2\right)
\end{split}
\end{equation}
where $\boldsymbol{X}$ and $\boldsymbol{x}$ are the combined matches for the session $s$. A similar loss function $\rho$ as in Equation~\eqref{eq2} is defined to down-weigh potential outliers. To minimize Equation~\eqref{eq4}, a number of methods can be employed. In this work, we use the OpenCV Levenberg--Marquardt implementation~\cite{Schonberger2016}. It is important to notice that the resulting $\boldsymbol{T}_{i_s}$ is not yet scaled. To solve the scale, we identify three cases to estimate the scale that will occur in XR data capture. %MDPI: please check if colon should be full stop % confirm


%\input{Tables/fig_method_4}


\paragraph{Two Overlapping References} If a session image $i_s$ can be matched with at least two overlapping reference images $I'_r=\{i_{r_1}, i_{r_2}\}$, the global pose of the session image $\boldsymbol{T}_{i_{s,g}}$ is retrieved by solving Equation~\eqref{eq1} using at least 6 3D--2D point correspondences. These correspondences $\boldsymbol{X}$ are already determined in the global coordinate system due to the global pose of $i_{r_1}$ and $i_{r_2}$ (Figure~\ref{fig:method_4}a). 

\paragraph{Two Separate References}
In the case that two non-overlapping reference images $I_s=\{i_{r_1},i_{r_2}\}$ can be matched to the session image $i_{s}$, the global pose of the session image $\boldsymbol{T}_{i_{s,g}}$ is retrieved by evaluating the relative transformations between $i_{s}$ and $i_{r_1}$ and $i_{r_2}$, respectively (Figure~\ref{fig:method_4}b). To this end, the average pose is taken considering the accuracy of the absolute poses $\boldsymbol{T}_{i_{r_1,g}}$ and $\boldsymbol{T}_{i_{r_2,g}}$.


\begin{figure}[H]
  \begin{subfigure}[]{0.44\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/incremental matching.png}
        \caption{ }
        % \label{fig}
    \end{subfigure} 
    % \\
  \begin{subfigure}[]{0.36\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/Cross reference.png}
        \caption{ }
        % \label{fig}
    \end{subfigure}
      \\[1.9ex]

  \begin{subfigure}[]{0.34\textwidth}
    \centering
   \hspace{24mm}      \includegraphics[width=1\textwidth]{Figures/raycasting.png}
        \caption{}
        % \label{fig}
    \end{subfigure}
%   \begin{subfigure}[]{0.49\textwidth}
%    \centering
%        \includegraphics[width=1\textwidth]{Figures/raycasting.png}
%        \caption{Geometry reference: Cuboid images are generated at virtual %setups in the geometry. The pose estimation is then similar to the %situation c with one reference image with geometry.}\vspace{15pt}
%        % \label{fig}
%    \end{subfigure}
  \caption{(\textbf{a}) Two %MDPI: we moved subfigure caption here, please confirm % confirm
 overlapping references: $\boldsymbol{T_{i_s}}$ is retrieved from a direct pose estimation of the reconstructed $\boldsymbol{X}$ matches between $i_{r_1}$ and $i_{r_2}$. (\textbf{b}) Two separate references: $\boldsymbol{T_{i_s}}$ is retrieved by triangulating its pose from the individual matches between $i_{s}$ and $i_{r_1}$ and $i_{r_2}$, respectively. (\textbf{c}) One reference image with geometry: $\boldsymbol{X}$ is scaled based on the raycast-distance $\boldsymbol{L}$ to $\boldsymbol{x}$ on the geometry. Overview of scale estimation for different XR image alignment possibilities: (\textbf{a}) multiple reference matches with a single session image, (\textbf{b}) single reference match with multiple session images, and (\textbf{c}) single reference match with a single session image but depth information is present.}
  \label{fig:method_4}
\end{figure}

% take intersection of the unscaled directions of the transformations (which are represented by lines originating from the camera poses)


% \begin{equation}
% \begin{split}
% \label{eq5}
% % estimate the scale or Two references, one XR image
% & \nu = \frac{|\boldsymbol{T}_{i_{r_2,g}}-\boldsymbol{T}_{i_{r_1,g}}|}{|\boldsymbol{T}_{i_{r_2}}-\boldsymbol{T}_{i_{r_1}}|} \\
% & \boldsymbol{T}_{i_{s,g}} = - \boldsymbol{T}_{i_{r_1}} \boldsymbol{T}_{i_{r_1,g}} \nu \boldsymbol{T}_{i_{s}}
% \end{split}
% \end{equation}

% \paragraph{Cross alignment}
% If we can find 2 separate matches between a test image and the reference image set, we can estimate the final pose by finding the intersection between the 2 direction, starting from the reference positions. This method becomes more reliable when the angles of the matched images becomes large enough, because this creates a smaller overlap and e much more clear intersection point.

% The images are matched using state of the art computer vision software packages, this ensures fast and reliable matches are found. Since the images of the reference data are assumed to be localised, we can use that position to estimate the poses of the test images.


\paragraph{One Reference Image with Geometry} In the case that only a single reference image $i_{r}$ can be matched to the session image $i_{s}$, but their point cloud present in the session or the reference, the global pose of the session image $\boldsymbol{T}_{i_{s,g}}$ is retrieved by ray tracing $\boldsymbol{x}$ (Figure~\ref{fig:method_4}c). To this end, a set of rays $l(c,x) \in L$ is constructed from the focal point $i_{s}(c)$ or $i_{r}(c)$ through $\boldsymbol{x}$ depending on whether the geometry is part of the session or the reference. The intersection between the geometry $P$ and $L$ than yields the 3D coordinates of the 3D correspondences $\boldsymbol{X}$ (Equation~\eqref{eq5}).
\begin{equation}
\begin{split}
\label{eq5}
% Determine X by computing the intersection between L and P 
& \boldsymbol{X}=\left\{ p \in P \Big|l(c,x) \in L: p= l(c,x) \cap P\right\}  \\
\end{split}
\end{equation}
% voxel traversal 
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\subsubsection{Final Pose Estimation}
% Averaging quaternions => math
% per testafbeelding in de sessie wordt de beste match (of meerdere beste matches afh. een treshhold op basis van de overlap (fitness) en RMSE van die matching) => hier komen hele hoop parameters bij kijken :
%  1. reprojectiefout (rmse)
%  2. aantal matches
%  3. fitness ( inlier percentage)
%  4. welke afbeeldingen en welke sessies werden gebruikt met initiele nauwkeurigheid
%  5. bounding box van de overlapping features (inliers)
%  6. type methode
%  => dit geeft een betrouwbaarheidsfactor (confidence 0-1)
%  pose wordt dan berekend obv gewogen gemiddelde met confidence => voer opnieuw een rho in voor outlierremoval

Given the above pose candidates per session and per resource, the final transform $\boldsymbol{T}_{s}$ is computed based on a weighted pose vote over all point cloud and image transformations $\boldsymbol{T}=\{\boldsymbol{T_{P}}, \boldsymbol{T}_{I}\} $ in a session (Equation~\eqref{eq6}). 
\begin{equation}
\begin{split}
\label{eq6}
% Determine the transform of the session by a pose vote
& \boldsymbol{T}_{P}=\frac{1}{n} \sum_{\boldsymbol{T}} \omega \left(\boldsymbol{T_{P}}, \boldsymbol{T}_{I}\right)   \\
\end{split}
\end{equation}
where the weight $\omega$ of each resource is computed for image transformations based on the reprojection error, the number of matches, inlier percentage, and bounding box of the inliers. For point cloud transformations, $\omega$ is established based on RMSE on the matches, the number of matches, the bounding box of the matches. For these transformations, the theoretical sensor accuracy is also taken into account. Every parameter is normalized to ensure larger numbers do not disproportionately affect the final weight. Every type of registration also contributes to the weight as follows based on empirical and theoretical evidence: Super4PCS alignment (1), SfM with two overlapping references (0.8), FPFH features with a measured point cloud(0.8), two separate reference images (0.5) and one reference image but with geometry (0.4).



%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Test Data}\label{sec:Test Data}

%\input{Tables/Dataset_overview}
Three periodic test cases were captured and processed of both operational facilities and buildings under construction (Table~\ref{tab:dataset overview}). In total, 45 sessions were documented using static TLS with a Leica P30 and Leica BLK, the indoor mobile mapping system NavVis VLX, a CANON EOS 5D MARK II, the Microsoft Hololens 2, and conventional smartphones. The raw images and point cloud data were manually processed to serve as a baseline for the analysis of the two-step alignment. The camera poses of the images were retrieved by using the Reality Capture SfM pipeline and the optimized poses of the iMMs and TLS sensors. The Android smartphone and Hololens 2 datasets were then manually registered to these inputs and used as the baseline for the comparison. Specifically for the Hololens 2, the 3D meshes were captured with an average density of 0.08 m$^3$. The data were captured with a custom-made application created with the Unity3D game engine and send to the local web server. Some sessions were taken with an android smartphone using the same application, but due to hardware limitations, these sessions only contain images. The rough global pose was captured using regular android phones and the data were sent to the same server to store for the fine pose estimation.

The first test case is the soil technology lab on our Campus in Ghent (Table~\ref{tab:dataset overview} row 1). It is a laboratory space that resembles an industrial site and houses working desks, machinery, bulk materials, and so on. Between the periodic data captures taken in several months, the lab was operational and, thus, all movable objects were displaced in the documented period. As such, this test cases focuses on the ability of the fine-alignment to register multi-temporal inputs of an operational environment. With a size of 10 m $\times$ 30 m, %MDPI: we adjusted it from 10 × 30 m into 10 m × 30 m, please confirm if the meaning has been retained. %confirm
 it is also circa the size the Hololens 2 can capture conform LOA20 [2$\sigma~\leq$ 0.05 m] without the inclusion of the control points. For this test case, a basic BIM is available of the structure. 


The second test case is the construction of a prefab living lab on the Technology Campus (Table~\ref{tab:dataset overview} row 2). This project was periodically captured with the different sensors starting from the early stake-out all the way to the MEP installations. The structure itself is a three-storey building that resembles an office/housing space and was constructed from prefab structure elements. As such, this test case focuses on the robustness of the algorithm to match the data of different sensors in an outdoor construction site environment that is drastically changing in both texture and geometry. For this test case, a detailed as-designed BIM is available of the structure, architectural finishes, and MEP installations. The full construction was documented using both a Sony a6400 camera, used for photogrammetric reconstruction, and the Hololens 2, for subsections of the building.

\begin{table}[H]
    \caption{Overview of the provided reference data of test cases 1 and 2.}
    \label{tab:dataset overview}
    \resizebox{1.003\textwidth}{!}{%
    \renewcommand{\arraystretch}{1.2}
        \begin{tabular}{r   r       c       c       c       c       c       c}
        \toprule
             &      & \textbf{\# Sessions} & \textbf{ \# Images}   & \textbf{\# 3D Objects} & \textbf{\# Points} & \textbf{\# Downsampled Points} & \textbf{Typical Image Resolution}\\
        \midrule
            \multirow{3.1}{*}{\rotatebox{90}{1 Campus} }
            & BIM   & 3           & /            & 186             & 3,980         & 82,050 %MDPI: please check if comma should be added % confirm
                     & /         \\
            & VLX   & 3           & 241          & 3             & 162,108,105    & 2,481,633               & 2048 $\times$ 1042         \\
            & Hololens& 6           & 35            & 483             & 1,089,574         & 16,679         & 3904 $\times$ 2196         \\
     \midrule
            & \multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/Campus Dataset.png}} \\
            & \multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/Campus Image Dataset.jpg}} \\
               \midrule
             &      & \# sessions %MDPI: please check if this row can be removed % we feel the images cause too much seperation
 &  \# images   & \# 3D objects & \# points & \# downsampled points & Typical image resolution\\
     \midrule
            %
            \multirow{4.2}{*}{\rotatebox{90}{2 Living Lab} }
            & BIM  & 1           & /            & 368             & 41,825         & 41,351                     & /         \\
            & P30  & 1           & 274            & 2             & 7,638,931         & 565,938                     & 640 $\times$ 640         \\
            & Photogrammetry & 3    & 642            & 3             & 1,013,736         & 75,103           & 4240 $\times$ 2832         \\
            & Hololens  & 5           & 30            & 15             & 439,816         & 32,584                 & 3904 $\times$ 2196         \\
     \midrule
            & \multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/LivingLab Dataset.png}} \\
            & \multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/LivingLab Image Dataset.jpg}} \\
               \bottomrule
        \end{tabular}
     }
\end{table}
\vspace{-6pt}
\begin{table}[H]
    \caption{Overview %MDPI: This Table is not mentioned in the main text, please modify
 of the provided reference data of test case 3.}
    \label{tab:dataset overview2}
    \resizebox{1.003\textwidth}{!}{
    \renewcommand{\arraystretch}{1.2}
        \begin{tabular}{r   r       c       c       c       c       c       c}
        \toprule
             &      & \textbf{\# Sessions} &  \textbf{\# Images}   & \textbf{\# 3D Objects} & \textbf{\# Points} & \textbf{\# Downsampled Points} & \textbf{Typical Image Resolution}\\
         \midrule
            %
            \multirow{5}{*}{\rotatebox{90}{3 House} }
            & BIM   & 1           & /            & 594             & 24,783         & 84,031                     & /         \\
            & P30   & 1           & /            & 20             & 264,525,819         & 19,597,665                     &  /        \\
            & Photogrammetry   & 1           & 430            & 0             & /         & /                     & 5616 $\times$ 3744         \\
            & Hololens  & 20           & 91            & 18             & 378,075         & 28,010                     & 3904 $\times$ 2196         \\

            & \multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/House Dataset.png}} \\
            & \multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/House Image Dataset.jpg}} \\
            \bottomrule
        \end{tabular}
     }
\end{table}

%&\includegraphics[width=0.4\textwidth]{Figures/y1} & \includegraphics[width=0.4\textwidth]{Figures/x1}






The last test case is a renovation of a house in Brugge (Table~\ref{tab:dataset overview2}). The refurbishment was periodically captured both by smartphones and the Hololens 2 and has Leica BLK data as a baseline. The structure consists of a four-storey building including a full-storey basement and attic. This test case focuses on the robustness of the algorithm to match data of different sensors on an indoor construction site environment that is drastically changing in texture and where there is a lot of clutter and temporary storage of materials. For this test case, a detailed as-built BIM is available of the structure and architectural finishes.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Experiments}\label{sec:Experimental results}


In the following section, the different pose estimation methods and the final parameters to determine the best overall pose are evaluated. The accuracy is based on the distance error (m) and angle error (deg), which indicates the distance and angle difference between the estimated transformation and the correct pose, respectively.  Additionally, as stated before in Section \ref{sec:pose estimation}, the pose is determined based on a weighted pose vote of five methods (\mbox{two Lidar} and three image-based). Each method has distinct advantages and disadvantages in specific use cases, and so there are a number of parameters to evaluate which method works best in which case. Each method is evaluated using the same parameters based on the best fit parameters that were empirically determined over all measurements. The global distance threshold $t_d = 10$ m was set based on a relevant ground sampling distance for smartphones (avg. 12 MP), mirrorless cameras (avg. 24 MP), the Hololens 2 (12 MP) and the TLS and iMMs image (5 MP) and Lidar resolution (avg. 10 MP). The feature correspondence functions $\rho$ for the image and Lidar matches was set, respectively, to 50 pixels and 1.5 times the voxel size (0.05 m) to mitigate the noise on the changed environments. The weights for each method $\omega$ were distributed based on the number of successful test cases of each method: FPFH features with a measured point cloud (0.8), SfM with two overlapping references (0.8), one reference image but with geometry (0.4), two separate reference images (0.5), and SUPER4PCS alignment (1).

The order in which the experiments are presented is the following. First, the performance of each method is presented based on good and bad performances of the methods on test case 2 as it is the most varied dataset with significant matching challenges. The quality and parameters of each method are discussed in detail to conclude where each method will fail and succeed (Figure~\ref{fig:Confidence parmeters}). Second, the general pose voting results are discussed over all three test cases given the confidence levels of each method. Based on the test results, it is determined in which type of scenarios XR-devices will be able to align with preexisting datasets and which sensor data are preferred to achieve the highest quality alignment in construction site environments.

\begin{figure}[H]
    \includegraphics[width=\textwidth]{Figures/Confidence Calculations.png}
    \caption{The weighted values of each parameter per method to calculate to confidence.}
    \label{fig:Confidence parmeters}
\end{figure}

\subsection{Two-Dimensional Alignment}
The 2D alignment methods are based on the OpenCV ORB feature matching. The accuracy of the alignment is, therefore, reliant on the quality of each match. Since only one or two matches are required for the alignment, only the best matches are retained for the next step. The quality of each match is directly evaluated by comparing the reprojection error, inlier percentage and overlap of the proposed transformation between the two images. Table \ref{tab:2D results} shows clear examples of a correct and incorrect match between two session images. The incorrect match easily stands out due to its 5\% inliers and limited overlap. These parameters are, thus, excellently suited to determine the matching quality with varying combinations of reference and test data.

%\input{Tables/results_image_matching}
\begin{table}[H]
\caption{Examples of a correct and incorrect image match.}
\label{tab:2D results}
\resizebox{1.003\textwidth}{!}{%
\renewcommand*{\arraystretch}{1.2}
\begin{tabular}{lcccccc}
\toprule
\multicolumn{7}{c}{\includegraphics[width=0.9\textwidth]{Figures/experimental results/Good Image Inliers.png}} \\
\midrule
\multirow{2}{*}{Sensor}    & reprojection  & Overlap   & Inliers   & Time          & Match         & Match         \\
            & error (pix)   & (\%)      & (\%)      & Passed (days) & Distance (m)  & Angle (deg)   \\
\midrule
Hololens    & 30.4          & 60.0       & 53.7     & 3             & 3.41          & 11.28\\
\midrule
\multicolumn{7}{c}{\includegraphics[width=0.9\textwidth]{Figures/experimental results/Bad Image Inliers.png}} \\
\midrule
\multirow{2}{*}{Sensor}      & reprojection  & Overlap   & Inliers   & Time          & Match         & Match         \\
            & error (pix)   & (\%)      & (\%)      & Passed (days) & Distance (m)  & Angle (deg)   \\
\midrule
Hololens    & 38.6          & 5.7       & 5.0       & 3             & 8.01          & 71.80\\
\bottomrule
\end{tabular}
}
\end{table}

%\begin{figure}[t!]
%	 \begin{subfigure}[]{0.99\textwidth}
%    \centering
%        \includegraphics[width=1\textwidth]{Figures/experimental results/Good Image Inliers.png}
%        \caption{Correct image match between consecutive Hololens 2 sessions. Avg. reprojection error is 30.4 pix, inliers 54\% and %overlap 50\%.}\vspace{5pt}
%        % \label{fig}
%    \end{subfigure}
%    % \\
%  \begin{subfigure}[]{0.99\textwidth}
%    \centering
%        \includegraphics[width=0.99\textwidth]{Figures/experimental results/Bad Image Inliers.png}
%        \caption{Incorrect image match between consecutive Hololens 2 sessions. Avg. reprojection error is 38.6 pix, inliers 5\% and %overlap 10\%.}%\vspace{15pt}
%        % \label{fig}
%    \end{subfigure}
%      \\
%	\caption{Examples of the feature matches quality parameter values for the pose voting.}
%	\label{fig:2D_results}
%\end{figure}


\subsubsection{Two Overlapping References}
The first method is used when there are sufficient overlapping images between the reference and test session. The results can be found in Table \ref{tab:Incremental} where it is clear that this method works best for reference sessions with a large amount of images with high overlap. This is found mostly with photogrammetry reconstructions, since this is essentially the same method being used. 

\begin{table}[H]
\caption{Two-dimensional matching estimations based on 2 linked reference images.}
\label{tab:Incremental}
\resizebox{1.003\textwidth}{!}{%
\renewcommand*{\arraystretch}{1.2}
\begin{tabular}{lcccccccc}
\toprule
\multicolumn{9}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/overlapping good match.png}} \\
\midrule
\multirow{2}{*}{Sensor}  & Distance  & rotation  & Reprojection  & Overlap & Inliers   & Time       & Match     & Match\\
        & Error (m)  &Error(deg) & Error (pix)  & (\%)      &   (\%)    & Passed (days)  & Distance (m) &Angle (deg)\\
\midrule
Sony APSC& 0.10      & 0.46      & 28.10         & 49.5       & 50.0       & 1      & 2.09        & 7.85\\
\midrule
\multicolumn{9}{c}{\includegraphics[width=\textwidth]{Figures/Incremental Good Match.png}} \\
\midrule
\multirow{2}{*}{Sensor}  & Distance  & rotation  & Reprojection  & Overlap & Inliers   & Time       & Match     & Match\\
        & Error (m)  &Error(deg) & Error (pix)  & (\%)      &   (\%)    & Passed (days)  & Distance (m) &Angle (deg)\\
\midrule
Hololens& 0.39      & 3.35      & 28.77         & 31.7       & 34.3       & 1      & 4.50        & 12.6\\
\bottomrule
\end{tabular}
}
\end{table}

Since the session scaling comes from the relative distance between the two reference images, larger distances generally yield more exact scale estimations. However, as the relative distance increases, the overlap of the image generally also decreases, resulting in a worse match. For instance, the matching of the top image in Table \ref{tab:Incremental} yields a distance error of 0.1 m and is based on two highly reliable image matches on two images taken 2 m apart on consecutive days. The bottom image has matched to two images taken 4.5 m apart which would theoretically increase the accuracy. However, the poor image matching (only 34\% inliers and 20\% overlap) actually leads to an inferior pose estimation. As such, good image matches are prioritized over larger baselines for the final pose estimation.


As expected, this method under-performs in sparse image datasets, where reference session matches are both rare and of poor quality. This is the case when significant texture changes have taken place on the construction site or facility, e.g., plastering or painting of the interior. Additionally, in case a good match is found between a test image and a reference image, but the reference image does not have a good other match, the method will under-perform. It is, therefore, essential that the bad reference match has enough weight in the pose estimation to ensure it does not obtain a high confidence, e.g., by only retaining the parameters of the worst match of the pair.
%\input{Tables/results_incremental_matching}


\subsubsection{Two Separate References}
The second method is used when two separate matches are found between session and reference images. Both matches are then cross referenced to calculate the final pose of the image. The results can be found in Table \ref{tab:least distance} where it is clear this method works best for reference sessions where sporadic images are taken in a large area of the site. This case happens mostly with mobile mapping systems, such as XR datasets or the VLX datasets, that only store imagery at key locations that do not necessarily have overlap between them.

For the cross referencing of the pose estimation, an important factor is the relative angle between the two estimated positions to ensure a high confidence. When the direction of the matches are more perpendicular than parallel, small deviations in the directions become less pronounced and, thus, increase the accuracy of the resulting intersection. Similarly, near parallel matches have a larger depth error. The relative distance also negatively impacts the result as larger distances increase the deviation of small directional errors. This effect is demonstrated in Table \ref{tab:least distance} where the top match yields a descent pose estimation despite the average matching statistics due to the high rotational angle between both reference images (64.38$^\circ$). Instead, the better matching bottom image only has an angle of 7.85$^\circ$ between both references, causing a significant distance error. 

This method will also under-perform when the reference images are positioned too close to the session image as this drastically amplifies the rotational error. As such, the best match for this method is based on the matching angle of both references and the intermediate Euclidean distance between them. 

%\input{Tables/results_leastDistance_matching}
\begin{table}[H]
\caption{Two-dimensional matching estimations based on 2 separate reference images.}
\label{tab:least distance}
\resizebox{1.003\textwidth}{!}{%
\renewcommand*{\arraystretch}{1.2}
\begin{tabular}{lcccccccc}
\toprule
\multicolumn{9}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/Good Least distance match.png}} \\
\midrule
\multirow{2}{*}{Sensor}  & Distance  & rotation  & Reprojection  & Overlap & Inliers   & Time       & Match     & Match\\
        & Error (m)  &Error(deg) & Error (pix)  &  (\%)         &   (\%)    & Passed (days)  & Distance (m) &Angle (deg)\\
\midrule
Sony APSC& 0.15      & 0.92      & 27.66         & 57.6       & 21.4       & 1      & 12.81        & 64.38\\
\midrule
\multicolumn{9}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/Bad Least distance match.png}} \\
\midrule
\multirow{2}{*}{Sensor}  & Distance  & rotation  & Reprojection  & Overlap & Inliers   & Time       & Match     & Match\\
        & Error (m)  &Error(deg) & Error (pix)  & (\%)      &   (\%)    & Passed (days)  & Distance (m) &Angle (deg)\\
\midrule
Sony APSC& 2.20      & 0.69      & 33.06         & 17.2       & 28.9       & 1      & 2.09        & 7.85\\
\bottomrule
\end{tabular}
}
\end{table}




\subsubsection{One Reference Image and 3D Data}
The third method is used if only one good match can be found in the reference images and there are 3D data available, either in the test or reference session. In this case, the correspondences of the matching images are raytraced on the present geometries to determine the scale. The results can be found in Table \ref{tab:raycasting}. Since this method requires 3D data to be available, it rules out some of the more basic datasets that lack 3D data. However, when such a dataset is available and if the 3D data have enough coverage of the area from the camera’s point of view, this method shows promising results. This methods preforms well on all test cases that contain 3D datasets and the minimal required images can be very low since only one images needs to be matched. 

There are three important factors that influence the pose estimation: (1) A sufficient distribution of depth information of the raycast image matches is necessary to establish the correct scale of the pose. (2) Any artefacts in the 3D data such as ghosting or noise on windows can obstruct the raycasting, resulting in an erroneous scale. RANSAC filtering is therefore mandatory for retrieving the correct distances. (3) The accuracy of the 3D data itself directly effects the accuracy of the pose estimation. For instance, the Hololens 2 has a limited depth accuracy compared to high-end TLS or iMMs which translates to a reduced pose accuracy. It should also be noted that since point clouds lack a surface definition, the resulting voxel raytracing algorithm \cite{Amanatides1987} will result in less precise results compared to meshes, which do have a surface definition. Overall, the top image shows a high distance (0.02 m) and rotation accuracy (0.01°) can be obtained when matching with combined high-end TLS and Hololens 2 data.

%\input{Tables/results_raycasting_matching}
\begin{table}[H]
\caption{Two-dimensional matching estimations based on 1 reference image and a Mesh.}
\label{tab:raycasting}
\resizebox{1.003\textwidth}{!}{%
\renewcommand*{\arraystretch}{1.2}
\begin{tabular}{lcccccccc}
\toprule
\multicolumn{9}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/Good raycasting match.png}} \\
\midrule
\multirow{2}{*}{Sensor}  & Distance  & Rotation  & Reprojection  & Overlap & Inliers   & Time       & Match     & Match\\
        & Error (m)  &Error(deg) & Error (pix)     & (\%)          &   (\%)    & Passed (days)  & Distance (m) &Angle (deg)\\
\midrule
Sony APSC& 0.02      & 0.01      & 23.8         & 36.8       & 52.2       & 2      & 2.22        & 15.22\\
\midrule
\multicolumn{9}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/Bad raycasting match.png}} \\
\midrule
\multirow{2}{*}{Sensor}  & Distance  & rotation  & Reprojection  & Overlap & Inliers   & Time       & Match     & Match\\
        & Error (m)  &Error(deg) & Error (pix)  & (\%)          &   (\%)    & Passed (days)  & Distance (m) &Angle (deg)\\
\midrule
Sony APSC& 1.88      & 3.45      & 36.2         & 31.7       & 10.9       & 4      & 4.54        & 18.89\\
\bottomrule
\end{tabular}
}
\end{table}

This method fails when the available 3D object either has to little coverage from the camera's point of view, since there will be insufficient data points to compare, or the dataset contains to much noise, resulting in several scale factors that cannot be reliable filtered by RANSAC. This is the case with the bottom image in Table \ref{tab:raycasting} where the image matches lead to an erroneous raycasting in occluded areas. 


\subsection{Three-Dimensional Alignment}
The 3D matching methods are based on two different feature matching frameworks. Open3d uses FPFH features while Super4PCS is a more robust matching algorithm that requires less matches. In contract to the image matches, a single alignment between a reference and a test session dataset is sufficient to position a session. The quality of each match is directly evaluated by comparing the percentage of feature inliers, a measure for the distribution of the overlap and the RMSE of the matches. 

\subsubsection{FPFH Feature Matching}
FPFH feature matching is specifically designed to estimate the transformation between two observed point clouds as it determines features of all points in the cloud. This is why all 3D data are both converted to point clouds and subsampled to improve the speed of the algorithm. As seen in Table \ref{tab:fpfh}, this method works best for point clouds with limited geometric changes over time, e.g., after the structure phase. As long as a large portion of the point clouds remains the same, the algorithm is able to correctly determine the correct pose. For instance, the top image in Table \ref{tab:fpfh} shows a good match between the finished ground works and the placement of the foundations since the majority of the excavation pit was unaltered. However, after the structure was completed, the bottom image in Table  \ref{tab:fpfh} shows an incorrect match even though the surroundings of the structure are still the same. 

An important factor for the success of FPFH or other point-based features is the presence of geometric detailing in the scene. The excavation of a construction site offers a large number of unique points of which the gradients results in a distinct and reliable feature. The method will, thus, underperform in scenes where only flat non-distinct geometries are found. Additionally, small amounts of overlap or ill-distributed feature matches will result in incorrect alignments as evidenced in the bottom figure of Table \ref{tab:fpfh}.
%\input{Tables/results_fpfh_matching}

\begin{table}[H]
\caption{Three-dimensional matching estimations based on FPFH feature matching.}
\label{tab:fpfh}
\resizebox{1.003\textwidth}{!}{%
\renewcommand*{\arraystretch}{1.2}
\begin{tabular}{lccC{2cm} C{1.7cm} C{1.7cm} C{1.7cm}}
\toprule
\multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/good fpfh.png}} \\
\midrule
\multirow{2}{*}[-0.6em]{Sensor}  & Distance  & Rotation  & RMSE  & Overlap & Inliers   & Time       \\
        & Error (m)  &Error(deg) & Error (m)         & (\%)          &   (\%)    & Passed (days)  \\
\midrule
P30 & 0.04      & 0.02      & 0.03         & 95.0       & 100.0       & 3      \\
\midrule
\multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/bad fpfh.png}} \\
\midrule
\multirow{2}{*}[-0.6em]{Sensor}  & Distance  & Rotation  & RMSE  & Overlap & Inliers   & Time       \\
        & Error (m)  &Error(deg) & Error (m)         &  (\%)         &   (\%)    & Passed (days)  \\
\midrule
BIM& 3.56      & 90.45      & 0.70         & 20.0       & 15.0       & 31      \\
\bottomrule
\end{tabular}
}
\end{table}

\subsubsection{SUPER4PCS Feature Matching}
When certain 3D datasets lack geometric detailing, such as a BIM model, a more robust method is required to match the different datasets. SUPER4PCS is specifically designed to overcome this lack of detailing by evaluating the geometric ratios between feature points. As seen in Table \ref{tab:4pcs}, this method works best for database matching, such as with the as-designed BIM model and only requires a small subset of well documented planar objects to retrieve the correct alignment. For instance, the top figure in Table \ref{tab:4pcs} shows only a 0.09 m error between a Hololens 2 dataset and the BIM model despite that only of small portion of the front of the structure (15\% overlap) was captured in an area filled with noise and ghosting from the main window on the ground floor. 

\textls[-15]{In contrast to the FPFH matching, the lack of geometric primitives in the scene can hinder the alignment. For instance, the bottom figure in Table \ref{tab:4pcs} shows an incorrect match between two  Hololens 2 data captures 5 days apart with mostly geometric details and not so much of the structure being documented. As such, FPFH and SUPER4PCS are complementary techniques that, if used in parallel, will lead to a more robust pose estimation framework. }
%\input{Tables/results_super_4PCS_matching}
\begin{table}[H]
\caption{Three-dimensional matching estimations based on Super 4 PCS matching.}
\label{tab:4pcs}
\resizebox{1.003\textwidth}{!}{%
\renewcommand*{\arraystretch}{1.2}
\begin{tabular}{lcccccc}
\toprule
\multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/good 4pcs.png}} \\
\midrule
\multirow{2}{*}{Sensor}  & Distance  & Rotation  & LCP score  & Overlap & Inliers   & Time       \\
        & Error (m)  &Error(deg) & (\%)         & (\%)          &   (\%)    & Passed (days)  \\
\midrule
BIM& 0.09      & 3.35      & 70.4         & 15.0       & 60.0       & 31      \\
\midrule
\multicolumn{7}{c}{\includegraphics[width=\textwidth]{Figures/experimental results/bad 4pcs.png}} \\
\midrule
\multirow{2}{*}{Sensor}  & Distance  & Rotation  & LCP score  & Overlap & Inliers   & Time       \\
        & Error (m)  &Error(deg) & (\%)        & (\%)          &   (\%)    & Passed (days)  \\
\midrule
Hololens& 4.56      & 90.50      & 3.2         & 30.0       & 3.0       & 5      \\
\bottomrule
\end{tabular}
}
\end{table}
\subsection{Weighted Pose Estimation}
\textls[-15]{Each method returns an estimated pose with various parameters. Table \ref{tab:results-all} shows the accuracy of the pose estimation of each method in relation to its calculated confidence based on the chosen parameter modifiers. The exact values of these modifiers were determined based on empirical evidence from the test data and are outlined in Table \ref{fig:Confidence parmeters}. The resulting confidence is the combination of the best estimations per evaluated sessions. These confidence parameters are each multiplied by its empirically determined method weight. The combined result determines the influence of each method on the final pose estimation. The accuracy is similarly computed by determining the weighted average for each axis based on the combined confidence and method weight parameters. As such, the outcome of the method is both a pose and a measure of agreement in confidence, position, and rotational accuracy.   }

The average positioning accuracy across the different test cases is 0.06 m, the rotational accuracy is 0.34$^\circ$ and the confidence is circa 50\%. These are quite promising results given the many different low- and high-end sensors that were used in the large number of sessions throughout the different test cases. This is especially true for the geometric alignment methods that achieved a similar metric accuracy as the Hololens 2 \mbox{(LOA20 [2$\sigma \leq$ 0.05 m])} which was used in the majority of test cases. On average, the point cloud alignment methods yielded the highest alignment confidences (avg. 75\%) compared to the image-based methods (avg. 25\%). However, the image-based methods rely on significantly more matches which increases the robustness of the matching. This is evidenced by the significant differences in alignment confidence of the point cloud methods, of which the single point clouds either converged very accurately or completely failed to align, e.g., method 4 in test case 1 yielded very poor results. 


%\begin{table}[H]
%    \caption{\hl{Positional} and rotational results and the confidence of each method %across the sessions of each test case.}
%    %MDPI:  1. Tables should be prepared in tex table format, not images, please revise. 
%%    2. Is the bold of the data in the table necessary? if so, please add explanation for %the bold in the table.
%%    3. and we suggest removing the unnecessary background colors and verticle line in a %table; if the color need to be retained, please add explanation for the color
%    
%    \includegraphics[width=0.99\textwidth]{Figures/Final Results Graph.png}
%    \label{tab:results-all}
%\end{table}

% Please add the following required packages to your document preamble:
% \usepackage{multirow}
\begin{table}[]
 \caption{Positional and rotational results and the confidence of each method across the sessions of each test case.}
 \resizebox{1.003\textwidth}{!}{%
\renewcommand*{\arraystretch}{1.2}
\begin{tabular}{lr|ccccc|c}
\hline
\multicolumn{2}{r|}{Method}                                         & Linked Ref    & Separate Ref  & Raycasting     & FPFH        & SUPER4PCS    & Combined          \\
\multicolumn{2}{r|}{Estimated time}                                 & 1-10 images/s & 1-10 images/s & 0.2-2 images/s & 5s-2min/pcd & 20s-5min/pcd & 20s-15min/session \\
\multicolumn{2}{r|}{Method weight}                                  & 0.8           & 0.5           & 0.4            & 0.8         & 1            &                   \\ \hline
\multicolumn{1}{c}{\multirow{9}{*}{Case 1}} & Positional RMSE (m)   & 0.145         & 0.189         & 0.021          & 0.159       & 0.061        & 0.065             \\
\multicolumn{1}{c}{}                        & Δ Pos x (m)           & -0.095        & 0.062         & -0.018         & -0.070      & 0.010        & -0.017            \\
\multicolumn{1}{c}{}                        & Δ Pos y (m)           & -0.002        & -0.165        & 0.008          & 0.030       & -0.060       & -0.038            \\
\multicolumn{1}{c}{}                        & Δ Pos z (m)           & 0.110         & 0.068         & 0.008          & 0.140       & 0.008        & 0.050             \\ \cline{2-8} 
\multicolumn{1}{c}{}                        & Rotational RMSE (deg) & 0.960         & 0.636         & 0.189          & 1.507       & 0.463        & 0.538             \\
\multicolumn{1}{c}{}                        & Δ Rot x (deg)         & 0.600         & 0.450         & 0.080          & 0.300       & 0.040        & 0.207             \\
\multicolumn{1}{c}{}                        & Δ Rot y (deg)         & 0.600         & 0.450         & 0.170          & 0.470       & 0.300        & 0.369             \\
\multicolumn{1}{c}{}                        & Δ Rot z (m)           & -0.450        & -0.003        & 0.020          & 1.400       & 0.350        & 0.332             \\ \cline{2-8} 
\multicolumn{1}{c}{}                        & Confidence (\%)       & 30.100        & 29.900        & 52.200         & 33.900      & 77.300       & 46.951            \\ \hline
\multirow{9}{*}{Case 2}                     & Positional RMSE (m)   & 0.183         & 0.133         & 0.277          & 0.059       & 0.071        & 0.035             \\
                                            & Δ Pos x (m)           & -0.143        & 0.043         & 0.037          & 0.050       & 0.003        & 0.007             \\
                                            & Δ Pos y (m)           & -0.084        & -0.095        & 0.266          & 0.004       & 0.070        & 0.032             \\
                                            & Δ Pos z (m)           & 0.078         & -0.082        & 0.069          & 0.032       & -0.010       & 0.013             \\ \cline{2-8} 
                                            & Rotational RMSE (deg) & 0.993         & 1.207         & 0.018          & 0.156       & 0.028        & 0.074             \\
                                            & Δ Rot x (deg)         & -0.887        & 0.706         & 0.006          & 0.020       & 0.001        & -0.055            \\
                                            & Δ Rot y (deg)         & 0.229         & -0.960        & 0.017          & -0.120      & 0.020        & -0.049            \\
                                            & Δ Rot z (deg)         & -0.384        & 0.190         & -0.003         & 0.097       & 0.020        & 0.010             \\ \cline{2-8} 
                                            & Confidence (\%)       & 25.100        & 17.000        & 19.200         & 80.000      & 90.000       & 54.360            \\ \hline
\multirow{9}{*}{Case 3}                     & Positional RMSE (m)   & 0.149         & 0.103         & 0.106          & 0.067       & 0.078        & 0.074             \\
                                            & Δ Pos x (m)           & 0.099         & 0.098         & 0.050          & -0.005      & 0.030        & 0.031             \\
                                            & Δ Pos y (m)           & 0.099         & 0.026         & 0.078          & 0.030       & 0.004        & 0.028             \\
                                            & Δ Pos z (m)           & 0.053         & 0.013         & 0.052          & 0.060       & 0.072        & 0.061             \\ \cline{2-8} 
                                            & Rotational RMSE (deg) & 0.852         & 1.073         & 0.713          & 0.966       & 0.171        & 0.399             \\
                                            & Δ Rot x (deg)         & 0.340         & -0.890        & 0.670          & 0.890       & 0.030        & 0.322             \\
                                            & Δ Rot y (deg)         & 0.179         & 0.600         & -0.050         & 0.430       & 0.100        & 0.234             \\
                                            & Δ Rot z (deg)         & -0.760        & -0.005        & 0.240          & -0.120      & 0.135        & -0.033            \\ \cline{2-8} 
                                            & Confidence (\%)       & 20.200        & 21.900        & 31.400         & 67.000      & 75.000       & 48.077            \\ \hline
\multirow{3}{*}{Average}                    & Positional RMSE (m)   & 0.159         & 0.141         & 0.135          & 0.095       & 0.070        & 0.058             \\
                                            & Rotational RMSE (deg) & 0.935         & 0.972         & 0.307          & 0.886       & 0.221        & 0.337             \\
                                            & Confidence (\%)       & 25.133        & 22.933        & 34.267         & 60.300      & 80.767       & 49.796            \\ \hline
\end{tabular}
}
\end{table}


Overall, the average differences for the pose estimation between the three test cases is minimal. This is due to the fact that each test case contained a significant number of sessions (avg. 15) that were captured by different sensors, at different construction stages and at different time periods. However, it is also because of the weighting of the pose voting method that mitigates a lot of the outliers computed by the different methods. When looking closer to the individual performances of each method across the different test cases, there is significant variance in the performance, especially for the confidence. For instance, method 1 has an avg. $-10\%$ deficit between test case 1 (30\%) and test case 3 (20\%) due to high texture changes in the house renovation of test case 3. Method 2 shows a similar trend as it relies on the same features. Method 3 has the largest variation in confidence with test case 1 (52\%) and test case 2 (19\%) due to the geometric differences in both datasets. The structure in test case 1 remained static and was well-documented with both high-end and low-end sensors, which positively affected the accuracy and confidence of the raycasting. Test case 2 had the most geometric changes due to the prefab building method and its occlusions and noise negatively impacted the performance of method 3. As discussed above, method 4 and 5 align very well or completely fail. However, test 2 yielded noticeably better results across both methods, indicating that outdoor scenery with its larger baselines results in a better performance. Method 5 on average outperformed method 4 by 10\% confidence not considering the outliers of test case 1 due the planar nature of the site's scenery. However, this doesn't necessarily translate to a more accurate pose estimation which is still driven by the initial metric data quality.  

The computational time required for the alignment also differs significantly between the image and point cloud methods. Where the image-based methods achieved a speed of nearly 1 to 10 images per second, the methods that involved geometries on average took 1 to 5 min with the failed alignments taking the most computational effort. However, with each session containing on average 150 m$^3$ point cloud and 20 images, all methods performed nearly equally with the complete alignment on average taking 5 min. Although the complete alignment is too long for a real-time pose estimation, XR-devices do not have to wait for the full alignment procedure to complete. Instead, based on Table \ref{tab:results-all} the pose can be initialized in under 1 minute and then further optimized through background processes as more pose estimations become available. 

%\include{Tables/results_all.tex}
%\input{Tables/results_all.tex}



%\todo{update with final values}
%\begin{table}[h]
%    \caption{Positional and rotational estimations and the confidence of each method per test case}
%    \label{tab:results-all}
%     \resizebox{\textwidth}{!}{%
%    \renewcommand{\arraystretch}{1.2}
%        \begin{tabular}{r c | C{2.7cm} C{2.7cm} C{2.7cm} C{2.7cm} C{2.7cm} C{2.7cm}}
%             & method & overlapping reference &  separate reference & geometry reference & fpfh features & Super 4PCS & Final\\
%             & weights & 0.8 &  0.5 & 0.4 & 0.8 & 1 & \\
%             & Process time & 1-10 images/s & 1-10 images/s & 0.2-2 images/s & 5s-2min/pcd & 20s-5min/pcd & 20s-15min/session \\
%            \hline
%            \multirow{4}{*}{\rotatebox{90}{Case 1} }
%            & Pos RMSE (m)      & 0.145         & 0.189         & 0.02         & 0.159         & 0.061         & 0.065\\
%            & Rot RMSE (deg)      & 0.960         & 0.636         & 0.189         & 1.507         & 0.463         & 0.538\\
%            & Confidence (\%)    & 30.1         & 29.9         &  47        & 33.9         & 77.3         & 47.0\\
%            &               &           &           &           &           &           & \\ 
%            \hline
%            %
%            \multirow{4}{*}{\rotatebox{90}{Case 2} }
%            & Pos RMSE (m)      & 0.183         & 0.133         & 0.277         & 0.059         & 0.071         & 0.035\\
%            & Rot RMSE (deg)      & 0.993         & 1.207         & 0.018         & 0.156          & 0.028         & 0.074\\
%            & Confidence (\%)    & 25.1         & 17.0         & 19.2         & 80.0          & 90.0         & 54.4\\
%            &               &           &           &           &           &           & \\ 
%            \hline
%            %https://www.overleaf.com/project/61a0c6dbaa398157130e2d33
%            \multirow{4}{*}{\rotatebox{90}{Case 3} }
%            & Pos RMSE (m)     & 0.149         & 0.103         & 0.106         & 0.067         & 0.078         & 0.074\\
%            & Rot RMSE (deg)     & 0.853         & 1.073         & 0.713         & 0.996         & 0.171         & 0.399\\
%            & Confidence (\%)   & 20.2        & 21.9         & 31.4         & 67.0         & 75.0         & 48.1\\
%            &               &           &           &           &           &            \\ 
%            \hline
%        \end{tabular}
%     }
%\end{table}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Discussion}\label{sec:Discussion}
In this section, the pros and cons of the pipeline are discussed and compared to the alternative methods presented in the literature. A first aspect to evaluate is the method’s robustness to position itself in building scenes without the use of exact, approximate, or indirect correspondences. The results indicate this method can provide an accurate pose estimation of a given session up to or better than the resolution and accuracy of the sensor without the use of any manually placed landmarks or markers. This rivals the state-of the art methods but is significantly more robust and less costly that landmark-based methods as it relies on the combination of both 3D and 2D data that are prominently present on most sites. Specifically when compared to exact correspondences, it is stated that while exact correspondences offer an exact millimeter accurate alignment at the start of the session, the accuracy and drift of the sensor will ultimately dictate the accuracy of the dataset and, thus, these methods lose a lot of their initial accuracy unless landmarks are placed all throughout the scene which is immensely costly. 

% key takeaways
From the test results it becomes clear that the different methods all have circumstances where they perform better or worse. The calculated confidence factor ranks the best matches and allows for an accurate final pose estimation. Additionally, this gives vital feedback to the user and potential quality control algorithms that can take into consideration the confidence by which the pose was estimated. Apart from the specific method that is being used, the impact of some parameters seem to be consistent throughout the test cases. In contrast to what was expected, the recording date provides little value apart from checking if the data are taken at the same moment or not, since the texture and/or geometry of an environment can change significantly or very little over any period of time. The change in the environment remains very relevant, and, thus, methods relying on both 2D and 3D data are an absolute must-have to achieve the robustness needed for market adoption.

By comparing the different methods, it is revealed that 2D methods are more robust due to the many image sources, resulting in more viable estimations, but they lack the precision of the 3D methods. The 3D methods on the other hand return fewer viable estimations, but the estimations are much more accurate, especially if high-end TLS or iMMs are used as the reference data. Because the method only uses the best matches to make an estimation, it becomes apparent that more test data might not necessarily result in a more accurate estimation, but it does increase the chances of obtaining a correct estimation.

The biggest obstacle in the method is the change of the site over time. The results show that small incremental changes to the environment can be overcome and accurate pose estimations can still be calculated. This implies that this method has higher chances of success with more smaller incremental data recordings rather than fewer big recordings. The proposed method is, therefore, ideally suited to fill in the gaps between consecutive large data captures.  Alternatively, at least a portion of the site should remain unaltered for extended periods of time until another large data capture is conducted on the site. 

\textls[-15]{A key factor in the applicability of this method is the availability of useful reference data without the need to import all the data to the device. Through the use of RDF graphs which contain the metric and non-metric metadata of each session and resource, it becomes possible to geolocate the device during the data capture by sending the data directly to the server and quickly process the captured session. However, the FPFH alignment method currently still requires a downsampled point cloud which slows down the alignment process. }

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Conclusions}\label{sec:Conclusions}

In this paper, a novel framework is presented to position XR devices within the built environment of either existing facilities or construction sites. Concretely, we combine state-of-the art image and Lidar-based registration techniques in an online pose voting algorithm that geolocates the captured session data using preexisting 2D and 3D data repositories of the built environment. The method consists of two consecutive steps. First, a global alignment is established using GNSS positioning to isolate the relevant reference data for the pose estimation. This coarse alignment exploits the preprocessing of the preexisting 2D and 3D data to metric and non-metric metadata in RDF graphs so no large data have to be transferred over the server for the pose estimation. Second, the selected image data are subjected to three image-based alignment methods including conventional SfM, cross-referencing isolated image matches, and raytracing images with the present geometries. Simultaneously, the selected point cloud data are subjected to two Lidar-based alignment methods including FPFH feature matching and SUPER4PCS.  

The method is evaluated on three test cases with a total of 45 captured sessions by different RGB and Lidar sensors including handheld cameras, smartphones, the Hololens 2, a low-end and a high-end TLS, and a high-end iMMs. Each test case has distinct challenges and include an operational lab, a prefab construction site and a complete house renovation. The experiments indicate that relying on both 2D and 3D alignment methods is an absolute must-have for the pose estimation as individual methods are prone to misalignment. Furthermore, by using different data sources, the user is presented with a confidence and pose estimation accuracy measure which is vital to asses downstream processes, such as quality estimations, and so on. 

Overall, this method provides an extension to the state of the art in regards to existing localisation methods. Landmark-based methods solely rely on the location of artificial markers or beacons which are highly labor-intensive to materialize. This method however, relies on any data that is captured of the site which offers great re-usability of existing data and comes at no extra effort. Landmarks can still speed up and improve the registration due to the fact that they provide great and unique tracking features. By adding a small amount of strategically placed markers, our method will benefit greatly while simultaneously lowering the effort of materializing markers in the entire facility.

Some key takeaways from the experiments are that the time period between data captures is not the key bottleneck but rather the degree of change. As such, any data remain relevant as long as a portion of the scene is unaltered. The experiments also show that sensor resolution is an important metric for the final accuracy of the estimation. By making this method sensor agnostic, not only will this method keep performing with different sensors, it will likely improve over time as sensor capabilities improve as well.

This method leaves room for improvement, in more granular voting with more specific parameters. This method also works best when there is a large amount of 2D and 3D data available. This is, however, not always the case. Future work can look into using virtual imagery created from digital BIM or point cloud models. This will ensure all the available estimation methods can be used. Currently, this method is heavily reliant on large existing datasets, with multiple gigabytes of data. Analysing all these resources requires a significant amount of computing power and time. This is mostly avoided by pre-processing the data and only storing relevant information in the RDF graph. This can sill be improved further as certain methods, namely the FPFH method still relies on the subsampled point cloud or mesh. The used matching algorithm tries to match every existing point. Future work could could include new 3D matching methods, where similar to existing 2D matching methods, only certain feature points are used to obtain an estimation which would significantly lower the computation time and storage cost.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\authorcontributions{J.V., M.B., and M.V. contributed equally to the work. All authors have read and agreed to the published version of the manuscript.}
 %MDPI: newly added information, please confirm.
% Please check if the individual contribution of each co-author has been stated correctly.
%confirm


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\funding{This project has received funding from the FWO Postdoc grant (grant agreement: 1251522N) and the Geomatics research group of the Department of Civil Engineering, TC Construction at the KU Leuven in Belgium.}
%MDPI: Information regarding the funder and the funding number should be provided. Please check the accuracy of funding data and any other information carefully.



%\institutionalreview{\hl{                     } %MDPI: please complete this part.
%In this section, please add the Institutional Review Board Statement and approval number for studies involving humans or animals. Please note that the Editorial Office might ask you for further information. Please add “The study was conducted according to the guidelines of the Declaration of Helsinki, and approved by the Institutional Review Board (or Ethics Committee) of NAME OF INSTITUTE (protocol code XXX and date of approval).” OR “Ethical review and approval were waived for this study, due to REASON (please provide a detailed justification).” OR “Not applicable” for studies not involving humans or animals. You might also choose to ex-clude this statement if the study did not involve humans or animals.
%}
%\informedconsent{\hl{                     } %MDPI: please complete this part.
%Any research article describing a study involving humans should contain this statement. Please add “Informed consent was obtained from all subjects involved in the study.” OR “Patient con-sent was waived due to REASON (please provide a detailed justification).” OR “Not applicable” for studies not involving humans. You might also choose to exclude this statement if the study did not involve humans.
%
%Written informed consent for publication must be obtained from participating patients who can be identified (including by the patients themselves). Please state “Written informed consent has been obtained from the patient(s) to publish this paper” if applicable.
%}

\dataavailability{\href{https://github.com/JelleKUL/SessionAlignment}{https://github.com/JelleKUL/SessionAlignment} %MDPI: please complete this part.
%In this section, please provide details regarding where data supporting reported results can be found, including links to publicly archived datasets analyzed or generated during the study. Please refer to suggested Data Availability Statements in section “MDPI Research Data Policies” at \href{https://www.mdpi.com/ethics}{https://www.mdpi.com/ethics}. You might choose to exclude this statement if the study did not report any data.
} 


\conflictsofinterest{There are no conflicts of interest to report.}

%=====================================
% References, variant A: internal bibliography
%=====================================

\begin{adjustwidth}{-\extralength}{0cm}
%\centering %% If there is a figure in wide page, please release command \centering
\reftitle{References}
%\externalbibliography{yes}
%\bibliography{bibliography.bib}
\begin{thebibliography}{999}

\bibitem[{Perkins Coie LLP}(2020)]{PerkinsCoieLLP2020}
{Perkins Coie LLP}.
\newblock {\em {2020 Augmented and Virtual Reality Survey Report: Industry
  Insights into the Future of Immersive Technology}};  \hl{Perkins Coie LLP: Seattle, WA, USA,} %Newly added publisher information, please confirm. %confirm
2020; Volume~4.

\bibitem[Alizadehsalehi \em{et~al.}(2020)Alizadehsalehi, Hadavi, and
  Huang]{Alizadehsalehi2020a}
Alizadehsalehi, S.; Hadavi, A.; Huang, J.C.
\newblock {From BIM to extended reality in AEC industry}.
\newblock {\em Autom. Constr.} {\bf 2020}, {\em 116},~103254.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.autcon.2020.103254}{\detokenize{https://doi.org/10.1016/j.autcon.2020.103254}}}.

\bibitem[Zhang \em{et~al.}(2020)Zhang, Liu, Kang, and Al-Hussein]{Zhang2020a}
Zhang, Y.; Liu, H.; Kang, S.C.; Al-Hussein, M.
\newblock {Virtual reality applications for the built environment: Research
  trends and opportunities}.
\newblock {\em Autom. Constr.} {\bf 2020}, {\em 118},~103311.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.autcon.2020.103311}{\detokenize{https://doi.org/10.1016/j.autcon.2020.103311}}}.

\bibitem[Wu \em{et~al.}(2021)Wu, Hou, and Zhang]{Wu2021}
Wu, S.; Hou, L.; Zhang, G.K.
\newblock {\em {Integrated Application of BIM and eXtended Reality Technology:
  A Review, Classification and Outlook}}; Springer International
  Publishing:  \hl{Berlin/Heidelberg, Germany,} %newly added publisher information, please confirm %confirm
2021; Volume~98, pp. 1227--1236.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1007/978-3-030-51295-8_86}{\detokenize{https://doi.org/10.1007/978-3-030-51295-8_86}}}.

\bibitem[Du \em{et~al.}(2018)Du, Shi, Zou, and Zhao]{Du2018a}
Du, J.; Shi, Y.; Zou, Z.; Zhao, D.
\newblock {CoVR: Cloud-Based Multiuser Virtual Reality Headset System for
  Project Communication of Remote Users}.
\newblock {\em J. Constr. Eng. Manag.} {\bf 2018},
  {\em 144},~04017109.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1061/(asce)co.1943-7862.0001426}{\detokenize{https://doi.org/10.1061/(asce)co.1943-7862.0001426}}}.

\bibitem[{Pour Rahimian} \em{et~al.}(2019){Pour Rahimian}, Chavdarova, Oliver,
  and Chamo]{PourRahimian2019}
{Pour Rahimian}, F.; Chavdarova, V.; Oliver, S.; Chamo, F.
\newblock {OpenBIM-Tango integrated virtual showroom for offsite manufactured
  production of self-build housing}.
\newblock {\em Autom. Constr.} {\bf 2019}, {\em 102},~1--16.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.autcon.2019.02.009}{\detokenize{https://doi.org/10.1016/j.autcon.2019.02.009}}}.

\bibitem[Boton(2018)]{Boton2018}
Boton, C.
\newblock {Supporting constructability analysis meetings with Immersive Virtual
  Reality-based collaborative BIM 4D simulation}.
\newblock {\em Autom. Constr.} {\bf 2018}, {\em 96},~1--15.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.autcon.2018.08.020}{\detokenize{https://doi.org/10.1016/j.autcon.2018.08.020}}}.

\bibitem[Coupry \em{et~al.}(2021)Coupry, Noblecourt, Richard, Baudry, and
  Bigaud]{Coupry2021}
Coupry, C.; Noblecourt, S.; Richard, P.; Baudry, D.; Bigaud, D.
\newblock {BIM-Based digital twin and XR devices to improve maintenance
  procedures in smart buildings: A literature review}.
\newblock {\em Appl. Sci.} {\bf 2021}, {\em 11}, {6810}. 
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.3390/app11156810}{\detokenize{https://doi.org/10.3390/app11156810}}}.

\bibitem[Chu \em{et~al.}(2018)Chu, Matthews, and Love]{Chu2018}
Chu, M.; Matthews, J.; Love, P.E.
\newblock {Integrating mobile Building Information Modelling and Augmented
  Reality systems: An experimental study}.
\newblock {\em Autom. Constr.} {\bf 2018}, {\em 85},~305--316.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.autcon.2017.10.032}{\detokenize{https://doi.org/10.1016/j.autcon.2017.10.032}}}.

\bibitem[Chalhoub and Ayer(2018)]{Chalhoub2018a}
Chalhoub, J.; Ayer, S.K.
\newblock {Using Mixed Reality for electrical construction design
  communication}.
\newblock {\em Autom. Constr.} {\bf 2018}, {\em 86},~1--10.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.autcon.2017.10.028}{\detokenize{https://doi.org/10.1016/j.autcon.2017.10.028}}}.

\bibitem[Chen \em{et~al.}(2019)Chen, Chen, Li, and Cheng]{Chen2019c}
Chen, K.; Chen, W.; Li, C.T.; Cheng, J.C.
\newblock {A BIM-based location aware AR collaborative framework for facility
  maintenance management}.
\newblock {\em J. Inf. Technol. Constr.} {\bf 2019},
  {\em 24},~360--380.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.36680/j.itcon.2019.019}{\detokenize{https://doi.org/10.36680/j.itcon.2019.019}}}.

\bibitem[Diao and Shih(2019)]{Diao2019}
Diao, P.H.; Shih, N.J.
\newblock {BIM-based AR maintenance system (BARMS) as an intelligent
  instruction platform for complex plumbing facilities}.
\newblock {\em Appl. Sci. }{\bf 2019}, {\em 9}, 1592.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.3390/app9081592}{\detokenize{https://doi.org/10.3390/app9081592}}}.

\bibitem[Park \em{et~al.}(2013)Park, Lee, Kwon, and Wang]{Park2013}
Park, C.S.; Lee, D.Y.; Kwon, O.S.; Wang, X.
\newblock {A framework for proactive construction defect management using BIM,
  augmented reality and ontology-based data collection template}.
\newblock {\em Autom. Constr.} {\bf 2013}, {\em 33},~61--71.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.autcon.2012.09.010}{\detokenize{https://doi.org/10.1016/j.autcon.2012.09.010}}}.

\bibitem[Al-Sabbag \em{et~al.}(2022)Al-Sabbag, Yeum, and
  Narasimhan]{Al-Sabbag2022}
Al-Sabbag, Z.A.; Yeum, C.M.; Narasimhan, S.
\newblock {Interactive defect quantification through extended reality}.
\newblock {\em Adv. Eng. Inform.} {\bf 2022}, {\em 51},~101473.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.aei.2021.101473}{\detokenize{https://doi.org/10.1016/j.aei.2021.101473}}}.

\bibitem[Vlx(2021)]{Vlx2021}
NavVis VLX.
\newblock {\em Evaluating indoor \& Outdoor Mobile Mapping Accuracy}; NavVis: Munchen, Germany 2021; %MDPI: please add publisher name and its location % check
\newblock {pp. 1--16.}


\bibitem[Bassier \em{et~al.}(2016)Bassier, Vergauwen, and {Van
  Genechten}]{Bassier2016TLS}
Bassier, M.; Vergauwen, M.; {Van Genechten}, B.
\newblock {Standalone Terrestrial Laser Scanning for Efficiently Capturing Aec
  Buildings for As-Built Bim}.
\newblock {\em ISPRS Ann. Photogramm. Remote Sens. Spat. Inf. Sci.} {\bf 2016}, {\em III-6},~49--55.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.5194/isprsannals-III-6-49-2016}{\detokenize{https://doi.org/10.5194/isprsannals-III-6-49-2016}}}.

\bibitem[Taketomi \em{et~al.}(2017)Taketomi, Uchiyama, and Ikeda]{Taketomi2017}
Taketomi, T.; Uchiyama, H.; Ikeda, S.
\newblock {Visual SLAM algorithms: A survey from 2010 to 2016}.
\newblock {\em IPSJ Trans. Comput. Vis. Appl.} {\bf
  2017}, {\em 9}, {16.} 
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1186/s41074-017-0027-2}{\detokenize{https://doi.org/10.1186/s41074-017-0027-2}}}.

\bibitem[Cyrus \em{et~al.}(2019)Cyrus, Krcmarik, Moezzi, Koci, and
  Petru]{Cyrus2019}
Cyrus, J.; Krcmarik, D.; Moezzi, R.; Koci, J.; Petru, M.
\newblock {Hololens used for precise position tracking of the third party
  devices---Autonomous vehicles}.
\newblock {\em Commun. -Sci. Lett. Univ. Zilina}
  {\bf 2019}, {\em 21},~18--23.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.26552/com.c.2019.2.18-23}{\detokenize{https://doi.org/10.26552/com.c.2019.2.18-23}}}.

\bibitem[{U.S. Institute of Building
  Documentation}(2019)]{U.S.InstituteofBuildingDocumentation2016}
{U.S. Institute of Building Documentation}.
\newblock {\emph{USIBD Level of Accuracy ( LOA ) Specification Guide v3.0-2019}};
\newblock Technical Report; U.S. Institute of Building Documentation: Denver, CO, USA,  %MDPI: newly added publisher information, please confirm % confirm
 2019.

\bibitem[{De Geyter} \em{et~al.}(2022){De Geyter}, Vermandere, {De Winter},
  Bassier, and Vergauwen]{DeGeyter2022}
{De Geyter}, S.; Vermandere, J.; {De Winter}, H.; Bassier, M.; Vergauwen, M.
\newblock {Point Cloud Validation: On the Impact of Laser Scanning Technologies
  on the Semantic Segmentation for BIM Modeling and Evaluation}.
\newblock {\em Remote Sens.} {\bf 2022}, {\em 14}, 582.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.3390/rs14030582}{\detokenize{https://doi.org/10.3390/rs14030582}}}.

\bibitem[Marchand \em{et~al.}(2016)Marchand, Uchiyama, and
  Spindler]{Marchand2016}
Marchand, E.; Uchiyama, H.; Spindler, F.
\newblock {Pose Estimation for Augmented Reality: A Hands-On Survey}.
\newblock {\em IEEE Trans. Vis. Comput. Graph.} {\bf
  2016}, {\em 22},~2633--2651.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1109/TVCG.2015.2513408}{\detokenize{https://doi.org/10.1109/TVCG.2015.2513408}}}.

\bibitem[Liu \em{et~al.}(2018)Liu, Wu, Zhang, Lin, Yin, and Chen]{Liu2018b}
Liu, R.; Wu, J.; Zhang, J.; Lin, R.; Yin, K.; Chen, S. 
{Instant SLAM initialization for outdoor omnidirectional augmented reality}. 
\hl{In Proceedings of the 31st International Conference on Computer Animation and Social Agents (CASA 2018), Beijing, China, 21--23 May 2018};  %MDPI: we adjusted this reference as conference proceedings type, please confirm the type and newly added conference information. %confirm
%{\em ACM Int. Conf. Proc. Ser.} {\bf 2018}, 
  {66--70.} 
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1145/3205326.3205359}{\detokenize{https://doi.org/10.1145/3205326.3205359}}}.

\bibitem[Ventura \em{et~al.}(2014)Ventura, Arth, Reitmayr, and
  Schmalstieg]{Ventura2014}
Ventura, J.; Arth, C.; Reitmayr, G.; Schmalstieg, D.
\newblock {Global localization from monocular SLAM on a mobile phone}.
\newblock {\em IEEE Trans. Vis. Comput. Graph.} {\bf
  2014}, {\em 20},~531--539.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1109/TVCG.2014.27}{\detokenize{https://doi.org/10.1109/TVCG.2014.27}}}.

\bibitem[Brachmann and Rother(2019)]{Brachmann2019}
Brachmann, E.; Rother, C.
\newblock {Expert sample consensus applied to camera re-localization}.
\newblock {\em Proc.  IEEE Int. Conf. Comput. Vis.} {\bf 2019}, {\em 2019},~7524--7533.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1109/ICCV.2019.00762}{\detokenize{https://doi.org/10.1109/ICCV.2019.00762}}}.

\bibitem[Zollmann(2020)]{Zollmann2020}
Zollmann, S.
\newblock {Localisation and Tracking of Stationary Users for
  Extended Reality Lewis Baker. Ph.D. Thesis, \hl{University of Otago, Dunedin, New Zealand,} %MDPI: newly added information, please confirm % confirm
 2020}.

\bibitem[Liu \em{et~al.}(2020)Liu, Chen, and Chen]{Liu2020}
Liu, Y.C.; Chen, J.R.; Chen, H.M.
\newblock {System Development of an Augmented Reality On-site BIM Viewer Based
  on the Integration of SLAM and BLE Indoor Positioning}.
\newblock  In Proceedings of the 37th International Symposium on Automation
  and Robotics in Construction, ISARC 2020: From Demonstration to Practical Use---To New Stage of Construction Robot, \hl{Kitakyushu, Japan, 27--28 October  2020;} %MDPI: newly added conference information, please confirm % confirm
 pp. 293--300.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.22260/isarc2020/0042}{\detokenize{https://doi.org/10.22260/isarc2020/0042}}}.

\bibitem[Zhou \em{et~al.}(2018)Zhou, Park, and Koltun]{Zhou2018a}
Zhou, Q.Y.; Park, J.; Koltun, V.
\newblock {Open3D: A Modern Library for 3D Data Processing}.
\newblock {\em arXiv} {\bf 2018}, arXiv:1801.09847v1.

\bibitem[Zwierzycki \em{et~al.}(2016)Zwierzycki, Evers, Tamke, and
  Tools]{Zwierzycki2016}
Zwierzycki, M.; Evers, H.L.; Tamke, M.; Tools, A.D.
\newblock {Parametric Architectural Design with Point-clouds}.
\newblock In Proceedings of the 34th eCAADe Conference, \hl{Oulu, Finland, 22--26 August  2016;} %MDPI: newly added conference information, please confirm % co,firm
 {Volume
  2},~pp. 673--682.

\bibitem[Carl(2018)]{Carl2018}
Carl, B.
\newblock {rdflib: A high level wrapper around the redland package for common
  rdf applications.} https://github.com/ropensci/rdflib/tree/0.2.3 accessed on 20/04/2022 \hl{\textbf{2018}.} %MDPI: please add journal name, volume and page; 
%  or format as book type, please add publisher name + location; 
%  or format it as online resource, please add web link + accessed date (date should before 30 May 2022))
\linebreak https://doi.org/10.5281/zenodo.1098478.

\bibitem[Rusu \em{et~al.}(2009)Rusu, Blodow, and Beetz]{Rusu2009b}
Rusu, R.B.; Blodow, N.; Beetz, M.
\newblock {Fast Point Feature Histograms (FPFH) for 3D registration}.
\newblock  In Proceedings of the  2009 IEEE International Conference on Robotics and Automation,
 \hl{Kobe, Japan, 12--17 May   2009;} %MDPI: newly added conference information, please confirm %confirm
 pp. 3212--3217.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1109/ROBOT.2009.5152473}{\detokenize{https://doi.org/10.1109/ROBOT.2009.5152473}}}.

\bibitem[Wiemann \em{et~al.}(2015)Wiemann, Annuth, Lingemann, and
  Hertzberg]{Wiemann2015}
Wiemann, T.; Annuth, H.; Lingemann, K.; Hertzberg, J.
\newblock {An Extended Evaluation of Open Source Surface Reconstruction
  Software for Robotic Applications}.
\newblock {\em J. Intell. Robot. Syst.} {\bf 2015}, {\em
  77},~149--170.
\newblock {\changeurlcolor{black}\href{https://doi.org/10.1007/s10846-014-0155-1}{\detokenize{https://doi.org/10.1007/s10846-014-0155-1}}}.

\bibitem[Mellado \em{et~al.}(2014)Mellado, Aiger, and Mitra]{Mellado2014}
Mellado, N.; Aiger, D.; Mitra, N.J.
\newblock {SUPER 4PCS fast global pointcloud registration via smart indexing}.
\newblock {\em Eurograph. Symp. Geom. Process.} {\bf 2014}, {\em
  33},~205--215.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1111/cgf.12446}{\detokenize{https://doi.org/10.1111/cgf.12446}}}.

\bibitem[Bassier \em{et~al.}(2020)Bassier, Bonduel, Derdaele, and
  Vergauwen]{Bassier2020a}
Bassier, M.; Bonduel, M.; Derdaele, J.; Vergauwen, M.
\newblock {Processing existing building geometry for reuse as Linked Data}.
\newblock {\em Autom. Constr.} {\bf 2020}, {\em 115},~103180.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.autcon.2020.103180}{\detokenize{https://doi.org/10.1016/j.autcon.2020.103180}}}.

\bibitem[Ellefi \em{et~al.}(2018)Ellefi, Papini, Merad, Boi, Royer, Pasquet,
  Sourisseau, Castro, Nawaf, and Drap]{Ellefi2018}
Ellefi, M.B.; Papini, O.; Merad, D.; Boi, J.M.; Royer, J.P.; Pasquet, J.;
  Sourisseau, J.C.; Castro, F.; Nawaf, M.M.; Drap, P.
\newblock {Cultural Heritage Resources Profiling: Ontology-based Approach}.
\newblock   In Proceedings of the The Web Conference 2018 (WWW '18), 	\hl{Lyon, France, 23--27 April  2018}; %MDPI: newly added conference information, please confirm %confirm
  International World Wide Web Conferences Steering Committee: {Geneva,  Switzerland,} 2018; pp.~1489--1496.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1145/3184558.3191598}{\detokenize{https://doi.org/10.1145/3184558.3191598}}}.
  
\bibitem[W3Schools(2022)]{W3Schools2022}
W3Schools.
\newblock {\em HTML Geolocation API}; \detokenize{https://www.w3schools.com/html/html5_geolocation.asp} accessed on 20/04/2022  \hl{W3Schools:} %MDPI: please add publisher's location
 2022.
 
\bibitem[Rusu \em{et~al.}(2008)Rusu, Marton, Blodow, Dolha, and
  Beetz]{Rusu2008}
Rusu, R.B.; Marton, Z.C.; Blodow, N.; Dolha, M.; Beetz, M.
\newblock {Towards 3D Point cloud based object maps for household
  environments}.
\newblock {\em Robot. Auton. Syst.} {\bf {2008}}, \hl{\emph{56}, 927--941.} %MDPI: Newly added, please confirm. % confirm
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.robot.2008.08.005}{\detokenize{https://doi.org/10.1016/j.robot.2008.08.005}}}.



\bibitem[Zhou \em{et~al.}(2016)Zhou, Park, and Koltun]{Zhou2016}
Zhou, Q.Y.; Park, J.; Koltun, V.
\newblock {Fast global registration}.
\newblock {\em Lect. Notes Comput. Sci. } {\bf
  2016}, {\em 9906},~766--782.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1007/978-3-319-46475-6_47}{\detokenize{https://doi.org/10.1007/978-3-319-46475-6_47}}}.

\bibitem[Jinyu \em{et~al.}(2019)Jinyu, Bangbang, Danpeng, Nan, Guofeng, and
  Hujun]{Jinyu2019}
Jinyu, L.; Bangbang, Y.; Danpeng, C.; Nan, W.; Guofeng, Z.; Hujun, B.
\newblock {Survey and evaluation of monocular visual-inertial SLAM algorithms
  for augmented reality}.
\newblock {\em Virtual Real. Intell. Hardw.} {\bf 2019}, {\em
  1},~386--410.
\newblock
  {\changeurlcolor{black}\href{https://doi.org/10.1016/j.vrih.2019.07.002}{\detokenize{https://doi.org/10.1016/j.vrih.2019.07.002}}}.

\bibitem[Schonberger and Frahm(2016)]{Schonberger2016}
Schonberger, J.L.; Frahm, J.m.
\newblock {Structure-from-Motion Revisited}.
\newblock   In Proceedings of the Conference on Computer Vision and Pattern Recognition (CVPR),  \hl{Las Vegas, NV, USA, 27--30 June  2016;} %MDPI: newly added conference information, please confirm %confirm
  pp. 4104--4113.

\bibitem[Amanatides and Woo(1987)]{Amanatides1987}
Amanatides, J.; Woo, A.
\newblock {A Fast Voxel Traversal Algorithm for Ray Tracing}.
\newblock {\em Eurographics} {\bf 1987}, {\em 87},~3--10.

\end{thebibliography}

\end{adjustwidth}

\end{document}


%%
%% This is file `sample-sigconf.tex',
%% generated with the docstrip utility.
%%
%% The original source files were:
%%
%% samples.dtx  (with options: `sigconf')
%% 
%% IMPORTANT NOTICE:
%% 
%% For the copyright see the source file.
%% 
%% Any modified versions of this file must be renamed
%% with new filenames distinct from sample-sigconf.tex.
%% 
%% For distribution of the original source see the terms
%% for copying and modification in the file samples.dtx.
%% 
%% This generated file may be distributed as long as the
%% original source files, as listed above, are part of the
%% same distribution. (The sources need not necessarily be
%% in the same archive or directory.)
%%
%% Commands for TeXCount
%TC:macro \cite [option:text,text]
%TC:macro \citep [option:text,text]
%TC:macro \citet [option:text,text]
%TC:envir table 0 1
%TC:envir table* 0 1
%TC:envir tabular [ignore] word
%TC:envir displaymath 0 word
%TC:envir math 0 word
%TC:envir comment 0 0
%%
%%
%% The first command in your LaTeX source must be the \documentclass command.
\documentclass[sigconf]{acmart}
%% NOTE that a single column version may be required for 
%% submission and peer review. This can be done by changing
%% the \doucmentclass[...]{acmart} in this template to 
%% \documentclass[manuscript,screen]{acmart}
%% 
%% To ensure 100% compatibility, please check the white list of
%% approved LaTeX packages to be used with the Master Article Template at
%% https://www.acm.org/publications/taps/whitelist-of-latex-packages 
%% before creating your document. The white list page provides 
%% information on how to submit additional LaTeX packages for 
%% review and adoption.
%% Fonts used in the template cannot be substituted; margin 
%% adjustments are not allowed.
%%
%%
%% \BibTeX command to typeset BibTeX logo in the docs
\AtBeginDocument{%
  \providecommand\BibTeX{{%
    \normalfont B\kern-0.5em{\scshape i\kern-0.25em b}\kern-0.8em\TeX}}}

%% Rights management information.  This information is sent to you
%% when you complete the rights form.  These commands have SAMPLE
%% values in them; it is your responsibility as an author to replace
%% the commands and values with those provided to you when you
%% complete the rights form.
\setcopyright{rightsretained} 
\copyrightyear{2022} 
\acmYear{2022} 
\acmConference{SIGGRAPH '22 Posters}{August 07-11, 2022}{Vancouver, BC, Canada}
\acmBooktitle{Special Interest Group on Computer Graphics and Interactive Techniques Conference Posters (SIGGRAPH '22 Posters), August 07-11, 2022}
\acmDOI{10.1145/3532719.3543258}
\acmISBN{978-1-4503-9361-4/22/08}

%%
%% Submission ID.
%% Use this when submitting an article to a sponsored event. You'll
%% receive a unique submission ID from the organizers
%% of the event, and this ID should be used as the parameter to this command.
%%\acmSubmissionID{123-A56-BU3}

%%
%% For managing citations, it is recommended to use bibliography
%% files in BibTeX format.
%%
%% You can then either use BibTeX with the ACM-Reference-Format style,
%% or BibLaTeX with the acmnumeric or acmauthoryear sytles, that include
%% support for advanced citation of software artefact from the
%% biblatex-software package, also separately available on CTAN.
%%
%% Look at the sample-*-biblatex.tex files for templates showcasing
%% the biblatex styles.
%%
%\usepackage{todonotes}
%%
%% The majority of ACM publications use numbered citations and
%% references.  The command \citestyle{authoryear} switches to the
%% "author year" style.
%%
%% If you are preparing content for an event
%% sponsored by ACM SIGGRAPH, you must use the "author year" style of
%% citations and references.
%% Uncommenting
%% the next command will enable that style.
\citestyle{acmauthoryear}

%%
%% end of the preamble, start of the body of the document source.
\begin{document}

\acmBooktitle{SIGGRAPH ’22 Posters} 
%\acmPrice{15.00}
\acmISBN{978-1-4503-XXXX-X/18/06}

%%
%% The "title" command has an optional parameter,
%% allowing the author to define a "short title" to be used in page headers.
\title{Automatic alignment and completion of point cloud environments using XR data}

%%
%% The "author" command and its associated commands are used to define
%% the authors and their affiliations.
%% Of note is the shared affiliation of the first two authors, and the
%% "authornote" and "authornotemark" commands
%% used to denote shared contribution to the research.
\author{Jelle Vermandere}
\authornote{All authors contributed equally to this research.}
\orcid{0000-0002-7809-9798}
 \email{jelle.vermandere@kuleuven.be}
\author{Maarten Bassier}
\authornotemark[1]
\orcid{0000-0001-8526-8847}
 \email{maarten.bassier@kuleuven.be}
\author{Maarten Vergauwen}
\authornotemark[1]
\orcid{0000-0003-3465-9033}
 \email{maarten.vergauwen@kuleuven.be}
\affiliation{%
  \institution{Dept. of Civil Engineering, TC Construction - Geomatics, KU Leuven - Faculty of Engineering Technology}
  \city{Ghent}
  \country{Belgium}
  \postcode{9000}
}

%%
%% By default, the full list of authors will be used in the page
%% headers. Often, this list is too long, and will overlap
%% other information printed in the page headers. This command allows
%% the author to define a more concise list
%% of authors' names for this purpose.
\renewcommand{\shortauthors}{Vermandere, Bassier \& Vergauwen}

%%
%% The abstract is a short summary of the work to be presented in the
%% article.
%\begin{abstract}
%  A clear and well-documented \LaTeX\ document is presented as an
%  article formatted for publication by ACM in a conference proceedings
%  or journal publication. Based on the ``acmart'' document class, this
%  article presents and explains many of the common variations, as well
%  as many of the formatting elements an author may use in the
%  preparation of the documentation of their work.
%\end{abstract}

%%
%% The code below is generated by the tool at http://dl.acm.org/ccs.cfm.
%% Please copy and paste the code instead of the example below.
%%
\begin{CCSXML}
<ccs2012>
   <concept>
       <concept_id>10003120.10003121.10003124.10010392</concept_id>
       <concept_desc>Human-centered computing~Mixed / augmented reality</concept_desc>
       <concept_significance>500</concept_significance>
       </concept>
 </ccs2012>
\end{CCSXML}

%\ccsdesc[500]{Human-centered computing~Mixed / augmented reality}

%%
%% Keywords. The author(s) should pick words that accurately describe
%% the work being presented. Separate the keywords with commas.
%\keywords{XR, Pointclouds, Lidar, Terrestrial Laser Scanning}

%% A "teaser" image appears between the author and affiliation
%% information and the body of the document, and typically spans the
%% page.
\begin{teaserfigure}
  \includegraphics[width=\textwidth]{images/Teaser_image.png}
  \caption{The XR session data (a) and pre-existing point clouds (b) are linked through semantic web technologies. This enables the automated alignment, querying and updating of the existing data which increases the longevity of geospatial data without the need to recapture the entire environment (c).}
  \Description{The XR session completes the point cloud session}
  \label{fig:teaser}
\end{teaserfigure}

%%
%% This command processes the author and affiliation and title
%% information and builds the first part of the formatted document.
\maketitle

\section{Introduction}\label{sec:Introduction}

%This abstract presents a novel way to localize generic XR devices in existing geo-localized %reference structures and environments.
%
%- Increasing realism in the gaming industry
%- TLS and Photogrammetry very time consuming
%- Why XR -> user feedback on the completion
%- Xr devices can capture both 2D and 3d data in the form of images and meshes. 
%
%Intro structure
%
%- Explain the current state of the art
%  - the rising need for detailed environments, both in the AECO industry as the entertainment %industry
%  - the hassle and time consuming nature of 
%
%- Highlight the weaknesses of current methods
% they are manually aligned
% they don't cover 

% problem statement
% - Making recordings of environments is very handy (site monitoring, visualisations, ect...)
% - TLS scanners can make very good scans, but are slow and expensive, so they are rarely up to date with the current reality
%VERSION1
%With the ever increasing digitisation of the real world comes the inevitable challenge of capturing environments rapidly and precisely. One good method for doing this is using TLS (terrestrial laser scanners) to capture a point cloud of the environment. These have a number of downsides as well, namely that recording a full site is time consuming and it is difficult to get full coverage of a site. This is why taking multiple recordings at throughout time of a building site is difficult and is not often done. It is difficult to get fast feedback on what parts are recorded correctly and what is missing. Keeping the digital twin of the real environment up to date in a constant changing environment is challenging, due to the significant time and hardware investment in making consecutive recordings.

%VERSION2
% up to date representatie van de facility -> kostelijk en tijdrovend

% industry needs up-to-date and complete geospatial data of their facilities
% an enormous amount of data is captured in the industry but it doesn't pay of. why?
    % 1. the data is incomplete
    % 2. the data has a very short lifespan
    % 3. most data is redundant
    
% 3. capturing data is very expensive. how can low-end systems contribute?
% 4. data from low-end systems does not end up in the repository or isn't even processed at all

%VERSION3
The AECO industry needs up-to-date and complete geo-spacial data of their facilities, ranging from construction site monitoring to immersive visualisations. Despite an enormous amount of data being captured, this goal remains a great challenge due to several factors. First, the captured data is often incomplete due to occlusions or limited sensor range. Second, captured data quickly becomes outdated, while making numerous consecutive full recordings is very time-consuming and expensive. Finally, the data coming from lower-end, cheaper capture devices is rarely used due to a lack of a proper framework to incorporate them into larger datasets. A way to combat these shortcomings is to make better use of the lower-end data. While these sensors lack range and precision, the increased recording speed and lighter data load is a big advantage over traditional devices. As shown in figure \ref{fig:teaser}, combining high-end with lower-end datasets, by updating small parts of the full point cloud, could extend their relevance and decrease the total recording investment.

%VERSION2
%With the increasing demand for high-quality 3D representations of real environments, ranging from construction site monitoring to immersive visualisations, comes the inevitable challenge of capturing them with high fidelity. Currently, highly detailed but time consuming methods like photogrammetry and laserscanning are being used to capture these environments. While these methods provide great results, it is difficult to evaluate the quality and coverage of the recordings on site due to their need for additional post processing. The slow and expensive workflow hinders their ability to be used repeatedly to fix any coverage errors or update the data should anything change on site.

% how XR devices bring improvements
% - 2 big advantages: Speed and interaction to get feedback on site
% meer gericht te werk gaan op zoek naar 
% - also disadvantages: Low resolution (accuracy) and range
% VERSION 1
%This is where XR devices like the Microsoft Hololens 2 can bring a number of great advantages. These devices can capture both 2D localised Images and 3D meshes in realtime, allowing for much faster recordings and visual coverage feedback. They do however have a limited range and resolution. making them less viable for large-scale and highly-detailed recordings.

%Version2
Using XR devices like the Hololens 2 as the lower-end sensor can bring additional advantages because they can capture both 2D localised Images and 3D meshes, and get realtime coverage feedback. Allowing users to seek out occlusions and inconsistencies of the previously captured high-end dataset on-site.

% Stating the potential improvement to the SOA
%A potential approach to improve the capture workflow is combining the XR data with the pre recorded point cloud data. to update small parts of the full point cloud, for a faster iteration of the capturing process. They can provide visual feedback to see incomplete or outdated parts of the captured point cloud. 

% overview of existing works
% - which works already try to combine datasets with different scanners
% - what are their shortcomings
Existing works rely on using multiple sensors to capture data at different scales and levels of detail, ranging from adding highly detailed scans of small selected objects \cite{Lachat2016}, to filling in large occluded areas using drone footage\cite{Bolkas2020}. These methods, however, rely on a (semi-)manual alignment step, making the quick feedback loop from an XR device not feasible. Merging different datasets in the current state of the art is mostly focused on improving the coverage of the initial scan \cite{Partovi2021}. Updating the initial point cloud with supplementary data captured at a different point in time is a field of little research.

% The goal of this research
% - What is the main goal of the research
% - how will this work combat the shortcomings and gaps in the SOA
The goal of this work is to set up a pipeline to complement and update existing point clouds using automatically aligned XR datasets captured on site. This will keep the dataset relevant over a longer period of time, requiring fewer full-scale recordings and better overall coverage of the site.



\section{Methodology}\label{sec:Methodology}

%The completion process can be divided in three distinct parts:
%- Creating a unified data capture system for compatibility between sessions
%- Aligning the XR session with the existing reference session
%- Using the XR data to complement the Existing Data

% option one, explain the three methods very briefly, elaborate on the SOA in each section
% option 2, explain the SOA here and focus on my method in the next parts

% The approach
% Standardised data capture
%VERSION1
%The completion of pointclouds will be performed in 3 steps, all essential to the process. The first step is Capturing the data with the XR device using a standardised ontology to make the different datasets compatible. The next step is aligning the XR dataset with the existing point cloud data. The final step is updating the point cloud dataset with the XR data.

% Practical structure
%The whole pipeline is constructed using the Unity Game engine for the front end and a python webserver for the backend.

% Brief overview of the methodology
%\todo{kies versie}
%OPTIE 1
Before 2 datasets can be combined, as explained in \ref{dataCompletion}, they need to be aligned. Each recording set is stored as a session, containing all the metadata and raw files. To get feedback on the current state of the scan, the alignment process needs to happen while the user is on-site. This is why an automatic method is proposed in \ref{sessionAlignment}. This relies on a connection between a python webserver and the XR device to access the reference datasets and increased computational performance.

%OPTIE 2
%We propose a two step approach where first, the XR device is geo-localised on site as explained in \ref{sessionAlignment}. After the new scans are made, they get merged using a novel algorithm \ref{dataCompletion}.

%\subsection{Data Capture}
%
%\todo{might scratch this, and put a couple of sentences in the intro, does not really add %anything}
%%- XR Data is captured using Unity and AR foundation
%%- Reference data can be TLS, drone, Photogrammetry
%%- all data is stored using an RDF schema
%%- open standards for global coordinate systems
%
%% The need for standardisation using RDF sessions
%To enable the inter-compatibility between the different datasets. There is a need to standardize %the way the information is stored. This can be done using RDF Ontology's \cite{Bassier2020}, %they allow an easy way to read and write data with different programs and sensors. each data %recording is stored together in a session. this contains all the data taken on site with that %sensor at that time. This ensures all the data in a session is correctly localised in relation %to each other.
%
%% How the data was captured in practice
%In this case, the data capture application was created in Unity using the AR Foundation %framework, this enables a multiplatform rollout so it is not limited to using the Hololens in %this case. The Application allows the user to walk around a site and capture an environmental %mesh [resolution?] and capture images. These images, because the Hololens has constant awareness %of its local position, also contain their positional information. This is crucial for the %alignment process.
%
%In figure \ref{fig:teaser} an example recording can be seen where both the mesh and the %localised images are displayed.


\subsection{Session Alignment} \label{sessionAlignment}

%- XR session is aligned to itself, no large drift
%- existing methods use either 2D or 3D
%- Use multi method alignment with Opencv and open3D and openGR
%- Pose voting to select the best position
%- don't mention the global alignment, assume we go the a site with the goal to capture it

%method overview
%VERSION1
XR devices can record both images and meshes, so multiple pose estimations can be made using 2D and 3D matching techniques. These estimations all have matching parameters like the reprojection error, overlap, inliers and sensor type. These normalized values are used as weights to evaluate the confidence of the proposed pose. The final pose is determined as a weighted average of all the estimations.

\subsubsection{Image alignment}
%2D matching
The 2D images are matched with the reference dataset, using the OpenCV framework to generate ORB features and state-of-the-art 2D matching algorithms to estimate a pose for each captured image \cite{Luo2019}.
%The Parameters of the match are stored and used as weights to determine the confidence of the pose as seen in table \ref{tab:pose_estimation_table}
%, . (Images have been used to match against generated BIM imagery for indoor localisation \cite{Baek2019}. However,) 

\subsubsection{Point cloud alignment}
%3D matching
The 3D mesh is matched against the point cloud using Open3d FPFH Feature matching, using the Fast global registration algorithm to determine the transformation \cite{Zhou2016}. The point cloud is subdivided into parts of similar size to the mesh to make multiple estimations.

\subsubsection{Pose Estimation}
%pose estimation
The final pose estimation is the weighted average of the different methods and their confidence factor. The accuracy of this method is precise up to the resolution of the sensor as seen in table \ref{tab:pose_estimation_table} where it was tested on multiple datasets.

\begin{table}[h]
  \centering
  \caption{The results of the different matching methods, showing the accuracy is up to the resolution of the Hololens sensor}
  \includegraphics[width=\linewidth]{images/Pose_estimation_table.png}
  \Description{A table showing the results of the pose estimation}
  \label{tab:pose_estimation_table}
\end{table}

\subsection{Session Merging} \label{dataCompletion}

%\todo{Show before and after of completion}
%- Combining the Pointcloud data with new XR data
%- Check occlusion by checking where there is mesh but no pointcloud
%  - Mesh to point distance
%- Pointsampling the mesh and using a xoxelgrid to check occupancy 
%- using a boundingbox to determine what should e updated in case stuff gets deleted
%- using normal visibility check to see if point should have been visible to the user
%If you keep only seeing on convex rooms, than the boundingbox is enough, this method fails in %more complex rooms where the XR dataset also does not full capture the environment, that is why %we also add a visibility check  to check if the point that should be compared could have been %visible by the User
%- this can be done by tracking the user path throughout the recording session
%- after the fact by comparing the normals of the neighbouring mesh

% concept and existing works
% To much Info
%The final step of the completion process is update the point cloud. This has been done in a number of studies [cite], but the combination of 2 roomscale datasets, one being a XR device has rarely been explored. A promising method is proposed in \cite{Bolkas2020}, where 2 point clouds are compared by subdividing the dataset into small Grids and filling in occlusions of the master dataset, since this focuses on using drone footage, the method is limited to 2D space. A significant lack in the SOA in updating the pointclouds with new data from a different moment in time, allowing change in the environment is crucial to the goal of this work to allow continuous udating of the base pointcloud. \cite{Partovi2021} Focusses on completing occlusions in pointclouds with photogrammetric images. This work is however does not concern itself with changes in the already captured environment. A promising method is proposed by \cite{Alteirac2021}, This is however focused on simultaneous data acquisition with different sensors at the same time.

% Occlusion vs. not scanned vs. new objects
% Explain the full plan in large strokes
The main challenge is determining which points in the point cloud are still up to date. This includes the removal and addition of the newer XR data onto the existing reference dataset. We propose a 3 step process as outlined in figure \ref{fig:completion_schema}.
%First, only the re Secondly, the removal of the out-of-date points in the reference point cloud is performed. Finally, new points are added from the XR dataset to the existing reference pointcloud.


%datasets is determining which one is correct. To enable compatibility between the datasets, the XR mesh is sampled to a resolution of 1cm. Points that only appear in the reference  dataset could be remnants of an object that is no longer there, or the could not have been scanned by the XR device, so additional steps need to be taken to check if the point is correct or not. Points that are in the XR dataset but not in the reference pointcloud on the other hand, should be added because they can be either new objects or capture occlusions. Because the reference pointcloud has a much higher resolution and accuracy, simply replacing all the overlapping parts with the XR dataset will result in a worse overall combined dataset, even in the parts that have not been altered.

\begin{figure}[h]
  \centering
  \includegraphics[width=\linewidth]{images/mergingWorkflow.png}
  \caption{A three-step process for merging datasets with a subselection of relevant reference data(a), the removal of out-of-date points (b) and the addition of new points(c).}
  \Description{A schema showing the workflow}
  \label{fig:completion_schema}
\end{figure}

\subsubsection{Relevant point selection}
%To make sure none of the points that could be missing in the XR dataset because they simply are not scanned are removed from the reference dataset, only the points in the convex hull of the XR dataset are considered. This is more precise than a bounding box and leads to fewer false positives. 

After the XR dataset is cleaned up, the reference points are sub-selected using the convex hull of the XR dataset to ensure only points within the newly scanned area can be altered. This method is more precise than a bounding box and leads to fewer false positives.

% step 1 remove old points
% de grenzen, de normal checking noise
\subsubsection{Out-of-date point removal} \label{sec:Out-of-date point removal}
%All the reference points in the sub-selection are distance checked against the XR dataset and if they are further than a threshold value, they are considered not covered by the XR dataset. These points then are checked if they should have been visible to the user of the XR device at the time of recording. (This is done using a raycast check against the recorded trajectory of the device). If the points should have been visible, and are not covered, they are removed from the point cloud.

%VERSION2
To determine which points are out of date, each one must pass at least one of the following checks: a coverage or occlusion check. The coverage is determined with a threshold distance query against the cleaned-up XR mesh. The visibility check serves to maintain points that were not visible to the XR device due to occlusions and is determined by comparing the normals of the $n$ closest faces from the flagged point. If the RANSAC filtered majority of faces are facing away from the point, it is flagged as occluded and fails the check.

%(, this flags all the points that do not have a corresdonding face of the mesh.) 

% step 2 add new points
\subsubsection{New point addition}
The reference point cloud is assumed to have much a higher accuracy than the sampled XR data, so only the not covered XR points are added. To determine these points, an inverse distance query is performed compared to \ref{sec:Out-of-date point removal}. By default, the XR mesh does not contain any colour information, this is retrieved from the localised XR images by determining the corresponding pixel value through a raycast to the new points.
%by raycasting to the new points from the image position and coloring the points the corresponding pixel value if there is a valid Image. 


\section{Discussion}\label{sec:Discussion}

%- the standardisation helps to automate the process and improves future compatibility
%- The Multisensor alignment process provides and accuraccy upto the resolution of the sensor, so %it is adequate for this usecase
%- The Data completion provides a useful way to complete and update the existing data

% main takeaways
This work has shown a method to update large existing point cloud datasets without the need to recapture the whole site. XR devices provide multi-sensory data that can not only be used to locate the user in a reference point cloud to get visual feedback of the site but also to update and complete the existing dataset with new, more focused data.

% evaluate the alignment with the standardised data capture
%The multisensory alignment shows great potential, especially for sites with a large amount of data, even from different sensors. As more data gets captured over time, the results will improve due to the increase in new geo-referenced data.

% evaluate the completion of the point clouds
%The completion process can update point clouds with minimal false positives, because of the visual feedback, the user is able to catch errors due to occlusion or outdated points in the dataset and correct them on site. This is very valuable on building sites where changes are constant and where making full TLS or photogrammetric recordings at a frequent pace is not feasible.
% talk about the long term adding of the data -> adding a new pointcloud to the old one?

%%
%% The acknowledgments section is defined using the "acks" environment
%% (and NOT an unnumbered section). This ensures the proper
%% identification of the section in the article metadata, and the
%% consistent spelling of the heading.
\begin{acks}
This project has received funding from the FWO Postdoc grant (grant agreement: 1251522N) and the Geomatics research group of the Department of Civil Engineering, TC Construction at the KU Leuven in Belgium.
\end{acks}

%%
%% The next two lines define the bibliography style to be used, and
%% the bibliography file.
\bibliographystyle{ACM-Reference-Format}
\bibliography{bibliography}

%%
%% If your work has an appendix, this is the place to put it.
%\appendix

%\section{Research Methods}


%\section{Online Resources}


\end{document}
\endinput
%%
%% End of file `sample-sigconf.tex'.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% 
%                                                                 %
%                            CHAPTER                              %
%                                                                 %
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% 
\chapter{Tests and limitations}

The chapter presents and analyzes the results of the developed algorithm. Here, various tests are performed to evaluate the performance of the algorithm and to assess its robustness and accuracy. It looks at where the algorithm works well but more importantly, where its limitations lie. Possible improvements and future research directions are discussed. The first chapter \ref{Finding an appropriate dataset}, discusses the search for a suitable scene on which to build and test the algorithm. Chapters \ref{H4:Cutting out objects}, \ref{H4:Repairing the holes} and \ref{H4:Texture reconstruction} then examine the various sub-studies and discuss where the sensitivities and difficulties lie. Finally, the last chapter \label{H4:Completely cleaned scene}, gives an example of what is envisaged when the full algorithm works smoothly.

\section{Finding an appropriate dataset} \label{Finding an appropriate dataset}

Finding a suitable data set seems obvious at first glance. But on closer inspection, this is initially an all but easy choice. The dataset must contain several occluded parts of the geometry, but at the same time it must not be too complicated, so as not to make the research process too challenging right away. Secondly, the dataset must be of sufficient quality. This is determined by the density of the mesh, water-tightness or other defects and impurities.

\subsection{ScanNet dataset}

The first dataset used throughout the beginning of this research is a set taken from Scannet. The dataset describes a student room in which a lot of furniture and objects can be found. To learn how to work with datasets and learn about the structures of meshes, this is a good set to start with. In a later part of the research where the objects were removed, it quickly became clear that this dataset is too complicated and crowded to start with. Figure \ref{fig:ScanNet}, shows the complete set of data. Upon removing all the objects, it is visible that almost a very large part of the set has disappeared, see figure \ref{fig:ScanNet_Cleaned}. The structure is no longer closed and contains few parts of the geometry that can be relied upon to fill in the gaps. This immediately identifies a first limitation that is encountered. The dataset should not be overfilled and enough geometry should be visible to fill the holes properly. From here it is necessary to look for a better set of data for object removal and hole filling.

\begin{figure}[h]
    \centering
    \includegraphics[width=0.6\linewidth]{fig/ScanNet.png}
    \caption{The complete dataset from ScanNet}
    \label{fig:ScanNet}
\end{figure}
\vspace{5mm}
\begin{figure}[!h]
    \centering
    \includegraphics[width=\linewidth]{fig/ScanNet_Cleaned.png}
    \caption{All objects removed from the ScanNet dataset. Very large and nearly insoluble holes are obtained.}
    \label{fig:ScanNet_Cleaned}
\end{figure}

\subsection{MatterPort3D dataset}

Often entire buildings or floors of buildings are scanned, resulting in a large dataset describing the entire geometry. The algorithm set up in this study was initially built on a single dataset describing a specific room. This requires a manual operation to cut out a single room from the dataset and store it as a separate dataset. Figure \ref{fig:Largedataset} shows the complete dataset of a vacation house as an example. A room (in this case, the office) is cut out and obtained manually, using Blender \textsuperscript{\textregistered}, after which it is stored separately. 

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/LargeDataset.png}
    \caption{The entire data set from which a piece was cut out and stored as a separate data set containing a single room.}
    \label{fig:Largedataset}
\end{figure}

The reason this particular set of data is chosen is the fact that the room chosen -an office-, is a room that is not too large but contains enough furniture and meets a number of criteria that were established in advance, see Figure \ref{fig:Matterport}. These \textbf{three criteria} are:

\begin{enumerate}
    \item The room should contain enough furniture but should not be overly crowded with it so that a sufficient amount of the room's geometry and texture is still visible. This makes it easier for the hole filling part to resemble the original geometry of the room.
    \item The room must contain texture in the form of UV coordinates such that after removing objects and repairing the scene the texture can be reconstructed according to the method provided.
    \item The dataset must contain sufficient difficulties, but not so that they are insurmountable or too difficult from the start. By this is meant that the difficulties present should include the possible connections that are addressed and should be solved in this study, such as a hole in a) a flat surface, b) floor/wall or c) floor/wall/wall, as shown in Figure \ref{fig:3-Difficulties}.
\end{enumerate}

\begin{figure}[!h]
    \centering
    \includegraphics[width=0.6\linewidth]{fig/Matterport3D.png}
    \caption{The complete dataset from Matterport3D}
    \label{fig:Matterport}
\end{figure}

\begin{figure}[!h]
    \centering
    \includegraphics[width=0.6\linewidth]{fig/3-Difficulties.png}
    \caption{The three possible connections addressed in this study and presented in the chosen dataset of Matterport3D. a) Plane, b) Floor/Wall connection and c) Floor/Wall/Wall connection.}
    \label{fig:3-Difficulties}
\end{figure}

Table \ref{tab:MeshSpec} gives the specification of the mesh used.

\begin{table}[]
    \centering
    \caption{Specifications of the used dataset}
    \begin{tabular}{|l|c|}
        \hline
        Source: & Matterport3D \\
        \hline
        Dataset: &	5ZKStnWn8Zo \\
        \hline
        Room: & 91.27 \\
        \hline
        N Cells: & 6450 \\
        \hline
        N Points: & 19350 \\
        \hline
        N Strips:&0 \\
        \hline
        X Bounds:&-9.634e+00, -3.991e+00 \\
        \hline
        Y Bounds:&4.391e-02, 2.794e+00\\
        \hline
        Z Bounds:&1.249e+00, 8.083e+00\\
        \hline
        N Arrays:&14\\
        \hline
    \end{tabular}
    \label{tab:MeshSpec}
\end{table}

\subsubsection{Position and rotation relative to the origin}

When cutting out part of the mesh, it is important that the position and rotation of the dataset remain the same. If the dataset is moved and/or rotated this brings problems later for reconstructing the texture using uv-coordinates linked to the vertices with their specific 3D coordinates.

However, for object detection using LabelCloud, the position and orientation of the loaded dataset makes the selecting of the boundingboxes much easier. Placement of boundingboxes must be done manually in LabelCloud making this ability to rotate and translate the dataset essential. Since LabelCloud uses the origin as the rotation point, manual placement of boundingboxes can be complicated for datasets located far from the origin. This slows down the process and increases the manual labor that must be performed. 

\vspace{5mm}

Thus, it is important to choose whether the dataset is left in its original position from the beginning of the process, or whether for ease of handling, it is first translated to the origin and then, after recovery process and before reconstructing the texture, it is translated back to its original position.
Figure \ref{fig:LabelCloud_TR} shows the two datasets that a) respectively remained at its original position and b) transleaved to the origin. 

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/LabelCloud_TR.png}
    \caption{(a) dataset at its original position and (b) dataset transposed to the origin using Blender\textsuperscript{\textregistered} .}
    \label{fig:LabelCloud_TR}
\end{figure}

\subsection{Errors and difficulties in/from the original input data}

Before going into the limitations and successes of the designed algorithm, an overview is given of the errors initially present in the original input data and causing the malfunction of this algorithm. 

\subsubsection{Non-connected vertices in original data} \label{Non-connected vertices in original data}

The occurrence of unconnected vertices is a problem that only came to light later in the research process. When visualizing the vertices and edges of the mesh, at first glance no problem seems to be present. However, in the process of filling the holes, some vertices are located on an edge but are not connected to it. This creates a boundary where the hole is not completely sealed. The fact that the mesh coming from ScanNet or Matterport3D is not a watertight mesh, where all surfaces of the object are correctly connected and sealed, can cause problems when filling the holes.

Figure \ref{fig:FeatureEdges} shows the detected edges from the input data. From vtk documentation \cite{PyVista}, the edges of a mesh are one of the following:

\begin{enumerate}
    \item boundary (used by one polygon) or a line cell
    \item non-manifold (used by three or more polygons)
    \item feature edges (edges used by two triangles and whose dihedral angle > feature angle)
    \item manifold edges (edges used by exactly two polygons)
\end{enumerate}

\begin{figure}
    \centering
    \includegraphics[width=0.5\linewidth]{fig/FeatureEdges.png}
    \caption{Extracted feauture edges form the input data with PyVista.}
    \label{fig:FeatureEdges}
\end{figure}

Because the algorithms are set up to work on a watertight mesh, manual operation is required in the process. (Resolving these initial errors in the input data is not part of this research). Blender\textsuperscript{\textregistered} is used for waterproofing the mesh. This is an easy and not time consuming solution for waterproofing a mesh before it is inputted in the algorithm. 

For a better visual representation of the error, Figure \ref{fig:NonConnectedVert} shows an example in which it can be seen that the mesh appears to be watertight, but when the vertex is shifted it appears not to be connected to the neighboring vertices.

\vspace{5mm}

\textbf{This problem of unconnected vertices occurs again after clipping the mesh with PyVista and poses a major problem for the entire algorithm. It caused a great loss of time in the research process trying to fix this and causes poor performance of/in:}

\begin{itemize}
    \item \textbf{filling gaps.} This is because when objects are cut out and the edges around them contain vertices that are not connected, borders are created in the edge, which means PyMeshFix cannot consider the geometry to be closed. The gaps are therefore not filled. Repairing these edges manually takes a lot of time and complicated the creation of the algorithm.
    \item \textbf{data processing before hole filling.} Because the edges are in the edges of the objects, certain functions cannot be performed. For example, if an object is cut out but there is still a piece of a neighbouring object in the edge. PyVista has an 'extract largest' function that removes residues that are not connected to the largest connected set in the dataset. When this function is performed on a piece of edge where two borders exist, the smallest part of the edge is removed, resulting in an open border that cannot be repaired. This results in the removal of correct parts of the geometry.
\end{itemize}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/NonConnectedVert.png}
    \caption{\textbf{Left:} The vertex seems connected to the horizontal edge on which it is located. \textbf{Right:} When moving it manually, this connection does not appear to exist, causing a loose edge.}
    \label{fig:NonConnectedVert}
\end{figure}

\section{Cutting out objects} \label{H4:Cutting out objects}

\subsection{All together or certain selection}

A selection of objects to be clipped from the scene can be made using LabelCLoud. This can be used to determine whether all objects should be removed or only certain objects, if it is not necessary to remove all objects for certain purposes. Figure \ref{fig:CuttingObjects} shows an example of a scene from which all objects have been cut out or a selection respectively.

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/CuttingObjects.png}
    \caption{\textbf{Left:} All the objects are cut out of the scene. \textbf{Right:} A selection of certain objects is cut out of the scene.}
    \label{fig:CuttingObjects}
\end{figure}

\subsection{Limitation due to the use of PyVista}

Chapter \ref{Non-connected vertices in original data} raised the problem of vertices located on edges but not being connected to another edge. Moving these vertices shows that the mesh is not watertight and a loose edge is created. By using the crop function in PyVista where the contents of the boundingboxes are cut out from the contents of the $\Delta \sigma$ larger boundingboxes, this problem occurs again. Even though the problem is fixed in the original mesh this problem is created again. By cutting out, faces and edges are intersected by the boundingbox and new vertices are formed at these locations. Figure \ref{fig:MeshError} shows an example of a cropped edge. The vertex in the corner appears connected to the edges intersecting the vertex. When moving the vertex, it appears that the vertex is only part of the vertical edge and not of the horizontal edge. Thus, at this location separate edge is created through the border which poses a problem in the repair phase since PyMeshFix can only fill fully closed gaps (Figure \ref{fig:NotClosedEdge}).

Solving this problem requires manual intervention at this stage of the process. All the $\Delta \sigma$ larger boundingboxes with the objects removed can be imported separately into Blender\textsuperscript{\textregistered}, where the problem can be solved by merging two vertices. It has to be said that no modifications can be done on the outer boundary of the $\Delta \sigma$ larger boundingbox. This would give problems for detecting the vertices that lay on the inner boundary that is created in the $\Delta \sigma$ larger boundingbox by removing the boundingbox of the object. Once solved, the fixed geometries can be exported back and loaded into Python to continue the process.

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/MeshError.png}
    \caption{\textbf{Left}: The vertex seems connected to the horizontal edge on which it is located. \textbf{Right}: When shifting manually, this connection does not appear to exist, causing an edge.}
    \label{fig:MeshError}
\end{figure}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.5\linewidth]{fig/NotClosedEdge.png}
    \caption{Edge through edge making the structure not fully closed}
    \label{fig:NotClosedEdge}
\end{figure}

\section{Repairing the holes} \label{H4:Repairing the holes}

\subsection{Densely furnished datasets}

\subsubsection{Erratic and unusable edges of objects}

In many rooms objects are close to each other. The section on cutting out boundingboxes (\ref{Cutting out boundingboxes}) explained how to remove these objects from the dataset. By using the method where the objects defined by the smallest possible boundingbox are cut out of the scene cut out by a $\Delta \sigma$ larger boundingbox, additive data (the edge along the object) is cut out each time. If the scene is densely furnished and the objects are within this $\Delta \sigma$ distance of each other, then when one object is cut out, part of the other object will be cut out with it. This makes the edge of the object, which is used to detect the planes and fill the gaps, unusable and erroneous.

\vspace{5mm}

In Figure \ref{fig:ErraticNonUsableEdges}, the edge represents a carved seat with another object behind it. Because this object is within a $\Delta \sigma$ distance of the carved seat, part of this object is included in the edge of the object used for the rest of the process. This shows that not all parts correctly describe the surrounding geometry of the seat (which should normally be a plane describing the geometry of the floor on which the seat is positioned). Due to the large number of closely spaced outliers, the number of surfaces detected by the RANSAC algorithm will be greater than 1. This leads to an incorrect application of the solution method, resulting in an incorrect recovery of the hole. Figure \ref{fig:ErraticNonUsableEdgesEdge} shows the edge of the hole containing part of the neighboring object.

\vspace{5mm}

Part of this problem can be overcome by adjusting the value of $\Delta \sigma$, which reduces the size of the edge around the object being cut out, thereby reducing the chance of including parts of neighbouring objects. However, this value of $\Delta \sigma$ should not be too small so that the edge of the object can still be considered useful to the RANSAC-algorithm and the recovery method in the rest of the process. Another solution can be to manually remove the parts of the mesh that will cause errors in a separate program such as Blender\textsuperscript{\textregistered} after the objects have been cut out. This obviously increases the manual work and  therefore makes the algorithm a lot less efficient. At some it has to be considered if the algorithm still has a purpose. 

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/ErraticNonUsableEdges.png}
    \caption{The plant and the seat are at a distance d from each other smaller than $\Delta \sigma$. As a result, part of the plant will be present in the edge geometry of the cut-out seat.}
    \label{fig:ErraticNonUsableEdgesEdge}
\end{figure}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.5\linewidth]{fig/ErraticNonUsableEdgesEdge.png}
    \caption{The edge of the hole containing a part of the neighboring object, causing faulty recovery of the hole.}
    \label{fig:ErraticNonUsableEdges}
\end{figure}

\subsubsection{Erratic but usable edges of objects}

Contrary to the previous paragraph, there is a possibility that part of an adjacent object may be included in the edge of the object, but this does not necessarily lead to an incorrect repair of the hole. In several cases there is a possibility that the geometry of the object is not that different from the geometry of the edge of the hole. This is mainly the case with paintings that are attached to walls or carpets with a nameable thickness. These objects also have a planar geometry but have a small deviation $\Delta$h with respect to the geometry of the floor/wall/ceiling. If $\Delta$h is small enough, depending on the parameters set for the RANSAC-algorithm, these points are still considered to be inliers of the plane. Consequently, no redundant planes are detected and the algorithm will still call the correct solution method to repair the hole. Although the hole is recovered, due to the slight deviation in the geometry, there will also be a slight deviation in the recovered area from where the hole was originally located. This deviation flattens out to the original geometry of the floor/wall/ceiling described by the remaining edge of the hole.

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/ErraticUsableEdges.png}
    \caption{The areas marked in \textcolor{red}{red} contain parts of a painting that will be cropped together with the cropping of another adjacent object defined by a \textcolor{green}{green} boundingbox. As a result, the edges of the holes will show a slight deviation in geometry at the location of these paintings.}
    \label{fig:ErraticUsableEdges}
\end{figure}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/ErraticUsableEdgesFilled.png}
    \caption{(a) The cut-out edge of the desk and chair from Figure \ref{fig:ErraticUsableEdges}, with a part of the painting in the top right corner. (b) The correctly repaired hole with visible traces of a geometry difference in the edge that flattens out through the new geometry.}
    \label{fig:ErraticUsableEdgesFilled}
\end{figure}

\subsection{Order and combinations of detection}

The previous two chapters have shown that objects that are in the close proximity of each other leads to poor or even incorrect repair of the holes of cut away objects. Possible solutions to this problem will be explored. In the chapter Erratic and unusable edges of objects, possible solutions were given. These include reducing the size of the cut edge or manually repairing the edge in a separate program, by removing the unwanted parts of the mesh. 

However, before considering these options, another principle can be used to deal with this problem. It can be decided not to remove all the furniture at once. An analysis can be made beforehand of possible sensitive and faulty areas. In the example used in the previous two chapters, it can be decided in a first phase to remove the paintings on the wall. In a second phase, it can be decided to remove the objects whose boundingboxes cut the ones of the paintings and thus received a less accurate repair (Figure \ref{fig:ItterationRepair}). This is because the part of the edge that's part of the painting has a deviation from the original geometry of the floor or wall it is situated in. Note that with this method the process will take longer again, and after an iteration the recovered scene will have to be loaded as an input dataset at the beginning.

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/IteerationRepair.png}
    \caption{Iterative process to remove objects. First, the painting is removed after which the desk is removed.}
    \label{fig:ItterationRepair}
\end{figure}

\subsubsection{Limitation due to the use of PyMeshFix}

This iterative process raises another issue that needs to be considered. At first glance, an iterative process seems obvious and easy to implement. The use of PyMeshFix is considered to be a good method of fixing the holes in the correct way. However, the algorithm turns out to be less accurate than first thought. When filling the hole where the painting was situated, faces are created between vertices belonging to the inner and outer boundary. This new geometry overlaps the existing geometry of the edge of the hole in some places. In a subsequent phase, when the intention is to remove the desk, there is a bubble layer of faces on a part of the edge where the painting originally was. This bubble layer poses a problem for PyMeshFix as the algorithm no longer considers the edge to be closed and therefore does not fill the holes. Figure \ref{fig:PymeshfixGeometry} shows the new geometry at the restored hole in the painting. It can be seen that some of the new geometry touches the outer edge. Figure \ref{fig:DubbleFaces} shows the edge of the cut-out desk. At first glance there appears to be nothing wrong with it. However, when one of the faces is moved, the layer of faces underneath the moved layer becomes visible. This double layer must be removed manually to achieve the result shown in Figure \ref{fig:ItterationRepair}. This problem doesn't occur in any of the other possible connections the algorithm is tested on. 

\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.5\linewidth]{fig/PymeshfixGeometry.png}
    \caption{Restored hole with PyMeshFix where faces filling the hole do not stop at inner boundary but continue to outer, resulting in double faces along the edge.}
    \label{fig:PymeshfixGeometry}
\end{figure}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.8\linewidth]{fig/DubbleFaces.png}
    \caption{Double layer of faces along the edge disrupting the hole refill pores.}
    \label{fig:DubbleFaces}
\end{figure}

Another possibility is to cut out certain objects, which are close to each other together in one boundingbox. This means that these two objects are considered as one object in the object recognition phase and  are cut out by one boundingbox that's larger. In previous examples, the overlapping of the boundingboxes was already the case with the desk and the office chair. They are cut out together in one larger boundingbox and so there is less chance of errors around the edge of the two objects. However, this increases the size of the data being cut out at once, making the hole much larger and potentially making the hole repair less accurate. In extreme cases with a dens furnished room, each time objects are considered together, the cut-out geometry may be so large that the hole cannot be repaired. The example in Figure \ref{fig:CutMultiple}, shows that the desk and the painting are detected together to avoid problems such as a less accurate repair or double faces in the edge. However, when considering these two objects together, it is clear that there is again a problem in that a third object is too close to the boundingbox and, as a result, is included in the edge. This results in incorrect plane detection by RANSAC. Again, another solution is possible. PyVista's "extract largest" function could be used to remove the loose piece of the third object visible in Figure \ref{fig:CutMultiple}. However, due to the presence of the loose vertices, it is possible that this function will also remove part of the edge (discussed in Chapter \ref{Non-connected vertices in original data}). So here it becomes clear that the problem of unconnected edges creates problems well into this study, which otherwise, had this problem not been there, would not be a problem for the rest of the algorithm. 
A better solution would be to cut out overlapping boundingboxes together. This way less geometry would be lost in the object removal phase. however this falls outside the scope of this study.

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/CutMultiple.png}
    \caption{The removal of the desk and the painting together creates another obstacle through a third object in the boundingbox.}
    \label{fig:CutMultiple}
\end{figure}

\subsection{Difficult connections}

In this study, three different possible scenarios for hole repair were discussed. These scenarios: (1) a flat surface, (2) a joint between two surfaces, (3) a joint between three surfaces are the most common. However, it is always possible that other scenarios may occur that have not been addressed in this study. It is therefore impossible for the algorithm to repair the holes correctly where they occur. A manual approach is therefore required.

\subsection{Repairing holes with manually repaired edges}

Despite the difficulties discussed above, when cutting out objects with no other objects in a \(\Delta\)\(\sigma\) environment, the edge can easily be restored manually in Blender\textsuperscript{\textregistered}. This takes a little more time, but gives a good result. Figure 2.3 shows the scene with furniture removed, where the problems were repaired manually. The result is a well-recovered dataset that preserves the geometry of the room and is rendered without the selected objects.

\subsection{Results of the removal of objects and completion of the geometry}

As mentioned before the algorithm is able to fill three types of holes. With the previously mentioned limitations in mind the following figures show the newly added faces by the algorithm for each of these types of holes. Each one of these types are filled. Two out of the three possible types that are discussed in this study result in the desired outcome. By this is meant that the hole is filled correctly also considering that there are no unwanted creations of new geometry. This problem of unwanted faces that connect the newly added geometry to the outer boundary of the existing geometry was discussed earlier.
\ref{fig:RepairedDatasetResults} gives an overview of the total fixed scene where some selected objects were removed.

\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.5\linewidth]{fig/PymeshfixGeometry.png}
    \caption{Restored hole in wall.}
    \label{fig:wall}
\end{figure}
\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.5\linewidth]{fig/VM.png}
    \caption{Restored hole for connection between the wall and floor.}
    \label{fig:Floor-Wall}
\end{figure}
\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.5\linewidth]{fig/V2M.png}
    \caption{Restored hole for connection between the floor and two walls.}
    \label{fig:Floor-2Walls}
\end{figure}
\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.6\linewidth]{fig/RepairedDataset.png}
    \caption{Recovered dataset where all object belonging to a particular scenario was removed.}
    \label{fig:RepairedDatasetResults}
\end{figure}
\section{Texture reconstruction} \label{H4:Texture reconstruction}

\subsection{Unwrapping the mesh to UV map}

As discussed in \ref{Assign texture to repaired scene}, the difficulty of texture reconstruction was underestimated. As an alternative method of skipping the automation process of reassigning the texture to the mesh by assigning UV coordinates to each vertex, the bake function in Blender\textsuperscript{\textregistered} is substituted. 

\subsubsection{Unwrapping the fixed dataset}

For the first steps, following the procedure in \ref{Baking texture using Blender}, the seams of the repaired scene are marked and the mesh is unwrapped to obtain the uv map. The UV map of the repaired scene in Figure \ref{fig:WrongUVMap} shows that there are errors in the mesh. The unwrap process includes an error message that certain islands from the dataset cannot be unwrapped, resulting in very large triangles that overlap much of the rest of the uv map. When the texture is baked, these triangles will not be assigned a texture and the parts of the uv map that are overlapped by these triangles will not be textured either. The result is shown in Figure \ref{fig:WrongTexured}, where only part of the dataset is re-textured.

\vspace{5mm}

The analysis of the dataset results in the occurrence of overlapping faces, which causes this error message when parts of the mesh cannot be unwrapped. As in \ref{fig:DubbleFaces}, PyMeshFix fills the gap with new geometry that runs to the outer edge in addition to the inner edge, which was not anticipated. As a result, there are overlapping faces wherever there were objects before. 

Also, by adding the fixed geometries in the cropped dataset, due to the malfunctioning of PyVista's merge function, there are duplicate vertices along the boundary, which also cause a false unfolding of the mesh.

\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.5\linewidth]{fig/WrongUVMap.png}
    \caption{Incorrect unwrapped uv map of the dataset due to the occurrence of duplicate faces. These errors result in large triangles overlapping much of the UV map.}
    \label{fig:WrongUVMap}
\end{figure}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/WrongTexured.png}
    \caption{Incorrectly re-textured datasets, where areas overlapped by large triangles in the UV map are also not textured, resulting in grey areas.}
    \label{fig:WrongTexured}
\end{figure}

\subsubsection{Unwrapping the manually fixed dataset}

To test the baking and reconstruction of the texture anyway, the problems were solved manually in Blender\textsuperscript{\textregistered}. All duplicate faces and vertices were removed. Thus, it can be determined whether the used technique works if no errors are present in the repaired mesh. 

\vspace{5mm}

Figure \ref{fig:UndubbledFaces} shows the difference between the manually recovered mesh and the mesh filled by the hole filling algorithm. All duplicate faces and vertices have been removed so that there are no errors in the UV map when the dataset is unwrapped (Figure \ref{fig:Uvmapping}). In contrast to \ref{fig:WrongUVMap}, all coarse triangles have been removed. As a result, when the texture is baked, there are no more errors and the expected result is obtained, as shown in Figure \ref{fig:RightTexured}.

\vspace{5mm}

Where original objects are present, the texture of the original scene is projected onto the flat recovered geometry of the holes. The texture image can now be used in the inpainting algorithm to restore and correct the texture in these locations. 

\begin{figure}[!ht]
    \centering
    \includegraphics[width=0.5\linewidth]{fig/UndubbledFaces.png}
    \caption{Removed double faces and edges at the location of the edges of the repaired holes.}
    \label{fig:UndubbledFaces}
\end{figure}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/Uvmapping.png}
    \caption{Left: Repaired scene with \textcolor{red}{seams} indicated. Right: UV map of the unwrapped scene.}
    \label{fig:Uvmapping}
\end{figure}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/RightTexured.png}
    \caption{Correctly textured scene with original texture projected onto new geometry of repaired holes.}
    \label{fig:RightTexured}
\end{figure}

\subsection{Inpainting the texture image from the re-textured repaired dataset}

Before the inpainting algorithm is run, white masks are applied to the areas where objects were located in the texture image, as shown in Figure \ref{fig:InpaintingMask}. Once these masks are applied and the rest of the image is blackened, the algorithm can be run. The made faces are filtered from the texture image and then painted using the OpenCV - Telea algorithm \cite{Telea2004}. Figure \ref{fig:BA_Inpainting} shows the results. It can be seen that the cabinet in the corner, the seats, the desk and the paintings on the walls have been successfully painted. The result is not completely correct in places where the holes were very large, and slight texture variations can still be seen in these places. 

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/InpaintingMask.png}
    \caption{\textbf{Left:} Places where the texture of objects should be masked, indicated in \textcolor{red}{red}, \textbf{Right:} Applied masks in blackened texture image.}
    \label{fig:InpaintingMask}
\end{figure}

\begin{figure}[!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/BA_Inpainting.png}
    \caption{\textbf{Left:} Errors in textured mesh at the locations where objects were previously located, \textbf{Right:} Re-textured mesh with inpainted texture at locations previously occupied by objects.}
    \label{fig:BA_Inpainting}
\end{figure}

\section{Completely cleaned scene} \label{H4:Completely cleaned scene}

Ideally, all furniture will be successfully removed from the scene. Figure \ref{fig:TotalFix} shows an example of the final result when all objects are removed from the dataset. It can be seen that the reconstruction of the texture using OpenCV's inpainting algorithm is not perfect and leaves visible traces. Nevertheless, the final result seems to be generally successful. Admittedly, with the necessary manual input to correct errors in places where cutting out the furniture causes errors in the algorithm.

\begin{figure} [!ht]
    \centering
    \includegraphics[width=\linewidth]{fig/TotalFix.png}
    \caption{Representation of a fully cleared dataset}
    \label{fig:TotalFix}
\end{figure}

\cleardoublepage

%=================================================================
\documentclass[remotesensing,article,submit,moreauthors,pdftex]{Definitions/mdpi} 

%=================================================================
\firstpage{1} 
\makeatletter 
\setcounter{page}{\@firstpage} 
\makeatother
\pubvolume{xx}
\issuenum{1}
\articlenumber{5}
\pubyear{2021}
\copyrightyear{2021}
%\externaleditor{Academic Editor: name}
\history{Received: date; Accepted: date; Published: date}
%\updates{yes} % If there is an update available, un-comment this line

\usepackage{graphicx}
\usepackage{amssymb}
\usepackage{pdfpages}
\usepackage{lineno}
\usepackage{hyperref}
\usepackage{mathtools}
\usepackage{booktabs}
\usepackage{kpfonts}
\usepackage{algpseudocode}
\usepackage{algorithm}
\usepackage{gensymb}
\usepackage{subcaption}
\usepackage{todonotes}
\usepackage{soul}
\usepackage{booktabs}

\usepackage{placeins}
\usepackage{setspace}
\usepackage{geometry} % added 27-02-2014 Markus Englich
\usepackage{epstopdf}
\usepackage{breqn}
\usepackage{dblfloatfix}
\usepackage{url}
\usepackage{multirow}
\usepackage{textgreek}
\usepackage[T1]{fontenc}
\usepackage{lmodern}
\usepackage{lscape}

\usepackage{pgfplots}
\usepackage{rotating}
% \usepackage{makecell}
\usepackage{tabu}

% \usepackage{multirow}
% \usepackage[utf8]{inputenc}
% \newcolumntype{C}[1]{>{\centering\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}


%\DeclareMathOperator*{\min}{min} 
%\DeclareMathOperator*{\max}{max} 
\DeclareMathOperator*{\argmin}{argmin} 
\DeclareMathOperator*{\argmax}{argmax} 

\newcolumntype{L}[1]{>{\raggedright\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}
\newcolumntype{C}[1]{>{\centering\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}
\newcolumntype{R}[1]{>{\raggedleft\let\newline\\\arraybackslash\hspace{0pt}}m{#1}}


%=================================================================
% Full title of the paper (Capitalized)
\Title{GEOMAPI: A 3D data processing library with semantic web technologies}

% Author Orchid ID: enter ID or remove command
\newcommand{\orcidauthorA}{0000-0002-5231-2853} % Add \orcidA{} behind the author's name
\newcommand{\orcidauthorB}{0000-0002-7809-9798} % Add \orcidA{} behind the author's name
\newcommand{\orcidauthorC}{0000-0003-4894-6965} % Add \orcidA{} behind the author's name
\newcommand{\orcidauthorD}{0000-0001-8526-8847} % Add \orcidB{} behind the author's name
\newcommand{\orcidauthorE}{0000-0003-3465-9033} % Add \orcidB{} behind the author's name
\newcommand{\orcidauthorF}{0000-0001-6368-4399} % Add \orcidB{} behind the author's name

% Authors, for the paper (add full first names)
\Author{Maarten Bassier $^{1,*}$\orcidD{}, Jelle Vermandere$^{1}$\orcidB{}, Sam De Geyter$^{1,2}$\orcidA{}, Heinder De Winter$^{1,3}$\orcidC{} and Maarten Vergauwen $^{1}$\orcidE{}}

% Authors, for metadata in PDF
\AuthorNames{Maarten Bassier, Jelle Vermandere, Sam De Geyter,  Heinder De Winter and Maarten Vergauwen}

% Affiliations / Addresses (Add [1] after \address if there is only one affiliation.)
\address{
$^{1}$ \quad Dept. of Civil Engineering, TC Construction - Geomatics, KU Leuven - Faculty of Engineering Technology, Ghent, Belgium \\ (maarten.bassier, jelle.vermandere, sam.degeyter, heinder.dewinter, maarten.vergauwen)@kuleuven.be\\

$^{2}$ \quad MEET HET BV, Mariakerke, Belgium\\

$^{3}$ \quad DIRK BAUWENS NV, Evergem, Belgium\\
}
% Contact information of the corresponding author
\corres{Correspondence: maarten.bassier@kuleuven.be}

% Current address and/or shared authorship
%\firstnote{Current address: Affiliation 3} 
\secondnote{The authors contributed equally to this work.}

\abstract{ The AEC industry still struggles with processing and incorporating remote sensing data. Whether remote sensing is used for digital twinning existing assets, monitoring the construction of new assets or conducting maintenance, the vast quantity of unintelligent remote sensing data is a key bottleneck. Current state-of-the-art methods rely on performant proprietary software to render and evaluate remote sensing data. While this is superb for isolated analyses by individual stakeholders, the overall framework to manage remote sensing data during a construction's life-cyle is missing. In this work, an innovative toolbox is presented that bridges the gap between various close-range sensing resources for AEC industry tasks. More specifically, we combine semantic web technologies with state-of-the-art open source geomatics APIs to process and analyse big data remote sensing repositories in various construction applications. Our novel GEOMAPI Python Package and ontology are fully documented online along with the testcases that showcase the potential of this technology. Overall, the innovation and detailed documentation of our new GEOMAPI Package will allow construction stakeholders to better access remote sensing data, calculate complex geomatics tasks and store multi-temporal analyses results.}

% Keywords
\keyword{Geomatics; Semantic Web Technologies; Construction; Remote sensing; BIM; Point clouds; Photogrammetry}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\begin{document}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Introduction}
\label{Introduction}
With the rise of increasingly faster and cheaper remote sensing data acquisition tools, the demand for construction digitization is skyrocketing. Never before have existing assets been captured in such high detail and point density. Every year, numerous laser scanning and photogrammetric projects are issued of construction sites and existing constructions from roads and tunnels to bridges and buildings~\cite{McKinseyGlobalInstitute2017}. The amount of captured data is astonishing. The data captured in a single documentation procedure of an average two-storey building of 30,000 m² with Lidar technologies can easily amount to over 200 scans (>40GB of point cloud data) or some 10,000 pictures in a photogrammetric pipeline, resulting in a point cloud of some 100 million points~\cite{DeGeyter2022}. The processing of this data has gotten so computationally demanding so that nearly every type of processing and analysis requires unique highly-performant commercial software. There currently is a massive gap between the very accessible remote sensing documentation tools and the various needs of construction digitization processes~\cite{Khallaf2021}. The first key issue is the lack of generalization and performance to run different types of analyses on this data and store results in a standardized manner.  

On top of this, most constructions are re-documented during their life-cycle i.e. for renovations, facility management, project planning, etc.~\cite{Volk2014,Wang2019d}. This is especially true for construction sites that demand periodic digitization to asses the progress and quality on site~\cite{Golparvar-Fard2015}. For instance, to asses site progress based on an as-design Building Information Model (BIM), one needs to evaluate the presence of objects from repeated measurements as the works progress. It is common practice to completely redocument the existing situation and discard any prior measurements. This significantly lowers the lifespan of the remote sensing data which is extremely wasteful since most of the asset documentation is still valid. Additionally, this  negatively impacts the detection rate of any analyses that benefits from an information buildup in consecutive measurement epochs. A second key issue is therefore the limited longevity and the lack of multi-temporal possibilities for the analysis of remote sensing data.   

The goal of this work is to address both these key issues using semantic web technologies. In this research, we present the fundaments of GEOMAPI, a novel Python Package which builds upon state-of-the-art geomatic and semantic web technologies APIs. GEOMAPI provides highly-performant functionality to process and analyse remote sensing data such as point clouds, polygonal meshes, geolated images and orthomosaics (Figure~\ref{fig:remote sensing}). Additionally, the core innovation of GEOMAPI is to manage the geospatial metadata of these assets and to serialize and consume this metadata to leverage multi-temporal and multi-source geomatics analyses for the construction industry. In summary, the works main contributions are:

\begin{enumerate}
	\item Literature study on remote sensing processing for the construction industry.
	\item The theoretical design of GEOMAPI core and functionalities.
	\item Fully documented Open source API of GEOMAPI with tutorials and examples. 
	\item Four in-depth empirical testcases on the use of GEOMAPI for construction applications. 
\end{enumerate} 

The remainder of this work is structured as follows. The background and related work is presented in Section~\ref{sec:Background_Related work}. In Section~\ref{sec:Methodology}, the GEOMAPI methodology and core functionality is presented. In section~\ref{sec:Testcases}, the four test sites are introduced along with their corresponding results. The API pros and cons are discussed in Section~\ref{sec:Discussion}. Finally, the conclusions and future work are presented in Section~\ref{sec:Conclusions}.

% %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Background \& Related work}
\label{sec:Background_Related work}
In this section, the related work for the key aspects of this research are discussed: (1) the use and needs of remote sensing data in the construction industry, (2) state-of-the-art multi-source and multi-temporal processes (3) semantic web technology initiatives in the construction industry.

\subsection{Remote sensing in the construction industry}
In an attempt to optimize construction and maintenance operations, the construction industry is increasingly investing in asset digitization~\cite{Coupry2021}. Depending on the type of asset and the application, a range of Lidar and photogrammetric remote sensing techniques have become sufficiently mature to provide reliable remote sensing data~\cite{Liu2021,Rebolj2017} (Figure~\ref{fig:remote sensing}). However, given the domain, there is a clear preference either for photogrammetric approaches, which yield highly-detailed textures, or Lidar approaches that have superb metric accuracy~\cite{Chen2018a}. Considering the domain layers of the IFC kernel, one can make the following distinctions (Figure~\ref{fig:ifc_schema})~\cite{IFC4}. In terms of the structural elements domain, Lidar is preferred in most cases during or post construction due to the low texture variance of these assets~\cite{Bassier2019,Xue2019}. This also applies to structure related elements in the architecture domain such as walls, ceilings, beams and columns~\cite{Bassier2020Scan2BIM}. For the remainder of the architecture domain, a mix of both methods yields the highest detection rate for mid-sized objects such as doors and ceilings~\cite{Mahami2019}. For small scale-objects, photogrammetric approaches are preferred because of their high detailing and texture documentation. The HVAC and plumbing domains contain highly occluded objects that also rely on both methods. Pipe and installation dimensions and locations are best determined from Lidar~\cite{Bosche2013}, but the increased coverage and detailing from photogrammetric techniques is more than welcome to identify different components~\cite{Kim2020a}. For small objects, such as architectural ornaments or electrical equipment in the Electrical domain, there is a clear preference for photogrammetry where computer vision is a must-have to detect these assets~\cite{Hamledari2017}. There are of course alternative techniques such a structured light methods which offer a very competitive solution for small-scale scenes. However, none of these methods can be deployed on the scale that is required for asset digitization. 

\begin{figure*}[h!]
  \centering
  \begin{subfigure}[]{0.64\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/week22_img.png}
        \caption{Geolocated imagery}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
    \begin{subfigure}[]{0.35\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/Vlucht 101_0360 Putdeksel.png}
        \caption{Orthomosaic}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
    \\
  \begin{subfigure}[]{0.57\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/week22_pcd.png}
        \caption{Point cloud data}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
    \begin{subfigure}[]{0.41\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/NewGeometry.png}
        \caption{Polygonal meshes}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
  \caption{Overview of the documentation approaches for construction domains.}
  \label{fig:remote sensing}
\end{figure*}

\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/IFC4_layered_architecture - Copy.png}
  \caption{Overview of IFC domain layer at the top of the layered IFC architecture~\cite{IFC4}. }
  \label{fig:ifc_schema}
\end{figure*}

Overall, there is a plethora of works describing data acquisition metrics for construction applications, showcasing that remote sensing is in fact a valid approach to document an asset~\cite{Albeaino2019,Braun2018}. However, these works overlook the fact that the production of this big data is only a first step, and that there are many processing steps that applications have to implement to consume the vast amounts of point and pixel information to extract the desired outputs. The result is that most remote sensing data are only used once, and typically only by the party that acquires the data or conducts the analysis. For one of the industries most promising, versatile data repositories, there is an immense waste of digital assets such as remote sensing data. The key issue is that the industry lacks the tools to properly manage remote sensing data during a construction's life-cycle~\cite{Liu2021}. 

There is for instance the application of building construction monitoring and it hemorrhaging failure costs, which is also one of the testcases in this work~\cite{Love2018}. Remote sensing has been proposed as the baseline to track progress, quality and stockpiles on site~\cite{Maalek2019}. There are several promising approaches ranging from UAV and body-cam videogrammetry to Simultaneous Localisation and Mapping (SLAM) mobile laser scanning approaches~\cite{Lehtola2017,Han2017}. Several works report accuracies of LOA20 $[\sigma\leq0.05m]$ and even LOA30 $[\sigma\leq0.015m]$ \cite{U.S.InstituteofBuildingDocumentation2016} for the evaluation of structural elements all the way to MEP upon installation~\cite{Kropp2018}. However, no reports are found where consecutive datasets were integrated for a joint analysis, nor where Lidar and photogrammetric outputs where used together in such an application despite the obvious benefits.

\subsection{multi-modal analyses}
%As stated above, there currently is a gap in the literature in data fusion of consecutive construction documentations and the joint and temporal analysis of Lidar and photogrammetry for construciton applications. Instead, we look at related disciplines for the state of the art in these technologies. 

This work presents a framework to manage remote sensing data from multiple sources. As such, we must consider the type of data fusions and multi-modal frameworks that are the subject of ongoing research. Overall, this is a very novel field in construction digitization that has its roots in Earth observation (EO). In recent years, much research in EO has focused on the ability to comprehensively analyze and interpret strongly heterogeneous data such as multi-and hyperspectral data in combination with inSar, Lidar and RGB data~\cite{li2022}. 

This data fusion is now transitioning to more close-range remote sensing, especially for semantic segmentation procedures~\cite{Grilli2017}. Until recently, semantic segmentation operated separately on an image-based (e.g. VGG16) or point cloud-based (e.g. UNET) backbones, and most semantic segmentation networks still do~\cite{Bello2020}. However, recent works have started exploring both late and early data fusion~\cite{Gadzicki2020}. In late fusion, only the results of separate classification methods on each dataset are fused such as in Braun et al.~\cite{Braun2019} e.g. merging a labelled set of images and point cloud. In contrast, early data fusion, either the input data itself or the extracted features are merged and fed to a single deep learning model, which is extremely promising~\cite{Guo2021}. For instance, the most popular data fusion technique includes projecting 2D pixel information onto 3D points or vice versa~\cite{Khallaf2021}. The most mentioned connection is between RGB imagery and point cloud data, although polygonal meshes and thermal imagery~\cite{Adan2017} fusion is also being pursued. The fusion of methods is also possible. For instance, Hackel et al.~\cite{Hackel2017b} combine point-wise classification with line-extraction to improve the detection rate of urban scene classification on the ISRPS Lidar benchmark dataset. However, similar to other approaches, the processing parameters, timestamp and metadata of the analyses are not made part of the result, which again limits its re-usability. 

This data, feature or method structuration framework -- early vs late fusion and the serialization of results and analyses parameters -- is exactly one of the key aspects that GEOMAPI intends to facilitate. This is indeed needed since nearly all presented methods focus on performance but fail to provide a methodology to actually store the intermediate steps and outputs to be used in parallel or downstream processes and without copying and multiplying the data many times over.

\subsection{multi-temporal analyses}
The most popular construction application for using consecutive datasets is change detection. Mostly performed on point cloud data, change detection evaluates the point distances between consecutive point clouds to asses changes in the scene~\cite{Mukupa2017}. However, most application limit themselves to the changes between only two datasets, and do not use these results to build upon in later analyses~\cite{Xu2015,Du2016,Meyer2022,Huang2022}. This is more than a simple software limitation (although processing hundreds of point clouds is utterly challenging). Instead, construction stakeholders struggle to organise their remote sensing data in a spatio-temporal data structure that is not plagued with excessive overlap, redundant measurements, single use analyses, and so on~\cite{Nikoohemat2018}. 

\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/time_series1.png}
  \caption{Overview of data cubes as defined in Simoes et al.~\cite{Simoes2021}. }
  \label{fig:data cubes}
\end{figure*}

A promising data structuration are data cubes (Figure~\ref{fig:data cubes}. Used in multiple fields, data cubes are massive multi-dimensional (nD) arrays of values that represent target data along a set of dimensions of interest. For instance, in the field of Earth observations, where data structuration is a known issue, data cubes are defined as collections of spatio-temporal attributes~\cite{Simoes2021} i.e. a fixed set of coordinates for which the (multi-)spectral bandwidths are captured in time-series e.g. of a year. In the construction industry, the most prominent application is building performance monitoring from a set of IoT sensors~\cite{Leprince2021}. Overall, the transfer of file-based data structure to attribute-based data cubes is particularly interesting to fuse multi-modal and multi-temporal data. However, where data cubes typically require a rigid data definition, GEOMAPI will leverage semantic web technologies to achieve a similar metadata structuration for close-range remote sensing that can deal with any input. 

\subsection{semantic web technology initiatives}
Semantic Web Technologies and Linked Data in particular are a set of design principles to share machine-readable interlinked data assets. While mostly used for online resources, the same principles can be used to organise data in local networks. At the core of this technology is the Resource Description Framework (RDF)\footnote{\url{https://www.w3.org/TR/rdf11-concepts/}}, a data model that represents a directed graph of resources or nodes. Each resource is defined by a unique subject, i.e. a Uniform Resource Identifier (URI), and a set of triples. These triples each consist of the subject, a predicate (relationship or attribute) and a object or property value. Objects can be typical variables i.e. floats, string or more complex classes as defined in the RDFS\footnote{\url{https://www.w3.org/TR/rdf-schema/}} (RDF Schema) and OWL\footnote{\url{https://www.w3.org/TR/owl2-overview/}} (Web Ontology Language) framework. These standards also provide the terminology to conceptually describe a certain domain of interest by defining classes, properties and their formal logics. The result of such a description is called a web ontology or vocabulary (terminology layer or TBox). The immense strength of this technology is the wide variety and flexibility of the ontologies that allow the description of nearly every resource in a standardised manner. As such, the heterogeneous construction and remote sensing data can be described in a common language that can be efficiently queried~\cite{Bassier2020a}. 

There are already several initiatives in the construction industry to harness this Linked Building Data. For instance, Beetz et al. translated the rigid IFC exchange datastructure of BIM into a web ontology referred to as ifcOWL~\cite{Beetz2009a}. In~\cite{Niknam2017}, the BIM Shared Ontology (BIMSO) and the BIM Design Ontology (BIMDO) are presented to describe an as-design building. The BRICK ontology is developed to support the modeling of operational building systems, including sensors, controls and actuators~\cite{Balaji2018}. In road construction, The Flemisch road agency AWV has created its own Open Source AWV-OTL initiative ontology to manage road assets. In terms of buildings, most ontologies deal with the design or performance of the building itself and the exchange between stakeholders and do not include remote sensing concepts ~\cite{Rasmussen2018,Pauwels2017}. Instead, there are dedicated geometry and photogrammetry ontologies such as Arpenteur~\cite{Ellefi2018}. Related to 3D geometry annotation, there are the 3D Modeling Ontology\footnote{\url{http://web.archive.org/web/20180831114523/http://3dontology.org/3d.ttl}} (3DMO)~\cite{Sikos2017a} and our own VforDesign (V4D) ontology~\cite{Bassier2020a} for reuse of preexisting remote sensing data. It is this specific ontology we extend in this work to manage the metadata of the remote sensing inputs. 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/GEOMAPI_general.drawio (1).png}
  \caption{Overview of the GEOMAPI architecture with the conceptual layers for nodes (Nodes) and their corresponding APIs. }
  \label{fig:architecture}
\end{figure*}

\section{GEOMAPI technology}\label{sec:Methodology}
The GEOMAPI API represents a framework to exchange and analyse remote sensing data among various stakeholders in the Architecture, Engineering and Construction (AEC) industry. The framework contains the schema and definitions to operate remote sensing data (images, point clouds, polygonal meshes) as linked data resources, represented as RDF graphs (Figure~\ref{fig:architecture}). In addition to this scientific description of the API, we also strongly recommend readers to consult public documentation of GEOMAPI which is available at our wiki~\footnote{\url{https://geomatics.pages.gitlab.kuleuven.be/research-projects/geomapi/}}. All the technologies and testcases discussed below are very well documented in the wiki to maximise reproducability of the code. Overall, the GEOMAPI architecture is organised into four main layers (Figure~\ref{fig:architecture}).
\begin{enumerate}
	\item \textbf{Utilities}: The core of geomapi builds upon highly-performant Open Source APIs including Open3D~\cite{Zhou2018a}, OpenCV, IfcOpenshell and RDFlib to consume multi-temporal and multi-source remote sensing and construction data.  
 	\item  \textbf{Nodes}: The node definitions are a practical implementation of a resources data and metadata. It has close ties to the RDF serialization and consists of premade data cubes and methods to manage frequently used remote sensing data. 
	\item \textbf{V4D}: The v4d ontology is the web ontology that defines the RDF concepts for the Nodes and their relationships. It builds upon existing ontologies wherever possible and governs the metadata of the Nodes. Additionally, it houses the concepts to store any analyses results and process parameters so they can be used across multiple applications.
	\item \textbf{Tools}: The tools and applications consume the above RDF graphs and Node data cubes to efficiently conduct multi-temporal and multi-modal analyses.
\end{enumerate} 

Note that each layer consumes the above layers as a dependency i.e. the applications are the highest layer that consume both the Utilities, Node and V4D layers. Cross dependencies within the same layer are avoided as much as possible. As such, GeometryUtils can be adopted for an unrelated API without having to import the entire GEOMAPI API as a dependency. 

To increase the method’s readability, the following terms and concepts are used in GEOMAPI. The API uses plain English and the data items have the following naming convention. 

\begin{enumerate}
	\item \textbf{Instance and class attributes} are denoted as CamelCase starting with the first letter as a lowercase i.e. \textit{name}, \textit{pointCount}.
 	\item  \textbf{Functions} are typed as lowercase words with underscores i.e. \textit{get_graph_subject()}.  
	\item \textbf{Class names} are written as CamelCase starting with an uppercase letter i.e. \textit{MeshNode} or \textit{PointCloudNode} 
	\item \textbf{Function variables} are denoted as CamelCase starting with the first letter as a lowercase i.e. \textit{predicate} and \textit{excludedList}.
\end{enumerate} 

\subsection{Scope \& Dependencies}
GEOMAPI is constructed as a general remote sensing processing library for construction applications. As such, the API can be deployed for any task involving optical remote sensing data during the life-cycle of built assets. This includes documentation of the built environment for the planning of new projects, the monitoring of construction sites (both of buildings and infrastructure) and the capture and updating of facility documentation with or without linking it to the present Building Information Models. In general, GEOMAPI builds upon existing concepts and libraries wherever possible. Currently, GEOMAPI builds upon the following Open Source APIs. 

\begin{enumerate}
    \item \textbf{OPEN3D} ~\footnote{\url{http://www.open3d.org/}}: Open3D is an open-source software library that specializes in point cloud and mesh manipulation. As such, the geometry-based resources such as point clouds, polygonal meshes and bounding boxes build upon Open3D concepts. 
    \item \textbf{OPENCV} ~\footnote{\url{https://opencv.org/}}: OpenCV is an open-source software library that specializes in image manipulation. As such, the image-based resources and methods for imagery, orthomosaics and texture operations build upon OpenCV concepts. 
    \item \textbf{IfcOpenShell} ~\footnote{\url{http://ifcopenshell.org/}}: IfcOpenShell is an open-source software library that specializes in IFC manipulation. As such, it is the base for manipulation of BIM elements within GEOMAPI.
    \item \textbf{RDFlib} ~\footnote{\url{https://rdflib.readthedocs.io/}}: RDFlib is an open-source software library that specializes in RDF Graph manipulation. As such, the parsing and serialization of the GEOMAPI concepts conform the v4d and related ontologies is handled by RDFlib functionalities.  
    \item \textbf{TensorFlow}~\footnote{\url{https://www.tensorflow.org/}}: TensorFlow is an open-source software library that specializes in machine learning tasks. As such, it is the base for many of GEOMAPIs machine learning applications in the tools layer. 
\end{enumerate} 

In addition to the above libraries, GEOMAPI builds upon a number of open source libraries for smaller tasks from documentation to geometry and image manipulation. The complete overview of the dependencies can be found in the documentation.

\subsection{Utilities}
There currently are four modules in the GEOMAPI utilities, divided into independent containers including geometry, image, geospatial and linked data processing (Figure~\ref{fig:utils}). Each module is fully documented in the wiki and extensively tested which is also made part of the compilation process.    

\begin{figure*}[h!]
  \centering
  \begin{subfigure}[]{0.52\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/crop_geometry1.png}
        \caption{Geometry utilities module.}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
  \begin{subfigure}[]{0.43\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/generate_virtual_image1.png}
        \caption{Image utilities module.}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
  \caption{Overview of the utilities modules with some example functions.}
  \label{fig:utils}
\end{figure*}

\paragraph{utilities}
This module contains base concepts that are used by other utility modules including IO functionality, search queries, conversion methods, and so on. While the thematic modules each are independent, they import the general utilities as a dependency and thus it should be copied as well if the thematic module is to be used in a standalone application.

\paragraph{GeometryUtils}
This module deals with all the geometry processing in GEOMAPI (Figure\ref{fig:utils} a). It extends the functionality of Open3D to deal with e57 point clouds, integrate multi-processing in point cloud handling, extend the functionality of Open3D.geometries such as PointCloud, OrientedBoundingBox, AxisAlignedBoundingBox, TriangleMesh and LineSet to (1) fit with the GEOMAPI metadata definitions and (2) perform construction application tasks such as visibility analysis, volume estimations, collision detection, and so on.

\paragraph{GeospatialUtils}
This module deals with the geospatial transformation of the assets. This includes both dealing with geographical coordinate systems (wGS84 and local coordinates), the conversions between coordinate systems and the offset rotations and translations of the resources. It also contains the IO functions to extract these metadata parameters from a range of different inputs such as XML, EXIF, etc.

\paragraph{ImageUtils}
This module extends the Open3D and OpenCV image processing functionalities (Figure\ref{fig:utils} b). It concretely focuses on the use of image resources in construction applications such as aligning and taking virtual images of BIM models, evaluating cracks through machine learning, change detection, and so on. The module forsees the processing of multiple image types including regular pinhole model cameras, panoramic imagery, thermal imagery, and orthomosaics.

\paragraph{LinkedDataUtils}
This module contains the functions to translate GEOMAPI's classes and attributes to RDF URIs and literals. Concretely, it calls upon the v4d ontology as well as RDFS, RDF, XSD and some other built-in ontologies in RDFlib to serialize the Python values. If an attribute is not present in v4d or another library, an ad-hoc v4d predicate is instantiated for the corresponding literal. Analogue, if a serialized object does not correspond to any of the predefined v4d or XSD types, it is returned as a string format. 

\subsection{Nodes}
GEOMAPI currently defines 9 nodetypes including 6 actual data classes, 1 combined session class and 2 supertypes that serve as templates for the Nodes. Their definitions are the following (Figure~\ref{fig:classes}). 


\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/GEOMAPI General.drawio.png}
  \caption{Overview of the GEOMAPI class diagram for the Node structure. }
  \label{fig:classes}
\end{figure*}

\paragraph{Node}
This is the archetype class for the other node classes. As such, this Node serves as a template for other Node Classes and should not be used unless the user wants to define an unknown resource. The Node class defines the core attributes of the node and the Graph functionality. It has a unique URI subject, name, graph and/or optional graphPath, timestamp, placeholders for the resource and its optional path and the cartesianTransform. The subject is identical to the graph subject and should not be altered unless upon creation as it has a significant deal of functionality tied to it. For instance, the subject complies with URI formatting rules and is stripped from all characters that would prevent it from being used as a filename in any OS. In contrast, no restrictions are placed upon the name of the Node, which consequently can be used to tie the Node to an instance within a resource e.g. the name of an IfcElement within an ifcFile. Note that a Node can be initialised from all the different inputs separately to maximise flexibility. For instance, one can initialise a set of nodes from some resources (e.g. polygonal meshes) or paths. If no subject is passed, a unique subject will be created for each node upon initialisation. Additionally, each property can be reconstructed from the present metadata with the appropriate get\_attribute() functions. For instance, if no name is given. get\_name() will attempt to reconstruct the name from the path or subject if present.  

\paragraph{GeometryNode}
This is the superclass for the currently four geometry classes that have some common properties i.e. PointCloudNode, MeshNode, BIMNode and LineSetNode. The GeometryNode class defines the orientedBoundingBox, orientedBounds, cartesianBounds attributes and the functionalities for the cartesianTransform (Figure~\ref{fig:GeometryNode}). These metadata are essential to manage remote sensing data as geospatial data cubes as discussed in the related works. They allow to query, evaluate, merge and transform geometries without actually importing the data which is significantly more efficient and accessible (Figure~\ref{fig:querry}).

\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/geometryNode1.png}
  \caption{Overview of the GeometryNode metadata including the orientedBoundingBox, orientedBounds, cartesianBounds  and the cartesianTransform. }
  \label{fig:GeometryNode}
\end{figure*}

\begin{figure*}[h!]
  \centering
  \begin{subfigure}[]{0.49\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/selection_BB_intersection2.png}
        \caption{Geospatial metadata cube}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
  \begin{subfigure}[]{0.465\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/selection_BB_mesh2.png}
        \caption{Resource segmentation}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
  \caption{Overview of a geospatial querry on the metadata of a set of Nodes that streamlines remote sensing processing. }
  \label{fig:querry}
\end{figure*}

\paragraph{PointCloudNode}
This class governs the data and metadata of a point cloud. The point cloud definition itself is the Open3D.geometry.PointCloud Class, which is a highly efficient data structure constructed from numpy arrays. GEOMAPI defines the metadata layer on top of this definition including the metadata extraction from E57, XML and PCD file formats. The RDF graph serialization only includes the metadata and not the actual point cloud as this would defy the purpose of the geospatial data cube. 

\paragraph{MeshNode}
This class governs the data and metadata of a polygonal mesh. The point cloud definition itself is the Open3D.geometry.TriangleMesh Class, which is a highly efficient data structure constructed from numpy arrays. GEOMAPI defines the metadata layer on top of this definition including the metadata extraction from OBJ and PLY file formats. The RDF graph serialization only includes the metadata and not the actual mesh as this would defy the purpose of the geospatial data cube. 

\paragraph{BIMNode}
This class governs the data and metadata of a BIM Element. This node is considerably different from both the above nodes as a BIM Element has significantly more metadata than a point cloud or a mesh. However, its metadata i.e. materials, properties and relationships typically only makes sense within the context of the BIM Model. The IFC datastructure or RDF variants i.e. IFCOWL which are very established definitions and also excel at governing this metadata, should therefore be preserved. GEOMAPI instead defines the geospatial metadata on top of the IFC datastructure without needlessly copying all of this metadata. 

As such, the BIMNode is defined as the link between an IFC file and the extracted geometry and geometric metadata of an IfcElement as described in the GeometryNode. The result is that the BIMNode has both a metric resource i.e. a TriangleMesh, and an IFC pointer to where its non-metric metadata is stored. The class relies on IFCOpenShell functionality to query the IFC properties and extends the Open3D.geometry.TriangleMesh functionality to allow for the geospatial metadata querying. The RDF graph serialization includes the geospatial metadata, the globalId, className and ifcPath. Additionally, the code foresees the buffering and storage of IFC geometries so not to defy the purpose of the geospatial data cube. 

\paragraph{LineSetNode}
This class governs the data and metadata of a (set of) polyline(s). This class is aimed at the processing of 2D or 2.5D plans, mostly in function of image processing. The vector data in the plans or edges extracted from imagery can than be defined as LineSetNodes which are useful for change detection or parameter extraction of construction objects. The LineSet definition itself is the Open3D.geometry.LineSet Class, which is a highly efficient data structure constructed from numpy arrays. GEOMAPI defines the metadata layer on top of this definition including the metadata extraction from DXF files. The RDF graph serialization only includes the metadata and not the actual linesets as this would defy the purpose of the geospatial data cube.

\paragraph{ImageNode}
This class governs the data and metadata of an image. The image definition itself is a numpy array conform OpenCV. GEOMAPI defines the metadata layer on top of this definition including the metadata extraction from EXIF, XML and XMP file formats. The XML and XMP files originate respectively from Agisoft Metashape\cite{Agisoft2018} and RealityCapture\cite{CapturingReality2017} and are the result of a structure-from-motion pipeline that computes the external en internal orientation for each image within a local or global coordinate system. This information, that is unique for both software, is standardised in GEOMAPI by defining common image properties such as the imageWidth and imageHeight, as well as the focallength35mm and the principal point information. Additionally, GEOMAPI extends the Open3D.geometry.Image functionalities to work in tandem with the OPENCV functions to maximise the class' usability. The geospatial components are defined as the cartesianTansform and a Open3D.geometry.TriangleMesh, which represents a geometric visibility cone defined by a field-of-view and the viewing depth (Figure~\ref{fig:ImageNode}). This geometry facilitates the joint geometric analysis of ImageNodes and other geometric Nodes. Analogue to the above Nodes, the RDF graph serialization only includes the metadata and not the actual image as this would defy the purpose of the geospatial data cube. 

\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/virtual_image2.png}
  \caption{Overview of the ImageNode data and metadata including the Open3D.geometry.TriangleMesh (red) as defined by the field-of-view of the image. }
  \label{fig:ImageNode}
\end{figure*}

\paragraph{OrthoNode}
This class governs the data and metadata of an orthomosaic (Figure~\ref{fig:orthomosaic}). This class is similar to the ImageNode but instead of an Open3D.geometry.TriangleMesh for the field-of-view, this class has more geometric metadata such as the cartesianBounds, orientedBounds (with a fictional height), orientedBoundingBox convex hull as geometry. Being an output of a SfM pipeline, the OrthoNode also serializes the relation to the input images via their URI subjects.

As such, it consumes a number of ImageNodes and is treated as a 2D geometric asset. Analogue to the metadata extracted for the ImageNode, GEOMAPI defines functions to extract the relevant parameters of the orthomosaics from both MetaShape and RealityCapture including the Ground Sampling Distance (GSD) and geospatial transform. The methods also closely align with those of the LineSetNode, which are also 2D or 2.5D in nature and treat the Z-coordinate as an optional component. The RDF graph serialization is identical to that of other Nodes.

\begin{figure*}[h!]
  \centering
  \begin{subfigure}[]{0.65\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/ortho1.png}
        \caption{Polygonal mesh}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
  \begin{subfigure}[]{0.33\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/Vlucht 101_0360 Putdeksel.png}
        \caption{Orthomosaic}\vspace{15pt}
        % \label{fig}
    \end{subfigure}
  \caption{Overview of the orthomosaic production in a UAV-based Structure-from-Motion pipeline. }
  \label{fig:orthomosaic}
\end{figure*}

\paragraph{SessionNode}
This class introduces the concept of a session, and functions as a fixed multi-source data cube. A session combines a set of Nodes and functions as the overarching node for the joint metadata. The most straightforward session is a measurement epoch. During a construction documentation, various resources are captured i.e. point clouds, spherical imagery, etc. A sessionNode can then be constructed which is comprised of the data cube of these different resources, each with their own Node class. These subject nodes, referred to as linkedNodes, are linked to the sessionNode using RDF relationships based on each Node's subject. Each of the sessionNode's attributes i.e. cartesianTransform, orientedBounds, and so on, is the function of its linkedNodes (Figure~\ref{fig:sessionNode}). The sessionNode also has its own resource, which is represented as the convex hull of the orientedBoundingBoxes of the linked nodes. This class functionality consumes that of other classes. Furthermore, it implements multi-processing to rapidly process its linkedNodes i.e. when data needs to be imported or compartmentalised. Analogue to other nodes, a sessionNode can purely be initiated from a graph. Three graph variants are currently supported to initialise a sessionNode: (1) the graph of the sessionNode itself that solely contains the triples of said sessionNode, (2) a graph containing a set of serialized linked nodes. Upon initialisation, the overarching sessionNode's metadata is extracted from these nodes, (3) a combined graph that houses both a sessionNode and linkedNodes. Upon initialisation, both the session and the linkedNodes properties are directly parsed from the graph. Overall, this sessionNode was initially designed to govern different measurement epochs but its functionality can be employed to define any data cube of Nodes including a multi-temporal collection from different measurement epochs.   
\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/session2.png}
  \caption{Overview of a sessionNode with two linkedNodes depicting (green) each node's orientedBoundingBox and the sessionNode's overarching orientedBoudingBox (red). }
  \label{fig:sessionNode}
\end{figure*}

\subsection{Ontology}
GEOMAPI is developed in close collaboration the the V4Design ontology, which in turn is developed for the V4Design European project that pursued the reuse of digital resources in the cadre of digital design and creative content production of real world assets. The complete ontology is described in our previous work \cite{Bassier2020a} and defines the concepts to describe structure-from-motion pipelines, mesh and point cloud processing, semantic enrichment processes including semantic segmentation and instance segmentation and how to link different resources. 

Concretely, during each step, metadata is stored of the respective process phases, i.e. the image preprocessing, SfM reconstruction, existing 3D model preprocessing and semantic enrichment. The relevant information for the data management or analyses applications are published as RDF using existing or new URIs for each of the defined nodes. The data is also exchanged between the different processes and for internal process analysis. Table~\ref{tab:v4d} contains the prefixes and namespaces of the applied ontology. The used terminology and modeling structures for building-related content are mainly coming from ontologies such as BOT, PRODUCT and OMG/ FOG/GOM and IFC. Regarding the process metadata, several ontologies are considered including the Openlabel ontology for navigational processing data, XCR for photogrammetric information, E57 for point cloud data and IFC for Building Information Modeling information. 


\begin{table}[h!]
	\centering
	\caption{Listing of the used existing ontologies and prototype V4D ontology modules.}
	\label{tab:v4d}
	%\resizebox{\linewidth}{!}{%
	\setlength{\tabcolsep}{20pt}
	\begin{tabular}{c|l}
		\hline
		Prefix &Namespace \\
		\hline 
		rdf     &\url{http://www.w3.org/1999/02/22-rdf-syntax-ns#}\\
		rdfs    &\url{http://www.w3.org/2000/01/rdf-schema#} \\
		owl   &\url{http://www.w3.org/2002/07/owl#} \\
		xsd &\url{http://www.w3.org/2001/XMLSchema#} \\
		cc      &\url{http://creativecommons.org/ns#} \\
		dct      &\url{http://purl.org/dc/terms/} \\
		prov &\url{http://www.w3.org/ns/prov#} \\
		geo &\url{http://www.opengis.net/ont/geosparql#} \\
		wd &\url{https://www.wikidata.org/entity/} \\
		dbr &\url{http://dbpedia.org/resource/} \\
		dbo &\url{http://dbpedia.org/ontology/} \\ 
		\hline 
		bot   &\url{https://w3id.org/bot#}  \\
		product & \url{https://w3id.org/product#} \\
		omg & \url{https://w3id.org/omg#}  \\
		fog & \url{https://w3id.org/fog#} \\
		gom & \url{https://w3id.org/gom#} \\ 
		\hline 
        ifc &\url{http://ifcowl.openbimstandards.org/IFC2X3_Final#} \\
        xcr &\url{http://www.w3.org/1999/02/22-rdf-syntax-ns#} \\
        e57 &\url{http://libe57.org#} \\
		exif &\url{http://www.w3.org/2003/12/exif/ns#} \\
		openlabel &\url{https://www.asam.net/openlabel} \\ \\
		\hline 
		v4d & \url{https://w3id.org/v4d/core#} \\
		\hline
	\end{tabular}%
	%}
\end{table} 

The terminology for the definition of the Nodes and their relationships are defined in the V4Dcore module. This includes both the vertical relationships i.e. hasLinkedNodes and the horizontal relationships isSimilarAs. Figure~\ref{fig:v4d} describes an example graph representation of a sessionNode with multiple ImageNodes that are bundled in a single photogrammetric reconstruction. The resulting structure-from-motion mesh and the camera locations are linked to the initial ImageNodes. Analogue, the ifcobjects are parsed from the ifcpath and converted to open3d TriangleMesh objects. The sessionNode's geometry is represented by the Convex hull that covers all the linked resources and is mainly used to manage the coordinate system of the group and to limit the search space. 

\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/ontology.png}
  \caption{Overview of the v4d ontology behind GEOMAPI. }
  \label{fig:v4d}
\end{figure*}

\subsection{Tools}
The GEOMAPI tools are the highest layer of the GEOMAPI core that each target a certain construction application. It is a set of high-level tools that consume both the GEOMAPI Nodes and Utilities and can be directly used in interactive notebooks, GUIs or scripts. GEOMAPI currently has four tool modules including one general module to import the data from multi-sensory platforms or formats, and 4 thematic modules.

\paragraph{tools} 
This is a general module that supports other tool modules. It contains functions that produce, parse or serialize multiple Nodes. For instance, it contains functions to parse an entire e57 file as a list of PointCloudNodes, to parse an entire XML file as a list of ImageNodes, parse a complex graph as a list of different Nodetypes and so on. Additionally, this module contains geometric evaluation tools to construct data cubes such as evaluating the inclusion of certain Nodes given a spot coordinate or timeframe. 

\paragraph{ProgressTools}
This is a thematic module to evaluate the progress of construction sites given various remote sensing inputs. It contains the functions to consume IFC files and different point cloud formats and compute
the Percentage-of-Completion (PoC) in consecutive measurement epochs. The functionality is demonstrated in the testcase in section~\ref{ProgressTools} and \url{https://geomatics.pages.gitlab.kuleuven.be/research-projects/geomapi/testcases/site_progress.html}.

\paragraph{ValidationTools}
This is a thematic module to evaluate the accuracy of as-design and as-built BIM from a range of point clouds or polygonal meshes. It contains the functions to associate remote sensing observations to BIM elements, compute
the Level-of-Accuracy (LOA) for the different elements, link tolerance spreadsheets to the analysis, etc.. Its functionality is demonstrated in the testcase in section~\ref{ValidationTools} and \url{https://geomatics.pages.gitlab.kuleuven.be/research-projects/geomapi/testcases/validationtools.html}.

\paragraph{CompletionTools}
This is a thematic module to update the existing documentation of a facility with newly captured remote sensing data. Using various remote sensing inputs includes imageNodes, MeshNodes and PointCloudNodes, this module contains the functions to align these nodes with pre-existing datasets and to update the meshes and point clouds of a facility without replacing or invalidating them. This is a crucial step in increasing the longevity of construction remote sensing resources which are currently discarded upon every new measurement. The functionality of the module is demonstrated in the testcase in section~\ref{CompletionTools} and \url{https://geomatics.pages.gitlab.kuleuven.be/research-projects/geomapi/testcases/completiontools.html} and \url{https://geomatics.pages.gitlab.kuleuven.be/research-projects/geomapi/testcases/alignmenttools.html}.

\paragraph{VolumeTools}
This is a thematic module to track volume changes on construction sites. It contains the functions for automated structure-from-motion processing with MetaShape, segmenting remote sensing data, volume extraction and associating this information with IfcElements. Its functionality is demonstrated in the testcase in section~\ref{VolumeTools} and \url{https://geomatics.pages.gitlab.kuleuven.be/research-projects/geomapi/testcases/volume_calculation.html}.


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Testcases}\label{sec:Testcases}
Four testcases are presented that leverage the GEOMAPI concepts to facilitate construction digital processes that are based on remote sensing. To showcase the diversity of the technology, each testcase focuses on a state-of-the-art challenge in a different construction domain at different stages of an asset's life cycle. Below, the scientific summary of each testcase is briefly explained and the advantages of using GEOMAPI data cubes are illustrated. To maximise reproducability, each testcase is also performed live in iterative notebooks in the documentation pages. We strongly advise readers to visit the documentation pages and follow the code examples for a better understanding of the GEOMAPI functionality.

\subsection{Construction site progress}
\label{ProgressTools}
Progress monitoring during the construction phase of an asset is becoming increasingly popular in the construction industry~\cite{Golparvar-Fard2015,Xu2021}. Especially with the integration of 4D BIM, the progression of the construction process can be better quantified. A key aspect is the detection of the changes between consecutive epochs of measurements. However, the development of automated procedures is challenging due to noise, occlusions and the associativity between different objects. Additionally, the amount of data to be processed is phenomenal with weekly Lidar scans or even daily photogrammetric reconstructions~\cite{Bassier2019}.

\subsubsection{Dataset}
In this example, we determine the progress of IfcBeam and IfcColumns in a residential construction site in the structure phase. The dataset that is used for this analysis is a residential complex that includes 3 separate buildings and a common underground parking, each of which has a separate IFC file. Figure~\ref{fig:progress1} a shows Building 1 (1192 elements) and the parking dataset (2705 elements). Both datasets depict the construction site structural phase, with the main classes being IfcWallStandardCase (649), IfcSlab (1387), IfcBeam (425), IfcColumn (220) and IfcStairs (22). 

\begin{figure*}[h!]
  \centering
  \begin{subfigure}[]{0.99\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/testcase_progress1.png}
        \caption{IFC models of the structure phase of Building 1 and the underground parking.}\vspace{15pt}
    \end{subfigure}
    \\
  \begin{subfigure}[]{0.34\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/week22_pcd.png}
        \caption{70 Point clouds}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.30\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/week22_img3.JPG}
        \caption{> 700 images}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.32\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/week22_mesh1.png}
        \caption{2x 500k mesh}\vspace{15pt}
    \end{subfigure}
  \caption{Overview of week 22 and 34 remote sensing data and BIM models. }
  \label{fig:progress1}
\end{figure*}

The progress is estimated in two consecutive datasets -week 22 and week 34- with a total of close to 70 point clouds and meshes (circa 700 million points). In week 22, the site was captured with terrestrial laser scanners and hand-held imagery (Figure~\ref{fig:progress1} b). During that period, the ground floor of the parking and structural columns were built, and the formwork of the first level 1 was erected. Week 34 is similar. Documentation complexities are inherently present such as precipitation, occlusions, rebar, etc. (Figure~\ref{fig:progress1} c). These objects cause significant amounts of noise and clutter and are expected to hinder the progress estimation.

\subsubsection{Progress theorem}
\label{Progress theorem}
Concretely, this task can be formulated as a query to determine the Percentage-of-Completion i.e. the ratio of the observed boundary surface area of each object compared to the surface area that can theoretically be observed (Eq.~\ref{eq1}).

\begin{equation}
\begin{split}
\label{eq1}
% PoC definition
& \text{PoC}=\frac{\text{Observed surface Area}}{\text{Theoretical visibility}} 
\end{split}
\end{equation}

Where a threshold $t_v$ can be used to state which objects are built or not (Eq.~\ref{eq2}).

\begin{equation}
\label{eq2}
% built state calculation
\text{built state}=
\begin{cases}
\  1, & \text{if PoC} \geq t_v \\
\  0, & else\\
\end{cases}
\end{equation}

 Both the numerator and denominator are defined as follows. The observed surface area of a BIM element $n_i\in N$, which is represented as a polygonal mesh or BREP representation, is defined as the portion of the surface area that lies within a Euclidean distance of a collection of remote sensing data points. For instance, the Lidar sensor or SfM pipeline that captured a portion of $n_i$ will have points reconstructed on its surface. To this end, the Euclidean distance is observed between a fixed grid of points $P_i$ sampled on $n_i$ and nearby point clouds from Lidar or sampled meshes $\boldsymbol{Q}=\{Q_1,Q_2,\dots,Q_j\}$ (Eq.~\ref{eq3}). 
 
\begin{equation}
\begin{split}
\label{eq3}
% establish observed surface area population
& P_{o_i}=\left\{p_i\Big| p_i \in P_i, q_j\in \boldsymbol{Q}: \argmin\limits_{q_j}\|p_i-q_j\| \leq t_d\right\} 
\end{split}
\end{equation}

Where the distance threshold $t_d$ is set in function of the spatial sampling resolution $r$ of the fixed grid, e.g. 0.1m, which can vary based on the size and type of $n_i$. Analogue, the theoretical visibility is defined as the exposed surface area given a BIM element's surrounding objects. For instance, a column which is partly obscured by a connected wall will have a reduced theoretical visibility. The theoretical visibility of a BIM element is established by evaluating the Euclidean distance between the same grid of points $P_i$ and the sampled point clouds $\boldsymbol{P}=\{P_1,P_2,\dots,P_i,P_j\}$ on other nearby BIM Elements (Eq.~\ref{eq4}).

\begin{equation}
\begin{split}
\label{eq4}
% establish theoretical visibility population
& P_{v_i}=\left\{p_i\Big| p_i \in P_i, p_j\in \boldsymbol{P}\setminus P_i: \argmin\limits_{p_j}\|p_i-p_j\| \geq t_d\right\} 
\end{split}
\end{equation}

The PoC is then defined by the ratio of the observed population over the theoretically visible population. Note that while the population $P_i$ can be effaced from the PoC, this analysis is partially driven by the spatial resolution of the fixed grid and the thereof dependent distance thresholds (Eq.~\ref{eq5}).
 
\begin{equation}
\begin{split}
\label{eq5}
% theoreteical visibility definition
& \text{PoC}=\frac{|P_{o_i}|}{|P_{v_i}|} 
\end{split}
\end{equation}

So far, this analysis is straightforward with multiple variants being described both in our previous work~\cite{Bassier2019} and the state of the art~\cite{Golparvar-Fard2015}, although the use of a theoretical visibility parameter is not typically included. This method has several known drawbacks, such as false positives due to noise, the dependency on the spatial resolution, the point count being a too simplistic metric for PoC evaluation, occlusion causing false negatives and the theoretical visibility being negatively impacted the absence of constructed elements. Yet, it has an average detection rate of circa 80\% which still makes it competitive. 

\subsubsection{Technological issues}
The time complexity of this analysis $O(n^3)$ or state-of-the-art variants is typically cubic in relation to the 3D space. $r$ is primary benefactor to the detection rate but quadratically affects the time complexity. The remote sensing data $\boldsymbol{Q}$ factors are especially problematic as week 22 and 34 have a combined collection of over 700 million points. So is iterating over this collection over 800 times for every beam and column in the project. Only importing this data takes over 25 minutes with single core processing and already occupies 20-25GB of RAM. If left unattended, this analysis would take hours and cause severe memory issues. 

GEOMAPI solutions include multi-processing the import and analyses and compressing all point clouds and sampled BIM objects in singular kdtrees. By maximising the data structures' efficiency,  the time complexity can be reduced to a linear $O(n)$ dimensionality, with k number of distance calculations performed in the kdtree, typically around 12-16. Additionally, resources are buffered and stored out of core and the spatial resolution can be reduced upon import. However, these process optimizations do not address the incredible amount of unnecessary computations. Additionally, every new analysis restarts this entire process, which renders the number of computations infeasible in a rapidly changing construction environment. This is especially problematic since construction site monitoring is only at its infancy, and a manifold of remote sensing data will be captured in the near future. 

\subsubsection{GEOMAPI contribution}
The contribution of GEOMAPI, aside from making this analysis easily accessible through state-of-the-art optimizations, is in the formation of data cubes to better manage which information is fed to the analysis and generalizing the results so they can be reused. To this end, both the BIM models and the remote sensing repositories are preprocessed to RDF graphs. Note that this preprocessing does not involve the actual remote sensing data but rather the metadata wherever possible. For instance, the point clouds, stored in two e57 files, have their headers parsed to a set of PointCloudNodes. 

\textbf{BIM preprocessing}: The IFC files are parsed to set of BIMNodes which are serialised in an RDF Graph $G_{\text{BIM}}$ (Figure~\ref{fig:bimprocessing} left). For the IfcBeams and IfcColumns, the theoretical visibility is determined by looking at nearby sampled BIM elements, which is a highly efficient process (Figure~\ref{fig:bimprocessing} right). This information is stored in a lightweight separate graph $G_{\text{BIM},V}$ so the base graph can be reused while the new graph can be consumed by the PoC analysis. 

\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/RDF4.png}
  \caption{Overview of the BIM processing with (left) BIMNode metadata serialization and (right) sampled visibility points clouds $\boldsymbol{P}$ and serialized theoretical visibility in $G_{\text{BIM},V}$.}
  \label{fig:bimprocessing}
\end{figure*}

\textbf{Remote sensing preprocessing}: The remote sensing data of both weeks are parsed in two sessionNodes, one for each week. For the Point clouds, the e57 files are used to extract the metadata. For the images, the XMP files from RealityCapture that contain the pose and parameters of each image are parsed to a set of ImageNodes. However, for this analysis only the SfM resulting photogrammetric meshes are parsed directly from their .obj file formats. Figure~\ref{fig:sessionprocessing} shows the metadata of the sessionNode of week 22 including the convex hull as the sessions' resource and the metadata of the linkedNodes. Overall, the metadata layer and graph creation only takes several tens of seconds that represent the metadata of 693 linkedNodes with over 30GB of remote sensing data, while consuming little to no memory. Moreover, these graphs take up only a few MB and can be both stored locally with the data or in online triplestores since GEOMAPI handles all the path references. 

\begin{figure*}[h!]
  \centering
  \includegraphics[width=1\textwidth]{Figures/RDF6_1.png}
  \caption{Overview of the sessionNode URIRef{week 22} with (green) the sessionNode convex hull and (red) orientedBoundingBoxes of its 693 linkedNodes including the pointCloudNodes, ImageNodes and MeshNodes. The IfcBeams IfcColumns and SfM mesh are shown in the background. }
  \label{fig:sessionprocessing}
\end{figure*}

\textbf{data cube formulation}: GEOMAPI uses the above graphs to construct a data cube with only the relevant data. It therefore navigates the RDF graphs and conducts a lightweight spatial evaluation against the Nodes' metadata to reduce the search space. On the one hand, the data cube is conditioned to only contain BIMNodes with a sufficient theoretical visibility ($t_v$), have not already been assessed ($t_{\text{PoC}}$) and of which the orientedBoundingBox $\text{bbox}(n)$ intersect with the orientedBoundingBox of one of the sessionNodes' linkedNodes in its graph $G_{s22}$ (Eq.~\ref{eq6}).

\begin{equation}
\begin{split}
\label{eq6}
% node inliers BIM
& G'=\left\{n\Big|n \in G_{BIM,V}:n(v_i)\geq t_v \land n(\text{PoC}) \geq t_{\text{PoC}} \right\} \\
& G''=\left\{n_i\Big| n_i \in G', n_j \in G_{s22}:\text{bbox}(n_i) \cap \omega \text{bbox}(n_j)  \right\} \\
\end{split}
\end{equation}

with a scale function $\omega$ for the search box to avoid including BIMNodes at the very edge of a resource. This metadata analysis can prove quite effective. For instance, the theoretical visibility condition reduces the number of BIMNodes by approximately 10\% for both IfcClasses with the visibility threshold $t_v$ set to 0.1 (Figure~\ref{fig:PoC} a). The reduction of the spatial condition for the PointCloud and MeshNodes respectively is 5\% and 34\% for week 22 and 34 (Figure~\ref{fig:PoC} c). The limited effect in week 22 is due to the immense SfM mesh from UAV-footage. More importantly is the reduction of redundant analyses. For instance, of the detected elements in both weeks, nearly 60\% was already considered built in week 22, thus making a second evaluation unnecessary in week 34 (Figure~\ref{fig:PoC} b). Overall, a data cube containing on average only 60\% of the original Nodes could be constructed based on the above extremely fast metadata queries. 

\begin{figure*}[h!]
    \begin{subfigure}[]{0.49\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/PoC3.png}
        \caption{BIMNodes with sufficient theoretical visibility.}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.47\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/week34_poc2.png}
        \caption{Already constructed elements (orange).}\vspace{15pt}
    \end{subfigure}
    \\
    \begin{subfigure}[]{0.99\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/week34_poc4a.png}
        \caption{SessionNode outliers}\vspace{15pt}
    \end{subfigure}
    % \begin{subfigure}[]{0.47\textwidth}
    % \centering
    %     \includegraphics[width=1\textwidth]{Figures/selection_BB_intersection4.png}
    %     \caption{LinkedNode outliers}\vspace{15pt}
    % \end{subfigure}
  \caption{Overview of the search space reduction by the metadata analysis of the RDF graphs. }
  \label{fig:PoC}
\end{figure*}

Similar data cubes can be constructed from sessionNodes week 22 $G_{s22}$ and week 34 $G_{s34}$ graphs. To this end, only those nodes are retained for the analysis that meet the proximity condition to the target BIMNodes. To this end, the orientedBoundingBox $\text{bbox}(n_i)$ are conditioned to intersect with the orientedBoundingBox of at least one target BIMNode from $G''$(Eq.~\ref{eq7}). 


\begin{equation}
\begin{split}
\label{eq7}
% node inliers BIM
& G_{s22}'=\left\{n_i\Big| n_i \in G_{s22}, n_j \in G'' : \omega \text{bbox}(n_i) \cap  \text{bbox}(n_j)  \right\} \\
\end{split}
\end{equation}

where the same scale function $\omega$ is applied to the orientedBoundingBoxes of the Nodes in $G_{s22}$. However this condition does not reduce the search space for this specific example since the IfcBeam and IfcColumn of the entire project were selected for the data cube. Additionally, it is quite a compact construction site and Lidar and UAV photogrammetry easily capture objects from tens of meters away. However, this doesn't invalidate the method from a theoretical stance as close-range sensors such as an AR device very much can have its data discarded because of lack of BIMNode inliers. 

\textbf{Multi-temporal detection}: Given the data cubes for each week, the PoC can be determined for the relevant BIMNodes. First, in week 22, the PoC is determined for all IfcBeam and IfcColumn elements that meet the theoretical visibility condition as described in section~\ref{Progress theorem}. The resulting PoC and process parameters including the search distance, spatial resolution, sessionNode used for the analysis, and so on is serialized in a separate graph. In week 34, the intersection is computed between this analysis graph, the original BIM Graph and the session 34 Graph to build the data cube for week 34. As described above, this heavily optimizes the search space in week 34. The PoC results of week 34 are then serialized in a separate analysis graph with the same properties as the analysis graph of week 22. As the weeks progress, these light weights graphs can be used to further optimize the search space i.e. by selecting certain time frames of sessions or areas of interest.

\subsubsection{Conclusion}
In this testcase, we determined the work progress on beam and column elements in two measurement epochs using GEOMAPI functionality. The following conclusions can be drawn from the above tests.

\begin{enumerate}
	\item \textbf{GEOMAPI}: Over 70 point clouds (>700M points), meshes and 2 IFC structure models are processed in under 6-10 minutes by smartly dealing with the objects metadata, parallel processing wherever possible and building multi-temporal data cubes. This shows a core strength of GEOMAPI that looks to facilitate big data remote sensing processing.
 	\item  \textbf{RDF Graphs}: By using Graphs to store metadata, process parameters and analysis results, combining data from multiple sources becomes very intuitive. This promotes accessibility of the results and also increases their longevity which is a crucial problem in the state of the art.  
	\item \textbf{Documentation}: The progress estimation of both weeks shows that nearly 60\% of the observed elements were documented in both weeks, which indicates a significant overlap in both measurement epochs. This would hinder traditional analysis processes since the documentation is the most costly step that also computationally burdens downstream analyses. While GEOMAPI does not improve upon site documentation, it does significantly lower the computational burden to manage remote sensing data. This analysis showed that the search space could be effectively reduced by 40\% by smartly building data cubes.
	\item \textbf{Future work}: Future work will focus on the development of a more inclusive PoC method, possibly also based on image detection such as we are already pursuing~\cite{Cuypers2021}. The decision function can also be replaced with a more state-of-the-art machine learning method that looks are appearance descriptors instead of solely evaluating point inliers based on previous work~\cite{Bassier2019}. 
\end{enumerate} 



%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% JELLE %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\subsection{Dataset updating}
\label{CompletionTools}
There is a need to extend the lifespan of detailed dense recording data so it does not become obsolete as soon as small parts of the recorded environment change. The GEOMAPI CompletionTools were created to tackle this challenge by complementing existing datasets with newer (smaller) datasets of the updated environment. Because there is no guarantee that the quality of the different scans will be similar, there is the need to evaluate at a point-based level which parts of the datasets are the most reliable. A two-step process is proposed where first the out-of-date points are identified and isolated after which the zone is updated with new recordings where needed. The parts that are unaltered will use the more detailed dataset wherever possible.

\subsubsection{Datasets}
In this testcase, we combine two different datasets of the same site named "Lab 2 (Asphalt)", captured at different times, and with different sensors. The original dataset is captured by a high-end iMMs (NavVis VLX), while the new dataset is captured using a low-end Lidar sensor (Microsoft Hololens). The two datasets are captured a few months apart and show significant changes. There are certain objects that are no longer on the site and new objects there were not present before. There are also objects that are still present, but have moved from their original position. Table \ref{tab:completion_dataset_overview} shows an overview of the two datasets. The comparison clearly shows the original dataset is of much higher overall quality, both because of its higher density, but also coverage and accuracy. Therefore, it is clear the original dataset should be preserved as much as possible.

\begin{table}[H]
    \caption{Overview of the two used datasets, both taken from the same viewpoint. The VLX Dataset on the left covers a much larger area compared to the Hololens dataset on the right.}
    \label{tab:completion_dataset_overview}
    \begin{tabular}{|c|c|}
        \toprule
            \includegraphics[width=0.49\textwidth] {Figures/complete/VLX_Dataset.png}
            &
            \includegraphics[width=0.49\textwidth] {Figures/complete/Hololens_Dataset.png}
            \\
        \midrule
            \textbf{NavVis VLX}   & \textbf{Microsoft Hololens}    \\
            743,618 points      & 13,107 points    \\
            100 images          & 34 images   \\
            0.005m density      & 0.05m density\\
            0.005m accuracy     & 0.05m accuracy\\
        \bottomrule
    \end{tabular}
\end{table}

\subsubsection{Combination Algorithm}
The combination is performed in two phases: the removal and the addition phase. In the removal phase, the out-of-date points are removed from the original dataset and in the addition phase, new points are added from newly recorded data. Because of the difference in quality, the unaltered points should originate from the more accurate dataset. This is why it is important to define the more detailed dataset before the combination. In this case, the original dataset is taken by a iMMs, and has a much higher accuracy, making it the more accurate dataset. The process commences by creating two Geompai Sessionnodes, one for each dataset. The two datasets can be aligned using the GEOMAPI Alignmenttools, where the standardisation of the coordinate systems makes sure they are physically in the same place.

\paragraph{Removal phase}
The first step is the removal of the out-of-date points in the original point cloud. To prevent false positives, a relevant sub-selection is made from the original point cloud that only contains the area of the changes. To this end, the convex hull of the new measurement epoch is used to segment the relevant area in the original point cloud. This is important because there is a need to evaluate the scannability of each point in the dataset. The scannability checks if an out-of-date point could have been observed by the new sensor. This consists of two separate checks: the coverage and visibility check.

% coverage check
The coverage check determines if an original point is covered by a new point. This check is performed for each point separately. To this end, the nearest Euclidean distance between both datasets is observed using the ktree trees of each dataset. The Euclidean distance is used over other algorithms because it allows us to rapidly compare observations from two seperate datastructures. By comparing point-to-set we can ensure outliers do not affect our results. Each point $p_i$ of the original dataset $P$ that is further away from the new dataset $Q$ than a threshold distance $t_d$ is considered not covered and part of $P'$ (Eq.~\ref{eqremoval}). When a point is not covered, it is either no longer present in the new situation, or it was not captured during the new survey. To prevent false positives from the latter case, we also perform a visibility check.

\begin{equation}
\label{eqremoval}
% de deelverzameling P' bestaat uit: voor alle punten p_i in P, voor alle punten q_j in Q geldt: als de afstand tussen p_i en q_i kleiner is dan t_d
P' = \left \{ p_i \Big| \forall p_i \in P, q_j \in Q : \argmin\limits_{q_j}\|p_i-q_j\| \geq t_d\right\}
\end{equation}

% visibility check
The visibility check aims at filtering out the false positives in the uncovered points. Because the new dataset is considered smaller and less detailed, we can assume that it will have less coverage then the original dataset. The check is preformed by evaluating whether each original point $p_i$ is in front or behind the new dataset $Q$ with respect to the sensor's location at the time of recording. If a point $p_i$ is behind the new dataset $Q$, it could not have been visible to the new sensor and should be considered invisible. If the point is in front of the new dataset, the scanner should have been able to see the original point and can be considered out-of-date and should therefore be removed. To determine the side of the point, a raycasting scene is constructed. A ray $\overrightarrow{p_iq_j}$ is cast from each point $p_i$ in $P'$ to the $n$ closest points $q_j$ in the new dataset $Q$ and the normals $\overrightarrow{n(q_j)}$ of the raycast hits are calculated. If the percentage of dot products $\overrightarrow{p_iq_j} \cdot \overrightarrow{n(q_j)}$ of the $n$ closest points that is greater than $t_n$, is smaller than $t_p$ the point $p_j$ is considered visible and should be deleted (Eq.~\ref{eqnormals}). 

\begin{equation}
\begin{gathered}
\label{eqnormals}
Q_i = \left\{q_j \Big| p_i \in P', q_j \in Q :  \argmin\limits_{q_j,n} \|p_j-q_i\| \right\} \\
Q_i' = \left\{q_j \Big| \forall q_j \in Q_i, p_i \in P': \overrightarrow{p_iq_j} \cdot \overrightarrow{n(q_j)} \geq t_n\right\} \\
P'' = \left \{ p_i \Big| \forall p_i \in P': \frac{|Q_i'|}{|Q_i|} \leq t_p\right\} \\
\end{gathered}
\end{equation}

\begin{figure}[h]
    \label{fig:Comp_remove}
    \includegraphics[width=1\textwidth]{Figures/complete/Completiontools Removal Schema.png}
    \caption{(Left) The coverage check where points $p_i$ are compared against points $q_j\in Q$. (Right) The visibility check where the direction of the rays is compared to the normals of $q_j$. The grey points that fail both checks are considered out-of-date and are removed from the original dataset.}\vspace{15pt}
\end{figure}

\paragraph{Addition phase}
After the out-of-date points are removed, we need to add the new points. These are points that are present in the new dataset $Q$ but have no representation in the original dataset. To determine these points, we perform a distance query from the new points $q_j$ to the not-removed points $p_i$ from the original dataset ${P \textbackslash  P''}$. By using the same distance threshold $t_d$, we ensure a coherent combination of the datasets (Eq.~\ref{eqadddist}).

\begin{equation}
\label{eqadddist}
%  distance treshold
Q' = \left \{ q_j \Big| \forall q_j \in Q,p_i \in \{ P \textbackslash P'' \} : \argmin\limits_{p_i}\|q_j-p_i\| \leq t_d\right\}
\end{equation}

\begin{figure}[h]
    \label{fig:Comp_add}
    \includegraphics[width=1\textwidth]{Figures/complete/Completiontools Addition Schema.png}
    \caption{The coverage check where points $q_j$ are compared against points $p_i \in P'$.}\vspace{15pt}
\end{figure}

The final dataset is the combination of a number of different parts, each stored using the Geomapi GeometryNode and serialized to drive when the dataset is to big to reliably keep in the system memory. First, we have the original irrelevant part of the point cloud. These are the points that are filtered out by the convex hull and have not been affected by the algorithm. The second part is the still relevant part of the original point cloud. These are points that have passed both the coverage and visibility check of the removal phase. The final points are the new points from the the new dataset that were not covered by the original dataset. These three parts combined create a complete up-to-date dataset where the original dataset is kept as much as possible and only the out-of-date points are replaced by the new points.

\paragraph{Testcase statistics}

In our testcase, we have been able to reduce the original dataset by 95.5\% by using the convex hull filtering. 
During the removal phase, 36.2\% of the original filtered points were not covered by the new dataset and were subjected to a second visibility check. The visibility check flagged 56.6\% of the uncovered points as being visible, so they were deleted. This means 15.7\% of relevant original points are removed from the dataset.
In the addition phase, 26.4\% of the points from the new dataset had no existing counterpart in the original dataset and were added to the combined dataset.

In summary, the new relevant part of the combined dataset consists of: 
54.4\% still relevant existing points, 13.4\% inconclusive occluded points and 32.2\% new updated points. The total algorithm needed 1.473s to complete on a off-the-shelf notebook.

\begin{figure*}[h!]
    \begin{subfigure}[]{0.49\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/complete/PointcloudBoundingBox.png}
        \caption{Subselection $P$ of the original dataset made with the convex hull (red).}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.47\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/complete/UncoveredPointcloudPoints.png}
        \caption{Subselection $P'$ of the points in the original subselection $P$ made with the coverage check (red).}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.49\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/complete/InvisiblePointcloudPoints.png}
        \caption{Subselection $P''$ of points in $P$ which passed the visibility check (red). the failed points(green) are not removed from the dataset. }\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.47\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/complete/NewMeshPoints.png}
        \caption{ Subselection $Q'$ of the points in the new dataset that passed the distance query (red).}\vspace{15pt}
    \end{subfigure}
  \caption{Overview of the combination process.}
  \label{fig:completionToolsOverview}
\end{figure*}

\subsubsection{GEOMAPI contribution}
By storing all the datasets as linked graph using GEOMAPI SessionNodes, we eliminate the continued struggle of automatically aligning data coming from different software or sensors. By standardising the geo-referencing and bounding box data structure, the developed methods and functions will need no further pre-processing to ensure a correct alignment. 

The use of GEOMAPI's data cubes allows different datasets to be linked both in space and through time. By referencing other Nodes in the graph structure of a certain dataset, it becomes trivial to find two relevant datasets to combine into a new dataset. That combined dataset also references the originating dataset, making it possible to revert certain changes or further improve the combination over time. This will also make sure earlier metadata or linked analysis models can easily be linked to the most recent version of the environment.

\subsubsection{Conclusion}
In this testcase, we showcased a combination algorithm that fuses different datasets. This was validated against a dataset using GEOMAPI functions. The following conclusions can be drawn from these tests.

\begin{enumerate}
	\item \textbf{GEOMAPI}: The ease of use and the modularity of the different functions in GEOMAPI allows users to quickly combine different datasets while also making sure the user has the ability to change certain parts of the algorithm.
    \item  \textbf{RDF Graphs}: The RDF Graphs make sure the different datasets are compatible and are geo-referenced correctly using the same coordinate system. The graphs also ensure the link between the three datasets can be retained should one dataset be needed for a certain process.
	\item \textbf{Extended Lifespan}: The completiontools functionality store points clouds separately during each recording so one can rapidly construct a point cloud from any timeframe. This reduces overall data-waste and the need to recapture big construction sites.
	\item \textbf{Future work}: Future work will focus on the improvement of the removal phase by improving the checks to be more accurate. The addition phase can also be improved by better using the existing data in case certain parts that have slightly moved, making the points still available, but not in the correct place.
\end{enumerate} 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% SAM %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsection{Scan-to-BIM}
\label{ValidationTools}
At key stages of a building's life-cycle, a structure is documented and its as-built conditions made part of the digital twin i.e. for project planning, or construction delivery~\cite{Patraucean2015,Macher2017a}. In this testcase, we evaluate two common procedures with BIM Models. First, we demonstrate GEOMAPI functionality to use point cloud data to asses whether a construction was properly built based on the as-design BIM. To this end, the accuracy of the elements captured on site is determined with respect to the as-design BIM and reported conform the LOA specification~\cite{U.S.InstituteofBuildingDocumentation2016}. Second, we can estimate the modeling accuracy of as-built or as-is documentation procedures. In this case, the point cloud data is used as a reference to validate the resulting as-built BIM of a scan-to-BIM process.

\subsubsection{Dataset}
The dataset used in this testcase is a two-storey steel structure that was erected within the scope of a renovation of an old military base in the City Center of Ghent. The BIM model is provided in IFC and the visible beams and columns are modelled conform Level-of-Development 400~\cite{BIMFORUM2016}.
In this analysis, we validate the IfcBeams or IfcColumns elements of the structure as part of the project delivery. Other elements such as anchors for the concrete floors, connection plates between steel elements, bolts, screws and so on are not validated due to their limited size, making them difficult to detect with laser scanning. The point cloud used in this case is captured using a high-end mobile mapping system, the NavVis VLX, which is reported to achieve point cloud accuracy's of LOA20 (95\% inliers on 5cm) to LOA30 (95\% inliers on 1.5cm) \cite{NavVisGmbH2020}~\cite{DeGeyter2022}. 
\begin{figure*}[h!]
\centering
    \begin{subfigure}[]{0.45\textwidth}
        \begin{table}[H]
        % \caption{LOA brackets}
        \label{tab:LOA brackets}
        \begin{tabular}{|c|c|c|}
            \toprule
                \textbf{LOA bracket} & \textbf{Lower bound} & \textbf{Upper bound} \\
            \midrule
                \textbf{LOA30}   & 0.000  & 0.015 \\
                \textbf{LOA20}   & 0.015  & 0.050 \\
                \textbf{LOA10}   & 0.050  & 0.100 \\
            \bottomrule
        \end{tabular}
        \end{table}
    \end{subfigure}
    \begin{subfigure}[]{0.45\textwidth}
        \centering
         \includegraphics[width=1\textwidth]{Figures/QualityCompare/BIMclasses.png}
    \end{subfigure} 
  \caption{Overview of the Level-of-Accuracy (LOA) brackets (left) BIM dataset colored by Ifc class (right) yellow are IfcColumn elements, green represent IfcBeam elements and red indicate the ignored elements.}
  \label{fig:QualityCompareDataset}
\end{figure*}

% \begin{figure*}[h!]
% \centering
%     \begin{subfigure}[]{0.45\textwidth}
%         \centering
%         \includegraphics[width=\textwidth]{Figures/QualityCompare/BIM.png}
%     \end{subfigure}
%     \begin{subfigure}[]{0.45\textwidth}
%         \centering
%          \includegraphics[width=1\textwidth]{Figures/QualityCompare/BIMclasses.png}
%     \end{subfigure} 
%   \caption{Overview of the BIM dataset (left) BIM dataset colored by Ifc class (right) yellow are IfcColumn elements, green represent IfcBeam elements and red indicate the ignored elements.}
%   \label{fig:QualityCompareDataset}
% \end{figure*}

\subsubsection{Quality theorem}
The as-built accuracy of a building element can be determined by studying the per-point distances between the captured point cloud and the BIM model. To this end, the presented testcase computes the per-element Level-of-Accuracy (LOA) percentages and displays them in an intuitive manner. The LOA brackets, as defined by ~\cite{U.S.InstituteofBuildingDocumentation2016}, state that when 95\% of the distances satisfy the conditions shown in figure ~\ref{fig:QualityCompareDataset} the project, element or set of elements can be labeled accordingly. The upper bound of the LOA10 bracket, which is user defined, is set to $0.10m$ for this testcase. 

% \begin{table}[H]
%     \caption{LOA brackets}
%     \label{tab:LOA brackets}
%     \begin{tabular}{|c|c|c|}
%         \toprule
%             \textbf{LOA bracket} & \textbf{Lower bound} & \textbf{Upper bound} \\
%         \midrule
%             \textbf{LOA30}   & 0.000  & 0.015 \\
%             \textbf{LOA20}   & 0.015  & 0.050 \\
%             \textbf{LOA10}   & 0.050  & 0.100 \\
%         \bottomrule
%     \end{tabular}
% \end{table}

To establish the LOA percentages, the distance between every point $p_{i}\in P$ of the captured point cloud and the closest point $q_{j}\in Q$ of the reference cloud is computed up to a threshold $t_d$ (Eq. ~\ref{initialdistancefiltering}). 

\begin{equation}
\begin{gathered}
\label{initialdistancefiltering}
P' = \left\{p_i \Big| \forall p_i \in P , Q_j \in Q: \|p_i-q_j\| \leq t_d \right\} \\
\end{gathered}
\end{equation}

Where $Q$ is the joint visibility point cloud sampled from the BIM objects analogue to Section~\ref{Progress theorem}. Distributing $P'$ over the LOA brackets per object then yields the LOA percentages. However, this popular analysis is flawed due to the presence of noise, clutter and nearby target objects. GEOMAPI therefore conditions ${p_i,q_j}$ correspondences to better fit the surface shape of the as-design BIM objects.

Concretely, the best fit neighbor for each point $p_{i}\in P'$ remaining in the captured point cloud $P'$ is established. First, the k nearest potential matches for $p_{i}$ in $Q$ are determined through a hybrid nearest neighbor search (Eq.~\ref{QCeqKNN})(Figure ~\ref{fig:QualityCompareMath}). 

\begin{equation}
\label{QCeqKNN}
Q_i = \left\{q_j \Big| \forall q_j \in Q, p_i \in P': \argmin\limits_{q_i k=20} \|p_i-q_j\| \right\} \\
\end{equation}

\begin{figure*}[h!]
\centering
    \includegraphics[width=1\textwidth]{Figures/QualityCompare/Math.png}
  \caption{Overview of ${p_i,q_j}$ correspondences conditions (left) for every point $p_i\in P$, the k nearest points $q_j$ of the reference point cloud Q are selected within distance $t_d$. (right) resulting set $Q'$ conditioned on both distance $d_{i,j}$ and the normals $n_i$ of $p_i$ and $n_j$ of $q_j$.}
  \label{fig:QualityCompareMath}
\end{figure*}

The result is a set $Q_i$ for every $p_i$. Next, the suitability of every potential match $q_j$ is determined based on the distance and the orientation similarity to $p_i$. Concretely, the inverse distance $d_j$ and the dotproduct between the normals $\overrightarrow{n(p_i)}$ and $\overrightarrow{n(q_j)}$ is observed (Eq. ~\ref{QCparam}).

\begin{equation}
\begin{gathered}
\label{QCparam}
D_i = \left\{d_j\Big| \forall q_j \in Q_i, p_i \in P': d_j = \frac{t_d - \|p_i-q_j\|}{t_d}\right\} \\
N_i = \left\{n_j \Big| \forall q_j \in Q_i, p_i \in P': n_j = 
\overrightarrow{n(p_i)} \cdot \overrightarrow{n(q_j)} \right\} \\
\end{gathered}
\end{equation}

Based on $D_i$ and $N_i$, the best matching point for $p_i$ is selected from $Q_i$ by an empirically determined decision function (Eq.~\ref{QCsimilarity}). 


\begin{equation}
\begin{split}
\label{QCsimilarity}
Q'= \left\{ q_j \Big| \forall d_j \in D_i, n_j \in N_i: \argmax\limits_{q_j} = ((w_d*d_j + w_n*n_j^3) \geq t_s) \right\} \\
\end{split}
\end{equation}

This results in the set $Q'$ containing the best matches for all points of $P'$. In this testcase, the weight factors $w_d$ and $w_n$ for respectively the distance and the normal are set to 0.62 and 0.37 respectively. Additionally a threshold $t_s$ is introduced to filter points without a decent match and is set to 0.7. Filtered matches are ignored in the LOA computations. The final LOA percentages are then determined by the ratio of the individual bracket inliers over the total population of $P'$ for which a match was found.  

\begin{figure}
    \label{fig:overviewc2cGEOMAPI}
    \begin{tabular}{|c|c|c|c|}
        \toprule
            \includegraphics[width=0.23\textwidth]{Figures/QualityCompare/BIMColumn.png}
            &
            \includegraphics[width=0.23\textwidth]{Figures/QualityCompare/SourceColumn.png}
            &
            \includegraphics[width=0.23\textwidth]{Figures/QualityCompare/C2CColumn.png}
            &
            \includegraphics[width=0.23\textwidth]{Figures/QualityCompare/GEOMAPIColumn.png}
            \\
        \midrule
            \textbf{BIM - reference}   & \textbf{Point cloud - source}  & \textbf{Result - cloud-to-cloud}  & \textbf{Result - GEOMAPI} \\
            & & $p_{LOA10}$ = 0.83 & $p_{LOA10}$ = 1.00 \\
            & & $p_{LOA20}$ = 0.75 & $p_{LOA20}$ = 0.99 \\
            & & $p_{LOA30}$ = 0.71 & $p_{LOA30}$ = 0.93 \\
            & & |LOA00| = 7618 & |LOA00| = 5612 \\
        \bottomrule
    \end{tabular}
    \caption{Comparison of the results of a general cloud-to-cloud computation and the GEOMAPI LOA computation.}
\end{figure}

Overall, by conditioning ${p_i,q_j}$ correspondences, the analysis is far more accurate than generic cloud-to-cloud computations. Table~\ref{fig:overviewc2cGEOMAPI} shows the drastic difference between standard methods and if conditioning is applied. GEOMAPI also implements tools to visualize the results, either by object label, distance gradient or where objects had insufficient matches to reliable determine a quality label. The results can be reported using CSV files or even exported directly in excel. More visual representations such as histograms can be created both for independent elements or for the entire point cloud at once (Figure~\ref{fig:analysisOutputs}).

\begin{figure*}[h!]
\centering
    \includegraphics[width=1\textwidth]{Figures/QualityCompare/UPDATE-QC-results.png}
  \caption{Overview of different possible analysis outputs. Microsoft Excel, CSV, histograms, colored point clouds or meshes.}
  \label{fig:analysisOutputs}
\end{figure*}


% While all points within a certain threshold distance $t_d$ which is 0.15 in this case are considered. 


% In most cases this is done by using only the points with a cloud-to-cloud distance smaller than a predefined threshold yielding not optimal results as will be discussed in section ~\ref{QCtechnological issues}. 

% The presence of points that are not part of both point clouds, for example structures that are not modelled such as furniture, but that are present inside the captured pointcloud will impact the LOA percentage significantly. 

% This because all those points will not be included in any LOA bracket, depending on threshold $t_d$ but will be counted in the overall point count used to determine the LOA percentage.  

% Especially in this case, where only the steel structure is included in the model. The analysis uses points that are clearly not part of the structure at all. To this end this testcase introduces some filtering algorithms leveraging the GEOMAPI toolbox to compute the LOA percentages more accurately.

% The first measure uses the GEOMAPI theoretical visibility functionalities and reduces the amount of wrongfully used points significantly. 

% On this testcase this still yields large amounts of wrong points because the BIM model only contains the steel structure while the point cloud was captured in a later phase of the construction already containing concrete floors, wood roof structures and insulation etc.). 

% In practice, this function is used to create a sampled point cloud of the BIM model geometries, which can be easily imported by GEOMAPI's combination of other SOA libraries and thus only contain theoretically visible points. 

% This is done for every BIM element for which the analysis must be performed, all these reference clouds are merged in one large reference cloud Q which will be used for the analysis. 

% For the presented case with the steel structure, this measure does not have a significant impact. This because the secondary, not steel, structures are not included in the model. 

% In a second step the captured point cloud P is loaded on a memory efficient way and the general cloud-to-cloud distances are computed. In this step this is the minimal distance for each point p to the closest point of Q. Here a first filtering is done by excluding all points which are located further then a certain threshold $t_d$ of the reference cloud (Eq. ~\ref{initialdistancefiltering}).

% This initial filtering step removing all points with a cloud-to-cloud distance larger than $t_d$ reduces the computation times significantly.


% For each point remaining in the captured point cloud P', a hybrid nearest neighbour search will be conducted using the requirements stated in equation ~\ref{QCeqKNN} selecting the k nearest points of the reference cloud Q with a maximal distance to the source point $p_i$ of $t_d$. (Figure ~\ref{fig:QualityCompareMath})

% \begin{equation}
% \label{QCeqKNN}
% Q_i = \left\{q_j \Big| \forall q_j \in Q, p_i \in P': \argmin\limits_{q_i k=20} \|p_i-q_j\| \right\} \\
% \end{equation}

\subsubsection{Technological issues}
\label{QCtechnological issues}
Unoptimized, state-of-the-art methods can be very time consuming and memory intensive. GEOMAPI's parallel processing and ability to apply spatial resolution upon import vastly reducing memory constraints and unnecessary downsampling. One of the main issues when using cloud-to-cloud distance analyses is the lack of element-wise data. The source point cloud needs to be separated per element and the cloud-to-cloud analysis has to be performed per element. This lead to large amounts of points being used which are not part of the targeted structure or element. In some cases, the point clouds are cleaned by removing the points which are not part of the targeted elements which gives better results, but this is mostly a manual procedure. 

To this end, GEOMAPI introduces a module to calculate per element accuracy's using point clouds and BIM models. The points of the captured point cloud are filtered based on their distance and normal compared to the reference BIM model. By including those filtering steps in the LOA computations, more accurate results are determined, reducing the need for an expert interpretation of the data and control measurements on site. As shown in table~\ref{tab:overviewc2cGEOMAPI} the results of a typical cloud-to-cloud analysis are compared to the results of the GEOMAPI LOA computation, which clearly yields better results.

\subsubsection{GEOMAPI contribution}
GEOMAPI contributes to this already existing pipeline by making the process more accessible. Analogue to the other testcases, the time needed to complete the total analysis is significantly reduced by using the GEOMAPI principles of multiprocessing and data cubes. Concretely, GEOMAPI implements efficient kd-tree datastructures that reduce the nearest neighbor calculation for the analysis from a quadratic to a linear computational complexity. Additionally, the search space is heavily optimised by only considering nearby observations for every BIM object. The BIM object's sampling is also formulated as a single tensor, which is significantly more memory efficient than making separate function calls for every BIM object. Also, by bundling and centralizing the remote sensing data of a project the errors detected with the toolbox can be easily visualized. This can be of major importance in the communication with different stakeholders with different backgrounds. Specifically in this case the output can differ from lists containing element IDs and LOA percentages, to point clouds colored per LOA class or deviation, to colored meshes per element indicating its LOA. The Linked Data principles again contribute to store the analysis results, and compare expected LOA classes with measured accuracy's to determine whether a modeling/construction is conform its specifications. By storing the results on a generalized measure and allowing the same results to be visualized in multiple ways instead of reprocessing the data in another setting, the lifespan of the results and the time consumed by the analysis are reduced significantly.

\subsubsection{conclusion}
The use of the GEOMAPI toolbox to compute, visualize and govern the remote sensing data and the analyses' results contributes to the lifespan of the data and the conducted analysis. The analyses are made significantly faster by using the SOA processing algorithms implemented in GEOMAPI. Also the possibility to visualize and show the analysis results in different settings depending on the users background will significantly increase the communication between different stake holders within the construction process.


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% Heinder %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\subsection{Volume changes in road construction}
\label{VolumeTools}
Road and infrastructure construction is trailing behind in asset and process digitization. With the increasing adoption of BIM, progress monitoring in this field is becoming increasingly accessible and less labor-intensive in comparison to conventional methods due to remote sensing. An important component of the monitoring is tracking material usage and displacement such as soil displacement, grout, foundation layers, etc. in between consecutive measurement epochs. The quantities of these materials are currently determined using a weighbridge, which is slow and highly susceptible to mass density changes depending on the moisture content. In this testcase, volume changes from remote sensing data are proposed. Both the flight-to-flight and flight-to-as-design BIM comparison are automated. This leads to more comprehensive and faster volume calculations. Additionally, volume calculations can be determined per element which is essential to asses the progress and as-built state.  

\subsubsection{Dataset} 
In this testcase, the volume changes in 2 consecutive datasets are assessed. The used dataset is a new roadsection in an allotment between two existing roads. Figure~\ref{fig:progress2} shows the BIM model with the new roadway, pavement, sewage and an embankment. The used IFC file contains 64 elements with different materials including a sidewalk in clinker brick pavement with a porphyry pavement underneath on a sparse concrete foundation. The road next to the sidewalk consists of a concrete pavement on a layer of sparse concrete. In parallel, a gravel lawn is found on an unbound crushed stone foundation with the presence of geotextile. Below the road we find a dry weather flow (dwf) utility line with three inspection wells. On the six lots, there are also twelve house connection manholes, six of which are dwf and six rain water flow (rwf). The dwf connection wells discharge into the dwf utility line and the rwf connection wells terminate in the embankment across the road.

\begin{figure*}[h!]
  \centering
  \begin{subfigure}[]{0.99\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/BIM1.PNG}
        \caption{The IFC-file from the testcase for volume changes in road construction}\vspace{15pt}
    \end{subfigure}
    \\
  \begin{subfigure}[]{0.34\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/PCD_101_0366.JPG}
        \caption{point cloud from the session 101-0366}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.30\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/Mesh101-0367.png}
        \caption{Mesh from the session 101-0367}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.32\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/OcclusionRC.jpg}
        \caption{An example of the occlusions in the datasets}\vspace{15pt}
    \end{subfigure}
  \caption{Overview of session 101-0366 and session 101-0367 remote sensing data and BIM model. }
  \label{fig:progress2}
\end{figure*}

Volume changes are estimated in two consecutive datasets -101-0366, captured begin June 2021 and 101-0367, captured the next week. Both datasets were collected with an RGB camera mounted on an RTK UAV. The captured images were then photogrammetrically processed into one point cloud and one mesh per epoch. An example of the point cloud captured in session 101-0366 can be seen in Figure~\ref{fig:progress2} b, and a mesh captured in session 101-0367 can be seen in Figure~\ref{fig:progress2} c. Some complexities are inherently present such as water accumulation due to precipitation, occlusions by machinery and materials, etc. (Figure~\ref{fig:progress2} d).

\subsubsection{Volume calculation theorem}
\label{Volume algorithm}
Two types of volume changes are assessed. (1) A comparison between flights for the whole site in order to assess the general volume displacements and (2) a comparison of the volume changes per BIM object to asses the progress and quantity take-offs on site. Both methods rely on the distance calculation between the depthmaps of the assets. 

Each depth map $\boldsymbol{D}$ is the matrix of which the depth assignment is equal to the distance calculation between a grid of 3D points $p\in P$ with a fixed resolution $\delta r$ and the corresponding intersection point $p(c)$ on the 3D mesh $M$ of a resource. The intersections are determined by vertically raytracing each $p$ per BIMNode and flight geometry (Eq.~\ref{eqVolume1}).

\begin{equation}
\begin{split}
\label{eqVolume1}
% depth map estimation -https://en.wikipedia.org/wiki/Line%E2%80%93plane_intersection
& \boldsymbol{D}=\left\{d(x,y) \Big| \forall p \in P, c \in M_{G_{\text{BIM}}}: d(x,y)=\frac{(p(c)-d(x,y))\cdot \overrightarrow{n(c)}}{\hat{u}_z\cdot \overrightarrow{n(c)} }\right\} 
\end{split}
\end{equation}

where $p(c)$ and $\overrightarrow{n(c)}$ respectively are the center and normal of the triangle $c$ in the Mesh $M$ of the BIMNode in $G_{\text{BIM}}$. $\hat{u}_z$ is the unit vector in Z-direction. The two volume estimations are then derived by combining the depthmap $\boldsymbol{D}_{f_i}$ of the current flight with (1) the depthmap $\boldsymbol{D}_{f_{i-1}}$ of the previous flight and (2) with the depthmaps $\boldsymbol{D}_{b_1->b_n}$ for each BIM object in the scene. For the first assessment, The volume difference between flights is split in added and subtracted volume, and a gradient chart is created that demarks the soil displacements (Eq.~\ref{eqVolume2}) (Figure~\ref{fig:colored_pointcloud}).

\begin{equation}
\begin{split}
\label{eqVolume}
% flight to flight
& V= \sum_{\boldsymbol{D}_{f_i},\boldsymbol{D}_{f_{i-1}}} \delta r^2(d_{f_i}-d_{f_{i-1}}) \\
\end{split}
\end{equation}

For the second assessment, the depth comparison between $\boldsymbol{D}_{f_i}$ and $\boldsymbol{D}_{b_n}$ is conditioned to be limited to $\{d_{b,min},d_{b,max}\} \in \boldsymbol{D}_{b_n}$ at $p(x,y)$. To this end, the depth is expressed as the height ratio given the lower and upper boundary of the BIM object at each point, (Eq.~\ref{eqVolume2}) and (Figure~\ref{fig:math_volumecalculation}).  

\begin{equation}
\label{eqVolume2}
% depth function
d(x,y)=
\begin{cases}
\  0 & ,  d_{f}\geq (d_{b,max}-d_{b,min}) \\
\  (d_{b,max}-d_{b,min}) & ,  d_{f}\leq d_{b,max} \\
\  (d_{f}-d_{b,min}) &,  else\\
\end{cases}
\end{equation}

\begin{figure*}[h!]
    \centering 
        \includegraphics[width=1\textwidth]{Figures/math_volume_calculation_zwart.JPG}
  \caption{The volume calculation between the bim and one of the flights.}
  \label{fig:math_volumecalculation}
\end{figure*}

Integrating this function over each BIM object's dimensions can be performed quite efficiently with only an $O(n^2)$ complexity for the object dimensions rather than for the dimensions of the entire project (Eq.~\ref{eqVolume3}).

\begin{equation}
\begin{split}
\label{eqVolume3}
% flight vs BIM
& V= \iint_{X_{b_n},Y_{b_n}}^{} \delta r^2 d(x,y) \,dx\,dy
\end{split}
\end{equation}

So far, this analysis is straightforward expansion of existing volume definitions to assign volume changes to a set of BIM elements. This method will yield slightly deviating results when there are more than two function returns for any $p(x,y)$ in objects with disproportionate geometries, but this localised and rare in road construction design. Overall, it is a very fast and fairly accurate method to asses volume changes on site.  

% \begin{figure*}[h!]
%   \begin{subfigure}[]{0.49\textwidth}
%     \centering
%         \includegraphics[width=1\textwidth]{Figures/Oriented_BoundingBoxes.JPG}
%         \caption{The mesh with the oriented bounding boxes of the IFC-elements}\vspace{15pt}
%     \end{subfigure}
%     \begin{subfigure}[]{0.49\textwidth}
%     \centering
%         \includegraphics[width=1\textwidth]{Figures/Cropped_Mesh.JPG}
%         \caption{The cropped meshes coloured in different colours with the oriented bounding boxes of the IFC-elements}\vspace{15pt}
%     \end{subfigure}
%   \caption{Cropping the meshes per element. }
%   \label{fig:cropmesh}
% \end{figure*}

\begin{figure*}[h!]
  \begin{subfigure}[]{0.49\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/axis_aligned_bounding_box.JPG}
        \caption{The axis aligned bounding box of the mesh}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.49\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/creation_grids.JPG}
        \caption{two 2D grids, grid+ and grid-, on the axis aligned bounding box of the mesh}\vspace{15pt}
    \end{subfigure}
  \caption{Overview of session 101-0366 and session 101-0367 remote sensing data and BIM model. }
  \label{fig:2dGrid}
\end{figure*}

\subsubsection{Technological issues}
\label{Technological issues}
Analogue to construction monitoring, there is a lot of data involved due to frequent documentations of the site. In road construction, the need for localised data cubes is even higher since construction projects can span over several kilometers. For instance, both flights already generate over 1k resources with 50k meshes for this relatively small project. This quickly becomes problematic considering the $O(n^3)$ computational complexity in relation to the XY dimensions multiplied by the number of objects. Without optimization, a relatively sparse grid sampling of 0.05m already results in a three-dimensional matrix of 64M values, which is simply unscalable to larger projects. The testcase concluded that a resolution of 1-2.5cm is optimal given the average accuracy of the RTK-UAV and SfM process, which is also around 2-3cm. On average, the volume assignment to the different BIM objects was accurate up to 3\% of a manual investigation in commercial software. However, the outer border of the SfM project, beyond the dimensions of the bundle adjustment, indicated a steep decline in accuracy and should be barred from the evaluation (Figure~\ref{fig:colored_pointcloud}). 
% figuur met rode rand volumes
% figuur met gefilterede vegetatie 

\begin{figure*}[h!]
    \centering 
        \includegraphics[width=1\textwidth]{Figures/Colored_pointcloud_two_epochs.JPG}
  \caption{The volume calculation between two epochs, where the border is colored darkred or green because of bad SfM at the borders.}
  \label{fig:colored_pointcloud}
\end{figure*}

\begin{figure*}[h!]
  \begin{subfigure}[]{0.99\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/Pointcloud_not_filtered.jpg}
        \caption{The not filtered point cloud.}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.99\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/Pointcloud_filtered.jpg}
        \caption{The filtered point cloud, where brown points are ground points.}\vspace{15pt}
    \end{subfigure}
    \begin{subfigure}[]{0.99\textwidth}
    \centering
        \includegraphics[width=1\textwidth]{Figures/mesh_filtered.jpg}
        \caption{The filtered mesh where only ground points are left.}\vspace{15pt}
    \end{subfigure}
  \caption{Filtering the RTK-data to the ground points. }
  \label{fig:filtering_RTK_data}
\end{figure*}

Road construction sites are plagued by severe occlusions due to vegetation, temporary stockpiles of materials or the presence of site vehicles and equipment. This can be solved by semantically segmenting the ground points from the photogrammetric point clouds obtained by the RTK UAV. The meshing then interpolates the holes so a closed and consistent model is obtained of the site (Figure~\ref{fig:filtering_RTK_data}). By performing the volume calculation over the entire site, we gain insight into temporary stockpiles between two epochs. When there are strong local changes but almost no changes in total, it may indicate movement of stockpiled soil or the like.

Another problem is that the accuracy of the calculations depends on the accuracy of the georeferenced output. The advantage of working with RDF graphs is that this accuracy of the mesh can be included in the analysis, which provides important nuance to the results and also allows for the validation of the flight.

\subsubsection{GEOMAPI contribution}
GEOMAPI's contributions again include on the one hand making this analysis easily accessible through the use of optimisations, and on the other hand better managing what information is provided to a given analysis and generalising the results so that they can be reused.

\textbf{RDF serialization}: The sessionNodes describe the metadata of over 700 resources of both flights and the BIM model. This includes Nodes for the geolocated imagery, point cloud, polygonal mesh and orthomosaic data that were processed by Metashape. The XML with interior and exterior camera parameters is parsed by GEOMAPI and stored in the ImageNodes. Notably, the RDF Graph serialization of the BIM data is conform the Flemish Agency of road construction (AWV) that will switch to Linked Data principles for road asset management in the near future. For instance, each element is designed in a different layer conform the AWV Object Type Library (OTL), through which it can be linked to the AWV accuracy requirements in planimetry and altimetry. It is therefore important that the accuracy of the georeferenced outputs is included in the RDF graphs as described in the technological issues. Additionally, in the RDF graphs of the IFC per element, a link to the quantity take offs can also be placed to automate billing in this way as mentioned in ~\ref{VolumeTools}.

\textbf{Volume extraction}: GEOMAPI introduces the necessary sparsity by forming data cubes of only the relevant parts of the flight depthmap $\boldsymbol{D}_{f_i}$ for each BIM object. Dense matrix calculations are retained due to their superior optimization compared to sparse matrices for smaller dimensions. This reduces the computational complexity quadratic propagation over the entire project to propagate only over each objects dimension. Additionally, depth maps of the BIM objects only need to be computed once and are then stored on drive for the duration of the project. GEOMAPI also facilitates the integration of resource accuracies to have more nuanced analyses i.e. the camera pose accuracy and RTK positioning. 

\begin{figure*}[h!]
    \centering 
        \includegraphics[width=1\textwidth]{Figures/101_0365_overview.JPG}
  \caption{Overview of the sessionNode from session 101-0367 }
  \label{fig:sessionNode}
\end{figure*}

\textbf{Multi-temporal detection}: The volume changes between the sessions are established given the data cubes of two sessions and the linked volumes of the BIM elements. Analogue to the construction monitoring, objects that have a low theoretical visibility or are already have 100\% of their volume detected can be filtered from the analysis. Additionally, construction stages can be used to limit the number of assessed BIM objects. GEOMAPI also implements functions to display volume changes in various manners i.e. with gradient color charts (Fig~\ref{fig:colored_pointcloud}). This improves the communication between stakeholders because not only values are shown but also where these changes are made. 

\subsubsection{conclusion}
Using the GEOMAPI toolbox to process, visualize and manage remote sensing data to compute and track volume changes on object level contributes to the lifespan of the data and performed analysis. GEOMAPI speeds up calculations by efficiently using matrix queries, multiprocessing and data cubes. In future work, the volume changes will be combined with material assessment and geometric analysis methods to nuance the volume changes and better establish progress.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Discussion \& Limitations}\label{sec:Discussion}
In this section, we reflect upon GEOMAPI's capabilities including the accessibility of remote sensing data for construction applications, using multiple data types for construction applications, processing and analyzing spatio-temporal datasets and the Linked Data framework.

\subsection{Discussion}
Using metadata instead of actual data significantly speeds up all analyses. One could argue that structuring all remote sensing data in a common database is even more performant, but that would result in yet another unique dataformat with a reduced longevity. By only standardizing the metadata and leaving the remote sensing data in their original formats, GEOMAPI attempts to embrace the best of both worlds.

The four testcases currently integrate point cloud and polygonal mesh geometries. This a straightforward conversion either by sampling or meshing and thus most utility methods accept both inputs. Integration of ImageNode resources is slightly more complex and is being worked on right now. The ImageNodes functionality is already demonstrated through the sessionNodes but the computer vision methods still have to be implemented. 

Three out of four testcases include a multi-temporal analysis, whether it is design-vs-remote sensing or measurement epoch vs measurement epoch. The testcases only show two epochs but the linked data serialization of the results ensures efficient scalability to multiple timeframes. 

The RDF serialization currently is local. Higher performances can be achieved by storing the graphs in triplestores or using event streams, at least for high-traffic applications such search engines or continuous sensor measurements. In contrast, remote sensing data is only sporadically consulted and the more frequent a dataset is consulted, typically how smaller it is. For instance, weekly site documentations do not contain as much data as a sporadic complete site documentation for project planning. By storing RDF graphs locally together with the remote sensing data, it's longevity also increases as this serialization requires little to no maintenance. Currently, GEOMAPI is compatible with such databases but does not plan to implement it. 

\subsection{Limitations}
A first limitation of GEOMAPI is it's attempt to describe any geospatial resoure in a standardised manner. As more resources are included, the harder it becomes to maintain this standardization. Even now, some resource and function definitions are deeply tied to popular API's such as OpenCV and Open3D. To preserve portability of GEOMAPI, we are currently investigating to which extend the tools should be part of the GEOMAPI core and not separate packages to limit the dependencies. However, by fracturing the API, it would become less portable by itself and less straightforward to use by AEC stakeholders which are the primary beneficiaries. 

A second limitation is the inclusion of machine learning methods in GEOMAPI. We are currently developing tools that leverage machine learning and deep learning concepts for various AEC tasks. However, we choose not to include them directly into GEOMAPI because of the severe hardware and software dependencies that are tied to the different processing toolboxes such as Pytorch and Tensorflow. We currently limit ourselves to CPU processing to lower the number of dependencies but GPU processing is needed to process geospatial inputs. Other packages split their API into multiple packages to deal with this problem, but as previously mentioned, this makes it less accessible for AEC stakeholders. 

In future work, we will continue to expand the GEOMAPI functionality. To facilitate this expansion, node templates are defined to facilitate node creation. For the linked Data, transformation methods, that are currently hard-coded, will be made part of conversion classes in the ontology to automate information exchange. This is especially useful for linking to external sources including the bill of quantities, tolerance spreadsheets and so on. For the methods themselves, we are currently working on machine learning methods for semantic segmentation and computer vision and how to use GEOMAPI to facilitate early data fusion. 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Conclusions}\label{sec:Conclusions}
In this paper, we address the construction industry's need to efficiently process and manage remote sensing data. More specifically, we propose a highly performant and accessible Python Package GEOMAPI that bridges the gap between various remote sensing data including geolocated imagery, polygonal meshes, point clouds and orthomosaics and popular remote-sensing driven construction applications such as construction monitoring, maintenance and documentation. GEOMAPI builds upon two principles to go beyond the state of the art: (1) Earth observation data cube principles are formulated for close-range remote sensing using resource Nodes that describe an asset's data and metadata and (2) all metadata and analysis results are managed with Linked Data techniques, enabling much needed standardisation that speeds up analyses and increases documentation longevity. This paper contains the detailed explanation of the GEOMAPI core including the utilities, node system and Linked Data principles. GEOMAPI is also fully documented in a wiki to maximise reproducability.

The testcases show the flexibility of the API to automate tasks throughout a construction's life cycle. Four construction applications are tackled with GEOMAPI functionality including progress and volume estimations during construction, as-built documentation validation upon delivery and keeping facilities up to date during the operation stage. Overall, GEOMAPI's node system to formulate data cubes of close-range remote sensing data drastically reduced the computational burden of the operations. Furthermore, analyses results could easily be consumed by multi-temporal tasks as the resulting graphs can be queried in a standardised manner. In addition to these results, the process parameters are stored as well to make downstream processes more nuanced and understandable. 

The AEC industry will benefit greatly from GEOMAPI. The presented package offers stakeholders a great many tools to -for the first time- jointly process and manage close-range sensing observations. Images, meshes, point clouds and other frequently used observations can be easily manipulated, interchanged and linked to Building Information Modeling resources. Furthermore, as GEOMAPI incorporates linked data techniques, the observations can also be linked to analysis results which is crucial for multi-temporal analyses. GEOMAPI can be employed for a plethora of geospatial AEC tasks a construction's life-cyle, and lower the threshold for doing so. 


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\authorcontributions{Maarten Bassier, Jelle Vermandere, Sam De Geyter, Heinder De Winter and Maarten Vergauwen contributed equally to the work.}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\funding{This project has received funding from the VLAIO BAEKELAND programme (grant agreement HBC.2020.2819) together with MEET HET BV, the VLAIO COOCK project (grant agreement HBC.2019.2509),  the VLAIO BAEKELAND programme (grant agreement HBC.2022.0153) together with BAUWENS NV, the FWO Postdoc grant (grant agreement 1251522N) and the Geomatics research group of the Department of Civil Engineering, TC Construction at the KU Leuven in Belgium.}

\conflictsofinterest{There are no conflicts of interest to report.}

%=====================================
% References, variant A: internal bibliography
%=====================================
\reftitle{References}
\bibliography{bibliography}

\end{document}

\documentclass{isprs}
\usepackage{subfigure}
\usepackage{setspace}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{geometry} % added 27-02-2014 Markus Englich
\geometry{a4paper, top=25mm, left=20mm, right=20mm, bottom=25mm, headsep=10mm, footskip=12mm} % added 27-02-2014 Markus Englich
%\usepackage{enumitem}
\usepackage{listings}
%\usepackage{isprs}
%\usepackage[perpage,para,symbol*]{footmisc}

%\renewcommand*{\thefootnote}{\fnsymbol{footnote}}



\begin{document}


\title{
Texture-based separation to refine building meshes 
}

% KAO: Remove extra spacing
\author{
 Jelle Vermandere\textsuperscript{1}, 
 Maarten Bassier\textsuperscript{1},
 Maarten Vergauwen\textsuperscript{1}
}

% KAO: Remove extra newline
\address{
	\textsuperscript{1} KU Leuven, Department of Civil Engineering, Ghent, Belgium \\
    (jelle.vermandere, maarten.bassier, maarten.vergauwen)@kuleuven.be\\
}

% If the corresponding author is NOT the final author, always add a % space before the subsequent comma, i.e.
% first author name\textsuperscript{a,}\thanks{Corresponding author} , % second author name \textsuperscript{b}, etc.
% thanks to Niclas Borlin 05-05-2016

% +-200 words
\abstract
{
% a quick overview
% what is the problem? how is it currently solved? what are the current drawbacks? what is needed for a good solution?
% what does this paper do? how does it work? what are the results?
In this paper, we present a texture-based separation approach to refine building meshes, which aims to address the challenges of detecting and isolating different objects in an indoor scene mesh. We propose a novel segmentation model based on the materials of the different parts of the scene. The proposed approach uses factorization-based texture segmentation to separate the different materials in the meshes and detect the edges on the segmented texture. To prevent large faces containing multiple materials from being segmented wrongly, the mesh is cut along the texture boundaries and new faces with a single material are created. Finally, we segment the new faces based on the material index and neighbouring faces using a region-growing algorithm. We evaluate the proposed approach on the Matterport indoor dataset, which shows that our approach performs well on detecting boundaries between distinct materials, but over-segments on complex shapes. Our proposed method improves the segmentation of large flat surfaces like posters or rugs.
}

\keywords{Mesh, segmentation, texture separation, edge detection, UV mapping}

\maketitle


%=====================INTRODUCTION=====================%
\section{Introduction}
\label{sec:introduction}
% current segmentation models rely on 
% We want to segment a mesh based on it's material and color, instead of pure geometric detail.
% We can use texture detection and grouping for that.
% But meshes are very sparse, so we need to slice them at the right place to ensure they are part of the correct segment

% Intro structure:
% The segmentation problem
Indoor scene segmentation of environments in the Architecture, Engineering and Construction (AEC) industry is a field of ongoing research. Current state-of-the-art data acquisition techniques can record highly detailed and dense 3D data but lack the required information layers to efficiently use the building data in applications \cite{DeGeyter2022}. One of these extra information layers is object segmentation, where the captured environment is split into distinct parts. This problem has been approached from many angles before \cite{Mao2022} and although the majority of the scenes can be automatically segmented, there are still a large number of edge cases where the segmentation process returns invalid results. These exceptions require tedious and labour-intensive human intervention to achieve good results.

% How does segmentation currently work?
% 2D method
Current segmentation methods largely rely on one of two data types. The first data types are 2D images, where by using trained networks, it is possible to find objects displayed on the images \cite{Zhang2021}. While these methods can accurately and precisely segment a scene, due to occlusions caused by overlapping objects and limited 3D representations, they provide less reliable results in more complex environments. Because the segmentation is only performed on a 2D image, it is difficult to accurately predict the 3D location of the object boundaries. 

% 3D segmentation
The second group uses 3D information as a base to segment the scene into 3D bounding boxes or regions. These mostly rely on geometric features like point locations or normals \cite{Schult2022}. Due to the need for a standardised input in neural networks, the 3D segmentation is mostly performed on normalised point clouds or voxel occupancy grids. Using points allows the data to be easily sub-sampled to the correct density, this is why most datasets which do not use this format get converted before the segmentation process. This is especially troublesome for data types like meshes, where a large amount of detail is often simplified in the geometry and only visible in the texture. 

% The sparse mesh Problem:
Meshes are an efficient way to store data compared to point clouds and are better at representing surface and texture detailing (fig.\ref{fig:introExample}). However, detecting and segmenting different objects in an indoor scene mesh can be challenging due to the sparsity of the faces. This sparsity can lead to ambiguity in large faces which are supposed to be part of multiple objects \cite{Bassier2020}.

The textures of a mesh are stored on a 2D image, where each vertex contains a UV coordinate which can be mapped onto the image \cite{Heckbert1986}. The colour of the face is then calculated based on the UV position of its three vertices. This emphasis on geometric abstraction is done to save on storage and processing costs, something very desirable in most industries. By sampling the mesh into a point cloud the texture detail gets lost and is reduced to a single color per point. The point color is often used as an extra parameter in geometry-based segmentation. However, due to the complex patterns present in some materials like bricks or wallpapers, this extra parameter can lead to more confusion than clarity.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_intro_example.jpg}
\caption{3D mesh (left) and its corresponding texture map (right) }
\label{fig:introExample}
\end{figure}

% Our research goal
Our goal is to create a texture-based segmentation model that can cleanly segment a mesh. We propose using a texture-based boundary detection method on a texture map to segment the different zones of the textured mesh. To ensure every face only belongs to a single element, new edges are created at the detected boundaries. Each face is then assigned a material index and connected faces are grouped together. This will create a number of discrete zones, segmented solely by their similar texture and adjacency.

%The main contributions
The main contributions of this work are twofold: The first is a novel segmentation model based on the individual materials of the different objects in the scene. Second, the subsequent face splitting based on the detected material patches prevents faces from being part of multiple segments.  

% The structure
The remainder of this work is structured as follows. The background and related work is presented in Section \ref{sec:background}. Following is the explanation of the proposed method in Section \ref{sec:methodology}. In Section \ref{sec:experiments}, an overview of the used datasets and their results is presented. Finally, the conclusions are presented in Section \ref{sec:conclusion}

%=====================BACKGROUND=====================%
\section{Background and related work}
\label{sec:background}

\subsection{Instance Segmentation}

% instance segmentation methods
Most of the state-of-the-art mesh segmentation models rely on the conversion to a normalized point cloud to perform the actual segmentation and return an oriented bounding box or point mask \cite{Schult2022}. While these data formats are sufficient for point clouds, these can lead to inconsistent results when converted back to meshes. Meshes require a vertex or face-based segmentation to determine the boundaries between objects \cite{Kaplansky2009}. 

Existing texture-based segmentation models rely on 2D images of the scene \cite{Zhang2021}. These do provide good and accurate results when the resulting masks are projected on the mesh, but due to the limited coverage of a single image, there are still some drawbacks. The single viewpoint of an image leads to a lot of occlusions which the segmentation model cannot predict. This can lead to either unsegmented parts, or wrongly segmented parts due to projecting through the geometry. To combat these occlusions, multiple images are used that are taken from different angles. While this solves the occlusion problem for the most part, it becomes very difficult to get a consistent segmentation index throughout the set due to variations in angles and lighting conditions. These problems do not arise when trying to segment a texture map, since every face is uniquely represented on the map. It is, however, much more difficult to detect objects in a randomly distributed patchwork of textures.

\subsection{Texture Segmentation}

Aside from image-based object segmentation, it is also possible to perform segmentation based on different types of textures or materials. Rather than trying to find each instance of a type of texture, these texture segmentation models find all similar patches of a single texture and group them. The field of texture segmentation has been researched thoroughly and already gives good results. Factorisation-based texture segmentation (FTS) \cite{Yuan2015} can efficiently segment different textures in images. By using local spectral histograms as features, this method relies on singular value decomposition and non-negative matrix factorisation to discriminate region boundaries. The resulting masks provide a great starting point for edge detection. 

% edge detection
When trying to find the boundaries of objects in a 2D image, a common technique used is edge detection. It can be performed by a number of performant detection methods like  Canny \cite{Ding2001} or Holistic Edge Detection (HED) \cite{Xie2015}. While these work great to detect all the edges in an image, that may not always be the end goal. This is where the Factorisation-based texture segmentation can provide a solution with its simple mask representation.

\subsection{Mesh Slicing}

Cutting meshes using lines can be performed in several ways. Popular methods rely on using planes as a virtual knife \cite{Minetto2017}. By determining which points of the face are in front or behind the section plane, new vertices can be created at the intersecting lines. Using a 2D pixel line however becomes a bit more complex, because it is no longer possible to easily determine which points are in front or behind a pixel line.

% Hough lines
Cutting a plane with a line requires a parameterised representation of pixel-based lines. Hough lines \cite{Illingworth1988} can simplify the detected edges by finding straight lines through the selected pixels. There are a couple of variations, each with distinct advantages. Hough lines are determined by finding a range of pixels that are close to forming a straight line. Hough lines are always looking for lines that cover the entire image, so smaller lines are harder to detect. This is where PHough lines can provide better results. These can detect smaller line segments in an image and provide a start and end point.

\subsection{Clustering}

% Region growing
Clustering a collection of similar objects can be achieved using different methods like k-means, DBScan or region growing clustering \cite{Xu2015}. 
k-means clustering \cite{Xu2015} finds an optimal grouping with a set amount of clusters with the lowest total point-to-centroid distance. While this method is often used to find a set amount of clusters in a collection, we do not know the amount of needed clusters beforehand. 
DBSCAN clustering \cite{Xu2015} does not rely on a predefined amount of clusters, rather, it tries to group objects based on their density. Due to its focus on density, it is able to non-linearly separate different clusters.

Existing region growing implementations largely focus grouping elements based on a single parameter like pixel color of surface normal \cite{Fan2005}. Region growing is performed by choosing a random starting seed and making a comparison against neighbouring elements. If those elements are similar, they are grouped together. This method works great to detect connected components, since different objects in a scene could have a similar material, but they should not be part of the same group.


%=====================METHODOLOGY=====================%
\section{Methodology}
\label{sec:methodology}

% - Use Texture segmentation to separate the different materials in the meshes
% - Detect the edges on the segmented texture
% - Convert the detected edges to parametrised hough lines
% - project the UV coordinates of the mesh onto the UV plane
% - Cut the faces with the detected hough lines
% - Define the new faces with the edge boundaries
% - segment the new faces based on the segmentation index and neighbouring faces

The proposed texture based segmentation is performed in a series of steps outlined in figure \ref{fig:workflow}. Where first, the UV texture map is extracted, then the different materials are segmented. After which the edges are extracted, from which the Hough lines can be determined, those are then used to slice the faces of the mesh to create clear boundaries. Finally the texture zones are used to segment the 3D mesh with the use of a region growing algorithm. 

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_workflow.jpg}
\caption{An overview of the proposed workflow.}
\label{fig:workflow}
\end{figure}

To illustrate the full methodology, a simplified textured mesh is used with a clear boundary as seen in fig. \ref{fig:methodData}. The mesh represents a wall-floor connection where the edge between them does not align with any existing geometry. Something which is encountered frequently in large datasets and will be evaluated in the experiments section.


\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_MethodologyMesh.jpg}
\caption{3D mesh (left) and its corresponding texture map (right) }
\label{fig:methodData}
\end{figure}

\subsection{Texture Segmentation}
\label{subsec:texseg}

% using the texturemap?
The full texture map of the mesh is rarely used to segment the scene, mostly because of the lack of a one-to-one relationship between the object's location in 3D space and on the 2D texture map. This often leads to disjointed parts of a single object spread around on the UV plane. Instead of mainly focusing on the specific object's boundaries, it is much easier to search for patches of similar materials on the texture map. The segmented objects can then be detected by their material instead of geometric shape.


%using the fts
We use factorization-based texture segmentation (FTS) \cite{Yuan2015} to detect the different materials in the meshes. The algorithm is fine-tuned with a number of parameters: the window size, segmentation number, the omega value and the non-negative constraint. The window size determines the sample area to group textures, for our purpose, a window size of $10 px$ was chosen to allow for the most common construction textures to  be properly segmented. Fig. \ref{fig:fts} shows an overview of the different window sizes and their accuracy. The segmentation number was left at $-1$ to allow the algorithm to dynamically chose the number of texture segments. The omega value was set to $0.2$, to allow it to distinguish between the the different types of homogeneous materials, while still being able to properly group the heavily patterned materials.

%gray problem
FTS is run on a grey-scale image, leading to some false matches between materials. To prevent similar patches of different colours from being grouped, all detected patches $F_i, F_j,...$ with the same label are compared using the average patch hue $h_{F_i}, h_{F_j},...$. When the patch hues are less than a certain threshold $t_h$ apart, they are considered as the same material and grouped as patch $F_i'$ as outlined in eq. \ref{eq:graycompare}. For our purpose, a $t_h$ value of $0.1$ was chosen, this can split the mesh more accurately and creates more distinct patches.

\begin{equation}
\label{eq:graycompare}
F_i` = \left \{ F_i, F_j \Big| \forall F_i,F_j \in \bold{F}: | h_{F_i} - h_{F_j} | \leq t_h \right\}
\end{equation}

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_FTSCompare.jpg}
\caption{FTS segmentation results: (top-left) the original texture, (top-right) over-segmented texture, (bottom-left) under-segmented texture, (bottom-right) correctly segmented texture.}
\label{fig:fts}
\end{figure}

\subsection{Edge Detection}

% Edge detection
Once the texture boundaries are defined, the edges can be detected. The Canny edge detector \cite{Ding2001} was chosen for its reliability and good results in similar applications. The segmented image is a collection of single-colour zones with very distinct edges, so the edge detector returns all the boundaries with near-perfect accuracy. Figure \ref{fig:canny} Shows examples of the detected edges from the segmented texture. A comparison is also made with the original textures to validate the accuracy of the texture boundaries. 

%texture spill precaution
It is important to note that due to the texture baking process, a small amount of texture spill can be present on the UV map at the edges of face groups as shown in figure \ref{fig:texture spill}. This problem can be avoided by only making use of detected edges that cross existing faces since there is no spill present there.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_EdgeDetection.jpg}
\caption{(left) The segmented Image, (right) Canny edge detection results.}
\label{fig:canny}
\end{figure}

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_textureSpill.jpg}
\caption{A zoomed in view of texture spill as a result from the texture baking process.}
\label{fig:texture spill}
\end{figure}

% Hough lines
The above-mentioned edge detection algorithm provides a binary raster image to represent the edges. Because our end goal is slicing the faces of the mesh with the detected edges, the edges need to be parameterized into straight-line equations. We can determine these straight lines by applying a Hough transform detection to the binary edge image \cite{Illingworth1988}. These lines $l$ are defined by their polar coordinates $(r_\theta,\theta)$

Because Hough lines are always represented as lines, they cross the whole image. Using the whole line to slice the faces of the mesh would create a large number of unnecessary faces. This is why Probabilistic Hough Line Transforms are chosen for smaller line segments which are defined by a couple of coordinates $(x_0,y_0,x_1,y_1)$. Both methods are compared in fig. \ref{fig:hough lines}.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_Hough.jpg}
\caption{(left) The standard Hough lines, (right) the Probabilistic Hough lines indicated in blue.}
\label{fig:hough lines}
\end{figure}

\subsection{Triangle Mesh Slicing}

After detecting the edges, we need to create new triangles at the boundaries of the different materials. To do this, we first project the faces $f_i$ of the mesh onto the UV plane as seen in fig. \ref{fig:uvproject}. We then use the detected lines $l$ from the previous step to slice the faces. We define the new faces $f_i'$ with the newly created edge boundaries $e_l$ and assigned them to the appropriate material index $i$.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_method_UVProject.jpg}
\caption{An Image of the projected faces of the mesh on the UV plane. The orange lines indicate the edges of the mesh.}
\label{fig:uvproject}
\end{figure}

The slicing process is performed in a series of steps. First, we check which faces $f$ are intersecting with the lines $l$, this is performed by a 2D raycast $r_l$ in the direction of the line. Once the intersected triangles $f_r$ are determined we perform a second intersection test, now on a per-edge basis to determine how many edges $e$ are intersecting.

There are 4 cases when checking for an intersection between a triangle and a line as seen in figure \ref{fig:newtriangleMethod}. Each case creates a different amount of new triangles:

\begin{itemize}
    \item No intersection:
    The line and triangle do not intersect, so no slicing is required.
    \item Point intersection:
    When the line intersects with one or 2 points and no edges, the original geometry does not need to be altered.
    \item Point-edge intersection:
    In this case, the line $p$ intersects the face $f_i$ with one point $p_{i}$ and the opposite edge $e_{i}$. This will result in 2 new triangles $f_{i1}, f_{i2}$, where the new point $p_{ei}$ is the intersection between $l$ and $e_i$. Both triangles are then defined by eq. \ref{eq:newTris}
    \begin{equation}
    \label{eq:newTris}
    \begin{gathered}
    f_{i1} = \left \{ p_{i}, p_{ei}, p_{i1} \right\} \\
    f_{i2} = \left \{ p_{i}, p_{ei}, p_{i2} \right\}
    \end{gathered}
    \end{equation}
    
    \item Edge-edge intersection:
    When the line intersects with 2 edges$e_{i}, e_j$, 2 new points $P_{ei}, p_{ej}$ are created. This results in 3 new triangles $f_{i1}, f_{i2}, f_{i3}$. The first triangle $f_{i1}$ consists of the 2 new points $p_{ei}, p_{ej}$ and the single existing point from the corner side $p_{i1}$. The second triangle $f_{i2}$ is created with the 2 new points $p_{ei}, p_{ej}$ and one of the existing edge points $p_{i2}$ on the edge side of the triangle. The last triangle is created with the 2 existing edge points $p_{i2}, p_{i3}$ and one new point $p_{ei}$. The order of the point allocation is critical to prevent a wrong combination of points. The new triangles are then defined by eq. \ref{eq:newTris3}
    \begin{equation}
    \label{eq:newTris3}
    \begin{gathered}
    f_{i1} = \left \{ p_{ei}, p_{ej}, p_{i1} \right\} \\
    f_{i2} = \left \{ p_{ei}, p_{ej}, p_{i2} \right\} \\
    f_{i3} = \left \{ p_{i2}, p_{i3}, p_{ei} \right\}
    \end{gathered}
    \end{equation}
\end{itemize}

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_method_NewTriangles.jpg}
\caption{The four cases of a line-triangle intersection. Top-left shows no intersection, top-right shows a point intersection, bottom-left shows a point-edge intersection and bottom-right shows an edge-edge intersection.}
\label{fig:newtriangleMethod}
\end{figure}


After the new triangles are created on the UV plane as seen in figure \ref{fig:cutfacesuv}, they need to be transferred to 3D. Since the new triangles are subdivided from an existing triangle. only the new points need to be interpolated to their new 3D position. This is done though linear interpolation where $p_1, p_2$ are existing points with 2D coordinates $p_i(x,y)$ and 3D coordinates $p_{i,3D}(x,y,z)$. The new point $p_e$ is only defined by its 2D coordinate $p_e(x,y)$ and its 3D coordinate $p_{e,3D}(x,y,z)$ can be determined according to eq. \ref{eq:interpolation}. The new normals $\Vec{n_{pe}}$ are determined in a similar way.

\begin{equation}
\label{eq:interpolation}
p_{e,3D} = p_{1,3D} + \frac{p_e - p_1}{p_2 - p_1} (p_{2,3D} - p_{1,3D}) \\
\end{equation}

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_method_UVCutfaces.jpg}
\caption{The resulting faces after the pHough lines are used to slice the faces which were overlapping with the lines. The rest of the faces remain unaffected.}
\label{fig:cutfacesuv}
\end{figure}


\subsection{Segmentation}


After the new faces are constructed, the segmentation can be performed without any risk of multiple labels on a single face. The segmentation is performed by a region-growing technique where each face is given a segmentation index $i$. This index is based on the FTS detection from \ref{subsec:texseg}. Because the segmented patches do not take adjacency into account, if we were to simply group the faces based on that index alone, different objects with the same material would also be grouped. To prevent this, we start by choosing a random face $f_i$ from the mesh and comparing each adjacent face $f_{i,adj}$ for an identical segmentation index $i$. This process is repeated recursively for each adjacent matching face. Once no more matching faces can be found, the patch is complete and another random, un-grouped, face $f_j$ is chosen.

To prevent over-segmenting small detailed objects with a too sporadic texture that cannot be grouped by FTS, there is also a minimal surface area check. For our purpose, we chose a minimal surface area of $1
0cm^2$ which allowed our method to group highly irregular textures while still being able to distinguish the most common indoor objects.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_segmentedmesh.jpg}
\caption{Left: the new faces, each marked with their respective segmentation index based on the texture zone. Right: The resulting segmented mesh.}
\label{fig:segmentedmesh}
\end{figure}

%=====================EXPERIMENTS=====================%

\section{Experiments}
\label{sec:experiments}

In this section, we evaluate our segmentation algorithm. First, the datasets and testing metrics are discussed. Then we evaluate the texture boundary detection and instance segmentation separately.

\subsection{Datasets and metrics}
\label{subsec:exp_datasets}

To evaluate the effectiveness of our proposed method, we conducted experiments on the Matterport \cite{Chang2017} dataset. This was chosen because of its size and already available metrics and reference studies. The large scenes were subdivided as seen in fig. \ref{fig:dataset} to increase the processing speed and reduce the complexity. Each zone has +- 200.000 points and 350.00 triangles. 

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_EXP_Dataset.jpg}
\caption{The Matterport dataset mesh}
\label{fig:dataset}
\end{figure}

\subsection{Texture boundary detection}
\label{subsec:exp_boundarydetection}

Because our method heavily relies on clearly defined boundaries between the textures. The textures need to be segmented accurately. The results show this method works well on large surfaces with repeated or small texture detail (fig. \ref{fig:newedge}). This is mostly because of the way the UV maps are laid out. Because large flat surfaces remain connected on the UV map, it becomes easier to segment them.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_EXP_newedge.jpg}
\caption{A close up example of a new edge generated by the texture boundary detection. This allows us to clearly assign each face with a unique label}
\label{fig:newedge}
\end{figure}

% The good results

Our method also performs well on large uniformly textured surfaces, like painted walls or floors, something which is also detectable with only edge detection. Our method, however also shows it is capable of segmenting more complex textures like brick walls or wood textures. The newly created edges provide a clear distinction between the different texture zones. Due to the PHough lines, they do not add any useless cuts in irrelevant areas. Fig. \ref{fig:resultWall} shows a good result on a largely uniform texture.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_result_wall.jpg}
\caption{Wall detail. (top-left) uv-texture, (bottom-left) original mesh, (top-right) new detected edges, (bottom-right) sliced mesh}
\label{fig:resultWall}
\end{figure}

% the bad results

Some patterns are hard to separate due to subtle colour shifts in the image. This causes inconsistent boundaries in the detected zones. The method also struggles with very dense texture maps, where a lot of the faces are laid out separately and do not cover a lot of surface area. fig. \ref{fig:resultCarpet} shows our method struggling with the clear boundaries that are still detected despite the texture segmentation. The irregular large patterns are not detected as a single texture, so the lines are still present in the final detected lines.

% Show images of the segmented dataset
\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_result_carpet.jpg}
\caption{Carpet detail. (top-left) uv-texture, (bottom-left) original mesh, (top-right) new detected edges, (bottom-right) sliced mesh}
\label{fig:resultCarpet}
\end{figure}



\subsection{Instance segmentation}
\label{subsec:exp_segmentation}

The results of our method are outlined below. From the results acquired in subsection \ref{subsec:exp_boundarydetection} it became clear this method would perform well on simple scenes where objects are largely composed of single materials as seen in fig. \ref{fig:resultDoor}.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_result_door.jpg}
\caption{Door detail. (top-left) uv-texture, (bottom-left) original mesh, (top-right) new detected edges, (bottom-right) sliced mesh}
\label{fig:resultDoor}
\end{figure}

% The good results

The method can detect geometry-less details on flat surfaces, something more traditional segmentation models struggle with as seen in fig. \ref{fig:nogeosegment}.

\begin{figure}[!h]
\centering
\includegraphics[width=\columnwidth]{figures/GEO_EXP_NoGeo.jpg}
\caption{A segmented example of a element that is only present in the texture and not in the geometry}
\label{fig:nogeosegment}
\end{figure}

% the bad results

The method still over-segments on highly varied objects. Finding the balance between over-segmentation and mismatching has proven tricky and has to be evaluated on a scene by scene basis.

% Show images of the segmented dataset


\section{Conclusions}
\label{sec:conclusion}

% evaluate the finished product
In this paper, we proposed a novel method for refining building meshes using texture-based separation. Our method leverages texture segmentation to segment the different materials in the mesh and create new triangles at the boundaries of the materials to refine the mesh. We also proposed a novel object segmentation method that uses region-growing to group faces based on their common texture. Our method outperformed other state-of-the-art mesh-based and point-based segmentation methods on meshes with sparse geometric detail and high texture detail. However, the method still performs worse on non-inform objects. This method could be valuable as a supplementary segmentation model to increase the overall accuracy. Future work could include improved ways of unwrapping the textured meshes for better grouping of similar faces.

\section*{ACKNOWLEDGEMENTS}\label{ACKNOWLEDGEMENTS}

This project has received funding from the FWO-SB grant (grant agreement 1S16923N ) and the Geomatics research group of the Department of Civil Engineering, TC Construction at the KU Leuven in Belgium.


{
	\begin{spacing}{1.17}
		\normalsize
		\bibliography{export} % Include your own bibliography (*.bib), style is given in isprs.cls
	\end{spacing}
}

\end{document}

\begin{equation}
\label{eqremoval}
% de deelverzameling P' bestaat uit: voor alle punten p_i in P, voor alle punten q_j in Q geldt: als de afstand tussen p_i en q_i kleiner is dan t_d
P' = \left \{ p_i \Big| \forall p_i \in P, q_j \in Q : \argmin\limits_{q_j}\|p_i-q_j\| \geq t_d\right\}
\end{equation}

% ---------------------------------------------------------------------
% EG author guidelines plus sample file for EG publication using LaTeX2e input
% D.Fellner, v2.04, Dec 14, 2023


\title[Semantic UV mapping to improve texture inpainting for 3D scanned indoor scenes]%
      {Semantic UV mapping to improve texture inpainting for 3D scanned indoor scenes}

% for anonymous conference submission please enter your SUBMISSION ID
% instead of the author's name (and leave the affiliation blank) !!
% for final version: please provide your *own* ORCID in the brackets following \orcid; see https://orcid.org / for more details.
\author[J. Vermandere et al.]
{\parbox{\textwidth}
    {\centering  
     J. Vermandere $^{1}$\orcid{0000-0002-7809-9798}, 
     M. Bassier $^{1}$\orcid{0000-0001-8526-8847},
     S. Cuypers $^{1}$\orcid{0000-0002-9196-2912},
     M. Vergauwen $^{1}$\orcid{0000-0003-3465-9033}
    }
\\
% For Computer Graphics Forum: Please use the abbreviation of your first name.
{\parbox{\textwidth}
    {\centering $^1$KU Leuven, Belgium\\}
}
}

% ------------------------------------------------------------------------

% if the Editors-in-Chief have given you the data, you may uncomment
% the following five lines and insert it here
%
% \volume{36}   % the volume in which the issue will be published;
% \issue{1}     % the issue number of the publication
% \pStartPage{1}      % set starting page


%-------------------------------------------------------------------------
\begin{document}

% uncomment for using teaser
% \teaser{
%  \includegraphics[width=\textwidth]{images/teaserImage.png}
%  \centering
%   \caption{The overview of the work}
% \label{fig:teaser}
%}

\maketitle
%-------------------------------------------------------------------------
\begin{abstract}
This work aims to improve texture inpainting following clutter removal in scanned indoor meshes. This is achieved through a new UV mapping pre-processing step that leverages semantic information from indoor scenes to more accurately align the UV islands with the 3D representations of distinct structural elements, such as walls and floors. Semantic UV Mapping enhances traditional UV unwrapping algorithms by incorporating not only geometric features but also visual features derived from the existing texture. This segmentation improves UV mapping and simultaneously simplifies the 3D geometric reconstruction of the scene after the removal of loose objects. Each segmented element can then be reconstructed separately, using the boundary conditions of the adjacent elements. Since this is performed as a pre-processing step, other specialized methods for geometric and texture reconstruction can be employed in the future to further enhance the results.

\begin{CCSXML}
<ccs2012>
   <concept>
       <concept_id>10010147.10010371.10010396.10010398</concept_id>
       <concept_desc>Computing methodologies~Mesh geometry models</concept_desc>
       <concept_significance>500</concept_significance>
       </concept>
   <concept>
       <concept_id>10010147.10010371.10010382.10010384</concept_id>
       <concept_desc>Computing methodologies~Texturing</concept_desc>
       <concept_significance>500</concept_significance>
       </concept>
 </ccs2012>
\end{CCSXML}

\ccsdesc[500]{Computing methodologies~Mesh geometry models}
\ccsdesc[500]{Computing methodologies~Texturing}



\printccsdesc   
\end{abstract}  
%-------------------------------------------------------------------------
\section{Introduction}
% BACKGROUND: the need for indoor scene defurnishing
Empty 3D indoor environments, captured from real locations, are in high demand in the gaming and Architecture, Engineering, Construction, and Operations (AECO) industries~\cite{Vermandere2022}. These environments can be used for a wide variety of applications, such as remodeling, renovations, and interactive simulations.
% why meshes
These environments can be captured and processed using different methods. One of the more popular methods involves using a 3D scanner to capture a full 3D point cloud accompanied by panoramic images that add more information. For efficient consumption, these models are converted to meshes, which retain much of the geometric detail while also embedding the textural information of the surfaces~\cite{Bassier2024}.
% problem : not empty rooms
However, these environments are rarely empty when captured. Loose objects present during the capture process can lead to occlusions, either because they are placed against a permanent element or because they block the view of another part of the room. Current semantic instance segmentation methods can automatically detect these objects~\cite{Dai2018}, enabling an automated removal process. Removing these objects from the scene reveals occlusions and holes, resulting in an incomplete environment. Therefore, there is a need to complete these missing parts.

% The Main Problem: The need to reconstuct the holes, both geometrically and texturally
Holes and missing regions in meshes can be completed in two steps: first geometrically and then texturally.
%Geometric completion
Geometric hole filling has been a field of much research, leading to very robust tools and algorithms~\cite{Dai2018, Mittal2022, Boissonnat2014} for filling these holes.
% texture inpainting
Image inpainting has recently gained popularity due to the rise of diffusion models, which dramatically improve inpainting results~\cite{lugmayrRepaint}. However, there are still some obstacles to using this method on 3D model textures. The visual appearance of a 3D object is created by using a texture map, which projects the faces onto a 2D plane. This projection is called UV projection. The projection process creates a disconnect between the 3D mesh and the UV texture map~\cite{Vermandere2023}, as the location of a face in 3D space does not necessarily correspond to the same location on the UV plane. This means adjacent faces do not always remain adjacent in 2D.

% SOTA solutions
Current SOTA works approach this problem in different ways. 
% Inpaint from a 2D view of the 3D scene
Works like \cite{Gkitsas2021,Slavcheva2024, Wei2023} use a 2D inpainting approach to paint on the camera views and reproject them onto the mesh. While this works well for objects close to walls or distant from the camera, these methods struggle with large occlusions due to the complex room geometry and multiple objects covering the views.
% Inpaint on the 3D mesh
Other works~\cite{Flynn2022, Oechsle2019} aim to directly predict the color in 3D space. However, these models are limited in resolution due to the use of vertex colors or texture fields.

% our goal
The main goal of this work is to improve the UV projection of the scene by leveraging semantic instance segmentation to separate loose parts from the scene, as well as distinct structural elements like walls and floors. Using these masks, the loose objects can be removed from the scene, and the resulting missing geometry can be reconstructed element by element. Furthermore, the segmented structural elements also allow for better UV mapping, ensuring the resulting UV map more closely matches the 3D mesh, minimizing distortion and keeping adjacent faces together. This new UV map will improve the texture inpainting process, leading to a more visually appealing mesh.

\begin{figure*}[!h]
    \centering
    \includegraphics[width=\textwidth]{images/methodology.png}
    \caption{Overview of the proposed pipeline, starting with a furnished mesh (left), featuring the parallel scene segmentation and geometric reconstruction (top), and the semantic UV mapping and texture reconstruction (bottom) to result in an empty room mesh (right).}
    \label{fig:methodology}
\end{figure*}

%-------------------------------------------------------------------------
\section{Background and related work}
\label{sec:background}

\subsection{Texture inpainting}
% Image based
%gaussian texture inpainting \cite{Galerne2017}
%inpaint anything \cite{Yu2023}
%mask based large hole inpainting \cite{Li2022}
Restoring missing parts of an image has evolved from algorithm-based methods like Gaussian inpainting \cite{Galerne2017} to machine learning-based approaches like Inpaint Anything \cite{Yu2023} and Mask-Based Inpainting \cite{Li2022}. These approaches have the advantage of being able to predict the missing pixels based on the surrounding data instead of solely extrapolating a pattern from the image.

% object removal
%no shadow left behind \cite{Zhang2021} pure 2d inpainting
%PanoDR \cite{Gkitsas2021}
%Defurnishing of indoor panoramas \cite{Slavcheva2024}
The shift towards diffusion-based inpainting has enabled works like No Shadow Left Behind \cite{Zhang2021} to remove masked objects completely from a picture, including the object's shadows. PanoDR \cite{Gkitsas2021} and the work that builds on it \cite{Slavcheva2024} take this a step further by training the diffusion model on spherical panoramic images to enable direct object removal on 360° images. Because these models operate purely in 2D, they do not contain any 3D representation of the scene.

%3D results
%clutter detection and removal \cite{Wei2023}
%nerfiller \cite{Weber2023}
%free form surface texture inpainting \cite{Flynn2022} (vertex colors)
%texture inpainting for photographic models \cite{Maggiordomo2023}
Diffusion-based inpainting has also been used to remove objects from a scene in 3D. Clutter Detection and Removal \cite{Wei2023} inpaints both RGB and depth images from multiple viewpoints of a single object and reconstructs the 3D mesh in those missing parts. NeRFiller \cite{Weber2023} uses a similar approach but creates a Neural Radiance Field (NeRF) instead. These view-based models are limited by what is visible to the camera in a single view. Instead of using a camera view of the missing region, Texture Inpainting for Photographic Models \cite{Maggiordomo2023} uses dynamic UV mapping to ensure the missing region is always centered and surrounded by reference pixels to perform the inpainting, but it is limited to small areas.

%Not image based inpainting
The missing color can also be predicted directly on the mesh. STINet \cite{Flynn2022} directly predicts the vertex colors of the missing regions, while Texture Fields \cite{Chen2022} creates an implicit neural field to generate the missing regions. However, these methods are limited by the resolution of the geometry and struggle to generate fine details.

\subsection{Scene Segmentation}

%object detection
%Votenet \cite{Ding2020}
%V detr \cite{Shen2023}
%ptv3 \cite{Wu2023}
When trying to segment a scene, the different objects need to be detected. Works like Votenet \cite{Ding2020} and V detr \cite{Shen2023} use a point-transformer model \cite{Wu2023} to create bounding boxes for each distinct object. While these work well, they only detect objects.

%Scene
%spherical mask \cite{Shin2023}
%unscene3d \cite{Rozenberszki2023}
%Sai3D \cite{Yin2023}
Full scene instance segmentation takes this a step further by labeling every face. Works like Unscene3D \cite{Rozenberszki2023} can perform class-agnostic segmentation completely unsupervised. Sai3D \cite{Yin2023} also enables CLIP-based embedding to search for specific objects in the scene.

\subsection{UV mapping}

%Graphseam \cite{Teimury2020}
%auv-net \cite{Chen2022}
%nuvo \cite{Srinivasan2023}
%flatten anything \cite{Zhang2024}
%texture based mesh refinement \cite{Vermandere2023}
The biggest obstacle in using 2D inpainting methods on 3D meshes is the lack of a UV map that is both efficient and retains the face-adjacent relationships of objects in a scene. Graphseam \cite{Teimury2020} uses a Graph Neural Network (GNN) to automate the UV mapping process while retaining semantic seams, while Flatten Anything \cite{Zhang2024} uses point-wise mappings between the 3D points and UV coordinates. However, these methods are difficult to generalize to a large scene. Nuvo \cite{Srinivasan2023} aims to address this by optimizing the UV layout for the visible parts using a neural field. This largely overcomes the challenges posed by the complex geometry of reconstructed scenes.




%-------------------------------------------------------------------------
\section{Methodology}
\label{sec:methodology}
The proposed method as shown in Figure \ref{fig:methodology} consists of multiple steps: First the input mesh is segmented, and the segmentation masks are used for both element separation and UV seam creation. Second, The loose objects are removed and the segmented structural elements are all completed geometrically. Third, the UV map is unwrapped following the semantic seams. Fourth, the texture is inpainted in the newly created geometry. Finally, the texture is reprojected on the empty mesh.


\subsection{Scene segmentation}
The first step is segmenting the full scene as seen in Figure \ref{fig:methodology}a. Before the segmentation is performed, due to the limited resolution of the mesh, we cannot guarantee that each face is exclusive to a single object. This is why we first perform a Geometry refinement step \cite{Vermandere2023} to split the faces according to their texture. Both the loose objects and the structural elements are detected using UnScene3D \cite{Rozenberszki2023} Which uses geometric and colour features to generate pseudo masks, these masks are then refined using a self-trained model. Since the model is optimised for object detection, structural elements like walls can sometimes remain clustered. We also perform a RANSAC plane segmentation \cite{Korman2018} to refine the walls.

\subsection{Geometric Reconstruction}
The loose objects, detected in the previous step, are removed from the scene as illustrated in Figure \ref{fig:methodology}b. This results in large holes that need to be reconstructed. Before each segmented structural element is reconstructed one by one, the RANSAC planes, detected in the previous step, are used to determine the intersection edges between the elements. These edges form the boundary conditions for the Delaunay reconstruction\cite{Boissonnat2014}.

%Depending on the number of intersecting planes different boundary faces are created according to Figure \ref{fig:geometryReconstruction}.

%\begin{figure}[!h]
%    \centering
%    \includegraphics[width=\columnwidth]{images/geometryReconstruction.png}
%    \caption{The two-plane-intersection case (left) and the three-plane-intersection case (right)}
%    \label{fig:geometryReconstruction}
%\end{figure}
%
\subsection{Semantic UV Mapping}
After the geometry has been reconstructed, the newly generated faces are given the same label as the original element. The boundaries between the different segmentation labels are marked as UV seams. These seams serve as the basis for the semantic UV mapping as seen in Figure \ref{fig:methodology}c. Since the end goal is to inpaint the missing areas, we optimize the UV map to minimize distortion by using Least Squares Conformal Maps\cite{Levy2002}, while still aiming to keep as many faces as possible from the same label joined. This is largely possible due to the flat nature of the structural elements in indoor scenes. Furthermore, when inpainting textures the orientation of the image is also relevant. By introducing a Y/Z up consistency in our unwrapping method, each element is oriented consistently. where vertical elements like walls and beams are always oriented with the up-direction facing up on the image, flat elements like floors and ceilings have their forward direction facing up.

\subsection{Texture reconstruction}
The final step after the mesh has been semantically UV mapped is painting in the missing regions. This is performed on the 2D texture of each element. The newly generated geometry serves as the inpainting mask, this ensures that only the new parts are altered. The rest of the UV island serves as a reference for the diffusion-based inpainting \cite{lugmayrRepaint}. Because each element is inpainted separately, there is no confusion from other adjacent materials possible. After the texture has been inpainted completely, the texture is reprojected on the 3D mesh. Because the UV map was optimized for inpainting, and not for efficiency, the resulting UV maps can be very large. This is why, for a final step we repack the UV layout for optimized space efficiency while keeping the semantic islands intact.


%-------------------------------------------------------------------------
\section{Experiments}
\label{sec:experiments}

\subsection{Dataset}
For our experiments, we used the ScanNet++ \cite{Yeshwanth2023} and Matterport 3D \cite{Chang2017} Datasets as seen in Figure \ref{fig:datasets}. The ScanNet++ dataset contains 460 high-resolution 3D reconstructions of indoor scenes with dense semantic and instance annotations. The Matterport 3D Dataset is a scanned dataset that consists of 90 fully textured building-scale scenes, including semantic labels of the whole dataset. We focused on single-room scenes with moderately dense furniture, pre-labelled. These labels serve as the baseline for both the loose object removal and the semantic UV mapping.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/DatasetExample.png}
    \caption{A scene from the ScanNet++ dataset (left) and Matterport3D dataset (right)}
    \label{fig:datasets}
\end{figure}

\subsection{Object detection and removal}
For our experiments, the instance masks from the Matterport3D dataset are used to separate the mesh as seen in Figure \ref{fig:segmentation}. The labels do not always align perfectly with the objects, this is why we removed all the faces inside the bounding box of the objects. this ensured a clean-cut line. The RANSAC plane segmentation performed well on the walls and floors but had difficulty with more complex geometry.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/Segmentation.png}
    \caption{A room(left) and its segmented labels (right)}
    \label{fig:segmentation}
\end{figure}

The experiments have shown that the geometry completion performs better when each object is removed sequentially, rather than in parallel. Furthermore, overlapping objects can disrupt the planar detection, so they should be removed together, while this leads to a higher amount of existing data that is removed, the reconstruction results, illustrated in Figure \ref{fig:wireframe}, will be better.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/Wireframes.png}
    \caption{The object-removed room (left) and the reconstructed room (right)}
    \label{fig:wireframe}
\end{figure}

\subsection{Semantic UV Mapping}
The semantic segmentation has created the UV seams at not just geometrically distinct edges, but also texturally (Figure \ref{fig:segmentation}). Due to the simple geometry of the structural elements the uv maps can be created without too much distortion using the segmentation seams as seen in Figure \ref{fig:uvmapping}. We do see, however, that due to the orientation constraint, the UV maps are laid out separately for each object. This means the texture size is larger than the original texture map from the dataset.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/UV mapping.png}
    \caption{The new seams in the un-texture-completed scene(top-left), the original texture map (top-right) the new semantic UV layout for the floor (bottom-left) and the inpainted texture (bottom-right)}
    \label{fig:uvmapping}
\end{figure}

\subsection{Texture reconstruction}
The texture inpainting process is able to use the existing texture as an example which creates mostly indistinguishable textures for the more basic surfaces (\ref{fig:uvmapping}). The faces of the new geometry provide a clear bounding mask, allowing the inpainting to only affect the required area. The final result can be seen in Figure \ref{fig:result}

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/GeometryReconstruciton.png}
    \caption{The object removed textured room (left) final unfurnished room (right)}
    \label{fig:result}
\end{figure}

%-------------------------------------------------------------------------
\section{Discussion}
\label{sec:discussion}

The resulting empty scenes after the loose objects have been removed show believable results. This is helped by the fact that the structural elements in indoor scenes are generally straightforward. By reducing the geometric reconstruction to a planar triangulation, the problem becomes much less complex and manageable. This is however not possible for all types of structural elements. More organic shapes require a more complex Reconstruction like AUTO-SDF \cite{Mittal2022}. The advantage of our pre-processing pipeline is that the methods are interchangeable, while still retaining the advantages of the semantic segmentation.
The inpainted textures show very good results for repetitive and basic materials. However, more graphic elements that are not properly segmented can lead to artifacting in the final results.
The effectiveness of this method is however difficult to quantify due to the lack of real ground truth data. This is why we visually evaluated each scene, checking for visual consistency and believability.

%-------------------------------------------------------------------------
\section{Conclusion}
\label{sec:conclusion}
This paper introduced a novel pre-processing step in the object removal pipeline for indoor scanned environments. By semantically labelling the different elements in the scene, both the geometry completion and texture reconstruction can be improved due to clearer boundaries between the different elements. The holes resulting from the removal of the detected loose objects can be better completed element-wise, rather than for the whole scene. Using the predicted intersection lines between the different elements, we can clearly define the boundary conditions for the geometric Reconstruction. 
The semantic UV mapping also ensures each element is mapped as close as possible to its 3D representation, making the inpainting process much more straightforward. The existing, textured parts of the elements serve as a reference for the newly created geometry.

\newpage
%-------------------------------------------------------------------------
% bibtex
\bibliographystyle{eg-alpha-doi} 
\bibliography{export}       

% biblatex with biber
% \printbibliography                

\end{document}

% Version 2022-09-20
% update – 161114 by Ken Arroyo Ohori: made spacing closer to Word template throughout, put proper quotes everywhere, removed spacing that could cause labels to be wrong, added non-breaking and inter-sentence spacing where applicable, removed explicit newlines
% update – 010819 by Dennis Wittich: made spacing and font size closer to Word template, updated references and refernces style
% update – 042319 by Dennis Wittich: font size of captions set to 'small', first author names are shortened, hyphenation fixed
% update – 010620 by Dennis Wittich: Footnotes alignment set to left
% update - 151220 by Clement Mallet: Template adapted for double blind full paper submissions
% update - 060321 by Christian Heipke: Template refined for double blind full paper submissions
% update - 090921 by Christian Heipke: Template refined for double blind full paper submissions
% update - 200922 by Christian Heipke: general template update
% update - 080124 by Christian Heipke: general template update

\documentclass{isprs} % isprs class modified 23-04-2019 (Dennis Wittich)
\usepackage{subfigure}
\usepackage{setspace}
\usepackage{geometry} % added 27-02-2014 Markus Englich
\usepackage{epstopdf}
\usepackage[labelsep=period]{caption}  % added 14-04-2016 Markus Englich - Recommendation by Sebastian Brocks
\usepackage[british]{babel} 
\usepackage[hang]{footmisc}
\def\footnotemargin{1em} % added 08-01-2020 Dennis Wittich

%\usepackage[authoryear]{natbib}
%\def\bibhang{0pt}

\geometry{a4paper, top=25mm, left=20mm, right=20mm, bottom=25mm, headsep=10mm, footskip=12mm} % added 27-02-2014 Markus Englich
%\usepackage{enumitem}

%\usepackage{isprs}
%\usepackage[perpage,para,symbol*]{footmisc}

%\renewcommand*{\thefootnote}{\fnsymbol{footnote}}
\captionsetup{justification=centering,font=normal} % thanks to Niclas Borlin 05-05-2016
\captionsetup[figure]{font=small} % added 23-04-2019 Dennis Wittich
\captionsetup[table]{font=small} % added 23-04-2019 Dennis Wittich

\begin{document}

\title{Guided object completion with interactive voxel editing}
\date{}


% KAO: Remove extra spacing

% KAO: Remove extra spacing
\author{
 Jelle Vermandere\textsuperscript{1}, 
 Maarten Bassier\textsuperscript{1},
 Maarten Vergauwen\textsuperscript{1}
}

% KAO: Remove extra newline
\address{
	\textsuperscript{1} KU Leuven, Department of Civil Engineering, Ghent, Belgium \\
    (jelle.vermandere, maarten.bassier, maarten.vergauwen)@kuleuven.be\\
}

% If the corresponding author is NOT the final author, always add a % space before the subsequent comma, i.e.
% first author name\textsuperscript{a,}\thanks{Corresponding author} , % second author name \textsuperscript{b}, etc.
% thanks to Niclas Borlin 05-05-2016
% information on the corresponding author should not be used any longer and has been commented out
% C. Heipke, Jan 03,2024

% the use of the information of commissions and working groups should not be used any longer and has been commented out
% C. Heipke, Sept. 20,2022
%\commission{XX, }{YY} %This field is optional. If filled, XX and YY should be replaced by adequate numbers. See https://www2.isprs.org/commissions/
%\workinggroup{XX/YY} %This field is optional.
%\icwg{}   %This field is optional.


% KAO: Use times symbol
\abstract{

Object completion in 3D scanned indoor scenes remains a challenging problem, as most current approaches either focus on completing entire scenes or isolated objects. Completing objects within their scene context is still an area of active research. A key limitation of existing methods is their disregard for the scene’s environmental cues—such as walls and floors—which could provide valuable information for defining the boundaries of incomplete objects. Additionally, object completion models are often trained on synthetic datasets, where objects are neatly aligned and centred, unlike real-world scanned data that is typically unaligned. This misalignment hinders the practical application of existing models, although some approaches have attempted to address this by estimating symmetry planes. State-of-the-art (SOTA) methods also face challenges in guiding object completion, often relying on a range of potential outputs with minimal user interaction.
In this work, we aim to improve the completion of objects from partially scanned indoor scenes by leveraging environmental cues to better inform the boundaries of incomplete objects. Furthermore, we introduce an interactive voxel editor that allows users to guide the object completion process toward more accurate results. Our contributions are twofold: (1) a novel boundary-defining and object-alignment method that integrates with existing object completion pipelines, and (2) the development of an interactive voxel editing tool that enhances user control over the completion process. Experimental results demonstrate the effectiveness of our approach in improving object completion in complex, real-world scanned scenes.

}

\keywords{GSW 2025, Voxel, Object completion, Generative modeling, interaction}

\maketitle

%=====================INTRODUCTION=====================%
\section{Introduction}
\label{sec:introduction}
% Problem statement: 
% The need for scene completion
%either Scene as a whole, or objects seperatly
Dynamic indoor environments originating from 3D scans are increasingly in demand within the gaming industry and the Architecture, Engineering, Construction, and Operations (AECO) sectors~\cite{Vermandere2022}. Similar to digitally created scenes, these environments consist of collections of digital objects that can be interacted with independently from static walls and floors (e.g., by modifying or removing objects).

3D-scanned environments are typically captured as a whole, not only for efficiency but also for cost-effectiveness. However, when isolating an object from a scene, it is often incomplete due to occlusions and contact with other objects. This missing information presents a significant bottleneck, as the aforementioned applications require complete object data for both geometry and texture~\cite{Vermandere2023}. Consequently, there is an urgent need for effective completion methods.

Current methods for object completion are either focused on completing the scene as a whole~\cite{Dai2018} or on completing already isolated objects~\cite{Mittal2022}. When completing the scene as a whole, missing regions due to sensor occlusions can be filled in; however, occlusions between contacting objects often remain unresolved. This is why many models first attempt to detect the objects in the scene, after which they are isolated from their context. This approach results in the environment being ignored in the final completion process. The fact that scene structures such as walls, floors, and other objects can provide key insights into the boundaries of partial objects remains largely unexplored in the state of the art (SOTA).

% Objects are not aligned, countered to there syntethic counterparts
Object completion models are often trained on synthetic data~\cite{Mittal2022,Cheng2023, Zhou2021}, primarily due to the limited availability of real-world data and the ease of use of properly aligned and clean 3D models. While synthetic data simplifies the training process, it also limits the models' effectiveness for real scanned objects, which are rarely aligned or centred around their approximate centres. Works such as~\cite{Sipiran2014, Mitra2006, Shi2020, Gao2019} attempt to address this problem by estimating symmetry planes of incomplete objects. Together with environmental structures like floors and walls, these symmetry planes can help match partial objects to synthetic input formats by aligning them according to their principal axes of symmetry and physical boundaries.

State-of-the-art (SOTA) object completion models~\cite{Mittal2022, Cheng2023} can provide a range of possible completion results due to the encoding-decoding process of the VAE model, increasing the likelihood that one of the provided options will be a good fit. However, the guiding capabilities of these models are currently limited to sub-bounding boxes within the voxel grid, as determined by their training methods. By incorporating a voxel editor tool, objects can be more precisely guided to the desired shape. These guiding voxels can be generated based on environmental alignment cues and further refined by the end user.

The main goal of this research is to improve object completion for objects originating from partially scanned indoor scenes. This goal is achieved by leveraging environmental cues and symmetry axes to more accurately define the physical boundaries for the completion network. Additionally, by enabling users to guide the object completion with a voxel editor, the final object completion can be significantly enhanced.

The main contributions of this work are twofold. First, we introduce a novel boundary-defining and object alignment method to better fit partial objects into existing object completion pipelines. Second, we develop an interactive voxel editor to more effectively guide object completion toward its desired shape.

% The structure
The remainder of this work is structured as follows. The background and related work is presented in Section \ref{sec:background}. Following is the explanation of the proposed method in Section \ref{sec:methodology}. In Section \ref{sec:experiments}, an overview of the used datasets and their results is presented. Finally, the conclusions are presented in Section \ref{sec:conclusion}

%=====================WIDE FIGURE=====================%

\begin{figure*}[!h]
    \centering
    \includegraphics[width=\textwidth]{figures/method-overview.png}
    \caption{Overview of the object completion pipeline, starting with an incomplete scene (left), followed by a object and plane detection (top-centre), and a symmetry detection to combine into Bounding box refinement (bottom-centre). The user-input (top-right) is combined with the predicted voxel input to result in a completed, textured mesh using AutoSDF (right).}
    \label{fig:methodology}
\end{figure*}

%=====================BACKGROUND=====================%
\section{Background and related work}
\label{sec:background}


\subsection{Object Detection}

%Votenet \cite{Qi2019}
%MLCVNet \cite{Qian2020}
%V-DETR \cite{Shen2023}

Object detection in a 3D scene is performed by clustering points or voxels belonging to a given object. VoteNet~\cite{Qi2019} achieves this by using deep Hough voting to cluster each point and identify clusters that could form a single object. This approach is improved in MLCVNet~\cite{Qian2020}, which introduces a multi-level context to enhance clustering accuracy. V-DETR~\cite{Shen2023}, on the other hand, applies DETR (Detection Transformer) in 3D with Vertex Relative Position Encoding to improve locality. These models produce a range of bounding boxes that encapsulate potential objects.

\subsection{Object Completion}
% Object completion
% why point clouds or SDF's?
3D scanned environments are typically captured as unstructured pointclouds or meshes. Both of these formats are irregular and are not easily used in machine learning networks. This is why the models are often converter to either standardized pointclouds or Signed Distance Fields (SDF) Both of which, can be mapped to a fixed input size. 

% point based
%Point-based methods 
%IF-Net \cite{Chibane2020}
%Point-voxel-diffusion \cite{Zhou2021}

Point-based geometry completion like Point-Voxel-diffusion~\cite{Zhou2021} uses a fixed-size pointcloud as input to predict the final shape through 3D diffusion. IF-Nets~\cite{Chibane2020} also use points, but employs implicit features generated from those points to predict the missing regions. These models provide good results for general shapes, but lack in fine detail completion due to the amount of noise and lack of a clear surface definition typically present in point clouds.

% SDF based
%Auto-SDF \cite{Mittal2022}
%SD-Fusion \cite{Cheng2022}

%PatchComplete \cite{Rao2022}
%Weakly supervised shape completion \cite{Wu2024}

%Shapeformer \cite{Yan2022}
%XCube \cite{Ren2023}

SDFs are an implicit representation of a 3D shape. They define a function which represents the distance to the boundary of the object from any point in space \cite{Mittal2022}. An SDF can be discretised into a voxelgrid to standardize the input size. These have become a popular input type because they retain the shape of the object while using less data points. Models like PatchComplete \cite{Rao2022} and WSSC \cite{Wu2024} use a coarse-to-fine approach by first predicting the general shape and then refining each sub-grid using multi-resolution priors.
Shapeformer \cite{Yan2022} is able to leverage the Transformer architecture by using a vector quantized deep implicit function (VQDIF) to represent an incomplete shape.

Models like AutoSDF~\cite{Mittal2022} are able to encode the SDFs and, by spliting the SDF in sub-grids during training, can predict the missing geometry. SD Fusion~\cite{Cheng2023} builds upon this by allowing multi-modal input types to guide the generation. XCube \cite{Ren2023} expands upon the object completion by employing a hierarchical voxel latent diffusion model which generates progressively higher resolution grids in a coarse-to-fine manner using a custom framework built on the highly efficient VDB data structure. This enables the model to generate much larger scenes. 

These works all rely on a voxelised Truncated Signed Distance Field(TSDF) as input, allowing the user to highlight the parts which need to be completed by highlighting certain voxels, however, most of these works are limited to range selections for this purpose.  XCube \cite{Ren2023} has stated a potential voxel guiding workflow using an off-the-shelf voxel editor to define the guiding voxels. This is however practically limited because there is no feedback loop between the generation and guiding due to the lack of software integration. Works like Interactive Voxel Editing \cite{Wegen2022} made it possible to edit the full scene by providing boolean tools to isolate and delete voxels from the scene. However, a true interactive voxel editor build for object completion does not exist yet.



\subsection{Environment aided Completion}
% 
%Object intersection constraints \cite{strecke2020does}
%EM-Fusion \cite{strecke2019fusion}
%Variational Taxonomy \cite{Schroers2014} -> not used
%tracking partially-occluded objects \cite{Wang2021}

The main cause of incomplete 3D scans often come from occlusions, be it from the object self, or the surrounding environment. Instead of ignoring the environment, some works like tracking partially-occluded objects \cite{Wang2021} try to look past these occlusions and use the environment estimate the hidden objects complete shape. Co-Section \cite{strecke2020does} on the other hand uses the object detection from EM-Fusion \cite{strecke2019fusion} and leverages intersection constraints from walls and floors to infer hidden shape information. This clearly defines object boundaries creates physically plausible 3D objects.

\subsection{Object Alignment}

%Approximate symmetry detection \cite{Sipiran2014}
%Partial approximate symmetry detection \cite{Mitra2006}
%SymmetryNet \cite{Shi2020}
%PRS-Net \cite{Gao2019}

Most object completion networks are trained on normalised training data, for object completion, this means the objects are scaled and aligned before they enter the network. This is trivial for most synthetic data, but aligning partially scanned 3D objects is still a field of ongoing research. Finding symmetry in complete objects is possible by works like PRS-Net \cite{Gao2019}, which employs a novel learning framework to automatically discover global planar reflective symmetry of a 3D shape by training an unsupervised 3D convolutional neural network to extract global model features.

Partial symmetry completion is more complex, because there is no guarantee to find symmetrical features. Early works like Partial approximate symmetry detection \cite{Mitra2006} developed an algorithm that processes geometric models and efficiently discovers and extracts a compact representation of their Euclidean symmetries. This was improved by Approximate symmetry detection \cite{Sipiran2014}, which uses local extrema to find corresponding symmetry points and aligns the partial mesh. SymmetryNet \cite{Shi2020} Only requires a single RGB-D input to discover global planar reflective symmetry  by using an unsupervised 3D convolutional network to extract global model features.

%=====================METHODOLOGY=====================%

\section{Methodology}
\label{sec:methodology}

The presented method (Figure~\ref{fig:methodology}) illustrates the full workflow. First, scene detection is performed by detecting the objects using Votenet \cite{Qi2019}, these are removed from the scene and the planes are detected in the remaining geometry using a RANSAC algorithm. 
Second, we perform the object alignment step where each object is processed to find its primary symmetry axis using Approximate symmetry detection \cite{Sipiran2014} and aligned accordingly. The scene planes are used to limit the object's bounding box. 
Finally, the user is able to refine the completion voxels using the voxel editor. Those voxels are used to predict the missing geometry from the incomplete inputs by utilizing implicit shape representations with AutoSDF ~\cite{Mittal2022}. This results in a list of complete objects.

\subsection{Scene Detection}
% Detect the loose objects in the scene
To isolate the objects from the scene, we perform an object detection using VoteNet\cite{Qi2019} on the whole scene as seen in Figure \ref{fig:method-object-detection}. Since the network requires a point cloud as an input, the incomplete scene is sampled to a pointcloud with a $5cm$ resolution. This provides a good balance between detail and execution speed. AutoSDF has a tendency to over-detect a scene, this is why overlapping boxes are combined into a larger boxes if the Intersection over Union (IOU) is larger then $80\%$. After the bounding boxes are cleaned up, they are used to remove the objects from the scene. This results in a list of unaligned objects and a remaining scene that can be further segmented.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{figures/method-object-detection.png}
    \caption{Left, an indoor scene filled with furniture. Right, The resulting detected bounding boxes of the objects highlighted in green.}
    \label{fig:method-object-detection}
\end{figure}

% Detect the planes in the scene
The resulting Empty scene is further segmented using a RANSAC plane detection as illustrated in \ref{fig:method-plane-detection}. Since the scene was sampled in the previous step, we can perform a point-wise RANSAC plane detection. To define proper boundaries, the intersections between the planes are computed and used to define boundaries. Those boundaries, together with the planes normal, these are used to compute the final quads used to limit the bounding box in the next step.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{figures/method-plane-detection.png}
    \caption{Left, an indoor scene where the detected objects have been removed. Right, the 3 detected planes from the RANSAC algorithm.}
    \label{fig:method-plane-detection}
\end{figure}

\subsection{Object alignment}
The detected incomplete objects have an initial axis aligned bounding box. However, this does not necessarily align with the objects orientation, which is needed to get a proper completion in the next step. Since most indoor objects have some sort of symmetry or orthogonal construction due to manufacturing constraints, we can estimate the object alignment by finding the principal symmetry axis. This is done by finding point-wise symmetry pairs in the incomplete mesh using Approximate symmetry detection \cite{Sipiran2014} as illustrated in Figure \ref{fig:method-object-alignment}.

Together with the planes computed in the previous step, the aligned bounding box is computed. This is done by first checking if the object is grounded or mounted to a wall by computing the adjacency to each plane. This is used to fix the first axis of rotation labelled as "up" as the normal to the detected plane. Then the symmetry axis is used to find the second rotation axis defining the "right" vector as the normal of the symmetry plane. Those two vectors define a unique rotation that is used to orient the incomplete object.

In a final step, the planes are used to refine the boundaries of the bounding box. limiting the range at which the object could theoretically extend. This is further enhanced by using the symmetry axis as the centre of the bounding box. When an object has multiple axis, the alignment can be further refined.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{figures/method-object-alignment.png}
    \caption{The detected symmetry plane of the partial object on the left and the resulting refined bounding box on the right.}
    \label{fig:method-object-alignment}
\end{figure}

\subsection{Object Completion}
The object completion network needs the incomplete TSDF and a list of voxels that represent the missing parts \cite{Mittal2022}. The TSDF is created from the incomplete mesh and is discretised using the refined bounding box divided into a $64^3$ voxel grid. 

The next step is defining the voxels to be completed, this is done using a user interface where the incomplete mesh is positioned within an empty voxel grid as seen in Figure \ref{fig:method-voxel-completion}. However, to provide an initial guess of the to-be-completed voxels, the symmetry axis is used to mirror the occupied voxels. 

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{figures/method-voxel-completion.png}
    \caption{The proposed voxels used to complete the partial object on the left and the resulting completed object on the right.}
    \label{fig:method-voxel-completion}
\end{figure}

The predicted voxels, together with the incomplete UDF form the basis for the user interface. The user is able to edit the initial guess by adding and removing new voxels using the left and right mouse buttons respectively. This is all performed in a intuitive 3D environment as seen in Figure \ref{fig:method-voxel-editor}.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{figures/method-voxel-editor.png}
    \caption{The interface for the voxel editor. The left side provides an intuitive set of buttons to manage data. The right slider limits the voxel drawing to a fixed height so the user can draw the voxel in the centre.}
    \label{fig:method-voxel-editor}
\end{figure}

%=====================EXPERIMENTS=====================%

\section{Experiments}
\label{sec:experiments}

To evaluate the effectiveness of our method, we compared the object completion results from a set of objects detected in a real scanned dataset. This evaluation begins by performing object completion on the partial object as it is detected in the scene, without any refinement. Next, we apply our method, which aligns the object to its principal axis and constrains the bounding box according to the physical boundaries. The results of our experiments are shown in Figure \ref{fig:experiment-results}

\begin{figure*}[!h]
    \centering
    \includegraphics[width=\textwidth]{figures/exp-results.png}
    \caption{Experimental results: The first column shows the objects in their context, the second the unaligned isolated objects, the third the unaligned completed objects using Auto-SDF, the fourth the aligned objects with refined bounding boxes, highlighting the detected planes used for refinement, and the final column shows our results.}
    \label{fig:experiment-results}
\end{figure*}

\subsection{Dataset}
For the experiments, we used the Matterport~\cite{Chang2017} dataset, a scanned dataset consisting of 90 fully textured building-scale scenes, each containing between 15-30 objects that can be detected and completed. Because no ground truth is available for this dataset, the completion is evaluated on a visual basis. We selected a few objects to highlight, which are shown in context in Column 1 of Figure \ref{fig:experiment-results}.

\subsection{Plane detection}

The effectiveness of the plane detection for bounding box refinement is evaluated on a freestanding object and an object against both an axis-aligned and a non-axis-aligned wall. The detected planes are shown in Column 3 of Figure \ref{fig:experiment-results}. When the object is not positioned against a wall, the impact is minimal, as the only relevant plane is the floor. Since the detection bounding boxes are aligned with the global axis, and the floor is typically level, the bounding box aligns naturally with the floor. However, for objects positioned against a wall, especially when the walls do not align with the global orthogonal axis, the additional plane provides a clear initial alignment and physical boundary for the object. This advantage is illustrated in Row 1 of Figure \ref{fig:experiment-results}, where the detected wall behind the cupboard provides a clear depth limit.

\subsection{Object alignment}

Proper alignment of the object has the greatest impact on the completion results. As shown in Column 2 of Figure \ref{fig:experiment-results}, the completion network struggles to interpret unaligned objects and attempts to create orthogonal shapes from them. This issue is most evident in Row 1 of Figure \ref{fig:experiment-results}, where the cupboard is rendered as a triangular shape instead of its original rectangular form. Even for objects with known boundaries, as in Row 3 of Figure \ref{fig:experiment-results}, the network fails to complete the object in a meaningful way.

\subsection{Object completion}

The final results show that the overall shape is largely preserved. However, due to the encoding-decoding process of the VAE model, not all details are retained. While the results appear plausible, we observe that some objects are slightly altered, even in regions of the object that were already known. The voxel editor offers a quick way to refine the voxels needed for the completion network.

%=====================CONCLUSIONS=====================%
\section{Conclusions}
\label{sec:conclusion}

% evaluate the finished product
%- Positive
%- The alignment is the most important factor for proper completion results
%- object positioned against other objects provide a better boundary definitionand improves the bounding box refinement.
%- When the object is aligned, the completion results are good, but still loose some detail due to the SDF conversion.
%- the voxel editor provides a quick way to specify the voxels to be completed

%- Limitations
%- The detection fails to segment very small details and objects with limited geometric 
%- freestanding objects have less context to align and have to rely more on the symmetry.
%- Not all objects are symmetrical, this can lead to wrong alignments

The evaluation of our proposed object completion method demonstrates its effectiveness, with key factors influencing the quality of results. Among these, object alignment proves to be the most crucial for achieving accurate and realistic completion results. Objects positioned against other surfaces or objects benefit from better boundary definitions, which enhance bounding box refinement and lead to more reliable completions. Aligned objects generally yield good completion outcomes; however, some fine details are inevitably lost due to the SDF conversion process. Additionally, the voxel editor serves as a quick and effective tool for specifying voxels for completion, allowing users to refine the final output further.

Despite these strengths, there are limitations to our approach. The detection model struggles to accurately segment very small details and objects with limited geometric information. Freestanding objects, in particular, have less environmental context to guide alignment and therefore rely more on symmetry, which is not always sufficient. Moreover, not all objects possess inherent symmetry, which can lead to alignment errors and reduce the precision of the completion process.

Overall, our method shows promise in refining object boundaries and improving completion in complex scenes, especially when alignment and boundary context are available. Future work will focus on addressing limitations related to asymmetry and enhancing segmentation for detailed structures.

\section*{ACKNOWLEDGEMENTS}\label{ACKNOWLEDGEMENTS}

This project has received funding from the FWO-SB grant (grant agreement 1S16923N ) and the Geomatics research group of the Department of Civil Engineering, TC Construction at the KU Leuven in Belgium.


{
	\begin{spacing}{1.17}
		\normalsize
		\bibliography{export} % Include your own bibliography (*.bib), style is given in isprs.cls
	\end{spacing}
}

\end{document}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% 
%                                                                 %
%                            CHAPTER                              %
%                                                                 %
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% 
\chapter{Experiments}
\label{ch:Experiments}
In dit hoofdstuk wordt het AutoSDF-model getest en ge\"{e}valueerd op echte data. De eerste stap in dit experiment bestaat uit het verzamelen van voldoende data, die vervolgens een pre-processing ondergaat, daarna wordt verwerkt met het model, en ten slotte wordt ge\"{e}valueerd.

Bij de verwerking van de verzamelde data wordt de methodiek, zoals beschreven in Hoofdstuk \ref{ch:Methodology}, gevolgd.

\section{Data verzameling}
\subsubsection{Soorten data}
Dit onderzoek richt zich op het evalueren van de inzetbaarheid van bestaande state-of-the-art (SOTA) object-completion-modellen op echte data. Daarbij is het van belang te definiëren wat onder 'echte data' wordt verstaan.

Allereerst wordt een onderscheid gemaakt tussen gescande data en synthetische data. Gescande data betreft informatie verkregen via laserscanners en bestaat uit puntenwolken, opgeslagen als .e57-bestanden of .obj-bestanden. Gescande data is vrijwel nooit volledig, omdat objecten vaak andere voorwerpen raken of occlusies bevatten. Daarnaast beïnvloeden factoren zoals ruis, reflecterende oppervlakten en zwarte stralers de precisie van de scan.

Synthetische data kan bestaan uit meshes of uit punten gesampled op meshes. Dit type data is doorgaans afkomstig van 3D-modellen of virtuele scans van 3D-omgevingen. Hoewel de synthetische data hetzelfde bestandsformaat hanteert als de gescande data, bevat deze geen ruis of andere fouten die voortkomen uit omgevingsfactoren, technische beperkingen of menselijke fouten.

In dit onderzoek worden beide databronnen gebruikt. In de onderstaande afbeelding wordt hetzelfde hangend vat weergegeven, waarbij het verschil in datatypes duidelijk zichtbaar is. De gescande data vertoont ruis en is onvolledig, terwijl de synthetische data de ideale vorm van het vat benadert.
\begin{figure}
[H]
    \centering
    \includegraphics[width=0.9\linewidth]{fig/Scanned vs Synthetic data.png}
    \caption{Scanned vs Synthetic data}
    \label{fig:enter-label}
\end{figure}

Bovenstaande afbeelding geeft een indicatie van de omgeving waarin de data werd verzameld. Binnen dit onderzoek wordt ervoor gekozen om het model toe te passen op elementen die kenmerkend zijn voor industri\"{e}le omgevingen. Specifieker richt de analyse zich op de vervollediging van industri\"{e}le buizen, leidingen en opslagvaten.

Deze objecten zijn doorgaans niet aanwezig in bestaande datasets zoals ShapeNet en Matterport, waardoor ze een geschikte test vormen voor het AutoSDF-model.

De verzamelde dataset introduceert in totaal drie nieuwe objectklassen voor het AutoSDF-model: industri\"{e}le vaten, rechthoekige buisdelen en ronde buisdelen.

\subsection{Dataverzameling}
Een eerste bron van data bestaat uit een eerder uitgevoerde scan van een brouwerij, gelegen in de M-blok op KULeuven campus Rabot. Deze brouwerij bevat diverse ronde en rechthoekige buisdelen, die op verschillende manieren met elkaar verbonden zijn en variatie vertonen in bochten, helling en ori\"{e}ntatie. Daarnaast zijn ook opslagvaten van verschillende groottes en vormen ingescand.

Deze scans werden uitgevoerd in het kader van een eerder opleidingsonderdeel (Scan-To-BIM) met behulp van een Leica P30 en Leica BLK laserscanner. Uit de verzamelde gescande data zijn 22 verschillende types buizen en 9 vaten geïdentificeerd.

De werkelijke vorm van deze gescande objecten werd opgenomen in het in Revit gebouwde model van de bovengenoemde brouwerij. Dit model bevat een groot deel van de ingescande industri\"{e}le elementen en is gemodelleerd volgens LOA 2 en LOA 3. Een overzicht van dit model wordt weergegeven in Figuur \ref{fig:Model brouwerij}.
\begin{figure}
    \centering
    \includegraphics[width=0.9\linewidth]{fig/Model brouwerij.png}
    \caption{Model brouwerij M-blok}
    \label{fig:Model brouwerij}
\end{figure}

In de gemodelleerde brouwerij lag de nadruk voornamelijk op de gedetailleerde representatie van de aanwezige vaten en stalen constructie-elementen. Enkel de grootste buizen aan het dak zijn gemodelleerd, maar hun nauwkeurigheid is onvoldoende om als betrouwbare evaluatiebasis te dienen. De gemodelleerde vaten daarentegen zijn met hoge precisie gegenereerd, waarbij een nauwkeurigheid van 1 mm werd nagestreefd. Als gevolg hiervan zullen uitsluitend de gemodelleerde vaten worden gebruikt als ground truth voor de gescande data.

Naast hun rol als referentie voor de gescande vaten, vormen deze modellen tevens de eerste bron van synthetische data. Ter aanvulling hiervan worden extra buiselementen handmatig geconstrueerd in Revit. Deze omvatten rechthoekige en ronde buizen, met variërende rondingen en onderlinge connecties.

Deze elementen worden gemodelleerd als families, met behulp van extrusions, revolutions, sweeps, swept blends en voids. Een overzicht van deze data wordt weergegeven in Figuur \ref{fig:Rechthoekige_buisdelen} en Figuur \ref{fig:Ronde_buisdelen}.
\begin{figure}[H]
  \centering
  \begin{minipage}{0.45\textwidth}
    \centering
    \includegraphics[width=\linewidth]{fig/Rechthoekige_buisdelen.png}
    \caption{Rechthoekige buisdelen}
    \label{fig:Rechthoekige_buisdelen}
  \end{minipage}%
  \hspace{0.05\textwidth} % Ruimte tussen de twee afbeeldingen
  \begin{minipage}{0.45\textwidth}
    \centering
    \includegraphics[width=\linewidth]{fig/Ronde_buisdelen.png}
    \caption{Ronde buisdelen}
    \label{fig:Ronde_buisdelen}
  \end{minipage}
\end{figure}

In totaal omvat de verzamelde synthetische data 59 verschillende objecten. Deze bestaan uit buisdelen met zowel ronde als rechthoekige doorsneden, elk met variërende rondingen, connecties en hoeken. Zo goed als elke mogelijke en realistische bocht of verbinding tussen twee buizen is in deze dataset opgenomen. Daarnaast zijn ook de 9 eerder verzamelde vaten in de set verwerkt.

\section{Pre-processing} De verzamelde gescande en synthetische data wordt in het pre-processingscript omgezet naar SDF-representaties. Dit script biedt de mogelijkheid om telkens een nieuwe ori\"{e}ntatie ten opzichte van de ground truth mee te geven. Hierdoor ondergaat de verzamelde data meerdere keren een pre-processing, waarbij telkens een nieuwe ori\"{e}ntatie aan de ground truth wordt toegekend.

\subsection{Pre-processing van gescande data} De gescande data wordt pre-processed via een AlphaShape-methode. Hierbij wordt de genormaliseerde mesh eerst omgezet naar een AlphaShape, met behulp van een gelijknamige functie. Voor de meeste objecten bleek een AlphaValue van 8 een geschikte keuze.

Ronde objecten vertoonden echter minder goede resultaten bij deze waarde. Deze objecten werden daarom individueel behandeld en kregen een specifieke, aangepaste AlphaValue. De impact van de toegepaste AlphaValue wordt geïllustreerd in Figuur \ref{fig:Invloed_alphavalue}.

\begin{figure}
[H]
    \centering
    \includegraphics[width=0.9\linewidth]{fig/Invloed_alphavalue.png}
    \caption{Invloed van de toegende AlphaValue}
    \label{fig:Invloed_alphavalue}
\end{figure}
Deze afbeelding toont enkele visuele representaties van de gegenereerde AlphaShapes uit de ground truth. Een pre-processing en bijbehorende AlphaShape wordt als adequaat beschouwd wanneer de vorm de ground truth nauwkeurig benadert. Hierbij wordt voorkomen dat inwendige hoeken en binnenruimtes onnodig worden opgevuld.

Voor de geselecteerde ronde buis met versmalling is een AlphaValue van 12 toegepast. Bij een waarde van 6 worden de inwendige hoeken te veel opgevuld, terwijl bij een AlphaValue van 20 het oppervlak van de buis zijn vorm verliest en holten ontstaan. De verzamelde gescande data werd slechts via één ori\"{e}ntatie ge\"{e}valueerd en verwerkt.

De gescande data wordt enkel via de oorspronkelijke ori\"{e}ntatie verwerkt. Hierdoor wordt deze ook slechts met één voxelselectie vervolledigd—de best passende selectie.

\subsection{Pre-processing van synthetische data} De synthetische data wordt niet verwerkt met een AlphaShape. In plaats daarvan onderging de dataset meerdere ori\"{e}ntaties tijdens de pre-processing. De verschillende geori\"{e}nteerde sets worden benoemd op basis van de toegepaste hoekverdraaiing en de rotatieas waarlangs de transformatie werd uitgevoerd. Dit zijn:
\begin{multicols}{2}
\begin{enumerate}
    \item 0
    \item 45X
    \item 36X-9Y-136Z
    \item 15Y
    \item 45Y
    \item 60Y
    \item 180Z
    \item 45Y-45X
    \item 60Y-60X
    \item 90X-90Y-90Z
\end{enumerate}
\end{multicols}

De verwerking van de synthetische data verliep niet geheel probleemloos. Bij pre-processed data die slechts een hoekverdraaiing rond één as ondergaat, ontstaan vaak holten in één van de oppervlakken. Dit wordt veroorzaakt doordat deze oppervlakken samenvallen met één van de vlakken die door de assen van het assenstelsel worden gevormd. Hierdoor valt een deel van het oppervlak buiten het grid, wat resulteert in een non-watertight SDF en nadelige gevolgen voor de evaluatie van het model.

Dit probleem wordt verholpen door de objecten vooraf te verschalen met een factor 0.95. Dit fenomeen werd onder andere waargenomen bij de volgende sets: 0, 45X, 36X-9Y-136Z, 180Z en 90X-90Y-90Z.

De impact van dit probleem en enkele gegenereerde ori\"{e}ntaties voor een rechthoekig buisdeel met bocht worden weergegeven in Figuur \ref{fig:Pre-processing_non_watertight_SDFs}.
\begin{figure}
[H]
    \centering
    \includegraphics[width=0.9\linewidth]{fig/Pre-processing_non_watertight_SDFs.png}
    \caption{Pre-processing non watertight SDF's}
    \label{fig:Pre-processing_non_watertight_SDFs}
\end{figure}
Links wordt een stuk rechthoekige buis weergegeven dat vooraf niet verschaald werd, waardoor de voor- en achterkant open blijven en de vorm niet watertight is.De andere vormen centraal en recht op de figuur ondergingen wel een schaalwijziging en kennen dus een volledig gesloten oppervlak. 

\section{Voxelselecties}
De complete verzameling bestaat uit ongeveer 70 voxelselecties:
\begin{itemize}
    \item $4 keer 1/2^e, 8 keer 1/4^e, 8 keer 1/8^e$ delen van het grid
    \item Schillen horizontaal en verticaal in alle richtingen. Steeds genummerd van 1-7.
    \item Schuine selecties, die ook in schillen werken parallel aan de diagonalen van het grid. 
\end{itemize}
Alle selecties worden manueel geselecteerd in Unity.

\section{Verwerking met het ongetraind AutoSDF-model}
\subsection{Vervollediging van gescanda data via het ongetraind AutoSDF-model}
Het AutoSDF-model vraagt 2 inputs: een te vervolledigen SDF en een voxelselectie die het te voorspellen deel beschrijft. De SDF is het resultaat van de pre-processing van de gescande en synthetische data. Elke SDF krijgt vervolgens een voxelselectie toegewezen die de ontbrekende vorm het beste beschrijft. 

Alle SDF's en hun bijpassende voxelselectie worden met het model verwerkt. Daarbij wordt de batch-size op 2 ingesteld, de top-k variabele krijgt waarde 30. Zo worden dus per vervollediging 15 batches gegenereerd, waarin telkens 2 van de 30 topk-candidates worden verwerkt. Het model biedt dus steeds 2 mogelijke oplossingen voor de aanvulling. Deze krijgen de namen 0.obj en 1.obj.

In Figuur \ref{fig:Resultaten_untrained_scanned.png} worden enkele voorbeelden van deze verwerking op de rechthoekige- en ronde buisdelen weergegeven. 
\begin{figure}
[H]
    \centering
    \includegraphics[width=0.9\linewidth]{fig/Resultaten_untrained_scanned.png}
    \caption{Resultaten uit het ongetrainde AutoSDF-model voor gescande data}
    \label{fig:Resultaten_untrained_scanned.png}
\end{figure}

Bovenstaande figuur toont rechthoekige buisdelen met bochten van 60 en 90° vervolledigd (links). Daarnaast worden ook ronde rechte buisdelen (rechts), al dan niet met connectie van een extra ronde buis, getoond. De SDF van het gescande object bevindt zich steeds aan de linkerkant, de rechterkant toont een oplossing uit het AutoSDF-model. 

De oplossing uit het model zijn qua vorm ver van de werkelijkheid verwijderd. Toch was dit teleurstellende resultaat te verwachten. De gescande objecten zijn verre van volledig en kennen een zeer ruw oppervlak met veel ruis. Echt herkenbare vlakken ontbreken, wat de voorspelling via het ongetrainde AutoSDF-model zeer moeilijk maakt. 

Figuur \ref{fig:voorbeelden_buizen_scanned_ongetraind} bevat nog wat oplossingen uit het getraind model. De vorm wordt niet begrepen, er wordt dus voor de meeste objecten een willekeurig fout volume toegevoegd. Sommige oplossingen kennen zelfs geen noemenswaardige verandering met de input. 

\begin{figure}
[H]
    \centering
    \includegraphics[width=0.9\linewidth]{fig/voorbeelden_buizen_scanned_ongetraind.png}
    \caption{Oplossingen voor gescande buizen met het ongetrainde AutoSDF-model.}
    \label{fig:voorbeelden_buizen_scanned_ongetraind}
\end{figure}

\subsection{Vervollediging van synthetische data via het ongetraind AutoSDF-model}
De verwerkte synthetische objecten zijn van nature volledig en blijven dit ook na de pre-processing. Bij de verwerking met het AutoSDF-model wordt hun volume niet gewijzigd; ze worden volledig als input gebruikt. De voxelselectie bepaalt welk volume fictief ontbreekt, waardoor het model deze zone interpreteert als een gedeelte dat aangevuld moet worden. In de ideale situatie resulteert dit in een vervolledigde vorm die exact overeenkomt met de oorspronkelijke inputdata.

Elk synthetisch object uit de pre-processing wordt verwerkt met alle verschillende voxelselecties (zoals eerder opgelijst), in tegenstelling tot de gescande data, die slechts met één selectie werd verwerkt. Tijdens deze verwerking blijft de batch size ingesteld op 2 en krijgt de top-k variabele de waarde 30. Hierdoor worden er bij elke vervollediging 15 batches gegenereerd, waarin telkens 2 van de 30 top-k kandidaten worden verwerkt.

Het model biedt telkens twee mogelijke oplossingen voor de aanvulling van de synthetische objecten, die opnieuw worden opgeslagen onder de namen 0.obj en 1.obj.

In totaal werden alle objecten onder 10 verschillende ori\"{e}ntaties verwerkt. Dit levert een totaal van:
\[
aantal vervolledigingen = 10 ori * 58 obj * 68 voxel
* 2 opl
\]
\[
aantal vervolledigingen = +- 79.000 objecten
\]
De eerste visuele indruk van de gemaakte oplossingen met het ongetrainde model stelt teleur. Het lijkt alsof de objecten niet alleen in de voxelgeselecteerde regio's wijzigingen kregen, maar ook in het 'al volledige' deel van het object. Onderstaande Figuur \ref{fig:Resultaten_autosdf_voorbeelden1} geeft daarbij wat meer duiding. Op deze afbeelding bevindt de ground truth zich steeds bovenaan, een uitgewerkte oplossing door het AutoSDF-model onderaan. Het type buis en de gebruikte voxelselectie worden steeds vermeld. 

Helemaal links op de figuur is een vervollediging te zien waarbij de voxelselectie slechts 25 procent (Schijf links 2/8) links in het volume beschreef. Het volume kreeg wijzigingen in de te voorspellen zone, maar ook de rechterkant werd aangepast. Het volume had daar niet gewijzigd mogen worden, de holten hadden niet mogen ontstaan. Centraal op de figuur zien we hetzelfde buisdeel, dat een voxelselectie kreeg die bijna het volledige volume omvatte (Schijf links 7/8). Het model heeft hierbij te weinig bestaand volume om mee te werken en geeft dus bijbehorende teleurstellende resultaten.
\begin{figure}
[H]
    \centering
    \includegraphics[width=0.9\linewidth]{fig/Resultaten_autosdf_voorbeelden1.png}
    \caption{Resultaten Geometry Completion}
    \label{fig:Resultaten_autosdf_voorbeelden1}
\end{figure}
Niet alle resultaten waren teleurstellend. Helemaal rechts op Figuur \ref{fig:Resultaten_autosdf_voorbeelden1} werd een liggend vat vervolledigd, waarbij de voxelselectie de helft van het object omvatte. Ondanks de omvang van dit te vervolledigen volume, lijkt de oplossing erg op de ground truth. Vermoedelijk hielp de symmetrie en eenvoud van het object bij de vervollediging. 

Daarnaast wordt ook duidelijk waarop het model reeds getraind werd: de databases Matterport en ShapeNet. Zo zijn delen uit meubilair te herkennen in de gegeven oplossingen. In Figuur \ref{fig:Resultaten_autosdf_voorbeelden_meubels} werd voor de bovenste twee buisdelen (een verzamelpunt voor ronde en rechthoekige buizen) steeds de bovenste helft opnieuw vervolledigd. De oplossing bevat een onderkant die op de ground truth lijkt en een bovenkant die de vorm van een zetel/stoel kreeg. De rechthoekige, rechte buis werd voor bijna 80 procent opnieuw vervolledigd. Het resultaat vormt een vliegtuigje, een objectcategorie die ook in de ShapeNet data zit. 
\begin{figure}
[H]
    \centering
    \includegraphics[width=0.75\linewidth]{fig/Resultaten_autosdf_voorbeelden_meubels.png}
    \caption{Invloed trainingsdata op resultaten AutoSDF Geometry Completion}
    \label{fig:Resultaten_autosdf_voorbeelden_meubels}
\end{figure}

\section{Evaluatie en resultaten van het ongetraind AutoSDF-model}
De gecre\"{e}erde oplossingen worden steeds, via de eerder besproken 5 metrics, ge\"{e}valueerd t.o.v. hun ground truth. In onderstaande delen worden de globale resultaten voor het model besproken. 

\subsection{Gescande data}
Alle gescande data werd vervolledigd met de 'best passende' voxelselectie. Echter werd in de eerdere dataverzameling slechts van de gescande vaten een ground truth in het bijhorend model gevonden. Enkel deze vaten werden dus ge\"{e}valueerd op basis van de 5 metrics. De pre-processed gescande input en ground truth representaties van de 9 geselecteerde vaten worden weergegeven in Figuur \ref{fig:gescande_vaten_scan_gt}. De SDF uit de gescande data wordt daarbij steeds links getoond. 
\begin{figure}
[H]
    \centering
    \includegraphics[width=0.9\linewidth]{fig/gescande_vaten_scan_gt.png}
    \caption{Pre-processed representatie van de gescande vaten en hun ground truth.}
    \label{fig:gescande_vaten_scan_gt}
\end{figure}

Niet alleen de gecre\"{e}erde oplossingen werden met de ground truth vergeleken. Ook de inputdata voor het AutoSDF-model (de pre-processed SDF van de gescande puntenwolk) kenden een gelijkaardige evaluatie t.o.v. diezelfde ground truth. Deze tweede evaluatie geeft duidelijk weer wat de startsituatie was en zal een mogelijke verbetering na gebruik van het model verduidelijken. 

Tabel \ref{tab:startwaarden_vaten} geeft de resultaten van de inputdata (pre-processed SDF van de gescande data) weer. De tabel daaronder, Tabel \ref{tab:Prestaties_globaal_scanned} geeft het resultaat van de gecre\"{e}erde oplossingen (steeds 2) uit het ongetrainde model weer voor ieder verwerkt vat.
\begin{table}
[H]
  \centering

    \begin{tabular}{lccccc}
      \toprule
      \textbf{Vat} & \textbf{IOU} & \textbf{CD} & \textbf{F-score} & \textbf{Cov} & \textbf{NC} \\
      \midrule
      Vat1 & 5.0  & 0.002 & 0.142 & 0.156 & 67.8 \\
      Vat2 & 63.0 & 0.001 & 0.397 & 0.457 & 90.8 \\
      Vat3 & 27.5 & 0.009 & 0.143 & 0.169 & 68.2 \\
      Vat4 & 53.1 & 0.004 & 0.240 & 0.301 & 82.8 \\
      Vat5 & 5.0  & 0.021 & 0.061 & 0.080 & 68.0 \\
      Vat6 & 34.0 & 0.002 & 0.102 & 0.129 & 88.0  \\
      Vat7 & 24.8 & 0.046 & 0.041 & 0.039 & 77.3 \\
      Vat8 & 36.4 & 0.013 & 0.181 & 0.193 & 84.9 \\
      Vat9 & 58.3 & 0.008 & 0.349 & 0.445 & 96.0 \\
      \bottomrule
    \end{tabular}
  \caption{Startwaarden voor de gescande inputdata.}
  \label{tab:startwaarden_vaten}
\end{table}


\begin{table}
[H]
  \centering
    \begin{tabular}{lccccc}
      \toprule
      \textbf{Vat} & \textbf{IOU} & \textbf{CD} & \textbf{F-score} & \textbf{Cov} & \textbf{NC} \\
      \midrule
      Vat2 & 31.6 & 0.008 & 0.052 & 0.075 & 66.6 \\
      Vat2 & 40.3 & 0.007 & 0.055 & 0.088 & 66.7 \\
      Vat3 & 43.5 & 0.009 & 0.054 & 0.084 & 80.5 \\
      Vat3 & 64.4 & 0.011 & 0.048 & 0.072 & 84.5 \\
      Vat4 & 61.8 & 0.004 & 0.108 & 0.144 & 89.7 \\
      Vat4 & 64.4 & 0.004 & 0.102 & 0.125 & 86.1 \\
      Vat5 & 21.3 & 0.028 & 0.059 & 0.094 & 81.5 \\
      Vat5 & 35.1 & 0.020 & 0.054 & 0.096 & 79.7 \\
      Vat6 & 54.9 & 0.012 & 0.027 & 0.043 & 88.0 \\
      Vat6 & 57.2 & 0.018 & 0.010 & 0.017 & 86.4 \\
      Vat7 & 54.2 & 0.022 & 0.150 & 0.174 & 88.9 \\
      Vat7 & 57.7 & 0.019 & 0.145 & 0.162 & 88.9 \\
      Vat8 & 59.2 & 0.008 & 0.110 & 0.136 & 92.7 \\
      Vat8 &       & 0.008 & 0.184 & 0.226 & 94.5 \\
      Vat9 & 63.3 & 0.009 & 0.196 & 0.239 & 95.6 \\
      Vat9 &       & 0.008 & 0.192 & 0.232 & 95.1 \\
      \bottomrule
    \end{tabular}
    \caption{Prestaties van het ongetraind AutoSDF-model op gescande data}
    \label{tab:Prestaties_globaal_scanned}
\end{table}

In Tabel \ref{tab:Prestaties_globaal_scanned} worden de resultatenwaarden voor Vat1 en deels voor Vat8 en Vat9 niet weergegeven. In de evaluatie loopt soms iets mis. Voor het geval van Vat1 en Vat9 was de oplossing niet watertight. Door dit probleem konden de IOU en voor Vat1 ook de rest van de waarden niet berekend worden. Ook code zoals $SDF.fillholes()$ kon deze SDF's niet watertight krijgen.

\subsection{Synthetische data}
De verwerking van de synthetische data volgens 10 verschillende ori\"{e}ntaties levert een aanzienlijk aantal resultaten op. Het model genereert bij elke vervollediging steeds twee verschillende oplossingen. Om de analyse te vereenvoudigen, wordt enkel de oplossing met de hoogste Intersection over Union (IOU)-waarde behouden.

De IOU-metric wordt als meest algemeen beschouwd. In veel gevallen zal de oplossing met de hoogste IOU-score ook het beste presteren op de andere evaluatiemetrics.

Onderstaande Tabel \ref{tab:globale_prestaties_ongetraind} toont de globale prestaties van het model op synthetische data. De resultaten omvatten de gemiddelden (G) en standaardafwijkingen (SA) voor alle ori\"{e}ntaties. De gecombineerde gemiddelden en standaardafwijkingen worden per graad van onvolledigheid opgesplitst. De intervallen die volledigheid, IOU en Normal Consistency (NC) uitdrukken, worden weergegeven in procenten.

Deze intervallen representeren de hoeveelheid volume die per object opnieuw voorspeld werd. Dit voorspelde volume is afhankelijk van de geselecteerde voxelselectie. Daarom werd een specifiek script ontwikkeld om deze voorspelde volumes systematisch te berekenen. Deze berekening wordt uitgevoerd voor elk object, binnen elke ori\"{e}ntatie, en voor alle verschillende voxelselecties.

\begin{table}
[H]
  \centering
  \resizebox{\textwidth}{!}{%
    \begin{tabular}{lcccccccccc}
      \toprule
      \textbf{Interval} & \textbf{G IOU} & \textbf{SA IOU} & \textbf{G CD} & \textbf{SA CD} & \textbf{G F} & \textbf{SA F} & \textbf{G Cov} & \textbf{SA Cov} & \textbf{G NC} & \textbf{SA NC} \\
      \midrule
      0--5    & 69.0 & 8.0  & 0.489 & 0.337 & 0.117 & 0.030 & 0.124 & 0.032 & 91.5 & 4.0 \\
      5--10   & 68.8 & 6.9  & 0.574 & 0.369 & 0.108 & 0.029 & 0.115 & 0.031 & 91.4 & 4.3 \\
      10--15  & 69.3 & 7.6  & 0.601 & 0.426 & 0.106 & 0.030 & 0.113 & 0.032 & 90.9 & 4.9 \\
      15--20  & 68.9 & 7.2  & 0.578 & 0.368 & 0.109 & 0.029 & 0.116 & 0.031 & 90.3 & 4.6 \\
      20--30  & 68.2 & 8.9  & 0.602 & 0.534 & 0.115 & 0.030 & 0.121 & 0.031 & 89.2 & 5.5 \\
      30--40  & 61.4 & 11.0 & 1.170 & 1.148 & 0.111 & 0.032 & 0.120 & 0.033 & 84.9 & 6.6 \\
      40--50  & 59.1 & 9.4  & 1.427 & 0.866 & 0.098 & 0.029 & 0.108 & 0.032 & 82.9 & 6.4 \\
      50--60  & 58.9 & 10.1 & 1.433 & 0.887 & 0.103 & 0.028 & 0.111 & 0.031 & 81.5 & 7.0 \\
      60--70  & 53.3 & 11.5 & 1.709 & 1.056 & 0.106 & 0.026 & 0.118 & 0.032 & 77.6 & 7.1 \\
      70--80  & 48.3 & 12.7 & 2.441 & 1.397 & 0.092 & 0.030 & 0.106 & 0.034 & 75.0 & 7.8 \\
      80--90  & 48.3 & 13.9 & 2.668 & 1.660 & 0.093 & 0.024 & 0.107 & 0.029 & 73.2 & 8.6 \\
      90--100 & 35.5 & 15.0 & 3.663 & 2.314 & 0.082 & 0.028 & 0.101 & 0.038 & 65.5 & 8.6 \\
      \bottomrule
    \end{tabular}
  }
  \caption{Prestaties van het ongetraind AutoSDF-model op synthetische data}
  \label{tab:globale_prestaties_ongetraind}
\end{table}

De verwerkte elementen (objecten) bestonden uit rechthoekige- en ronde buizen, alsook verschillende vaten. Onderstaande tabellen (Tabel \ref{tab:globale_prestaties_ongetraind_rechth_buis}, Tabel \ref{tab:globale_prestaties_ongetraind_rond_buis} en Tabel \ref{tab:globale_prestaties_ongetraind_vat}) tonen de globale prestaties van het model, opgedeeld volgens deze verschillende categorie\"{e}n. 
\vspace{-5pt}
\begin{table}
[H]
  \centering
  \resizebox{\textwidth}{!}{%
    \begin{tabular}{lcccccccccc}
      \toprule
      \textbf{Interval} & \textbf{G IOU} & \textbf{SA IOU} & \textbf{G CD} & \textbf{SA CD} & \textbf{G F} & \textbf{SA F} & \textbf{G Cov} & \textbf{SA Cov} & \textbf{G NC} & \textbf{SA NC} \\
      \midrule
      0--5    & 67.4 & 5.3  & 0.516 & 0.353 & 0.124 & 0.028 & 0.133 & 0.030 & 89.7 & 3.6 \\
      5--10   & 65.8 & 5.5  & 0.614 & 0.398 & 0.116 & 0.028 & 0.125 & 0.030 & 88.8 & 4.1 \\
      10--15  & 65.5 & 5.9  & 0.669 & 0.466 & 0.117 & 0.029 & 0.126 & 0.030 & 87.6 & 4.6 \\
      15--20  & 65.7 & 5.8  & 0.618 & 0.385 & 0.119 & 0.028 & 0.127 & 0.031 & 87.6 & 4.3 \\
      20--30  & 65.2 & 6.3  & 0.637 & 0.431 & 0.123 & 0.028 & 0.129 & 0.030 & 86.3 & 4.7 \\
      30--40  & 59.1 & 9.3  & 1.335 & 1.285 & 0.113 & 0.032 & 0.122 & 0.032 & 82.7 & 5.9 \\
      40--50  & 54.5 & 8.0  & 1.610 & 0.898 & 0.101 & 0.029 & 0.111 & 0.031 & 78.9 & 5.5 \\
      50--60  & 53.2 & 8.3  & 1.677 & 0.906 & 0.103 & 0.028 & 0.113 & 0.033 & 77.2 & 5.9 \\
      60--70  & 49.6 & 8.1  & 1.976 & 1.034 & 0.106 & 0.026 & 0.116 & 0.031 & 74.1 & 5.7 \\
      70--80  & 45.9 & 9.1  & 2.717 & 1.454 & 0.090 & 0.028 & 0.106 & 0.034 & 72.1 & 5.8 \\
      80--90  & 41.2 & 9.6  & 3.062 & 1.546 & 0.091 & 0.023 & 0.104 & 0.028 & 68.0 & 6.4 \\
      90--100 & 31.3 & 12.4 & 3.875 & 2.242 & 0.082 & 0.029 & 0.101 & 0.039 & 62.2 & 6.9 \\
      \bottomrule
    \end{tabular}
  }
  \caption{Prestaties van het ongetraind AutoSDF-model voor rechthoekige buisdelen}
  \label{tab:globale_prestaties_ongetraind_rechth_buis}
\end{table}

\begin{table}
[H]
  \centering
  \resizebox{\textwidth}{!}{%
    \begin{tabular}{lcccccccccc}
      \toprule
      \textbf{Interval} & \textbf{G IOU} & \textbf{SA IOU} & \textbf{G CD} & \textbf{SA CD} & \textbf{G F} & \textbf{SA F} & \textbf{G Cov} & \textbf{SA Cov} & \textbf{G NC} & \textbf{SA NC} \\
      \midrule
      0--5    & 72.7 & 4.6  & 0.444 & 0.251 & 0.111 & 0.026 & 0.117 & 0.028 & 93.7 & 2.4 \\
      5--10   & 71.6 & 4.8  & 0.541 & 0.328 & 0.104 & 0.027 & 0.110 & 0.028 & 93.2 & 3.1 \\
      10--15  & 71.5 & 5.1  & 0.579 & 0.385 & 0.102 & 0.027 & 0.107 & 0.028 & 92.7 & 3.5 \\
      15--20  & 71.7 & 4.8  & 0.546 & 0.309 & 0.104 & 0.026 & 0.109 & 0.027 & 92.3 & 3.1 \\
      20--30  & 71.7 & 4.9  & 0.525 & 0.312 & 0.111 & 0.026 & 0.116 & 0.027 & 91.8 & 3.3 \\
      30--40  & 67.1 & 8.0  & 0.983 & 0.886 & 0.109 & 0.027 & 0.114 & 0.028 & 88.6 & 4.6 \\
      40--50  & 62.0 & 7.1  & 1.350 & 0.743 & 0.096 & 0.025 & 0.104 & 0.028 & 85.4 & 4.8 \\
      50--60  & 62.1 & 8.1  & 1.308 & 0.786 & 0.104 & 0.026 & 0.112 & 0.028 & 84.3 & 5.3 \\
      60--70  & 60.9 & 7.8  & 1.386 & 0.726 & 0.108 & 0.023 & 0.116 & 0.027 & 82.2 & 4.8 \\
      70--80  & 54.1 & 9.3  & 2.230 & 1.190 & 0.092 & 0.025 & 0.104 & 0.027 & 79.2 & 5.8 \\
      80--90  & 51.9 & 10.3 & 2.431 & 1.384 & 0.094 & 0.023 & 0.106 & 0.027 & 76.0 & 6.5 \\
      90--100 & 39.1 & 15.3 & 3.420 & 2.334 & 0.083 & 0.028 & 0.102 & 0.036 & 68.2 & 9.0 \\
      \bottomrule
    \end{tabular}
  }
  \caption{Prestaties van het ongetraind AutoSDF-model voor ronde buisdelen}
  \label{tab:globale_prestaties_ongetraind_rond_buis}
\end{table}

\begin{table}
[H]
  \centering
  \resizebox{\textwidth}{!}{%
    \begin{tabular}{lcccccccccc}
      \toprule
      \textbf{Interval} & \textbf{G IOU} & \textbf{SA IOU} & \textbf{G CD} & \textbf{SA CD} & \textbf{G F} & \textbf{SA F} & \textbf{G Cov} & \textbf{SA Cov} & \textbf{G NC} & \textbf{SA NC} \\
      \midrule
      0--5    & 64.5 & 14.7 & 0.507 & 0.400 & 0.102 & 0.031 & 0.111 & 0.034 & 92.0 & 4.7 \\
      5--10   & 67.9 & 11.3 & 0.543 & 0.314 & 0.094 & 0.028 & 0.102 & 0.030 & 93.0 & 3.8 \\
      10--15  & 71.7 & 11.2 & 0.509 & 0.342 & 0.095 & 0.027 & 0.100 & 0.029 & 93.4 & 4.2 \\
      15--20  & 69.1 & 12.0 & 0.571 & 0.421 & 0.100 & 0.028 & 0.108 & 0.030 & 91.6 & 4.6 \\
      20--30  & 67.3 & 15.7 & 0.710 & 0.872 & 0.102 & 0.030 & 0.111 & 0.033 & 90.6 & 7.0 \\
      30--40  & 53.5 & 14.5 & 0.892 & 0.810 & 0.113 & 0.033 & 0.135 & 0.032 & 83.4 & 7.8 \\
      40--50  & 63.8 & 11.8 & 1.090 & 0.781 & 0.099 & 0.033 & 0.110 & 0.040 & 87.2 & 5.2 \\
      50--60  & 65.5 & 11.5 & 1.080 & 0.762 & 0.100 & 0.026 & 0.108 & 0.030 & 86.1 & 6.2 \\
      60--70  & 47.0 & 16.4 & 1.575 & 1.267 & 0.105 & 0.028 & 0.138 & 0.035 & 77.8 & 8.8 \\
      70--80  & 44.6 & 18.9 & 2.136 & 1.320 & 0.098 & 0.034 & 0.113 & 0.043 & 75.4 & 10.0 \\
      80--90  & 54.4 & 19.4 & 2.223 & 1.743 & 0.097 & 0.028 & 0.116 & 0.032 & 78.1 & 9.7 \\
      90--100 & 36.6 & 17.6 & 3.748 & 2.015 & 0.080 & 0.026 & 0.097 & 0.039 & 67.8 & 8.7 \\
      \bottomrule
    \end{tabular}
  }
  \caption{Prestaties van het ongetraind AutoSDF-model voor vaten}
  \label{tab:globale_prestaties_ongetraind_vat}
\end{table}


\section{Modeltraining}
De transformer binnen het AutoSDF-model werd getraind in 399 epochs. Voor de training van de transformer wordt de synthetische data, die eerder aangemaakt werd, gebruikt. Deze bestaat uit de pre-processed representaties voor alle synthetische objecten volgens 12 verschillende ori\"{e}ntaties:
\begin{multicols}{2}
\begin{enumerate}
    \item synthetisch-90X-90Y-90Z
    \item synthetisch-45Y-v2
    \item synthetisch-15Y-180Z
    \item synthetisch-180-
    \item synthetisch-60Y-180Z
    \item synthetisch-45X
    \item synthetisch-15Y
    \item synthetisch-60Y
    \item synthetisch-0-v2
    \item synthetisch-45Y-45X
    \item synthetisch-15Y-15X
    \item synthetisch-60Y-60X
\end{enumerate}
\end{multicols}
De bovenstaande dataverzameling wordt random verdeeld in 70\% training-data, 20\% testing-data en 10\% validatiedata. Niet alle data die in de onderstaande resultaten gebruikt wordt, is opgenomen in de training.

Onderstaande figuur geeft het resultaat van de training. Op de x-as bevinden zich het aantal trainingsstappen. De y-as drukt de Negative log-likelihood (NLL), ook gekend als de cross-entropy loss, uit. Hoe lager de waarde voor de NLL, hoe beter. 
\begin{figure}
[H]
    \centering
    \includegraphics[width=1\linewidth]{fig/training_grafiek.png}
    \caption{Resultaat}
    \label{fig:training_grafiek}
\end{figure}

\section{Evaluatie en resultaten na training}
Na training wordt het model op dezelfde wijze verwerkt als in de niet-getrainde fase. Net zoals voorheen worden er per vervollediging twee verschillende oplossingen gegenereerd, waarbij de beste oplossing wordt geselecteerd (uitsluitend voor synthetische data).

In de onderstaande tabellen zijn de nieuwe resultaten van het getrainde model opgelijst volgens dezelfde methode als hierboven beschreven.

\subsection{Gescande data}
De 9 gescande vaten worden na modeltraining opnieuw verwerkt, waarbij steeds 2 oplossingen geproduceerd worden. Deze oplossingen en hun beoordeling volgens de verschillende metrics, worden in onderstaande tabel (Tabel \ref{tab:Prestaties_globaal_scanned_trained} weergegeven. In deze tabel kan meteen vastgesteld worden dat het model na training oplossingen geeft die qua beoordeling al een stuk dichter bij elkaar liggen. 
\begin{table}
[H]
  \centering
    \begin{tabular}{lccccc}
      \toprule
      \textbf{Vat} & \textbf{IOU} & \textbf{CD} & \textbf{F-score} & \textbf{Cov} & \textbf{NC} \\
      \midrule
      Vat1 & 41.2 & 0.004 & 0.045 & 0.063 & 86.2 \\
      Vat1 & 39.8 & 0.005 & 0.028 & 0.041 & 89.0 \\
      Vat2 & 46.5 & 0.004 & 0.058 & 0.076 & 86.9 \\
      Vat2 & 48.3 & 0.003 & 0.049 & 0.057 & 90.5 \\
      Vat3 & 51.8 & 0.013 & 0.063 & 0.085 & 85.6 \\
      Vat3 & 51.9 & 0.011 & 0.068 & 0.086 & 81.2 \\
      Vat4 & 65.0 & 0.005 & 0.026 & 0.036 & 93.6 \\
      Vat4 & 59.5 & 0.005 & 0.022 & 0.027 & 93.9 \\
      Vat5 & 31.8 & 0.023 & 0.048 & 0.073 & 68.8 \\
      Vat5 & 27.3 & 0.025 & 0.045 & 0.066 & 70.2 \\
      Vat6 & 0.0  & 0.006 & 0.007 & 0.011 & 93.9 \\
      Vat6 & 45.6 & 0.006 & 0.020 & 0.033 & 91.9 \\
      Vat7 & 31.0 & 0.039 & 0.079 & 0.086 & 71.7 \\
      Vat7 & 27.1 & 0.039 & 0.092 & 0.097 & 67.7 \\
      Vat8 & 66.9 & 0.008 & 0.104 & 0.120 & 95.6 \\
      Vat8 & 64.8 & 0.008 & 0.095 & 0.111 & 94.8 \\
      Vat9 & 97.1 & 0.010 & 0.161 & 0.190 & 95.0 \\
      Vat9 & 68.6 & 0.009 & 0.182 & 0.218 & 95.0 \\
      \bottomrule
    \end{tabular}
    \caption{Prestaties van het getraind AutoSDF-model op gescande data}
    \label{tab:Prestaties_globaal_scanned_trained}
\end{table}
Ondanks de betere scores op de metrics, blijven de resultaten visueel teleurstellen. Figuur \ref{fig:voorbeelden_buizen_scanned_getraind} geeft daarbij enkele voorbeelden. In deze figuur worden dezelfde buisdelen zoals in Figuur \ref{fig:Resultaten_untrained_scanned.png} weergegeven. Daarvan bevindt de pre-processed representatie van de input zich opnieuw links. Rechts van deze input wordt steeds een oplossing uit het getrainde model gegeven. 

\begin{figure}
    \centering
    \includegraphics[width=0.9\linewidth]{fig/voorbeelden_buizen_scanned_getraind.png}
    \caption{Resultaten uit het getrainde AutoSDF-model voor gescande data}
    \label{fig:voorbeelden_buizen_scanned_getraind}
\end{figure}

\subsection{Synthetische data}
Ook de synthetische data wordt analoog aan de verwerking met het ongetrainde model behandeld en ge\"{e}valueerd. Voor elk object, onder alle ori\"{e}ntaties, met elke voxelselectie worden twee oplossingen gegenereerd. Uit de beide oplossingen wordt enkel de best scorende op vlak van IOU geselecteerd. Met alle selecties wordt dan een globaal gemiddelde en standaardafwijking berekend per graad van volledigheid. Onderstaande tabellen (Tabel \ref{tab:globale_prestaties_getraind}, Tabel \ref{tab:globale_prestaties_getraind_rechth_buis}, Tabel \ref{tab:globale_prestaties_getraind_rond_buis} en Tabel \ref{tab:globale_prestaties_getraind_vat}) geven de resultaten voor de globale prestaties van het model weer en de prestaties per objectklasse. 
\begin{table}
[H]
  \centering
  \resizebox{\textwidth}{!}{%
    \begin{tabular}{lcccccccccc}
      \toprule
      \textbf{Interval} & \textbf{G IOU} & \textbf{SA IOU} & \textbf{G CD} & \textbf{SA CD} & \textbf{G F} & \textbf{SA F} & \textbf{G Cov} & \textbf{SA Cov} & \textbf{G NC} & \textbf{SA NC} \\
      \midrule
      0--5    & 70.9 & 7.2  & 0.334 & 0.096 & 0.121 & 0.031 & 0.128 & 0.034 & 93.3 & 3.3 \\
      5--10   & 72.0 & 5.9  & 0.337 & 0.099 & 0.119 & 0.031 & 0.125 & 0.033 & 93.7 & 3.3 \\
      10--15  & 72.4 & 6.5  & 0.351 & 0.127 & 0.121 & 0.031 & 0.127 & 0.033 & 93.3 & 3.9 \\
      15--20  & 72.0 & 6.3  & 0.352 & 0.123 & 0.123 & 0.031 & 0.129 & 0.034 & 93.0 & 3.8 \\
      20--30  & 71.3 & 7.3  & 0.371 & 0.168 & 0.124 & 0.032 & 0.131 & 0.034 & 92.2 & 4.4 \\
      30--40  & 68.4 & 7.8  & 0.448 & 0.278 & 0.126 & 0.034 & 0.134 & 0.036 & 90.6 & 4.7 \\
      40--50  & 69.8 & 7.1  & 0.465 & 0.280 & 0.125 & 0.035 & 0.134 & 0.038 & 90.3 & 4.8 \\
      50--60  & 69.9 & 7.2  & 0.498 & 0.338 & 0.130 & 0.037 & 0.138 & 0.040 & 89.5 & 5.4 \\
      60--70  & 65.7 & 9.6  & 0.584 & 0.405 & 0.130 & 0.036 & 0.139 & 0.040 & 87.3 & 6.2 \\
      70--80  & 63.9 & 10.7 & 0.705 & 0.547 & 0.126 & 0.042 & 0.134 & 0.046 & 86.1 & 6.4 \\
      80--90  & 65.0 & 10.9 & 0.829 & 0.657 & 0.124 & 0.040 & 0.130 & 0.044 & 84.8 & 7.3 \\
      90--100 & 58.6 & 13.2 & 1.164 & 0.987 & 0.120 & 0.045 & 0.128 & 0.051 & 80.7 & 8.4 \\
      \bottomrule
    \end{tabular}
  }
  \caption{Prestaties van het getraind AutoSDF-model}
  \label{tab:globale_prestaties_getraind}
\end{table}

\begin{table}
[H]
  \centering
  \resizebox{\textwidth}{!}{%
    \begin{tabular}{lcccccccccc}
      \toprule
      \textbf{Interval} & \textbf{G IOU} & \textbf{SA IOU} & \textbf{G CD} & \textbf{SA CD} & \textbf{G F} & \textbf{SA F} & \textbf{G Cov} & \textbf{SA Cov} & \textbf{G NC} & \textbf{SA NC} \\
      \midrule
      0--5    & 69.7 & 4.6  & 0.338 & 0.106 & 0.130 & 0.029 & 0.139 & 0.031 & 91.8 & 3.0 \\
      5--10   & 69.5 & 4.4  & 0.342 & 0.123 & 0.131 & 0.030 & 0.139 & 0.031 & 91.5 & 3.3 \\
      10--15  & 69.2 & 4.8  & 0.371 & 0.169 & 0.131 & 0.030 & 0.139 & 0.032 & 90.6 & 3.8 \\
      15--20  & 69.1 & 4.8  & 0.372 & 0.161 & 0.133 & 0.030 & 0.141 & 0.032 & 90.5 & 3.8 \\
      20--30  & 69.0 & 5.3  & 0.395 & 0.214 & 0.134 & 0.030 & 0.143 & 0.032 & 89.8 & 4.4 \\
      30--40  & 66.7 & 5.4  & 0.487 & 0.318 & 0.134 & 0.031 & 0.143 & 0.033 & 88.5 & 4.4 \\
      40--50  & 66.7 & 6.3  & 0.511 & 0.367 & 0.134 & 0.032 & 0.144 & 0.035 & 87.5 & 5.0 \\
      50--60  & 65.9 & 7.1  & 0.564 & 0.445 & 0.136 & 0.035 & 0.145 & 0.039 & 86.3 & 5.7 \\
      60--70  & 63.2 & 7.7  & 0.674 & 0.501 & 0.136 & 0.033 & 0.145 & 0.036 & 84.2 & 5.9 \\
      70--80  & 61.8 & 8.5  & 0.825 & 0.648 & 0.129 & 0.038 & 0.137 & 0.043 & 83.3 & 6.4 \\
      80--90  & 58.9 & 10.1 & 1.078 & 0.823 & 0.119 & 0.038 & 0.125 & 0.040 & 80.0 & 7.2 \\
      90--100 & 55.6 & 11.6 & 1.253 & 1.031 & 0.119 & 0.044 & 0.127 & 0.049 & 77.0 & 8.5 \\
      \bottomrule
    \end{tabular}
  }
  \caption{Prestaties van het getraind AutoSDF-model voor rechthoekige buisdelen}
  \label{tab:globale_prestaties_getraind_rechth_buis}
\end{table}
\begin{table}
[H]
  \centering
  \resizebox{\textwidth}{!}{%
    \begin{tabular}{lcccccccccc}
      \toprule
      \textbf{Interval} & \textbf{G IOU} & \textbf{SA IOU} & \textbf{G CD} & \textbf{SA CD} & \textbf{G F} & \textbf{SA F} & \textbf{G Cov} & \textbf{SA Cov} & \textbf{G NC} & \textbf{SA NC} \\
      \midrule
      0--5    & 74.4 & 3.7  & 0.325 & 0.077 & 0.114 & 0.028 & 0.119 & 0.030 & 95.4 & 1.7 \\
      5--10   & 74.5 & 3.4  & 0.331 & 0.077 & 0.115 & 0.028 & 0.119 & 0.029 & 95.3 & 2.0 \\
      10--15  & 74.8 & 3.3  & 0.337 & 0.084 & 0.118 & 0.029 & 0.122 & 0.030 & 95.0 & 2.4 \\
      15--20  & 74.8 & 3.3  & 0.333 & 0.078 & 0.118 & 0.029 & 0.122 & 0.030 & 95.0 & 2.1 \\
      20--30  & 74.3 & 3.8  & 0.352 & 0.118 & 0.119 & 0.030 & 0.125 & 0.031 & 94.4 & 2.4 \\
      30--40  & 73.2 & 4.6  & 0.414 & 0.209 & 0.118 & 0.032 & 0.123 & 0.033 & 93.4 & 2.7 \\
      40--50  & 72.4 & 4.4  & 0.440 & 0.181 & 0.120 & 0.035 & 0.128 & 0.037 & 92.5 & 2.9 \\
      50--60  & 72.3 & 4.9  & 0.461 & 0.211 & 0.129 & 0.037 & 0.136 & 0.039 & 91.7 & 3.1 \\
      60--70  & 71.1 & 5.1  & 0.527 & 0.260 & 0.125 & 0.034 & 0.131 & 0.037 & 90.4 & 3.9 \\
      70--80  & 69.4 & 6.3  & 0.638 & 0.404 & 0.123 & 0.043 & 0.130 & 0.047 & 89.5 & 3.7 \\
      80--90  & 68.6 & 7.0  & 0.737 & 0.482 & 0.125 & 0.040 & 0.130 & 0.044 & 87.6 & 5.1 \\
      90--100 & 62.1 & 12.3 & 1.016 & 0.803 & 0.124 & 0.044 & 0.132 & 0.050 & 83.8 & 6.9 \\
      \bottomrule
    \end{tabular}
  }
  \caption{Prestaties van het getraind AutoSDF-model voor ronde buisdelen}
  \label{tab:globale_prestaties_getraind_rond_buis}
\end{table}
\begin{table}[ht]
  \centering
  \resizebox{\textwidth}{!}{%
    \begin{tabular}{lcccccccccc}
      \toprule
      \textbf{Interval} & \textbf{G IOU} & \textbf{SA IOU} & \textbf{G CD} & \textbf{SA CD} & \textbf{G F} & \textbf{SA F} & \textbf{G Cov} & \textbf{SA Cov} & \textbf{G NC} & \textbf{SA NC} \\
      \midrule
      0--5    & 66.2 & 13.8 & 0.344 & 0.097 & 0.107 & 0.027 & 0.115 & 0.031 & 93.6 & 3.9 \\
      5--10   & 70.4 & 10.7 & 0.341 & 0.058 & 0.100 & 0.027 & 0.106 & 0.029 & 95.0 & 2.6 \\
      10--15  & 73.6 & 10.5 & 0.343 & 0.074 & 0.107 & 0.026 & 0.111 & 0.028 & 95.1 & 2.9 \\
      15--20  & 71.5 & 11.2 & 0.356 & 0.083 & 0.109 & 0.026 & 0.115 & 0.028 & 94.1 & 3.1 \\
      20--30  & 70.2 & 12.9 & 0.352 & 0.080 & 0.110 & 0.028 & 0.117 & 0.032 & 93.5 & 4.0 \\
      30--40  & 59.8 & 11.9 & 0.370 & 0.136 & 0.116 & 0.026 & 0.130 & 0.031 & 90.8 & 3.7 \\
      40--50  & 71.4 & 10.2 & 0.399 & 0.129 & 0.114 & 0.033 & 0.122 & 0.035 & 92.5 & 3.3 \\
      50--60  & 73.8 & 7.4  & 0.414 & 0.137 & 0.116 & 0.030 & 0.123 & 0.034 & 92.5 & 3.5 \\
      60--70  & 59.9 & 14.9 & 0.436 & 0.173 & 0.120 & 0.037 & 0.136 & 0.044 & 89.3 & 5.2 \\
      70--80  & 60.3 & 16.0 & 0.466 & 0.234 & 0.117 & 0.042 & 0.130 & 0.045 & 88.3 & 5.1 \\
      80--90  & 68.6 & 13.4 & 0.571 & 0.330 & 0.131 & 0.040 & 0.139 & 0.045 & 88.1 & 4.9 \\
      90--100 & 56.3 & 16.0 & 1.436 & 1.296 & 0.110 & 0.048 & 0.115 & 0.055 & 82.0 & 7.0 \\
      \bottomrule
    \end{tabular}
  }
  \caption{Prestaties van het getraind AutoSDF-model voor vaten}
  \label{tab:globale_prestaties_getraind_vat}
\end{table}
Een beoordeling en bespreking van alle resultaten in bovenstaande tabellen en figuren volgt in Hoofdstuk \ref{ch/Discussion}.




\documentclass[a4paper,twoside]{article}

\usepackage{epsfig}
\usepackage{subcaption}
\usepackage{calc}
\usepackage{amssymb}
\usepackage{amstext}
\usepackage{amsmath}
\usepackage{amsthm}
\usepackage{multicol}
\usepackage{pslatex}
\usepackage{apalike}
\usepackage{algorithm2e}
\usepackage[bottom]{footmisc}
\usepackage{cite}
\usepackage{SCITEPRESS}     % Please add other packages that you may need BEFORE the SCITEPRESS.sty package.

\begin{document}

\title{Geometry and Texture Completion of Partially Scanned 3D Objects Through Material Segmentation}

\author{
    \authorname{
    Jelle Vermandere\sup{1}\orcidAuthor{0000-0002-7809-9798}, 
    Maarten Bassier\sup{1}\orcidAuthor{0000-0001-8526-8847},
    Maarten Vergauwen\sup{1}\orcidAuthor{0000-0003-3465-9033}}
\affiliation{\sup{1}KU Leuven, Belgium}
\email{\{jelle.vermandere, maarten.bassier, maarten.vergauwen\}@kuleuven.be}
}

\keywords{Indoor 3D, Mesh geometry models, texturing}

\abstract{This work aims to improve the geometry and texture completion of partially scanned 3D objects in indoor environments through the integration of a novel material prediction step. Completing segmented objects from these environments remains a significant challenge due to high occlusion levels and texture variance. State-of-the-art techniques in this field typically follow a two-step process, addressing geometry completion first, followed by texture completion. Although recent advancements have significantly improved geometry completion, texture completion continues to focus primarily on correcting minor defects or generating textures from scratch. This work highlights key limitations in existing completion techniques, such as the lack of material awareness, inadequate methods for fine detailing, and the limited availability of textured 3D object datasets. To address these gaps, a novel completion pipeline is proposed, enhancing both the geometry and texture completion processes. Experimental results demonstrate that the proposed method produces clearer material boundaries, particularly on scanned objects, and generalizes effectively even with synthetic training data.}

\onecolumn \maketitle \normalsize \setcounter{footnote}{0} \vfill

%=====================Introduction=====================%
\section{Introduction}
% meer consise
% 
% BACKGROUND: Main Problem statement: the need for indoor scene/ object completion
Dynamic 3D scanned indoor environments are increasingly in demand within the gaming industry and the Architecture, Engineering, Construction, and Operations (AECO) sectors~\cite{Vermandere2022}. Like digitally created assets, these environments consist of collections of digital objects that can be interacted with (e.g., by modifying or removing objects) or utilized in computations (e.g., volumetric analysis).

% \cite{Heckbert1986}% meshes & texture mapping

% The ability to manipulate and compute with objects in a scanned environment is crucial to these applications as it elevates interactivity and allows numerical calculations.~\cite{rs14112680}. 

3D scanned environments are typically captured as a whole, not only for efficiency but also for cost-effectiveness. However, when isolating an object from a scene, it is often incomplete due to occlusions and contact with other objects. This missing information presents a significant bottleneck, as the aforementioned applications require complete object data for both geometry and texture~\cite{Vermandere2023}. As a result, there is an urgent need for completion methods. Traditionally, geometry and texture completion, typically represented as polygonal meshes, is performed through interpolation. With recent advancements in machine learning and neural networks, it is now possible to probabilistically predict these outputs~\cite{Mittal2022}. Meshes provide a lightweight and scalable representation of 3D scene data, making them effective for scanned environments as they can achieve a similar level of detail to point clouds, while retaining highly detailed texture representations.

% These objects are typically represented by either point clouds or polygonal mesh geometries, with the latter being the clear favorite as it is less computationally demanding and contains detailed textures~\cite{Vermandere2023}. 

% often result in incomplete representations due to occlusions and contact with other objects. These missing parts need to be completed in order to achieve a complete and functional 3D model. 
%how are meshes built and the 2d texture map
% 3D scenes are typically stored as polygonal meshes, due to their efficient way to represent 3D shapes and are typical ly coupled with a 2D texture map which contains the color information ~\cite{Vermandere2023}. The disconnect between geometry and texture has made it difficult to create singular methods that can complete both geometry and texture simultaneously.

% What does the SOA look like
Current state-of-the-art (SOTA) techniques typically divide the completion process into two stages, beginning with geometry completion, followed by texture completion. In recent years, significant research has focused on geometry completion~\cite{Liu2023, Lin2022, Gao2020, Zhou2021, Chibane2020}, while research on texture completion has also gained increasing popularity~\cite{Cheng2022, Oechsle2019, siddiqui2022texturify, lugmayrRepaint}. However, existing texture completion methods are primarily focused on either restoring minor defects~\cite{Maggiordomo2023} or generating complete textures from scratch~\cite{siddiqui2022texturify, Richardson2023}.

% (MB) dit is meer voor RL
Texture completion is currently achieved through texture inpainting techniques. However, these methods are often limited to filling very small missing regions~\cite{Maggiordomo2023}, which results in blurry outputs when applied to larger gaps, or they lack fine details due to the limited spatial resolution of 3D inpainting techniques~\cite{Chibane2020}. Additionally, deploying trained models on real-world captured data frequently leads to lower-quality results, as many machine learning models are predominantly trained on synthetic data. This discrepancy creates a gap between training data and real-world inputs.



% The goal of this work
The goal of this work is to improve both the geometry and texture completion on partially scanned meshes. Specifically, the proposed method predicts both the missing polygonal mesh faces and textures of objects segmented from 3D scanned environments. The procedure still treats geometry and texture separately. By splitting the texture completion process into a material prediction and texture inpainting step, as shown in Figure~\ref{fig:methodology}, the material boundaries can be more clearly defined. This gives the texture inpainting module a clear inpainting and reference area, which improves the final results. This also allows the usage of synthetic training data for the 3D material prediction step, as no real textures are needed. The realistic textures can then be inpainted on the 2D texture map of the object.

The insertion of the novel material prediction step in the object completion pipeline abstracts the texture inpainting process allowing better real-world results while still using synthetic material datasets.

% the structure
%The remainder of this work is structured as follows. The background and related work is presented in Section~\ref{sec:background}. Next is the explanation of the proposed method in Section~\ref{sec:methodology}. In Section~\ref{sec:experiments}, an overview of the used datasets and their results is presented. In Section~\ref{sec:discussion}, the method is discussed. Finally, the conclusions are presented in Section~\ref{sec:conclusion}.


%=====================BACKGROUND=====================%
% wat is het, waarom belangrijk, 
\begin{figure*}[!h]
    \centering
    \includegraphics[width=\textwidth]{images/methodology.png}
    \caption{Overview of the object completion pipeline, starting with an incomplete mesh (left), featuring the parallel geometry completion and material segmentation (center-left), followed by the texture completion (center-right) to result in a completed, textured mesh.}
    \label{fig:methodology}
\end{figure*}

\section{Background and related work}
\label{sec:background}

% Object generation
% Geometry completion
% Object texturing
% Texture inpainting
% Texture completion?
In this section, the state-of-the-art of the three main steps in the completion pipeline are discussed.

\subsection{Geometry completion}
%what are SDFs?
Mesh completion is a challenging task because meshes have no fixed input size, which is a requirement of machine learning networks. Some models aim to overcome this by using a retrieval-based method \cite{Gao2023, Siddiqui2021} which aims to replace the partial data with existing models from a library.However, this limits the generality of the objects that can be completed. This is why most works convert the meshes to either point clouds or Signed Distance Fields (SDFs).

%point based
Point-based geometry completion like Point-Voxel Diffusion~\cite{Zhou2021} uses a normalized point cloud as input to predict the final shape through 3D diffusion. On the other hand, IF-Nets~\cite{Chibane2020} use implicit features generated from the point cloud to predict the missing points. While these models can provide good results, the point cloud sampling can lead to a loss of detail in very dense areas and struggles with large missing parts, something which is very common in incomplete scanned objects.

%IF-Net \cite{Chibane2020} Implicit feature networks for shape reconstruction \\
%Point- Voxel Diffusion \cite{Zhou2021} 3d diffusion to complete shapes \\

%SDF based
SDFs are an implicit representation of a 3D shape. They define a function which represents the distance to the boundary of the object from any point in space. They are signed because they also define whether a point is inside or outside the object. An SDF can be voxelised to create a fixed amount of distances. These have become a popular input type due to their clear boundary definition. Models like AutoSDF~\cite{Mittal2022} are able to encode the SDFs and, by highlighting the voxels in the incomplete regions, can predict the missing geometry. SD Fusion~\cite{Cheng2022} builds upon this by allowing multiple input types to guide the generation simultaneously like text prompts or images. Because of the encoding, these can be used to generate a complete shape based on a very small existing part. Models like PatchComplete \cite{Rao2022} split the SDF into multiple smaller parts to increase its generalizability, while DiffComplete \cite{Chu2023} uses a diffusion-based approach to allow for a higher flexibility of inputs. While SDFs result in decreased resolution due to the voxelisation, they are much better at retaining the surface definition of the object compared to point clouds. Non-watertight meshes can be difficult to convert to an SDF due to the ambiguity of what is inside and what is not. Jacobson et al.\cite{jacobsen2013} aims to solve this by generalizing the winding number for arbitrary meshes, however, this method lacks when large parts of the mesh are missing. To handle real-world incomplete scanned objects, Unsigned Distance Fields (UDFs) can be used as a more generalized representation which only defines the absolute distance to the object. These can be generated for arbitrary meshes and are compatible with geometry completion networks like AutoSDF. Therefore, we use the UDF representation of incomplete meshes and complete them using AutoSDF.
 
%AutoSDF \cite{Mittal2022} Creating generic shape priors to complete partial shapes \\
%ShapeFormer \cite{Yan2022} Transformer based shape completion vector quantized deep implicit function (VQDIF) \\
%SD Fusion \cite{Cheng2022}

\subsection{Texture Completion}

A challenge when trying to complete the texture of a 3D mesh using its 2D texture map is the inconsistent layout of the UV texture map, where adjacent 3D faces are not always adjacent in 2D. Despite works like \cite{Maggiordomo2021} trying to improve this, this is still a field of ongoing research.  TUVF~\cite{Cheng2023} aims to create a standard UV layout for each object class, making it much easier to generate consistent textures for an object. This creates a much more predictable inpainting region but severely limits the geometric variation in the objects. Texture Inpainting for Photogrammetric Models~\cite{Maggiordomo2023} aims to overcome this by focusing on smaller patches that are dynamically unwrapped on the texture map. This minimizes distortion and ensures that the surrounding reference area is consistent. For larger areas, works like Image quilting for texture synthesis and transfer~\cite{Efros2001} use an input sample to learn how to inpaint the missing parts leading to very consistent results in distinct materials. When provided with a clear reference area of a single material, these models perform very well. Instead of inpainting directly on the uv map, TEXTure~\cite{Richardson2023} generates 2D textured renders of the object from different viewpoints using diffusion and projects them on the object. Circumventing the need for a clean uv map.
%Image inpainting has seen a big leap in the SOA with the advent of publicly available models like Stable Diffusion~\cite{Rombach2021}. These models use a diffusion-based process to predict a final image based on a prompt. It is also possible to predict a part of an image, based on the existing part. Models like RePaint~\cite{lugmayrRepaint} can infer new pixels based on the existing parts of the image, optionally guided by a text prompt. These models  %how does this background link to your method?

%RePaint \cite{lugmayrRepaint} diffusion based inpainting
%Texture inpainting for photogrammetric models \cite{Maggiordomo2023} \\
%Image quilting for texture synthesis and transfer \cite{Efros2001}\\

Recent works like Texture Fields~\cite{Oechsle2019} have tried to tackle the texture generation in a similar way compared to the geometry generation, by encoding the texture in 3D space instead of on the 2D plane. This has lead to a number of other works like Texturify~\cite{siddiqui2022texturify} uses texture fields to generate plausible textures for certain object classes. SD Fusion~\cite{Cheng2022} is able to directly colorize the generated geometry by using text prompts. While these models do not take the existing partial textures into account, the introduction of Texture Fields has lead to networks like IF-Net Texture~\cite{Chibane2021}, which uses partially colored point clouds to predict the remaining, uncolored points. The point-wise structure limits the spatial resolution which can be too low for fine details, leading to unclear boundaries of the different materials. Similar to Image Quilting~\cite{Efros2001}, this method can greatly benefit from a clear material boundary and is therefore implemented in our framework as texture completion network.

Point-UV Diffusion~\cite{YuTexturegeneration} aims to combine the two texture generation methods by working in a two-step process. First, a coarse 3D point-wise texture is generated. Second, a fine 2D texture map inpainting is performed based on the point colors. TSCom-Net~\cite{Karadeniz2022} uses the same method, but also focuses on texture completion. The completed 3D texture is projected back on a texture map and the coarse color is used to inpaint finer detail directly on the texture. While this improves the detail in the texture, the fuzzy edges of the materials still lead to inconsistent results.

%IF-Net Texture \cite{Chibane2021}
%TSCom-Net \cite{Karadeniz2022} 3D partial textured scans completion in 2 steps, extension on IF-NET \\
%Texture fields \cite{Oechsle2019} \\
%Texturify \cite{siddiqui2022texturify} \\
%TEXTure \cite{Richardson2023} \\
%Point UV diffusion \cite{YuTexturegeneration} \\
%TUVF \cite{Cheng2022} Generalizable texture UV radiance fields\\

\subsection{Material Detection}
The different materials of an object can be detected, both in 2D and 3D.
In 2D, material differences can be detected on the texture map using image segmentation models like Segment Anything Model (SAM)~\cite{Kirillov2023} which can detect distinct objects or textures by determining large similar areas in the image. Materialistic \cite{Sharma2023} specializes in detecting similar materials in a single image, however, does not allow for much granularity in the matching process.  Other works aim to segment the object based on an image of their 3D appearance using Material-Based Segmentation of Objects~\cite{Stets2019} creating a view-based segmentation. Some reflective materials can be hard to segment because they reflect the environment, resulting in visually confusing images. Multimodal Material Segmentation~\cite{LiangSegment} uses multiple camera types like RGB, near-infrared and polarized images to further improve the detection rate of these materials.

In 3D, models like TextureNet~\cite{Huang2018} leverage the color of the feature points in a 3D scene to create more distinct feature vectors, improving the segmentation results. This also allows the model to segment the different materials in a single object. These material segmentation techniques are underused to aid the texture inpainting process. Therefore, we aim to use a similar technique to segment the objects on a sub-material level.

%Material-Based Segmentation of objects \cite{Stets2019} \\
%Segment Anything Model \cite{Kirillov2023} \\
%Multimodal Material Segmentation \cite{LiangSegment}\\
%TextureNet \cite{Huang2018} Material based object segmentation

%=====================Methodology=====================%

\section{Methodology}
\label{sec:methodology}
The presented method (Figure~\ref{fig:methodology}) follows the SOTA approach to separate the geometry and texture completion. First, the missing geometry is predicted from the geometric inputs by utilizing implicit shape representations~\cite{Mittal2022}. In addition, the mesh textures are analyzed, and material information for the meshes is computed based on image segmentation. Second, these results are integrated, and the missing materials are predicted using the texture generation network, IF-Net~\cite{Chibane2021}. In the final step, a detailed inpainting of the missing regions is conducted, utilizing both shape and material information to complete the mesh representation. This process results in a comprehensive prediction of the object's shape and appearance, that can be rendered in photorealistic detail.

\subsection{Geometry Completion}
\label{meth:geometry}
The first step in the geometry prediction is the preprocessing of the mesh geometry to a suitable implicit shape representation i.e. a continuous volumetric field. In the literature, SDFs are typically used as it can be easily discretised into a voxel raster with a fixed number of distances, which is compatible with CNN architectures~\cite{Mittal2022}. However, as explained in the related work, conventional SDF assume the shape to be watertight, which is not the case for our geometry prediction. Instead, we employ a UDF to voxelise the mesh geometries. Concretely, we employ a dual octree graph as proposed by~\cite{Wang2022} with $128^3$ resolution to represent the geometry (Figure~\ref{fig:methodology-implicit}). Then, we use the open edges of the incomplete mesh surface to indicate the voxels for which a prediction must be computed. As shape completion networks currently only operate on geometries that are positioned symmetrically and centered, we also perform a grounding and symmetrisation step to optimize the objects' position based on~\cite{Sipiran2014}.



% \subsection{Implicit Surface Generation}
% \label{meth:implicit}
% use mesh2SDF
% use Unsigned distance field
%Before the partial mesh can be completed using the VAE-Network, it needs to be standardised into a rigid input structure. The method of our choice is using SDFs. Since the meshes are incomplete by definition, there is no guarantee that the mesh is watertight. This means it is not possible to define a traditional Signed Distance Field (SDF). It is however always possible to create an unsigned distance field (USDF) of an arbitrary triangle mesh. 
% The incomplete mesh is converted to a USDF to serve as the input for the geometry completion network with a resolution of $128^3$ voxels using Mesh2SDF, a python algorithm used for pre-processing~\cite{Wang2022}. The USDF is generated as seen in Figure~\ref{fig:methodology-implicit}.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/methodology-implicit.png}
    \caption{The incomplete mesh (left) and the meshed UDF (right).}
    \label{fig:methodology-implicit}
\end{figure}

% \subsection{Geometry Completion}
% \label{meth:geometry}
Next, the UDF is fed to a shape prediction model that samples vertices in the highlighted voxels. Specifically, we adjust the VQ-VAE autoregressive model proposed in AutoSDF~\cite{Mittal2022} to predict the distribution over the latent representation of 3D shapes and solve it for shape completion (Eq.~\ref{eq:AutoSDF}). The voxel selection process has been further refined, to allow for a more granular selection. This allows us to better define the correct parts of the partially scanned object. This can be formulated as the conditional probability optimisation of $k$ number of possible solutions of the 3D shape $\mathbf{X}$ given the partially observed shape $\mathbf{X}_p$, which are expressed as a set of latent variables $\boldsymbol{O}=\{z_{g_1},z_{g_2},...,z_{g_k}\}$ that are factorized to model the distribution over the latent variables $\mathbf{Z}$ (see VQ-VAE and AutoSDF for more details).

\begin{equation}
    \label{eq:AutoSDF}
    \begin{gathered}
    % shape completion conditional probability
    P(\mathbf{X}|\mathbf{X}_p) \approx p(\mathbf{Z}|\mathbf{O}) = \prod_{j>k} p_{\theta}(z_{\mathbf{g}_j}|z_{\mathbf{g}_{<j}}, \mathbf{O})
    \end{gathered}
\end{equation}

The network returns a number of possible solutions. The best option is selected based on the closest fitting geometry to the original incomplete edges based on the Euclidean distance of the vertices. Note that due to the encoding, the SDF representation was compressed and thus the overlap between the original edges and the sampled edges can be evaluated. To convert the object back into a mesh, we employ marching cubes~\cite{Lorenson1998}. The result is a watertight polygonal mesh geometry with a topological correct fit between the original and the predicted geometries (Figure~\ref{fig:methodology-geometry}).

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/methodology-geometry.png}
    \caption{Meshed representations of the incomplete UDF (left) and the completed SDF (right).}
    \label{fig:methodology-geometry}
\end{figure}

\subsection{Material Segmentation}
\label{meth:material}
Similar to the geometry completion, the different materials of the objects are identified to produce the inputs for the final texture prediction. Specifically, we compute indices for each distinct material in the object. First, we segment the different texture regions from the texture images of the objects. However, UV maps generated from scanned objects are not ideal for this purpose as these are typically optimised to minimize the texture footprint and maximise each triangle separately. As a result, there is no topological relationship between the adjacent pixels in the texture image compared to the 3D geometry. To counteract this, we re-unwrap each object's texture to preserve this topology while keeping connected parts together (Figure~\ref{fig:methodology-layout}). Building on previous works \cite{Vermandere2024}, this is done by performing a part-wise semantic segmentation \cite{Sun2022}, which splits the object into smaller geometrically more basic parts using 3D semantic instance segmentation. Each part is then unwrapped using Blender's unwrapping API \cite{Flavell2010} with the Angle Based Flattening (ABF) \cite{Chen2007} algorithm.

The resulting unwrapped texture images are then processed by an image segmentation network. Specifically, we transfer the zero-shot segmentation of the Segment Anything Model (SAM)~\cite{Kirillov2023} to our dataset. SAM is a powerful encoder-decoder network trained on over 1.1 billion masks and shows promising results for zero-shot generalization. The result is set of patches containing a large number of disjoint instances of the different materials.

%UV layout
%Captured scenes often use a variety of optimisation techniques to minimize the texture footprint of the 3D environment on the texture map. While this leads to higher overall texture quality and resolution, the layout does not take the 3D locations and relations into account. This can lead to disjointed triangles all over the UV map. For a proper material segmentation, we re-unwrap each object separately to optimise for geometric alignment while keeping the distortion to a minimum as seen in Figure~\ref{fig:methodology-layout}.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/methodology-layout.png}
    \caption{Overview texture preprocessing: (left) The original UV layout and (right) the re-unwrapped UV layout optimised for geometric topology.}
    \label{fig:methodology-layout}
\end{figure}

%material segmentation
% using sam
% using matrial based mesh refinement
% To segment the different materials of the object, we first perform an instance segmentation on the optimised texture map using SAM~\cite{Kirillov2023}. This creates a large amount of patches of the same material.

%patch grouping
Second, these patches $\boldsymbol{P_{set}}$ are grouped per distinct material set $\boldsymbol{S_{set}}$. To this end, the cosine similarity is evaluated between the image feature vectors $f_{P_i}$ of each patch, which are derived from the EfficientNet~\cite{Tan2019} network, given a matching threshold $t_c$. The unique set $\bigcup_{i=1}^{n} S_i$ of the grouped patches are then used to assign a unique material index to each $S_i$ (Figure~\ref{fig:methodology-segment}) as shown in Eq.~\ref{eq:patchgrouping}.

\begin{equation}
    \label{eq:patchgrouping}
    \begin{gathered}
    % Grouping patches into unique sets based on high cosine similarity
    \boldsymbol{S_{set}} = \bigcup_{i=1}^{n} \left \{ S_i=\{P_i, P_j\} \Big| \forall P_i,P_j \in \boldsymbol{P_{set}}: \frac{f_{P_i} \cdot f_{P_j}}{\|f_{P_i}\| \|f_{P_j}\|} \geq t_c \right\}
    \end{gathered}
\end{equation}

% mesh refinement
Next, the material indices are assigned to the partial mesh. However, because the 2D boundaries of the material patches do not necessarily align with the 3D mesh edges, an additional mesh refinement step is performed. Based on previous work~\cite{Vermandere2023}, the boundaries $\boldsymbol{S_{set}}$ between texture materials are baked as new edges in the mesh, and duplicate the involved vertices, so that each face shares the same material in its three vertices. This ensures that each material can be completely isolated in 3D with only a face selection.

%new 3D mesh vertices and edges are created at the 2D texture boundaries of each $S\in \boldsymbol{S}$. To this end, the 2D boundaries are projected on the 3D geometry and used as cutting lines to divide the existing faces. The result is an updated set of mesh faces for which each face only has a single material index. This ensures that each material can be completely isolated in 3D with only a face selection.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/methodology-segment.png}
    \caption{Overview of the texture segmentation using the Segment Anything Model (SAM) and subsequent clustering of the different patches through cosine similarity.}
    \label{fig:methodology-segment}
\end{figure}

\subsection{Texture Completion}
\label{meth:materialCompletion}
For the texture completion, we again employ an implicit representation that can be trained and decoded to predict color information of the missing parts. As we want the texture prediction to be shape sensitive, we retain the spatial encoding of the 3D geometry and expand it with additional color channels. Specifically, our work expands upon IF-Net~\cite{Chibane2021} which extracts a learnable multi-scale tensor of deep features from a spatial encoding of both shape and appearance. Concretely, we first assign the generic segmented material index labels to the completed mesh geometry. Each segment is given a unique color based on its index. Each index is encoded as a combination in three binary channels. Enabling the network to generate up to 8 materials per object. This ensures maximum separation between different materials to minimize potential confusion in the network.

Second, the partially textured mesh is sampled as a point cloud due to IF-Nets point-based encoding. The same voxel grid is employed as during the shape geometry completion. To generate the deep features grids $\boldsymbol{F}_k$, it subsequently convolutes the point cloud with learned 3D convolutions while decreasing the resolution. These features are then passed to the decoder $f(.)$, which predicts the point and material index values at the grid intervals (Eq.~\ref{eq:if-net})~\cite{Chibane2021}.

\begin{equation}
    \label{eq:if-net}
    \begin{gathered}
    % decoding
        f(\boldsymbol{F}_k): F_1 \times \ldots \times F_n \rightarrow [0, 1]
    \end{gathered}
\end{equation}

%\begin{figure}[!h]
%    \centering
%    \includegraphics[width=\columnwidth]{images/methodology-3Dtexture.png}
%    \caption{(left) The input point cloud with missing material indices and (right) the predicted material indices}
%    \label{fig:methodology-3Dtexture}
%\end{figure}

 %as seen in Figure \ref{fig:methodology-3Dtexture}
 Given the material indices, the final step is to compute the detailed textures for the complete mesh. To this end, we leverage patch-based inpainting~\cite{Efros2001}. Because it only uses the surrounding image for reference, the results can be more faithful to the original data compared to more recent generative approaches as it does not suffer from hallucinations. First, the UV layout of the original partial mesh is aligned with the newly created UV layout of the completed geometry. To achieve this, we project the original textures onto the completed geometry and unwrap it together with the material indices. Iteratively, all the patches in a material set $P\in S_i$ are used as reference samples to compute the average texture for the new regions (Figure~\ref{fig:methodology-inpainting}), while $P\notin S$ are masked out. For every new patch, arbitrary square blocks $\{B_1,B_2,...,B_n\}$ from $S_i$ are merged together with overlap to synthesize a new texture sample $P'$. The best fit cut between each two overlapping blocks is retrieved by minimizing neighboring contrasts $e_{ij}=f(B_i,B_j)$. The minimal cut is then obtained by traversing all cuts and computing the cumulative minimum error $E$ for each block (Eq.~\ref{eq:inpainting}).


\begin{equation}
    \label{eq:inpainting}
    \begin{gathered}
    % decoding
        E_{ij} = e_{ij} + \min(E_{i-1,j-1}, E_{i-1,j}, E_{i-1,j+1}).
    \end{gathered}
\end{equation}

% For the newly predicted region

% With the material predicted and remapped on the completed geometry, the last step is painting in the texture.
% To minimise the distortion of the inpainting process, the mesh is UV-unwrapped again while keeping the material patches together.


% Every material is inpainted separately. Using patch-based image inpainting~\cite{Efros2001}, the pre-existing patches of the texture are used as reference samples for the unknown patches as seen in . Using the different materials separately as a mask ensures clear boundaries between materials.

% The segmented partial material texture is projected on the newly created complete mesh. Each segment is given a unique color based on its index. Each index is encoded as a combination of RGB channels. Enabling the network to generate up to 8 materials per object. This ensures maximum separation between different materials to minimize potential confusion in the network.


% and the complete untextured mesh are used as the input for the IF-Net~\cite{Chibane2021}. Since the IF-Net uses a point-based colorization, the meshes are converted to point clouds. Each vertex of the incomplete mesh is given the corresponding material index color. The incomplete textured point cloud is used to generate a texture field for the whole 3D space. The network then generates a plausible color for each point in the complete, uncolored mesh using the generated texture field. The resulting point cloud is both complete and colored. The colors are then re-projected on the original completed mesh, giving each face a material index as explained in Figure~\ref{fig:methodology}. 

%as laid out in Table~\ref{tab:colorchannels}
%\begin{table}
%    \centering
%    \begin{tabular}{|r|c|c|c|c|c|c|c|c|}
%        \hline
%        Channel & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8\\
%        \hline
%        R & x & 0 & 0 & x & x & 0 & x & 0\\
%        G & 0 & x & 0 & x & 0 & x & x & 0\\
%        B & 0 & 0 & x & 0 & x & x & x & 0\\
%        \hline
%    \end{tabular}
%    \caption{The different combinations of color channels create 8 distinct combinations.}
%    \label{tab:colorchannels}
%\end{table}

% \subsection{Texture inpainting}
% \label{meth:texture}
% With the material predicted and remapped on the completed geometry, the last step is painting in the texture.
% To minimise the distortion of the inpainting process, the mesh is UV-unwrapped again while keeping the material patches together.
% Every material is inpainted separately. Using patch-based image inpainting~\cite{Efros2001}, the pre-existing patches of the texture are used as reference samples for the unknown patches as seen in Figure~\ref{fig:methodology-inpainting}. Using the different materials separately as a mask ensures clear boundaries between materials.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/methodology-inpainting.png}
    \caption{The inpainting process where each material is inpainted separately.}
    \label{fig:methodology-inpainting}
\end{figure}


%=====================Experiments=====================%
\section{Experiments}
\label{sec:experiments}

In this section, we will first discuss the dataset used, then the training of our models and finally discuss the results of our experiments.

\subsection{Dataset Preprocessing}
Two datasets are used for the experiments. ShapeNetCore~\cite{Chang2015} is a synthetic object library that we use for training and validation. It contains 55 common object categories like chairs, benches and tables, with about 51.300 unique 3D models. It is a good training dataset since it both contains the completed geometries and also the material indices for the textures so both the AutoSDF and IF-Net have correct ground truth data.

On the other hand, Matterport~\cite{Chang2017} is a scanned dataset that we use for the evaluation. It consists of 90 fully textured building-scale scenes, with each between 15-30 objects that can be segmented and completed (Figure~\ref{fig:experiments-dataset}). It is ideally suited to investigate the domain-transfer capabilities of the network to deal with realistic textures and incomplete geometries. No ground truth is available for this dataset so a visual study is made of the resulting reconstructions. 

A relevant subselection is made from both datasets for the experiments. The AutoSDF training dataset is generated  by converting the meshes to a normalised, aligned $128^3$ SDF grid as discussed in section~\ref{meth:geometry}. The IF-Net input data is created by separating each submesh and giving it a material index. Each mesh is sampled to a colored point cloud from which 4 incomplete variations are created by randomly removing parts of the point cloud. These four incomplete colored point clouds, along with the uncolored complete point cloud are used as the training input.

% Finally we use objects isolated from the Matterport dataset~\cite{Chang2017} to evaluate real-world performance of our method. The dataset It consists of 90 fully textured building-scale scenes. The meshes are semantically segmented making object isolation a straightforward process. These scans contain incomplete objects and a selection is made based on a wide variety in object types and occlusions as seen in .

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/experiments-dataset.png}
    \caption{Examples of isolated objects from the Matterport dataset with varying occlusions and shapes.}
    \label{fig:experiments-dataset}
\end{figure}

\subsection{Training}
For AutoSDF and SAM, we use the pre-trained models available to the public because they are trained on relevant datasets. However, the IF-Net was retrained using the Adam optimizer with a learning rate of $10^{-4}$ for 1000 epochs with a minimal loss of $65.81$ using our custom dataset. The query points for the training data are obtained by sampling the ground truth for 100,000 points. The partial scans are voxelised by sampling 100,000 points from the partial surface and setting the occupancy value in the nearest voxel grid to 1. Similarly, for the colored voxelisation, the value of the nearest voxel is set to the three-channel value of the corresponding material index.

%\begin{figure}[!h]
%    \centering
%    \includegraphics[width=\columnwidth]{images/IFNet Training.png}
%    \caption{The IF-NET training/loss graph}
%    \label{fig:experiments-training}
%\end{figure}

\subsection{Results}

%%%%%%%%%%%%%%%%%%%%% Geometry prediction %%%%%%%%%%%%%%%%%%%

\subsubsection{Geometry Completion}
For the geometry completion using AutoSDF, the validation is performed by completing the objects from the ShapeNet dataset (Figure \ref{fig:experiments-results-shapenet}) at different levels of completeness. Table \ref{tab:experiments-geometryresults} shows the resulting average MIOU and Chamfer distance of the dataset. Each object is completed with 25\%, 50\% and 75\% of the original mesh remaining. These result show that the MIOU and Chamfer distance increase when more of the original mesh is present. There is, however, still a loss in accuracy due to the voxel-based SDF conversion, leading to lower MIOU.

\begin{table}[!h]
\caption{The MIOU of and Chamfer Distance on the ShapeNet Core v2 dataset, completed at 25, 50 and 75\% respectively}
%\vspace{-4mm} % Adjust the height of the space between caption and tabular
\resizebox{\columnwidth}{!}{
\begin{tabular}{l|r|r|r}
Shapenet Core v2 & 25\% completion    & 50\% completion   & 75\% completion   \\ \hline
MIOU             & 25.26\% & 54.14\% & 62.17\% \\
Chamfer Distance & 0.09    & 0.06    & 0.06   
\end{tabular}}

\label{tab:experiments-geometryresults}
\end{table}

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/experiments-results-shapenet.png}
    \caption{The results from the ShapeNet Core dataset. The rows show the partial inputs, the completed geometry, the final output and the the ground truth.}
    \label{fig:experiments-results-shapenet}
\end{figure}

For the Matterport data, we focus on the visual fidelity and accuracy. Since the completion uses a VQ-VAE network, multiple probable solutions are generated, as seen in Fig~\ref{fig:experiments-geometry}. There is a large variety in proposed solutions due to the large and detailed voxel selection necessary because the UDFs can be hollow in the missing areas. The best option is determined based on the largest overlap with the original incomplete mesh.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/experiments-geometry.png}
    \caption{Examples of the multiple results returned from AutoSDF geometry completion with the input UDF (left) and four possible outputs (right).}
    \label{fig:experiments-geometry}
\end{figure}

%%%%%%%%%%%%%%%%%%%%% Material Prediction %%%%%%%%%%%%%%%%%%%

%\subsubsection{Material Segmentation}

%The Material segmentation is tested on the incomplete UV maps of the Matterport datasets as seen in Figure \ref{fig:experiments-inpainting}.

%\begin{figure}[!h]
%    \centering
%    \includegraphics[width=\columnwidth]{images/experiments-material-segmentation.png}
%    \caption{The segmentation results for the Matterport datasets}
%    \label{fig:experiments-material-segmentation}
%\end{figure}

\subsubsection{Material Prediction}

The material prediction is validated by calculating the percentage of correctly predicted points as seen in Table \ref{tab:experiments-materialresults}. The IF-Net can accurately predict the correct material index if the materials are all present in the partial scan. It does not introduce new materials, leading to lower correctness percentages at the lower completion levels. For smaller defects or missing parts, the material mostly stays consistent.

\begin{table}[!h]
\caption{The average material prediction accuracy on the ShapeNet Core v2 dataset, completed at 25, 50 and 75\% respectively}
%\vspace{-4mm}
\resizebox{\columnwidth}{!}{
\begin{tabular}{l|r|r|r}
ShapeNet Core V2            & 25\% completion & 50\% completion & 75\% completion \\ \hline
Material Correctness        & 60,56\%         & 80,78\%         & 92.07\%     
\end{tabular}}
\label{tab:experiments-materialresults}
\end{table}

The material completion returns good results on the Matterport data for large uniform areas as seen in Figure \ref{fig:experiments-callout}, where the highlighted areas A get a much better result due to a larger reference area. The highlighted B areas have a limited amount of reference area, so they have a very clear repeating pattern. SAM often overly segments because of color artifacts in the original scans, leading to a very high number of patches. Its drawback is that it is impossible to define a single cosine similarity threshold to group the different patches. Therefore, we adapt the threshold to fit the 8-materials constraint.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/experiments-callout.png}
    \caption{The inpainting results (right) with the partial texture (left). highlighted areas "A"and "B" indicate good and poor results respectively.}
    \label{fig:experiments-callout}
\end{figure}


%%%%%%%%%%%%%%%%%%%%% Texture inpainting %%%%%%%%%%%%%%%%%%%

\subsubsection{Texture Inpainting}
The performance of the texture inpainting is measured with the cosine similarity of the predicted patches compared to the ground truth. Table \ref{tab:experiments-textureresults} shows the results at 3 different completion levels. Due to the material prediction step, the inpainter only relies on one type of material as the training area, leading to high results across the board.

\begin{table}[!h]
\caption{The average texture inpainting similarity on the ShapeNet Core v2 dataset, completed at 25, 50 and 75\% respectively}
%\vspace{-4mm}
\resizebox{\columnwidth}{!}{
\begin{tabular}{l|r|r|r}
ShapeNet Core V2            & 25\% completion & 50\% completion & 75\% completion \\ \hline
Cosine Similarity           & 86.34\%         & 90.00\%         & 91.40\%     
\end{tabular}}
\label{tab:experiments-textureresults}
\end{table}

The patched-based inpainting model inpaints the textures as seen in Figure~\ref{fig:experiments-inpainting} with a patch size of $8$ and an overlap size of $2$. To increase the rotational invariance, we use rotations of $[0,45,90,135,180]$ degrees.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{images/experiments-inpainting.png}
    \caption{The inpainting results. The first row shows the predicted material indexes, the second row shows the UV texture map, the third row the mapped incomplete original texture and the final row the texture inpainting.}
    \label{fig:experiments-inpainting}
\end{figure}

%%%%%%%%%%%%%%%%%%%%% Full completion %%%%%%%%%%%%%%%%%%%

\subsubsection{Full Completion}
Figure~\ref{fig:experiments-results} illustrates the completion results on the Matterport dataset compared to the state of the art. Objects with near-complete scans, such as the sofa and stool, yield consistent geometry and texture completions. In contrast, less-scanned objects still produce plausible geometries but face challenges in texture inpainting. Extensive missing areas result in insufficient reference patches, leading to repetitive textures.

\begin{figure*}[!h]
    \centering
    \includegraphics[width=\textwidth]{images/experiments-results.png}
    \caption{The texture inpainting results from the Matterport dataset using our method compared against: patch-based inpainting, TEXTure, and IF-net Texture}
    \label{fig:experiments-results}
\end{figure*}

%%%%% END NEW STUFF %%%%

%=====================Discussion=====================%
\section{Discussion}
\label{sec:discussion}

% Evaluate the results
% comparing the synthetic data
% does the inclusion of a material prediction step improve the results for real-world data? while still being trained on synthetic data

% Where does the method succeed / fail?
    % Geometry
        % + can predict large missing areas because of the shape encoding
        % + returns clear boundaries and clean, but dense meshes
        % - very fine details get lost
        % - The existing parts of the input mesh also get changed due to the VAE encoding
        % + the network returns a lot of variations so the best match can be chosen
    % Material segmentation
        % + can segment basic materials (wood, fabric, plastic) very well
        % + the similarity can group patches that look the same
        % - dependant on the layout UV map
        % - baked lighting can influence the results
    % Material prediction
        % + Better boundaries between materials
        % - Fails if too much is missing
        % - fails if too many different materials
        % - strugles with graphic elements
    % Texture inpainting
        % + The material mask ensures clean reference areas for the inpainting algorithm
        % + Works wery well on repeating textures
        % - Struggles with inpainting orientation
        % - Very dependant on UV layout
    

The AutoSDF network demonstrates robust geometry completion, effectively predicting large missing areas even for meshes with limited ground truth. However, it sacrifices fine details, and the VAE encoding alters originally observed parts. Despite this, AutoSDF outperforms the SOTA Multimodal Point-cloud Completion method (MPC) in preserving existing parts, as shown in~\cite{Mittal2022}.

Material segmentation with SAM excels at accurately segmenting basic materials like wood, fabric, and plastic, regardless of orientation. However, it struggles with very small patches due to 2D resolution limits and is affected by lighting conditions, as shadows and reflections are baked into the object during capture, similar to~\cite{siddiqui2022texturify}.

Material prediction enhances boundary definitions between materials, improving representation. Challenges remain with extensive missing areas or objects featuring numerous distinct materials. Patch-based image inpainting struggles with irregular patterns, such as printed illustrations or intricate details, underscoring the need for better handling of nonuniform textures. Unlike~\cite{Stets2019}, which limits segmentation to predefined material classes, our method assigns generic material labels to patches, abstracting actual materials and shifting the challenge to 2D inpainting.

Incorporating the material mask—a map indicating patches with the same material index—into the completion process ensures cleaner reference areas and facilitates effective inpainting for repeating textures. However, difficulties in inpainting orientation and the reliance on UV layout pose challenges, potentially limiting the method’s broader applicability.



%=====================Conclusion=====================%
\section{Conclusion}
\label{sec:conclusion}

% We made a novel object and texture completion method
% the geometry is predicted based on USDF's 
% completing the texture in 3 steps
% first material segmentation step on the partial objects -> this abstracts the object to better suit the training data
% then predict the material of the generated geometry -> color channel prediction
% finally 2d inpainting of the texture on the UV map
% the results are good, giving good results for real scans
% future work
% improve the UV unwrapping
% improve the occlusion detection
% expand to full scene

This study presents a novel material prediction step in the geometry and texture completion pipeline for partially scanned 3D objects. The process begins with geometry prediction to establish the structure, followed by a three-step texture completion. First, the partial UV map undergoes material segmentation using SAM to abstract the object for alignment with training data. Next, the IF-Net network predicts the material for missing areas. Finally, a 2D inpainting refines the texture on the UV map for visual detail.

Our method delivers promising results, particularly with real scans, achieving clearer material boundaries and advancing the state of the art. However, areas for improvement remain: enhancing UV unwrapping could refine texture mapping, and better occlusion detection would improve scene accuracy. Future work may explore applying this approach to full-scene reconstructions.

%%
%% The acknowledgments section is defined using the "acks" environment
%% (and NOT an unnumbered section). This ensures the proper
%% identification of the section in the article metadata, and the
%% consistent spelling of the heading.
%\begin{acks}
%This project has received funding from the FWO SB grant (grant agreement: 1S16923N) and the Geomatics research group of the Department of Civil Engineering, Faculty of Engineering Technology at the KU Leuven in Belgium.
%\end{acks}
%%

\bibliographystyle{apalike}
{\small
\bibliography{export}}


%%\section*{\uppercase{Appendix}}

%%\begin{figure*}[!h]
%%    \centering
%%    \includegraphics[width=\textwidth]{images/experiments-results-Large.png}
%%    \caption{The inpainting results. The rows show the step-by-step process of the full completion workflow.}
%%    \label{fig:experiments-large}
%%\end{figure*}

\end{document}

% Version 2022-09-20
% update – 161114 by Ken Arroyo Ohori: made spacing closer to Word template throughout, put proper quotes everywhere, removed spacing that could cause labels to be wrong, added non-breaking and inter-sentence spacing where applicable, removed explicit newlines
% update – 010819 by Dennis Wittich: made spacing and font size closer to Word template, updated references and refernces style
% update – 042319 by Dennis Wittich: font size of captions set to 'small', first author names are shortened, hyphenation fixed
% update – 010620 by Dennis Wittich: Footnotes alignment set to left
% update - 151220 by Clement Mallet: Template adapted for double blind full paper submissions
% update - 060321 by Christian Heipke: Template refined for double blind full paper submissions
% update - 090921 by Christian Heipke: Template refined for double blind full paper submissions
% update - 200922 by Christian Heipke: general template update
% update - 080124 by Christian Heipke: general template update

\documentclass{isprs} % isprs class modified 23-04-2019 (Dennis Wittich)
\usepackage{subfigure}
\usepackage{setspace}
\usepackage{geometry} % added 27-02-2014 Markus Englich
\usepackage{epstopdf}
\usepackage[labelsep=period]{caption}  % added 14-04-2016 Markus Englich - Recommendation by Sebastian Brocks
\usepackage[british]{babel} 
\usepackage[hang]{footmisc}
\usepackage{amsmath}
\def\footnotemargin{1em} % added 08-01-2020 Dennis Wittich

%\usepackage[authoryear]{natbib}
%\def\bibhang{0pt}

\geometry{a4paper, top=25mm, left=20mm, right=20mm, bottom=25mm, headsep=10mm, footskip=12mm} % added 27-02-2014 Markus Englich
%\usepackage{enumitem}

%\usepackage{isprs}
%\usepackage[perpage,para,symbol*]{footmisc}

%\renewcommand*{\thefootnote}{\fnsymbol{footnote}}
\captionsetup{justification=centering,font=normal} % thanks to Niclas Borlin 05-05-2016
\captionsetup[figure]{font=small} % added 23-04-2019 Dennis Wittich
\captionsetup[table]{font=small} % added 23-04-2019 Dennis Wittich

\begin{document}

\title{Adaptive Scaling with Geometric and Visual Continuity of completed 3D objects}
\date{}


% KAO: Remove extra spacing

% KAO: Remove extra spacing
\author{
 Jelle Vermandere\textsuperscript{1}, 
 Maarten Bassier\textsuperscript{1},
 Maarten Vergauwen\textsuperscript{1}
}

% KAO: Remove extra newline
\address{
	\textsuperscript{1} KU Leuven, Department of Civil Engineering, Ghent, Belgium \\
    (jelle.vermandere, maarten.bassier, maarten.vergauwen)@kuleuven.be\\
}



% KAO: Use times symbol
\abstract{
Object completion networks typically produce static Signed Distance Fields (SDFs) that faithfully reconstruct geometry but cannot be rescaled or deformed without introducing structural distortions. This limitation restricts their use in applications requiring flexible object manipulation, such as indoor redesign, simulation, and digital content creation. We introduce a part-aware scaling framework that transforms these static completed SDFs into editable, structurally coherent objects. Starting from SDFs and Texture Fields generated by state-of-the-art completion models, our method performs automatic part segmentation, defines user-controlled scaling zones, and applies smooth interpolation of SDFs, color, and part indices to enable proportional and artifact-free deformation. We further incorporate a repetition-based strategy to handle large-scale deformations while preserving repeating geometric patterns. Experiments on Matterport3D and ShapeNet objects show that our method overcomes the inherent rigidity of completed SDFs and is visually more appealing than global and naive selective scaling, particularly for complex shapes and repetitive structures.
}

\keywords{Object completion, Scanning, SDF, Convex Decomposition}

\maketitle

% ---- MAIN TEXT ----
\section{Introduction}
\label{sec:introduction}

Dynamic and adaptable object representations are increasingly required in applications such as indoor scene editing, architectural design, simulation, and mixed reality. In the Architecture, Engineering, Construction, and Operations (AECO) industry, for example, renovation planning and design simulations require objects that can be interactively manipulated, resized, or repositioned within existing environments. Similarly, modern gaming and virtual environments demand flexible object representations that can be dynamically modified while maintaining visual realism and structural coherence \cite{vermandere_guided_2025}.

Objects reconstructed from real-world indoor environments are often incomplete due to sensor limitations, occlusions, or restricted viewpoints during data acquisition. As a result, object completion techniques have become an important research topic for reconstructing full watertight models from partial observations. Recent advances in deep learning have enabled high-quality object completion using implicit representations, particularly Signed Distance Functions (SDFs) \cite{mittal_autosdf_2022, vasu_hybridsdf_2022, hao_dualsdf_2020, vermandere_geometry_2025}. In this representation, the object surface is implicitly defined by a continuous function that encodes the distance from any point in space to the nearest surface. Furthermore, SDF-based models can be extended with texture fields \cite{oechsle_texture_2019}, enabling both geometry and appearance to be represented within a continuous functional space.

Although these approaches produce high-quality reconstructed objects, the resulting representations are typically \textit{static}. Once generated, completed SDF objects cannot easily be modified without introducing geometric distortions or visual artifacts. A common operation in scene editing is object scaling, where objects must be adapted to new spatial constraints or design requirements. However, uniform scaling of complex objects often produces unrealistic results, such as stretched structural elements or distorted repeated components. This limitation highlights the need for techniques that enable \textit{selective and structurally coherent object scaling}.

Existing part-aware scaling methods are primarily used in procedural modelling and game development. These approaches often rely on predefined modular components or manually defined scaling regions \cite{li_proc-gs_2024}. Other techniques, such as slicing-based deformation methods \cite{deftly_27_2021}, allow scaling of specific object regions but lack structural awareness of object parts. Consequently, these methods are difficult to generalize to arbitrary objects reconstructed from real-world scans.

To address these limitations, we propose a framework for \textit{part-aware scaling of completed implicit objects}. Starting from coloured Signed Distance Functions (CSDFs) produced by object completion pipelines, the proposed method first decomposes the object into approximately convex components that act as structural parts. Users can then define scaling regions through simple planar constraints. Within these regions, geometry and appearance are updated by jointly interpolating SDF values, colour information, and part indices, enabling smooth and consistent deformation of the object.

To support larger deformations and objects with repeating structural patterns, we further introduce a repetition-based scaling strategy that duplicates modular components instead of stretching them. This approach preserves structural proportions and prevents the distortions commonly observed in naive scaling operations.

The main contributions of this work are:

\begin{itemize}
    \item A \textbf{part-aware scaling framework for implicit 3D objects}, enabling selective deformation of completed SDF representations.
    \item A \textbf{joint interpolation strategy for geometry, colour, and part indices} within scaling zones, ensuring consistent deformation of both shape and appearance.
    \item A \textbf{repetition-based scaling mechanism} that preserves modular structures during large deformations.
    \item An \textbf{interactive pipeline for manipulating completed objects}, enabling intuitive scaling operations for reconstructed indoor objects.
\end{itemize}

The remainder of this work is structured as follows. The background and related work are presented in Section \ref{sec:background}. Section \ref{sec:methodology} describes the proposed method. Section \ref{sec:experiments} presents the experimental setup and the results are discussed in Section \ref{sec:discussion}. Finally, conclusions and future work are discussed in Section \ref{sec:conclusion}.



%=====================BACKGROUND=====================%
\section{Background and related work}
\label{sec:background}

Object scaling and deformation have been explored across various domains, including image processing, mesh deformation, procedural generation, and SDFs. In this section, we review prior work in these areas, highlighting the limitations addressed by our method.

\subsection{Object Completion}
Recent advancements in object geometry completion have shifted towards completing partial SDFs. Because they can be easily discretised into a voxel grid, they are the ideal input for machine-learning-based models like AUTO SDF\cite{mittal_autosdf_2022}, which trained a model on sub-selections of the voxel grid of complete objects. The model can then predict the missing sub-selections to complete the missing parts of the object. XCUBE \cite{ren_xcube_2023} Improves upon this method by introducing a hierarchical voxel octree representation allowing for a coarse to fine completion network which results in a much higher output resolution.
To improve the completion results for objects with more realistic occlusions, more recent works \cite{vermandere_geometry_2025} have focussed on adding more refinement to the voxel-based inputs.

% add more references
Texture completion models have tried to adapt the same function-based representation with the introduction of Texture fields \cite{oechsle_texture_2019} by encoding the texture in 3D space instead of on the 2D plane. IF-Net texture\cite{chibane_implicit_2021} uses this representation to complete missing colour information in geometrically complete objects. The network is trained to leverage the geometric point's features and adjacent colours to generate a colour-function space. The space can than be sampled at any given point. Other approaches use cascaded 3D convolutional network architectures, which learn to reconstruct corresponding colour information from noisy and imperfect RGB-D maps in a progressive and coarse-to-fine manner \cite{liu_high-quality_2021}. This allows larger missing regions to be reconstructed better.

\begin{figure*}[!h]
    \centering
    \includegraphics[width=\textwidth]{Figures/method_overview.png}
    \caption{Overview of the proposed pipeline, starting with the object completion (left), going into the part segmentation (centre-left), followed scaling zone definition (centre-right) to result in a scaled and coloured object (right).}
    \label{fig:methodology}
\end{figure*}

\subsection{Part Segmentation}
Object part segmentation enables an object to be divided into smaller parts, either by instance, semantics or both.
CSN \cite{loizou_cross-shape_2023} uses a cross-shape attention mechanism to enable interactions between a shape’s point-wise features and those of other shapes, improving the accuracy and consistency of the shape segmentation.
Mid-Net \cite{wang_unsupervised_2020} uses an unsupervised method for learning a generic and efficient shape encoding network for different shape analysis tasks. The key idea of the method is to jointly encode and learn shape and point features from un-labeled 3D point clouds.
FG-Net \cite{liu_fg-net_2020} is a highly efficient model for large-scale point clouds understanding without voxelizations. it employs a deep convolutional neural network leveraging correlated feature mining and deformable convolution based geometric-aware modelling, in which the local feature relationships and geometric patterns can be fully exploited. These models all aim to segment the models by their semantic label, something which is difficult to generalize for generic objects in a wide variety of scenes.

Approximate convex decomposition (ACD) has become a standard strategy for breaking complex 3D meshes into sets of nearly convex parts, enabling efficient collision detection, physical simulation, and shape analysis. Classical methods such as HACD \cite{mamou_simple_2009} rely on hierarchical clustering with concavity-driven merge heuristics, offering robustness but often producing redundant parts and overlaps. More recently, learning-based methods such as CvxNet \cite{deng_cvxnet_2020} represent shapes as unions of learned convex primitives, achieving compact decompositions but with limited generalization outside the training distribution. To address these challenges, Wei et al. introduced CoACD \cite{wei_approximate_2022}, a geometry-driven algorithm that directly cuts triangle meshes with planes, employs a collision-aware concavity metric sensitive to interior geometry, and explores cut sequences through tree search rather than greedy splitting. This yields intersection-free convex parts with fewer components and higher collision fidelity compared to prior baselines. While these convex parts do not necessarily represent each individual semantic component of an object, the granularity and generalisation of the method ensures the objects are well separated.  

\subsection{Surface-Based Deformation}
Techniques such as 9-slicing \cite{w3_css-backgrounds-3border-image-slice_2024} enable specific zones of images to be scaled while maintaining proportionality in other regions. This method has been extended to 3D environments, such as 27-slicing \cite{deftly_27_2021}, to scale 3D objects without distorting critical regions. However, these methods rely on predefined zones and don't have a scaling constraint, which limits the use of these methods to very regular and basic shapes. KeypointDeformer \cite{jakab_keypointdeformer_2021} tries to combat this by using automatic keypoint detection to guide mesh cage deformation, enabling more natural object transformations. While effective smaller deformations, this method starts to show its limits on large deformations.

Other approaches, like the procedural model generation method described in \cite{getto_automatic_2020}, use automated detection of object components by utilising object skeletons for limiting the deformation regions. Improving its results for more complex objects. These methods all have the same main weakness, that when the parts undergo a large deformation, the selected parts get scales to match the size without any constraint of part size consistency. Other procedural methods, such as Proc-GS \cite{li_proc-gs_2024}, divide 3D models of buildings into components for dynamic recombination. This enables large deformations by introducing modular parts that can be repeated indefinitely to match the desired size. These methods however rely on a clearly defined library of selected parts to build new geometry.

\subsection{SDF-Based Deformation}
SDFs have become a popular representation for 3D object generation due to their standardised size which is ideal for machine learning in-and-outputs. DIF-Net \cite{deng_deformed_2021} represents 3D shapes using a shared template implicit field, deformation fields, and correction fields, enabling non-destructive shape manipulation. Similarly, SALAD \cite{koo_salad_2023} uses a part-level latent diffusion framework for generating and editing 3D shapes, while DualSDF \cite{hao_dualsdf_2020} introduces a two-level SDF representation for semantic shape manipulation. HybridSDF \cite{vasu_hybridsdf_2022} further combines implicit shapes with primitives, allowing a balance between flexibility and structural coherence.

Recent advancements focus on editing SDFs at part and sub-part levels. NVIDIA's XCube \cite{ren_xcube_2023} employs hierarchical voxel latent diffusion models for large-scale 3D generative modeling, enabling low level voxel editing for fine object generation. SPAGHETTI \cite{hertz_spaghetti_2022} enables part-level affine transformations of implicit shapes, such as rotation and translation, while ensuring smooth transitions. However, free scaling at a sub-part level remains a challenge. SENS \cite{binninger_sens_2024} extends SPAGHETTI \cite{hertz_spaghetti_2022} by enabling sketch-based SDF editing but remains limited in its ability to manipulate large-scale deformations without predefined inputs.

\subsection{Texture Deformation}

Traditional mesh textures rely on either UV maps paired with 2D images or per-vertex colour information. Both methods implicitly bind texture appearance to the geometry of the mesh: when the mesh is deformed or scaled, the UV coordinates deform with it, while vertex colours simply interpolate across the new surface. However, image-based textures degrade under large deformations due to stretching or loss of resolution, and vertex colours lack the detail needed to represent high-frequency appearance.

Texture Fields \cite{oechsle_texture_2019} address these limitations by representing texture as a continuous function defined in 3D space, rather than on the surface of the mesh. This functional representation is conceptually similar to SDFs, as both are spatially defined over the object's volume. Because texture values are queried directly in 3D space, Texture Fields can naturally adapt to meshes of different shapes or scales, enabling consistent texture remapping under deformation without loss of detail.


Our work builds on these foundations by combining part-aware SDF deformation with selective scaling capabilities, enabling proportional scaling of arbitrary objects without relying on predefined part databases. This approach bridges the gap between procedural methods and SDF-based editing, providing a robust solution for dynamic object manipulation in indoor scenes.


%=====================METHODOLOGY=====================%

\section{Methodology}
\label{sec:methodology}

The proposed framework enables part-aware scaling of completed implicit objects. Starting from a colored signed distance function (CSDF), the object is decomposed into structural components, after which user-defined regions can be selectively scaled. Geometry and appearance are updated through joint interpolation of SDF values, color information, and part indices. For larger deformations, a repetition-based strategy is introduced to preserve repeating structural patterns. An overview of the pipeline is shown in Fig.~\ref{fig:methodology}.

\subsection{Preprocessing: CSDF Generation}

As a preprocessing step, partially scanned objects are completed using existing completion techniques. Geometry completion is performed using AutoSDF \cite{mittal_autosdf_2022}, while texture information is inferred using TextureFields \cite{oechsle_texture_2019}. 

The resulting implicit representation is discretized into a voxel grid of resolution $128 \times 128 \times 128$. For each voxel, both the signed distance value and the corresponding color information are stored, resulting in a colored signed distance function (CSDF). This representation encodes geometry and appearance in a unified volumetric format.

For efficient storage and rendering, the CSDF is stored as a 3D texture, where the RGB channels encode color values and the alpha channel stores the signed distance value (Fig.~\ref{fig:methodology-3Dtexture}). This unified representation forms the basis for subsequent geometric manipulation.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{Figures/3D_texture.png}
    \caption{The CSDF stored as a 3D texture, sliced in an 8x8 grid (left) and the rendered object (right).}
    \label{fig:methodology-3Dtexture}
\end{figure}

\subsection{Part Segmentation}

To enable part-aware deformation, the object is decomposed into approximately convex components using Approximate Convex Decomposition (ACD) \cite{wei_approximate_2022}. First, a watertight mesh is extracted from the SDF using the Marching Cubes algorithm. The resulting mesh is then recursively partitioned using plane-based cuts determined through a tree-search optimization process.

This decomposition produces a set of nearly convex components while minimizing redundant cuts. Adjacent components are subsequently evaluated for concavity and merged when their union remains convex. An example of this decomposition is shown in Fig.~\ref{fig:methodology-decomposition}.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{Figures/convex_decomposition.png}
    \caption{The CSDF rendered as a 3D texture (left) and the resulting convex decomposition (right).}
    \label{fig:methodology-decomposition}
\end{figure}

Unlike semantic segmentation approaches, this geometry-based method does not rely on object class information and can therefore generalize to arbitrary reconstructed objects. The resulting part indices are mapped back onto the CSDF grid using nearest-neighbour sampling. Consequently, each voxel stores three attributes: the signed distance value, the color $(r,g,b)$, and a discrete part index $i$.

\subsection{Scaling Zone Definition}

Selective scaling is controlled through a simple planar constraint system. A scaling zone is defined by two parallel planes along a chosen axis: a starting plane and an ending plane. The user can translate the ending plane along the axis to specify the desired deformation magnitude.

Objects located before the starting plane remain unchanged, while elements beyond the ending plane are translated to maintain spatial consistency. Components intersecting the scaling region are isolated and undergo deformation.

This planar representation allows intuitive control of the deformation region while maintaining consistent manipulation across different objects and use cases.

\subsection{Joint Interpolation of Geometry and Appearance}

Within the scaling zone, a new voxel grid is created to accommodate the updated spatial extent. Geometry and appearance are updated through interpolation of the SDF values, color fields, and part indices.

\subsubsection{SDF Interpolation}

Since SDF values represent continuous scalar fields, they can be interpolated directly. Given two SDF samples $\text{SDF}_1$ and $\text{SDF}_2$ at positions $x_1$ and $x_2$, the interpolated value at position $x$ is computed using linear interpolation:

\[
\text{SDF}(x) = \text{SDF}_1 + \frac{x - x_1}{x_2 - x_1} (\text{SDF}_2 - \text{SDF}_1)
\]

After interpolation, the SDF is locally re-evaluated to ensure surface consistency and avoid discontinuities. Only voxels potentially affected by the deformation are recomputed, limiting the computational cost.

\subsubsection{Color Interpolation}

Color values are interpolated in RGB space using the same linear interpolation scheme. Given two color vectors $\mathbf{C}_1=(R_1,G_1,B_1)$ and $\mathbf{C}_2=(R_2,G_2,B_2)$, the interpolated color is computed as:

\[
\mathbf{C}(x) = \mathbf{C}_1 + \frac{x - x_1}{x_2 - x_1} (\mathbf{C}_2 - \mathbf{C}_1)
\]

To reduce computation and memory requirements, interpolation is applied only to voxels close to the surface, while empty regions outside the object remain unassigned.

\subsubsection{Part Index Handling}

Part indices represent discrete labels and therefore cannot be interpolated directly. Instead, boundaries between adjacent parts are detected by identifying locations where the part index changes. The transition boundary $x_b$ is defined as:

\[
x_b = \arg\min_x \left(|\text{SDF}(x)| \; \text{where } p(x) \neq p(x+\delta x)\right)
\]

where $\delta x$ represents a small sampling offset used to detect label transitions.

\subsection{Repetition-Based Scaling}

For large deformations, simple stretching of geometry may introduce unrealistic distortions. To address this issue, we introduce a repetition-based scaling strategy for regions containing repeatable structural components.

Let $L$ denote the original length of the repeatable region and $L'$ the desired target length. The number of repetitions $n$ is determined as:

\[
n = \left\lfloor \frac{L'}{L} \right\rfloor
\]

The selected region is then duplicated $n$ times along the scaling axis. To exactly match the target length, each repeated section is slightly scaled using the interpolation scheme described previously.

This strategy preserves repeating geometric patterns such as shelves or cushions while preventing excessive geometric stretching. To ensure smooth transitions between repeated components, boundary layers between adjacent repetitions are blended.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{Figures/method_repeated.png}
    \caption{Example of repetition-based scaling. The selected region (left) is duplicated to form repeated structures in the scaled object (right).}
    \label{fig:methodology-repeating}
\end{figure}

\subsection{Output Representation}

After deformation, the updated CSDF is stored again as a 3D texture. For efficient storage and rendering, the grid may optionally be resampled to predefined output resolutions.

%=====================EXPERIMENTS=====================%

\section{Experiments}
\label{sec:experiments}

We evaluate the proposed part-aware scaling framework on a set of reconstructed indoor objects and high-quality 3D scans. The experiments focus on three aspects: (1) visual quality of scaled objects, (2) preservation of structural proportions, and (3) applicability to objects with both regular and repeating structures.

\subsection{Datasets}

Two sources of 3D objects were used in the experiments.

The first dataset consists of objects extracted from the Matterport3D Indoor Dataset \cite{chang_matterport3d_2017}. Individual objects were isolated from their original scenes and subsequently completed using state-of-the-art completion techniques. Geometric completion was performed using AutoSDF \cite{mittal_autosdf_2022}, while color information was inferred using IF-Net \cite{chibane_implicit_2021}. This process produced complete colored signed distance function (CSDF) representations for partially scanned objects.

To complement these reconstructed objects, we additionally collected a set of high-quality 3D models from Sketchfab. These models serve as examples of geometrically clean objects with well-defined repeating structures. Using both datasets allows us to evaluate the robustness of the method on both reconstructed and optimized geometry.

Examples from both datasets are shown in Fig.~\ref{fig:experiments_results}.

\begin{figure}[!h]
    \centering
    \includegraphics[width=\columnwidth]{Figures/experiments_results.png}
    \caption{Scaling results for different objects. Top row: objects reconstructed from Matterport3D scenes. Bottom row: high-quality models from Sketchfab.}
    \label{fig:experiments_results}
\end{figure}

\subsection{Interactive Editing Environment}

The generated CSDFs were visualized in the Unity game engine using a raymarching shader \cite{zhou_real-time_2008}. The CSDF data was stored as a 3D texture, enabling real-time rendering and manipulation of the implicit surface.

To facilitate interactive manipulation, the scaling region was defined using three control planes representing the start, end, and destination positions of the scaling zone. These planes were visualized within the Unity interface and could be adjusted through a graphical user interface, enabling real-time feedback during object manipulation (Fig.~\ref{fig:experiments_unity}).

\begin{figure}[!h]
    \centering
    \includegraphics[width=\linewidth]{Figures/dynamic_scaling_Unity.png}
    \caption{Interactive editing interface in Unity. The planes define the start, end, and target position of the scaling region.}
    \label{fig:experiments_unity}
\end{figure}

\subsection{Scaling Methods}

We compare three different scaling strategies:

\begin{itemize}
\item \textbf{Global Scaling}: The entire object is uniformly scaled to the desired size.
\item \textbf{Selective Scaling}: Only the region inside the scaling zone is stretched using interpolation of SDF and color values.
\item \textbf{Selective Tiling}: Repeating structural components are duplicated instead of stretched.
\end{itemize}

These approaches represent increasingly structure-aware manipulation strategies.

\subsection{Qualitative Results}

Figure~\ref{fig:experiments_results} illustrates representative results for all three scaling strategies.

Global scaling uniformly stretches the entire object, often leading to unrealistic geometric proportions. For example, chair legs become excessively wide and shelves become unnaturally spaced.

Selective scaling improves this behaviour by restricting deformation to the specified region. This approach preserves proportions outside the scaling zone and performs well for simple geometric components such as chair legs or tabletops.

However, when applied to objects containing repeating structures, selective scaling introduces visible stretching artifacts. In such cases, structural elements such as shelves or cushions become elongated and visually inconsistent.

Selective tiling addresses this issue by duplicating repeating elements instead of stretching them. As a result, structural patterns are preserved and the overall object remains visually coherent.

\section{Discussion}
\label{sec:discussion}

The experimental results reveal several important insights regarding part-aware object scaling using implicit representations.

First, selective scaling significantly improves upon global object scaling by restricting deformation to localized regions. This allows objects to maintain realistic proportions outside the modified region, which is particularly important for functional components such as chair legs or table surfaces.

However, selective scaling alone is insufficient for objects containing strongly repeating structural patterns. When such structures are stretched, geometric distortions accumulate and the resulting object appears unrealistic. This behaviour is especially visible in objects such as bookshelves or sofas, where multiple repeated elements interact.

The repetition-based scaling strategy effectively addresses this limitation. By duplicating structural components rather than stretching them, the method preserves both geometric proportions and visual consistency. This approach works particularly well for modular objects where repetition naturally occurs.

The experiments also highlight several limitations of the current framework. The CSDF representation stores both geometry and colour within a voxel grid, which introduces a trade-off between resolution and memory consumption. While the chosen $128^3$ resolution provides a reasonable balance, fine geometric details may still be lost during interpolation or resampling.

Furthermore, the convex decomposition used for part segmentation is purely geometry-based. Although this enables class-agnostic processing of arbitrary objects, the resulting parts do not always correspond to semantic object components. Future work could explore integrating learned segmentation approaches to better align structural parts with semantic object regions.

Overall, the results demonstrate that the proposed framework enables intuitive and flexible object manipulation within implicit representations. The integration of part-aware scaling and repetition-based deformation allows a wide range of objects to be resized while preserving structural coherence, making the method suitable for applications in interactive scene editing, architectural design, and mixed-reality environments.

%=====================CONCLUSIONS=====================%
\section{Conclusion}
\label{sec:conclusion}

We presented a part-aware scaling framework for completed 3D objects represented as coloured Signed Distance Functions (CSDFs). Our method integrates convex part segmentation, user-defined scaling zones, and joint interpolation of geometry, colour, and part indices to achieve proportional and visually coherent object deformations. For large deformations, a repetition-based scaling strategy preserves repeated structural components, maintaining structural integrity and avoiding distortion artifacts. 

The pipeline is integrated into a real-time Unity interface, enabling interactive manipulation, rapid prototyping, and mixed-reality applications. Experiments on both reconstructed indoor objects and high-quality 3D models demonstrate that the framework effectively preserves geometric and visual consistency, even in objects with complex structures or repeated patterns.

While limitations remain for highly organic or non-repetitive objects, the CSDF-driven approach provides a unified representation for consistent geometry and texture editing. Future work will explore adaptive voxel resolutions, learned segmentation priors, and strategies for scaling more organic shapes, further expanding the applicability of part-aware implicit object manipulation.


{
	\begin{spacing}{1.17}
		\normalsize
		\bibliography{Dynamic_Object_Completion} % Include your own bibliography (*.bib), style is given in isprs.cls
	\end{spacing}
}

\end{document}


%% 
%% Copyright 2019-2024 Elsevier Ltd
%% 
%% This file is part of the 'CAS Bundle'.
%% --------------------------------------
%% 
%% It may be distributed under the conditions of the LaTeX Project Public
%% License, either version 1.3c of this license or (at your option) any
%% later version.  The latest version of this license is in
%%    http://www.latex-project.org/lppl.txt
%% and version 1.3c or later is part of all distributions of LaTeX
%% version 1999/12/01 or later.
%% 
%% The list of all files belonging to the 'CAS Bundle' is
%% given in the file `manifest.txt'.
%% 
%% Template article for cas-sc documentclass for 
%% double column output.

\documentclass[a4paper,fleqn]{cas-sc}

% If the frontmatter runs over more than one page
% use the longmktitle option.

%\documentclass[a4paper,fleqn,longmktitle]{cas-sc}

%\usepackage[numbers]{natbib}
%\usepackage[authoryear]{natbib}
\usepackage[authoryear,longnamesfirst]{natbib}

%%%Author macros
\def\tsc#1{\csdef{#1}{\textsc{\lowercase{#1}}\xspace}}
\tsc{WGM}
\tsc{QE}
%%%

% Uncomment and use as if needed
%\newtheorem{theorem}{Theorem}
%\newtheorem{lemma}[theorem]{Lemma}
%\newdefinition{rmk}{Remark}
%\newproof{pf}{Proof}
%\newproof{pot}{Proof of Theorem \ref{thm}}

\begin{document}
\let\WriteBookmarks\relax
\def\floatpagepagefraction{1}
\def\textpagefraction{.001}

% Short title
\shorttitle{3D Scan Completion}    

% Short author
\shortauthors{J. Vermandere et al.}  

% Main title of the paper
\title [mode = title]{From Partial Scans to Editable Scenes: Hybrid Object and Scene Completion for Dynamic 3D Environments}  

% Title footnote mark
% eg: \tnotemark[1]
%\tnotemark[1] 

% Title footnote 1.
% eg: \tnotetext[1]{Title footnote text}
%\tnotetext[1]{} 

% First author
%
% Options: Use if required
% eg: \author[1,3]{Author Name}[type=editor,
%       style=chinese,
%       auid=000,
%       bioid=1,
%       prefix=Sir,
%       orcid=0000-0000-0000-0000,
%       facebook=<facebook id>,
%       twitter=<twitter id>,
%       linkedin=<linkedin id>,
%       gplus=<gplus id>]

\author[1]{}%[<options>]

% Corresponding author indication
\cormark[1]

% Footnote of the first author
\fnmark[1]

% Email id of the first author
\ead{}

% URL of the first author
\ead[url]{}

% Credit authorship
% eg: \credit{Conceptualization of this study, Methodology, Software}
\credit{}

% Address/affiliation
\affiliation[1]{organization={},
            addressline={}, 
            city={},
%          citysep={}, % Uncomment if no comma needed between city and postcode
            postcode={}, 
            state={},
            country={}}

\author[2]{}%[]

% Footnote of the second author
\fnmark[2]

% Email id of the second author
\ead{}

% URL of the second author
\ead[url]{}

% Credit authorship
\credit{}

% Address/affiliation
\affiliation[2]{organization={},
            addressline={}, 
            city={},
%          citysep={}, % Uncomment if no comma needed between city and postcode
            postcode={}, 
            state={},
            country={}}

% Corresponding author text
\cortext[1]{Corresponding author}

% Footnote text
\fntext[1]{}

% For a title note without a number/mark
%\nonumnote{}

% Here goes the abstract
\begin{abstract}
Real-world 3D scene reconstructions are typically partial and static, limiting their applicability in domains requiring complete and editable environments. Occlusions, sensor constraints, and incomplete viewpoints introduce missing geometry, while captured objects remain fixed in a single observed configuration. We present a hybrid framework for transforming partially scanned scenes into geometrically complete and dynamically adaptable 3D environments. Our approach decomposes reconstruction into object-level and scene-level processes. First, 3D object detection identifies and localizes scene components. Object completion and scene completion are then performed in parallel: object geometry is recovered using learned generative models guided by geometric and image-based cues, while structural elements are reconstructed using geometry-driven polygonal inference. To enable dynamic scene manipulation, we introduce an object dynamification mechanism operating on Gaussian splat representations, allowing controllable scaling and shape variation while preserving structural coherence. The completed and dynamically adaptable components are integrated into a unified scene representation. Our method bridges scene completion, object reconstruction, and shape manipulation, enabling the generation of complete, editable 3D environments from partial observations.
\end{abstract}

% Use if graphical abstract is present
%\begin{graphicalabstract}
%\includegraphics{}
%\end{graphicalabstract}

% Research highlights
\begin{highlights}
\item 
\item 
\item 
\end{highlights}


% Keywords
% Each keyword is seperated by \sep
\begin{keywords}
 \sep \sep \sep
\end{keywords}

\maketitle

\section{Introduction}
\label{introduction}

3D scene capture technologies, such as LiDAR scanning, RGB-D sensing, and photogrammetry, have enabled the efficient digitization of real-world environments \cite{geyter_point_2022}. However, reconstructed scenes are typically incomplete and static. Occlusions, sensor limitations, and limited viewpoints result in missing geometry, while the whole scene is unstructured and lacks any object-to-scene separation \cite{vermandere_geometry_2025}. Although such representations are sufficient for visualization and measurement tasks, modern applications increasingly demand \textit{dynamic} and \textit{complete} scene representations. Virtual environments for gaming, simulation, renovation planning, safety analysis, and training systems require the ability to modify objects, adjust layouts, and explore alternative configurations \cite{vermandere_guided_2025}. Static reconstructions limit such use cases, as objects cannot be meaningfully manipulated without introducing geometric inconsistencies or visual artifacts.

Existing research addresses related but fragmented aspects of this problem. Scene completion methods commonly operate at the global level, predicting volumetric occupancy \cite{hubner_voxel-based_2020} or implicit fields \cite{chen_circle_2021, weber_nerfiller_2023} for the entire environment. While effective at recovering large-scale structure, these approaches often lack object-to-scene separation or inter-object spatial relationships \cite{dai_scancomplete_2018, dai_spsg_2020}. Conversely, object completion methods focus on reconstructing isolated shapes but do not reason about the surrounding scene \cite{mittal_autosdf_2022, ren_xcube_2023, vermandere_geometry_2025}. Other techniques remove objects entirely to simplify reconstruction or enable background inpainting \cite{sun_behind_2024, wei_clutter_2023, zhang_no_2020}. Consequently, few methods explicitly model the space \textit{between} objects while preserving object identity and enabling controllable modifications. Furthermore, very limited prior work explores the joint problem of transforming partial scans into dynamic and editable scenes \cite{ardelean_gen3dsr_2025}. Bridging scene reconstruction, object completion, and shape manipulation remains a significant challenge.

To address these limitations, we propose a workflow that integrates 3D object detection, geometry-based scene reconstruction and controllable object completion. Our method takes as input a partially scanned scene together with aligned image observations as captured by must 3D capture devices. This workflow allows us to treat scene completion, not as a purely global inference problem, but as 2 parallel object-level and scene-level completion stages.

The key contributions of this work are:

\begin{itemize}
    \item A hybrid reconstruction framework that decouples object completion and scene completion while preserving geometric consistency.
    \item A unified pipeline for transforming partial scans into complete, editable 3D scenes.
\end{itemize}

% The structure
The remainder of this work is structured as follows. The background and related work is presented in Section \ref{sec:background}. Following is the explanation of the proposed method in Section \ref{sec:methodology}. In Section \ref{sec:experiments}, an overview of the used datasets and their results is presented. Finally, the conclusions are presented in Section \ref{sec:conclusion}

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% RELATED WORK %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\section{Background}
\label{sec:background}

Research on 3D scene understanding and reconstruction spans multiple interconnected sub-problems, including object detection and segmentation, object and scene completion, texture inpainting, and shape manipulation. This section reviews prior work along these axes.

\subsection{3D Object Detection and Semantic Segmentation}

Early learning-based 3D object detection methods focused on directly processing point clouds. VoteNet~\cite{qi_deep_2019} introduced an end-to-end architecture that combines deep point set feature learning with a Hough voting mechanism to robustly predict object centers and bounding boxes from raw point clouds. Subsequent works, such as VoteNet+\cite{ding_votenet_2020} and MLCVNet \cite{xie_mlcvnet_2020}, refined this paradigm through architectural and feature aggregation improvements.

Transformer-based approaches have recently gained prominence. V-DETR~\cite{shen_v-detr_2023} adapts the DETR framework to 3D object detection by introducing vertex-relative positional encoding, improving spatial locality in attention mechanisms. UniDet3D~\cite{kolodiazhnyi_unidet3d_nodate} further explores transformer-based detection by unifying label spaces across multiple indoor datasets, enabling joint training and improved generalization. Furthermore, it extracts point features using sparse 3D U-Nets, aggregates them via superpoint pooling, and employs a vanilla transformer encoder for object prediction.

Class-incremental detection has been addressed by AIC3DOD~\cite{cheng_aic3dod_nodate}, which incorporates point transformer architectures alongside room layout constraints. By enforcing physical priors derived from scene layout, AIC3DOD improves robustness and consistency in incremental learning settings.

For semantic segmentation, transformer-based models such as Point Transformer v3 (PTv3)~\cite{wu_point_2023} have demonstrated strong performance by modeling long-range dependencies in point clouds. Voxel-based approaches have also been explored for indoor reconstruction, such as voxelizing triangle meshes captured by mixed reality devices to enable structured processing~\cite{hubner_voxel-based_2020}.

Instance-level understanding remains a key challenge. SphericalMask~\cite{shin_spherical_2023} proposes a coarse-to-fine instance segmentation strategy based on spherical representations, avoiding the overestimation issues common in axis-aligned bounding boxes. UnScene3D~\cite{rozenberszki_unscene3d_2023} introduces a fully unsupervised pipeline for class-agnostic 3D instance segmentation by generating pseudo-labels using self-supervised geometric and color features.

For more basic shapes like walls and floors in a scene, plane detection plays a foundational role in quick and robust segmentation. Latent RANSAC~\cite{korman_latent_2018} accelerates hypothesis evaluation by learning to score RANSAC hypotheses in constant time, enabling efficient detection of dominant planar structures.

\subsection{Object Completion}

% Object completion
% why point clouds or SDF's?
3D scanned environments are typically captured as unstructured pointclouds and can be converted to (un-)textured meshes. Both of these formats are irregular and are not easily used in machine learning networks. This is why the models are often converter to either fixed-size pointclouds, voxelgrids or Signed Distance Fields (SDF). All of which, can be mapped to a fixed input size. Furthermore, more recent works have aimed to reconstruct 3D shapes based on a single image, greatly reducing the sensor costs and accessibility. However, due to their limited input dimension, these results are typically less accurate.

\subsubsection{3D Generative models}
Sparse voxel structures have been widely used for efficient 3D representation and completion. Hierarchical sparse voxel grids, such as Sparse Voxel Octrees (SVOs) and their extensions to Directed Acyclic Graphs (DAGs), enable compact encoding of empty and repeated spatial regions~\cite{mados_csvo_2022}. Further work decouples geometry from voxel attributes to compress arbitrary signals such as color and normals~\cite{dado_geometry_2016}.

Point-based geometry completion like Point-Voxel-diffusion~\cite{zhou_3d_2021} uses a fixed-size pointcloud as input to predict the final shape through 3D diffusion. IF-Nets~\cite{chibane_implicit_2020} also use points, but employs implicit features generated from those points to predict the missing regions. These models provide good results for general shapes, but lack in fine detail completion due to the amif net

SDFs are an implicit representation of a 3D shape. They define a function which represents the distance to the boundary of the object from any point in space \cite{mittal_autosdf_2022}. An SDF can be discretised into a voxelgrid to standardize the input size. These have become a popular input type because they retain the shape of the object while using less data points. Models like PatchComplete \cite{rao_patchcomplete_2022} and WSSC \cite{Wu2024} use a coarse-to-fine approach by first predicting the general shape and then refining each sub-grid using multi-resolution priors.
Shapeformer \cite{yan_shapeformer_2023} is able to leverage the Transformer architecture by using a vector quantized deep implicit function (VQDIF) to represent an incomplete shape.

Models like AutoSDF~\cite{mittal_autosdf_2022} are able to encode the SDFs and, by spliting the SDF in sub-grids during training, can predict the missing geometry. SD Fusion~\cite{cheng_sdfusion_2023} builds upon this by allowing multi-modal input types to guide the generation. XCube \cite{ren_xcube_2023} expands upon the object completion by employing a hierarchical voxel latent diffusion model which generates progressively higher resolution grids in a coarse-to-fine manner using a custom framework built on the highly efficient VDB data structure. This enables the model to generate much larger scenes. 

Most of the above mentioned object completion methods are not able to complete the texture as well. This is why there has been a lot of research in only completing the textures of completed objects. Classical approaches model textures as Gaussian processes and perform conditional sampling for inpainting~\cite{galerne_texture_2017}. if-net models have also been used to predict vertex colors based on the completed geometry\cite{chibane_implicit_2021}. Recent learning-based methods, such as MAT~\cite{li_mat_2022} and CM-GAN~\cite{zheng_cm-gan_2022}, employ transformer architectures and cascaded modulation schemes to handle large missing regions in images.

Object-aware and prompt-driven methods, such as Inpaint Anything~\cite{yu_inpaint_2023}, combine segmentation models (e.g., SAM) with diffusion-based inpainters to enable object removal, replacement, and semantic filling. Extending texture inpainting to surfaces, STINet~\cite{flynn_free-form_2022} formulates surface texture completion as a graph learning problem over mesh connectivity, allowing inpainting on arbitrary geometries.

\subsubsection{Image-Based 3D Reconstruction}

Recent methods aim to reconstruct full 3D objects from a single image. SAM3D~\cite{team_sam_2025} combines segmentation masks with large-scale latent flow transformers to estimate coarse geometry, pose, and texture, decoding results into meshes and Gaussian splats. Similarly, Hunyuan3D~\cite{hunyuan3d_hunyuan3d-omni_2025} \cite{zhao_hunyuan3d_2025} proposes a unified cross-modal framework that accepts images alongside optional 3D priors such as point clouds or bounding boxes, enabling controllable generation under missing inputs.

Trellis~\cite{xiang_structured_2025} introduces a Structured Latent (SLAT) representation that can be decoded into multiple 3D formats, including meshes, radiance fields, and 3D Gaussians, using rectified flow transformers.

\subsection{Texture inpainting}
Image inpainting has seen significant advances in recent years, with a variety of approaches targeting different aspects of the problem. Classical methods, such as Gaussian inpainting, focus on filling missing regions by modeling local textures \cite{galerne_texture_2017}, while applications in photogrammetry emphasize maintaining texture consistency in 3D models \cite{maggiordomo_texture_2023}. Learning-based methods, including Inpaint Anything \cite{yu_inpaint_2023} and PatchComplete \cite{rao_patchcomplete_2022}, leverage neural networks to achieve more realistic and semantically coherent results. More recent architectures, such as LaMa, introduce Fourier convolutions (FFCs) to capture both global and local image structures, improving the quality of large-area inpainting \cite{suvorov_resolution-robust_2022}.

\subsection{Multi-Object and Scene-Level Reconstruction}

Moving beyond isolated objects, several methods address scene reconstruction from sparse observations. CAST~\cite{yao_cast_2025} reconstructs object-level geometry from a single RGB image by combining segmentation, relative depth estimation, GPT-based spatial reasoning, and physics-aware optimization using signed distance fields. However, CAST focuses on object reconstruction and placement rather than full environment completion.

Full scene completion has been studied extensively under the umbrella of semantic scene completion (SSC)~\cite{roldao_3d_2022}. Early approaches such as ScanComplete~\cite{dai_scancomplete_2018} predict complete geometry and semantics from partial scans. Subsequent methods incorporate instance-level reasoning (SISNet~\cite{cai_semantic_2021}), implicit representations (CIRCLE~\cite{chen_circle_2021}), and adversarial training strategies operating on 2D renderings (SPSG~\cite{dai_spsg_2020}).

NeRFiller~\cite{weber_nerfiller_2023} leverages 2D generative inpainting models to complete missing regions in neural radiance fields, bridging 2D image priors and 3D scene representations. Other works address clutter removal and visibility reasoning to improve reconstruction quality~\cite{wei_clutter_2023,song_vis2mesh_2021}. Gen3Dsr \cite{ardelean_gen3dsr_2025} splits the objects from the scene using a 2D mask. Subsequently, using depth estimation and separate environment and object completion Gen3Dsr is able to reconstruct a dynamic 3D scene with a single RGB image.

\subsection{Occlusion Completion and Full Scene Infilling}

Occluded surface completion focuses on reconstructing hidden geometry between objects and the environment. Behind the Veil~\cite{sun_behind_2024} introduces a coarse-to-fine octree representation with dual decoders: a scene-specific geometry decoder optimized online and a pretrained inpainter that infers occluded geometry from learned priors. While effective for geometry completion, such methods typically do not address texture or object-level semantics.

Hybrid reconstruction approaches integrate geometric priors and CAD models. Methods such as~\cite{li_hybrid_2024} segment indoor point clouds into structural and object components, reconstruct large planar surfaces via plane fitting, and replace detected objects with matched CAD models. These approaches leverage strong geometric assumptions and model fitting to produce lightweight and semantically structured reconstructions.



%\subsection{Object Dynamification}

%Object dynamification encompasses shape editing, scaling, and procedural variation. Scale-aware techniques such as 9-slicing \cite{w3_css-backgrounds-3border-image-slice_2024} in 2D and its 3D extension (27-slicing) \cite{deftly_27_2021} preserve structural proportions during resizing. Mesh-based deformation methods, including KeypointDeformer~\cite{jakab_keypointdeformer_2021}, automatically infer deformation cages using learned keypoints.

%Procedural generation approaches, such as Proc-GS~\cite{li_proc-gs_2024}, decompose 3D Gaussians into semantic components for recombination into novel structures. Implicit representations enable more flexible deformation. DIF-Net~\cite{deng_deformed_2021} models shapes as deformations of a shared template implicit field, enabling dense correspondences and category-level editing.

%Diffusion-based methods such as SALAD~\cite{koo_salad_2023} and XCube~\cite{ren_xcube_2023} enable part-level generation and manipulation using hierarchical or part-aware latent representations. SPAGHETTI~\cite{hertz_spaghetti_2022} and its extension SENS~\cite{binninger_sens_2024} support part-aware implicit editing without explicit supervision, allowing transformations such as part recombination and sketch-driven edits.

%\subsection{Summary}
In summary, prior work has explored object detection, completion, and scene reconstruction largely as separate problems. While recent methods increasingly integrate object-centric reasoning with scene-level priors, few approaches jointly address object reconstruction, occlusion completion, texture inpainting, and controllable object dynamification within a unified pipeline. This gap motivates our approach.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% Methodology %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\begin{figure}
    \centering
    \includegraphics[width=1\linewidth]{img/Method.png}
    \caption{The proposed pipline outlined in the different steps. From left to right: The pre-prosessing, the object detection, the parallel scene (up) and object (down) completion and a final reconstruction.}
    \label{fig:methodology}
\end{figure}

\section{Methodology}
\label{sec:methodology}

Our framework transforms partially scanned indoor environments into geometrically complete and dynamically adaptable 3D scenes. The pipeline consists of four primary stages as shown in Figure \ref{fig:methodology}: pre-processing, object detection, object completion and scene completion, all brought together in the scene reconstruction.

\subsection{Pre-processing}

The incomplete indoor 3D scenes are captured using terrestrial laser scanners (TLS) or mobile mapping systems (MMS), producing colored, unstructured point clouds alongside panoramic image observations. All 360 panoramic images are localized within the same coordinate system as the point cloud, enabling consistent cross-modal projections. These modalities provide complementary information: point clouds offer accurate geometric measurements with reduced occlusions, while panoramic images provide dense appearance cues.

\subsection{3D Object Detection}

Because point cloud observations typically exhibit fewer occlusions compared to image-only inputs, we perform a 3D object detection directly on the point cloud using VoteNet style detector. 

The objects are detected by performing a center density estimation where each input point $\mathbf{x_i}$is mapped to a vote $\mathbf{v_i}$ by first extracting features $\phi(\mathbf{x}_i)$ and then predicting a centre offset using a trained voting function ${g(.)}$ formulated in Eq. \ref{eq:voting}.
\begin{equation}
    \label{eq:voting}
    \begin{gathered}
    \mathbf{v}_i = \mathbf{x}_i + g(\phi(\mathbf{x}_i))
    \end{gathered}
\end{equation}

Votes are then aggregated forming proposal features used to predict box parameters. To mitigate over-detection, overlapping bounding boxes are merged based on the intersection over union of $IOU > 80\% $.

Detected objects are subsequently isolated from the scene. Rather than removing only visible points, all points in the full bounding box volume are removed to ensure clean separation between object geometry and the surrounding environment. This process results in an incomplete scene with large rectangular holes that can be reconstructed in a later stage.

To refine object isolation, planar structures are detected using a RANSAC-based plane detector. Identified planes are used to remove floor, wall, and ceiling elements that may be incorrectly included within object bounding boxes.

\subsection{Object Completion}

The object completion pipiline uses a 2-stage workflow where first the geometry is completed based on an input image and the partial pointcloud, and secondly, the texture is generated based on the completed geometry and the input image.

\subsubsection{Image refinement}

Each isolated object is represented as a partial point cloud, enclosed by a bounding box defined by its 6 corner points $\mathbf{P}_i = (X_i, Y_i, Z_i)^\top$ in world coordinates. To increase the fidelity of the completion, a part of the localised 360 panoramic image will used as input for the completion network. The image is cropped to only contain the current object by projecting each point of the bounding box into the camera coordinate frame formulated in Eq. \ref{eq:objectPointProjection}.

\begin{equation}
    \label{eq:objectPointProjection}
    \begin{gathered}
    \mathbf{P}_i^{c} = \mathbf{R}(\mathbf{P}_i - \mathbf{t})
    \end{gathered}
\end{equation}

where $\mathbf{R}$ and $\mathbf{t}$ denote the camera rotation and translation, respectively. Each transformed point is then mapped to spherical coordinates:

\begin{equation}
    \label{eq:spherical}
    \begin{gathered}
    \theta_i = \arctan2(X_i^{c}, Z_i^{c}), \quad
\phi_i = \arcsin\left(\frac{Y_i^{c}}{\|\mathbf{P}_i^{c}\|}\right)
    \end{gathered}
\end{equation}

For an equirectangular panoramic image of width $W$ and height $H$, the angular coordinates are converted into pixel coordinates:

\begin{equation}
    \label{eq:equirectangular}
    \begin{gathered}
    u_i = W \left( \frac{\theta_i + \pi}{2\pi} \right), \quad
    v_i = H \left( \frac{\frac{\pi}{2} - \phi_i}{\pi} \right)
    \end{gathered}
\end{equation}

The resulting set of projected image points $(u_i, v_i)$ defines the 2D footprint of the 3D bounding box on the panorama. A convex hull is computed over these points to obtain a geometrically consistent region enclosing the object projection. This hull is rasterized to generate a binary mask, which is subsequently used to extract the corresponding image region for further processing.

\subsubsection{Geometry Generation}

The completion network \cite{hunyuan3d_hunyuan3d-omni_2025} leverages multimodal conditional encoder paired with a Diffusion Transformer (DiT) and VAE-based decoder to generate high quality complete 3D models.

The image and partial pointcloud are encoded in parallel using a DINO-based encoder for the image and a position embedding followed by a linear layer for the point cloud $P_c$ to extract the condition features.

The image feature $c_i$ and control feature $\beta$ form a joint feature $c' = [c_i, \beta]$ that is fed into the DiT model. The latent results are decoded using the VAE-Decoder resulting in a 3D shape represented by an SDF. This SDF is then converted to a 3D mesh using the marching cubes algorithm.

\subsubsection{Texture Generation}

Following geometric reconstruction, The masked image is used again for the texture generation. This is performed using 3 subsequent modules: An image delighting module, a multi-view image synthesis module and a texture baking module.

The image delighting module removes all the highlights and shadows of the image. this results in a uniformly lit image that is independent of the current lighting.

Geometry-conditioned multi-view image synthesis is the main step to generate the texture of the object. It is designed as a two-stage process producing high-quality textures while handling occlusions and geometric complexity. Given the cropped image and the generated 3D shape, the diffusion model synthesizes images from arbitrary viewpoints. The model leverages a reference-net to preserve appearance details, geometry conditioning to ensure textures adhere to surface structure, and multi-view attention to maintain consistency across views. A learnable camera embedding specifies the target viewpoint, enabling dense-view inference where images can be generated for many angles to maximize surface coverage and reduce missing regions.

The generated multi-view images are consolidated into a texture map through baking and refinement operations. Because some areas may remain uncovered due to self-occlusion, an inpainting procedure fills gaps by propagating information from nearby textured regions using geometric proximity. To enhance visual fidelity, each generated view is optionally passed through a single-image super-resolution model, improving sharpness without disrupting cross-view consistency. 

When the baked texture is applied to the generated 3D model, the resulting textured mesh is a complete object ready to be placed back into the scene using the bounding box parameters as reference points.

\subsection{Scene Completion}

The remaining scene after the detected object removal is a partial pointcloud with large missing areas. These, and other missing areas due to occlusions and sensor limitations also need to be reconstructed, both geometrically and texturally. Similar to the object completion, this also is done in a 2 step process: Geometry reconstruction and texture inpainting.

\subsubsection{Geometry Reconstruction}

%Due to the planar nature typically found in indoor environments we can make the assumption that the remaining scene consists of multiple large planes. This is why we perform a sequential iterative RANSAC \cite{korman_latent_2018} plane detection to find all the planes that contain more than a pre-defined minimal amount of points. The iterative approach removes the points that are fitted to the most recently detected plane. After the process is complete, only a small amount of points remain that do not fit any plane.

Due to the planar nature typically found in indoor environments we can make the assumption that the remaining scene consists of multiple large planes. This is why we perform a sequential iterative RANSAC \cite{korman_latent_2018} plane detection to find all the planes that contain more than a pre-defined minimal amount of points. At each iteration, the largest detected plane is extracted and removed from the remaining point set, and the procedure is repeated until no planes containing a sufficient number of inliers remain.

For each detected plane, missing regions are estimated by generating a densified planar point set limited by the convex hull of the original partial plane. The goal is to obtain a more uniform sampling density over the plane surface.

\subsubsection{Texture Inpainting}

After the geometry is reconstructed, the original and densified plane are projected onto a 2D grid representation. Given a reference point $\mathbf{p}_0$ on the plane and two orthonormal basis vectors $\mathbf{u}$ and $\mathbf{v}$ spanning the plane, a 3D point $\mathbf{x}$ is mapped to planar coordinates:

\begin{equation}
    \label{eq:projection}
    \begin{gathered}
        u = (\mathbf{x} - \mathbf{p}_0) \cdot \mathbf{u}, \qquad
        v = (\mathbf{x} - \mathbf{p}_0) \cdot \mathbf{v}.
    \end{gathered}
\end{equation}

These coordinates are discretized to image pixel indices according to the chosen grid resolution. This projection produces a colour image representing the observed plane points and a binary mask indicating the regions corresponding to missing data.

The masked image is then processed using the inpainting network LaMa \cite{suvorov_resolution-robust_2022}. The model generates plausible textures for the missing regions based on the surrounding visual context of the plane. After inpainting, the generated texture is transferred back to the 3D representation by assigning pixel colours to the newly sampled points. For each filled point $i$, the colour is obtained from the inpainted image $I$ as

\begin{equation}
    \label{eq:reprojection}
    \begin{gathered}
        \mathbf{c}_i = I(u_i, v_i),
    \end{gathered}
\end{equation}

where $(u_i, v_i)$ are the projected pixel coordinates of the point.

Finally, the completed planes are merged to form a reconstructed coloured point cloud representing the completed environment.


%\subsection{Object Dynamification}

%To enable controllable object manipulation, we introduce an object dynamification mechanism inspired by slicing-based deformation techniques.

%Each reconstructed object undergoes convex decomposition to obtain a set of geometrically plausible parts without requiring semantic part annotations. Part indices are encoded into vertex attributes, enabling part-aware transformations.

%Users define a slicing axis along which the object is partitioned into three regions. Parts entirely contained within the first region remain unchanged, parts within the final region undergo rigid translation, and parts intersecting the intermediate region are adaptively transformed.

%Transformations are realized through either continuous geometric stretching or discrete part repetition, depending on structural suitability. This design preserves part-wise proportions while enabling flexible scaling and shape variation.

\subsection{Scene Reconstruction}

After reconstructing both the objects and the environment, the completed and textured objects are reinserted into the reconstructed scene. During object reconstruction, each input is normalized and recentered around the origin; the corresponding transformation is recorded and subsequently reversed when reinserting the object. This procedure ensures that each object is positioned precisely according to its location in the original scan.

The resulting scene is a fully reconstructed, dynamic 3D model of the environment. Since all objects are maintained as separate entities within the scene, they can be manipulated independently without introducing additional occlusions or inconsistencies.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% Experiments %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\section{Experiments}
\label{sec:experiments}

We evaluate our framework on both real-world and synthetic datasets to assess reconstruction quality, completion accuracy, and visual fidelity.

\subsection{Datasets}

For the evaluation we use two types of data: A real dataset to visually evaluate real-world performance and a synthetic dataset to validate our pipeline using different metrics per step.

\subsubsection{Real Datasets}
We evaluate real-world performance using the Matterport3D \cite{chang_matterport3d_2017} dataset, which provides large-scale indoor RGB-D scans with diverse scene layouts and object categories. We use panoramic images, generated from the dataset images in conjunction with the raw pointclouds divided per room.

\subsubsection{Synthetic Datasets}
We employ a synthetic dataset called \textit{V-Scan}generated using a virtual scanning pipeline \cite{virtual scanner}. This dataset contains a large number of scanned single-room partial scans that are generated by simulating TLS/MMS acquisition processes, including viewpoint sparsity and occlusion effects. The main benefit of this dataset is the availability of ground truth data, both on a per object bases in the form of complete meshes and accurate bounding boxes as well as the empty scene without any objects.

\subsection{Object Detection}

The object detection is evaluated using the IOU, recall and precision of the detected 3D boundingboxes.

\subsection{Scene Completion}

The scene completion is evaluated, both geometrically and texturally.

\subsubsection{Geometric Evaluation}
Point accuracy through Chamfer distance
- sum of mean squared distances of each clostestpoint pairs both ways
- Smaller is better, up to point sample size

\subsubsection{Textural Evaluation}
We use 3 metrics:

peak signal to noise ration (PSNR)
Pixel-level reconstruction accuracy relative to a ground truth image.
- Based on mean squared error (MSE) between reconstructed and reference texture.
- Higher PSNR = better reconstruction.

SSIM (Structural Similarity Index)
Range: -1 to 1 (usually 0–1 in practice)
Higher = more similar
Can also compute per-channel instead of grayscale if needed

LPIPS (Learned Perceptual Image Patch Similarity)
Resize images to same size before comparing
LPIPS expects:
RGB
normalized to [-1, 1]
SSIM is sensitive to:
alignment
small shifts

\subsection{Object completion}
For the object completion we also use the chamfer distance on the sampled mesh
Normal consistency

\subsection{Baselines}

We compare against representative methods from three categories:

\textbf{Scene Completion Methods.}
Global volumetric and implicit completion approaches.

\textbf{Object Completion Methods.}
Object-centric reconstruction models operating on isolated geometry.

\textbf{Hybrid / Reconstruction Methods.}
Pipelines combining geometry reconstruction and image-based inference.


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% Discussion %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\section{Discussion}
\label{sec:discussion}

Our experiments demonstrate that separating object and scene completion yields substantial improvements in reconstruction quality. Unlike global completion approaches, which often blur object boundaries or hallucinate inconsistent geometry, our framework preserves structural coherence by explicitly modeling object–scene interactions.

The synthetic dataset evaluation confirms that geometry-driven scene reconstruction reliably recovers dominant planar structures, while learned object completion captures fine geometric details. This complementary behavior explains the observed reduction in reconstruction error.


However, several limitations remain. First, reconstruction quality depends on the accuracy of object detection. Missed detections propagate errors into subsequent stages. Second, geometry-based scene completion assumes the presence of dominant planar structures, which may not hold in highly irregular environments.

Future work may explore tighter integration between detection, completion, and manipulation stages, as well as extensions to outdoor or non-planar scenes.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% Conclusion %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

\section{Conclusion}
\label{sec:conclusion}

We presented a hybrid framework for transforming partially scanned environments into geometrically complete and dynamically adaptable 3D scenes. By decoupling object completion and scene completion, our method combines the strengths of learned generative models and geometry-driven reconstruction.

Experiments on real-world and synthetic datasets demonstrate improved geometric fidelity, visual consistency, and robustness under partial observations. Our approach bridges scene reconstruction, object completion, and shape manipulation, enabling the generation of editable 3D environments from incomplete inputs.




% To print the credit authorship contribution details
\printcredits

%% Loading bibliography style file
%\bibliographystyle{model1-num-names}
\bibliographystyle{cas-model2-names}

% Loading bibliography database
\bibliography{Full_Scene_Completion}

% Biography
%\bio{}
% Here goes the biography details.
%\endbio

%\bio{pic1}
% Here goes the biography details.
%\endbio

\end{document}








