---
date: 2024-05-06
tags:
  - 游戏开发
  - 游戏开发/引擎
toolSet:
  - "[[现代C++基础]]"
knowledgeLink:
  - "[[Vector向量]]"
status: true
---
# Vector in Unity
In any 3D game engine, vector is an important concept, as it helps manipulating the transformation and movement of GameObjects. In unity, such operation is done through the use of `Vector3` class.

## Vector3 Class
- Representation of 3D vectors and points.
- Used to pass 3D positions and directions around.
- Contains functions for performing common vector operations.
In Unity, the coordinate system follows a left-handed rules:

![[Pasted image 20240511104846.png|center|400]]

At the same time, most of the vector operations can be implemented in Unity through `Vector3` class.

## Vector3 Class Component
### Define Vector
Definition of a new vector can be done through the following commands:

```csharp
Vector3 direction = new Vector3(x, y, z);
```

There are also built in vector definition that can be used:

```csharp
var up = Vector3.up;
var down = Vector3.down;
var left = Vector3.left;
var right = Vector3.right;
...
```

These `Vector3` components are unit vectors points to various directions in coordinate systems. `var` is just a way to declare variables.
### Vector Properties
Unity also embedded multiple functions that helps user obtaining vector properties, includes:

```csharp
Vector3.magnitude; //Get manitude of any vectors
Vector3.Distance(position1, position2); //Get distance vector between two given coordinates
Vector3.normalized; //Normalized vector and return its unit vectors
```

Note that `Vector3.Distance()` is equitant to `vector1-vector2` as `vector` here means from origin to target position.  

### Vector Calculation
Unity has provided various ways to calculate [[Vector向量#Vector Multiplication|dot and cross product]] of vectors:

```csharp
Vector3.Dot(Vector1, Vector2);

Vector3.Cross(Vector1, Vector2);
```

> [!warning]
> Notice that in Unity, cross product follows lefthand rule.

![[Pasted image 20240511203315.png|center|]]

Because angles can be get using dot and cross product, Unity also provides a angle function that in the bottom uses dot and cross product to calculate angle:

```csharp
Vector3.Angle(vector1, vector2);
```

This means, the angle you find, lays on **the plane formed by these two vectors**. You can find similar logic in vector [[Vector向量#Cross Product|cross product]] 

> [!Definition] Signed Angle
> Measuring from the x-axis, angles on the unit circle count as positive in the counterclockwise direction, and negative in the clockwise direction.

Signed angle returned is the angle of rotation **from the first vector to the second**, when treating these first two vector inputs as **directions**. These two vectors also define the plane of rotation, meaning they are parallel to the plane. This means the axis of rotation around which the angle is calculated is the cross product of the first and second vectors (and not the 3rd "axis" parameter). You can use the "left hand rule" to determine the axis of rotation, given the two input vectors. 

```csharp
 Vector3.SignedAngle(vector1, vector2);
```

# Time in Unity
## Time.deltaTime

> [!Definition]
> `Time.deltaTime` provides the time between the current and previous frame. 

Delta-time and frame rate are not always related. Video games fall into one of two categories regarding frame rate: **frame rate dependent or frame rate independent**. Frame rate Dependent games have a frame rate that varies based on the computer running the software. For example, if a frame rate dependent game runs at 300 frames per second (fps) on a computer with a refresh rate of 120 hertz (Hz), then it would run at 150 fps on a computer with a refresh rate of 60 Hz. 

> [!tip]
> A standard delta-time expression can create pause screens and intentional slow-motion effects in frame rate-dependent games. 

Standard delta-time formulas are seldom used for standard game-play because the delta-time between frames varies greatly depending on the refresh rate of the computer on which it is running.

If a game is frame rate independent, the frame rate is preset and runs identically across all computers regardless of their specifications. Frame rate independence limits the maximum graphics quality to make the game available to more consumers. 

> [!important]
> We can understand Time.deltaTime simply as a function that changes in values occur **per second** rather than per frame

`Time.deltaTime` can be used as a constant, to modify any variables and right values. Below is an example

```csharp
transform.Translate(Vector3.forward * Time.deltaTime);
```
