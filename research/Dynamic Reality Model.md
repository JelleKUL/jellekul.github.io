---
layout: publication
title: Dynamic Reality Model
permalink:
---

# Dynamic Reality Model

## Introduction
- A representation of an object/scene that is:
	- efficient to store
	- quick to manipulate
	- easy to interchange 
- a collection of objects
	- each object has it's own datastructure and can be edited on its own

### Formats
- Geometry
	- voxel octree per object
- Texture
	- color / material representation
	- Point based
	- Texture field
	- uv map
- 
- Interaction
	- part index

### Shareability
- The data must be send across the network to be able to be used on different devices

### FileType
Linked data approach.
- object edits can be stored in the metadata, keep for future references without altering the original data
- do I need to separate the objects in the files, or can I keep them together? 
- Use 3D textures to store the data