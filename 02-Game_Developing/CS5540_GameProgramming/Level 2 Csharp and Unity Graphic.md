---
Date: 2024-05-06
tags:
  - 游戏开发
  - 游戏开发/引擎
ToolSet: "[[现代C++基础]]"
KnowledgeLink:
---
# Graphics Rendering in Unity
## Basic Graphic Concepts
Just like other PBR rendering engine, the default rendering system uses the same graphic pipeline with the following key elements:

1. Meshes: Main graphic primitive and define the shape of an object
		Vertex, Edges, Faces are the basic components of meshes
2. Material: Define the way a surface is render
3. Shaders: Small scripts that takes care of the mathematical calculation of ray and lights
4. Textures: Bitmap images applied over the mesh surface

## Environment Basic: Start from Skybox
