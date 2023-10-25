# Texture-based separation to refine building meshes 

## Introduction

- The Problem:
  - Meshes are an efficient way to store data compared to point clouds, better at representing a surface and texture detail.
  - We want to detect and isolate different objects in a indoor scene mesh.
  - but the sparse density of the faces means some faces can be part of multiple segments.
- The Goal:
  - We want to create a texture based segmentation model that can segment a mesh cleanly.
  - Can we use edge or boundary detection to segment the different zones of the image and create new triangles at the edges to make sure every face belongs to a single segment
- The Main contributions:
  - a novel segmentation model based on materials on a texture
  - texture boundary based face splitting
- The structure

## Background and related work

- segmentation methods
  - A lot of point based segmentation networks with normalized point clouds
  - This means they don't directly work with the geometry, but a abstraction of it
  - In meshes, the most detailed part is the texture information
  - rgb is mostly used as a extra parameter in geometry based segmentation, making it difficult to group repeating textures with distinct colors
  - a lot of image based segmentation from localised images
  - The uv map is rerely used to segment the scene, mostly because of the lack of a one to one relation between the scene and the taxture map.
- Boundary detection
  - Existing edge detection like (canny or HED) work well for clear geometrix boundaries, bt overdetect of rough textures like brick walls.
  - use Factorisation based Texture segmentation exists and can sample the texture to find similar patches to group them into a single texture group.

## Methodology

- Use Texture segmentation to separate the different materials in the meshes
- Detect the edges on the segmented texture
- Convert the detected edges to parametrised hough lines
- project the uv coordinates of the mesh onto the uv plane
- Cut the faces with the detected hough lines
- Define the new faces with the edge boundaries
- segment the new faces based on the segmentation index and neighbouring faces

### Texture Segmentation

- We want to group textures and reduce their detail for better edge detection
- use Factorisation based Texture segmentation to detect the different materials (explain the workings).

### Edge Detection

- The segmented image is pefect to detect the boundaries of the textures
- Use canny or HED edge detection to find the edges
- Find the hough lines to get parametrized straight lines in the detected edges, also use phoughlines for line segments

### Face Slicing

- project the faces on the uv texture
- slice the faces that are in the path of the new lines

### Object Segmentation

- we want to group the faces based on the common texture
- use the texture blobs and adjacent faces region growing to segment the different objects
- Each face with the same segmentation index and is connected to another same-index face is grouped.

## Experiments

### Data

- we use the matterport and s3dis to evaluate our method
- we need meshes with uv-unwrapped faces and texture maps.

### Region detection and separation

- check how good the method is at detecting the different materials 

### Comparing Segmentation methods

- evaluate the texture based segmentation vs ground truth

## Discussion

- The method performed well at detecting boundaries between distinct materials, but oversegments on complex shapes.
- it performs well on geometryless boundaries like posters or floormats

## Conclusion
- we created a new workflow to add more edges in meshes at distinct material discuntinuities.
- the method works great on meshes with sparse geometric detail, but high texture detail, like optimised photograpmmetry scans or handheld scan results.