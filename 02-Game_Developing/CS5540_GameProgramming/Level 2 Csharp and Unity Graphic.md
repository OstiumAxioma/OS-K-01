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

> [!tip]
> Just like other PBR rendering engine, the default rendering system uses the same graphic pipeline with the following key elements:

1. Meshes: Main graphic primitive and define the shape of an object
		Vertex, Edges, Faces are the basic components of meshes
2. Material: Define the way a surface is render
3. Shaders: Small scripts that takes care of the mathematical calculation of ray and lights
4. Textures: Bitmap images applied over the mesh surface

## Environment Basic: Start from Skybox

> [!Definition]
> Skybox is a dome (usually a sphere) that covers the entire 3D environment. Its nature is a material with special texture mappings called HDRI. 

The material system in Unity has provided various default mappings for skybox, including those who are not sphere. To use it, you need to create a new material first, and make it <mark class="hltr-purple">skybox</mark> type of material. This will give you the option to modified the material that fits the skybox requirement. 

> [!example] 
> Create a 6-sided skybox material, and set each side with different environment texture.

Till this point, we have only created a skybox material. But to create a skybox, we need light to associate with material. 

> [!tip]
> You can imagine skybox as a dome light

By default, each 3D scene in Unity contains a skybox system. You can find this in `Window->Rendering->Lighting Settings`. And you can applied the skybox material created here. 