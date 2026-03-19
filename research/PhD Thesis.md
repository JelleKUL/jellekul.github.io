# XR-empowered dynamic reality modeling for AECO applications

## Jury

- Kristof Vandenbogaerde
- Adalberto L. Simeone
- Corneel Cannaerts
External:
- ~~[Prof. Dr. Angela Dai]([https://www.3dunderstanding.org/team.html](https://www.professoren.tum.de/en/dai-angela)) TU Munich~~
- [Prof. Dr. Daniel Cremers](https://www.professoren.tum.de/en/cremers-daniel) TU Munich
- [Prof. Dr. Raphaelle Chaine](https://perso.liris.cnrs.fr/raphaelle.chaine/)Université de Lyon
	- 3D ML & inpainting
- [Prof. Dr. Lourdes De Agapito Vicente](https://profiles.ucl.ac.uk/40331-lourdes-de-agapito-vicente/about) (University College London)
	- 3D Vision & Generative AI 
- [Prof. Dr. Seyran Khademi](https://www.tudelft.nl/ewi/over-de-faculteit/afdelingen/intelligent-systems/pattern-recognition-bioinformatics/computer-vision-lab/people/seyran-khademi) TU Delft
	- CV & Architecture (floor plan generation)


## Main Structure

1. Introduction
2. Scene Dynamification (DRM) goal and background
	1. scope definitie1 ruimte
3. Data Acquisition, puntenwolk deficiencies
	> occlusie evaluatie, tussen brol en structuur, door object, in industriele scenes
	1. Real world data [[22-01-pointcloud-validation]]
		1. XR devices
		2. Laserscanners
		3. Datasets
	2. Synthetic data [[26-07-virtual-scanner]]
> hoe verschillen ze van echt > met ruis
		1. 3D models
		2. Virtual scanner
	3. Dataset standardisation [[24-05-geomapi]]
4. Alignment and Updating of the data
	1. Global and local alignment [[22-06-two-step-alignment]]
	2. Dataset updating [[22-07-automatic-alignment-and-completion]]
5. Scene/object segmentation/detection [[23-06-Dataset-Cleanup]]
		1. instance segmentation
6. Scene completion
	1. mesh refinement [[23-09-texture-mesh-refinement]]
	2. semantic uv mapping [[24-09-semantic-uv-mapping]]
	3. geometry reconstruction [[23-06-Dataset-Cleanup]] > niet nodig, gewoon nieuwe methode
	4. texture inpainting [[research/scene-completion]]
7. Object completion
	1. Voxel based object completion [[25-02-object-generation]]
		1. Voxel guiding [[25-04-voxel-editor]]
		2. evaluation [[25-06-completion-evaluation]]
	2. Image based completion [[research/scene-completion]]
	3. Completion evaluation
		1. evaluatie van de chamfer distance en andere 
8. Object dynamification [[26-07-object-dynamification]]
9. Dynamic scene interaction
	1. samenbrengen van de scene en toepassingen
	2. POC's
	3. Matterport completion
10. Conclusions & future works
11. Valorisation
	1. [[IOF Project]] 
	2. [[Exploitation Plan]]

alle headers uitwerken
> en literatuurstudies enkel voor zaken die nodig zijn

30 paginas per hstk MAX!
- De wiskundige termen 


## Snippets

```
\begin{figure}[h]
  \centering
  \medskip
  \includegraphics[width=1\textwidth]{PATH}
  \caption[SHORT_CAPTION]{LONG_CAPTION}
  \label{fig:LABEL}
\end{figure}
```

