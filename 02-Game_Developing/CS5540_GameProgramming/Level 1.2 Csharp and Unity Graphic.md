---
date: 2024-05-06
tags:
  - 游戏开发
  - 游戏开发/引擎
toolSet:
  - "[[现代C++基础]]"
knowledgeLink: 
status: true
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

> [!tip] Render Priority
> Skybox are usually rendered first prior to any other GameObjects in the scenes.  

Find settings in the skybox material properties. They gradually changed the way skybox texture is rendered.

# Scripting System: 

### Unity Script Component

> [!Definition]
> In Unity, C# scripts are also components.

Below shows the basic structure of script profile:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CubeBehavior : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```

It consists of two basic parts: 
- <mark class="hltr-orange">Namespace: </mark> The C based languages' running library
- <mark class="hltr-yellow">Class Definition: </mark> Name and GameObjects properties, <font color="#ff0000">keep it consistent through out the development</font>.
- <mark class="hltr-green">Start: </mark> Behavior of the GameObjects at the start of the program
- <mark class="hltr-cyan">Update: </mark> Behavior of the GameObjects that will updates in each frame

> [!info] MonoBehaviour
> MonoBehaviour is an open source implementation of Microsoft's .NET Framework

### Debug Log
You can always use the log system of unity C# library to debug. The format is shown below: 

```csharp
Debug.Log("Hello World!");
```

Anything inside the bracket will be printed both in the command bar at the bottom of Unity UI and at the console. 
### Variables in Script
Variables in Unity C# is controlled through the properties area of the GameObject class script. 

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CubeBehavior : MonoBehaviour
{
	// AKA this area
    void Start()
    {
        
    }

    void Update()
    {
        
    }
}
```

> [!important]
> Any variables declared here as public, will also be available in the script component UI for you to put in other GameObjects. 

> [!warning]
> Notify that anything changed during the game play state will not be saved!

# Basic Control System: Get Input from Users

Get method are used to get user inputs. However, the consists of different states. Take keyboard input as example, keyboard inputs varies includes:
1. `GetKey`: Returns true while the user holds down the key identified by name.
2. `GetKeyUp`: Return true until the user release the key.
3. `GetKeyDown`: Return true as soon as the user press the key.

> [!tip]
> Logical components in C# works the same as any other programing languages. So does the operation symbols like `||`, `&&` etc.

Input system can also obtain operations from mouse. But it's a bit more complex compare to keyboard inputs. Therefore, Unity used a new system called <mark class="hltr-cyan">Event Trigger</mark> to determine its input.

```csharp
private void OnMouseDown()
{
	
}
```

> [!important]
> By default, this function means the object is clicked. The subject in this case, is the **GameObject**.

> [!tip]
> You can always use `gameObject` to refer to the **GameObject of the current class**

Most functions in Unity library supports [[Polymorphism 多态]]. This means with different argument inputs, the function can perform in completely different way. 

> [!example]
> For example, the destroy function of Unity can be called with the format of `Destroy(gameObject);` or `Destroy(gameObject, 3);`. The latter format allows you to add a 3 seconds delay before destroying the object.

# Prefab System

> [!tip]
> Prefab is like a template or blueprint in Unity.

Prefab allows you to create, configure, and store a GameObject with all its components. And you can create multiple instances of a prefab in a single scene, like factory.

> [!NOTE]
> To create a prefab, simply drag objects from scene into asset explorer.