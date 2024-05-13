---
date: 2024-05-11
tags:
  - 计算机/图形学/计算几何
toolSet:
  - "[[现代C++基础]]"
  - "[[Graph 图]]"
knowledgeLink:
  - "[[Iteration and Recursion 迭代与递归]]"
  - "[[Time Complexity 时间复杂度]]"
status: true
---
# Physics Engine System

```embed
title: "Bullet Real-Time Physics Simulation | Home of Bullet and PyBullet: physics simulation for games, visual effects, robotics and reinforcement learning."
image: "https://pybullet.org/wordpress/wp-content/uploads/2022/03/teaser-2.gif"
description: "Kubric is an open-source Python framework that interfaces with PyBullet and Blender to generate photo-realistic scenes, with rich annotations, and seamlessly scales to large jobs distributed over thousands of machines, and generating TBs of data. Kubric can generate semi-realistic synthetic multi-object videos with rich annotations such as instance segmentation masks, depth maps, and optical flow."
url: "https://pybullet.org/wordpress/"
```
## 3 Basic Components of PES
### Integration
- Move objects (model forces with velocity, acceleration, and any other movement related program);
- Typically what you learned in Physics-101 kinematics class;
### Detection
- Detect the relationship between objects;
- Includes collision, intersection, etc.;
### Resolution
- Sort out the result of the collision (i.e. move objects into a position such that they do not overlap);
- Which also so called response, like when two balls hits each other, what should happen next: they fly way or being destroy etc.;

## Representing 3D Geometry
### Mesh 3D
3D geometry are usually represented in a structure called [polygon mesh](https://en.wikipedia.org/wiki/Polygon_mesh).

> [!Definition] Polygon mesh
> In 3D computer graphics and solid modeling, a polygon mesh is a collection of vertices, edges and faces that defines the shape of a polyhedral object. 

 The faces usually consist of triangles (triangle mesh), quadrilaterals (quads), or other simple convex polygons (n-gons), since this simplifies rendering, but may also be more generally composed of concave polygons, or even polygons with holes.

The study of polygon meshes is a large sub-field of computer graphics (specifically 3D computer graphics) and geometric modeling. Different representations of polygon meshes are used for different applications and goals. The variety of operations performed on meshes may include: Boolean logic (Constructive solid geometry), smoothing, simplification, and many others. Algorithms also exist for ray tracing, collision detection, and rigid-body dynamics with polygon meshes. If the mesh's edges are rendered instead of the faces, then the model becomes a wireframe model.

> [!tip] Nature of Mesh
> You can understand mesh as a <font color="#c0504d">graph</font> connecting individual vertices with edges connected vertices together. 

On computer side, we demonstrate such [[图的邻接矩阵|graph]] in a form of:
$$
G(V,E)
$$

> [!important]
> It is important to notice that meshes are formed based on triangles! This is because 3 points are minimum numbers of points required to form a plane. 
### Mesh Elements

![[Pasted image 20240512151743.png|center|400]]
Objects created with polygon meshes must store different types of elements. These include vertices, edges, faces, polygons and surfaces. In many applications, only vertices, edges and either faces or polygons are stored. A renderer may support only 3-sided faces, so polygons must be constructed of many of these, as shown above. However, many renderers either support quads and higher-sided polygons, or are able to convert polygons to triangles on the fly, making it unnecessary to store a mesh in a triangulated form.

> [!abstract]
> <mark class="hltr-orange">vertex</mark>
> A position (usually in 3D space) along with other information such as color, normal vector and texture coordinates.
> 
> <mark class="hltr-yellow">edge</mark>
> A connection between two vertices.
> 
> <mark class="hltr-green">face</mark>
> A closed set of edges, in which a triangle face has three edges, and a quad face has four edges. A polygon is a coplanar set of faces. In systems that support multi-sided faces, polygons and faces are equivalent. However, most rendering hardware supports only 3- or 4-sided faces, so polygons are represented as multiple faces. Mathematically a polygonal mesh may be considered an unstructured grid, or undirected graph, with additional properties of geometry, shape and topology.

These three are basic components of any 3D meshes. And they together formed a larger compact system, includes:

<mark class="hltr-cyan">surfaces</mark>
More often called smoothing groups, are useful, but not required to group smooth regions. Consider a cylinder with caps, such as a soda can. For smooth shading of the sides, all surface normals must point horizontally away from the center, while the normals of the caps must point straight up and down. Rendered as a single, Phong-shaded surface, the crease vertices would have incorrect normals. Thus, some way of determining where to cease smoothing is needed to group smooth parts of a mesh, just as polygons group 3-sided faces. 

<mark class="hltr-blue">groups</mark>
Some mesh formats contain groups, which define separate elements of the mesh, and are useful for determining separate sub-objects for skeletal animation or separate actors for non-skeletal animation.

<mark class="hltr-purple">UV coordinates</mark>
Most mesh formats also support some form of UV coordinates which are a separate 2d representation of the mesh "unfolded" to show what portion of a 2-dimensional texture map to apply to different polygons of the mesh

### Polygons
As mentioned above, multiple triangles can form polygons. And polygon is what we work with the most in computation geometry. 

> [!Definition] Polygon
> Polygons are plane shape (two-dimensional) with straight sides, vertices forms a polygon are also called a polygon set.

Polygon usually exists in two forms, **concave** and **convex**:

> [!Definition] Concave polygon
> A simple polygon is concave **iff** at least one of its <font color="#4f81bd">internal angles</font> is <font color="#c0504d">greater</font> than 180°. 

![[Pasted image 20240512202418.png|center|500]]
<center><sup>Types of polygon</sup></center>
The reason we need to identify convex shape is because we need to understand an important property of polygon: Convexity.

> [!abstract] Convexity
> A shape (or set) is convex if for any two points (A and B) in that set, the line segment forming AB lies entirely within that set.

Convex shape has all interior angle smaller than 180°, and therefore, has no lines connecting any two vertices exceeds the polygon boundary, as shown below:

![[Pasted image 20240512203455.png|center|]]
<center><sup>In convex shape, all lines falls within the polygon</sup></center>
When it comes to computational geometry, we prefer convex polygons more. This is because:

> [!abstract] Convex Polygon's pros
> 1. Convex polygons can be easily triangulated
> 2. Triangulating any convex polygon is trivial, using 'fan triangulation' method
> 3. This can be done in time complexity of $O(n)$

![[Pasted image 20240512204430.png|center|200]]
<center><sup>Diagram showing fan triangulation</sup></center>
In computational geometry, a fan triangulation is a simple way to triangulate a polygon by choosing a vertex and drawing edges to all of the other vertices of the polygon. **Not every polygon can be triangulated this way**, so this method is usually only used for **convex polygons**.

For **concave polygons**, this process is complex, since we need to decompose the polygon into a set of convex polygons. Because fan connection will cause at least one of the new edges falls out of the polygon.
### Polygons Mesh

> [!Definition]
> In 3D computer graphics and solid modeling, a polygon mesh is a collection of vertices, edges and faces that defines the shape of a polyhedral object. The faces usually consist of triangles (triangle mesh), quadrilaterals (quads), or other simple convex polygons (n-gons), since this simplifies rendering, but may also be more generally composed of concave polygons, or even polygons with holes.

![[Pasted image 20240512182621.png|center]]
<center><sup>Example of a low poly triangle mesh representing a dolphin</sup></center>

## 3D Intersection
How do we know whether two geometry collides? 
### Triangle-to-triangle intersection test
To test if and when two 3D geometries are colliding, we could essentially loop through every single triangle in both meshes (sets) and test if they intersect. Which by default, will be a $O(n^2)$ algorithm. 
### Separating Axis Theorem (SAT)
The Separating Axis Theorem, SAT for short, is a method to determine if two convex shapes are intersecting. The algorithm can also be used to find the minimum penetration vector which is useful for physics simulation and a number of other applications. SAT is a fast generic algorithm that can remove the need to have collision detection code for each shape type pair thereby reducing code and maintenance.

```embed
title: "SAT (Separating Axis Theorem)"
image: "https://dyn4j.org/assets/posts/2010-01-01-sat-separating-axis-theorem/convex-ex-1.png"
description: "This is a post I have been meaning to do for some time now but just never got around to it. Let me first start off by saying that there are a ton of resources on the web about this particular collision detection algorithm. The problem I had with the available resources is that they are often vague when explaining some of the implementation details (probably for our benefit)."
url: "https://dyn4j.org/2010/01/sat/"
```

By default, SAT theorem states that two convex polyhedra are separated by a plane:
- If they are otherwise making contact, the result will be a 'point', 'line segment', or another 'convex polygon'
- Such method **does not applied to concave polygons**, or a non-fully convex polygon. 

#### No Intersection
First lets discuss how SAT determines two shapes are not intersecting. In figure 5 we know that the two shapes are not intersecting. A line is drawn between them to illustrate this.

![[Pasted image 20240512214151.png|center]]
<center><sup>Two Separated Convex Shapes</sup></center>
If we choose the perpendicular line to the line separating the two shapes in figure 5, and project the shapes onto that line we can see that there is no overlap in their projections. A line where the projections (shadows) of the shapes do not overlap is called a separation axis. In figure 6 the dark grey line is a separation axis and the respective colored lines are the projections of the shapes onto the separation axis. Notice in figure 6 the projections are not overlapping, therefore according to SAT the shapes are not intersecting.

![[Pasted image 20240512214220.png|center]]
<center><sup>Two Separated Convex Shapes With Their Respective Projections</sup></center>
SAT may test many axes for overlap, however, the first axis where the projections are not overlapping, the algorithm can immediately exit determining that the shapes are not intersecting. Because of this early exit, SAT is ideal for applications that have many objects but few collisions (games, simulations, etc.).

To explain a little further, examine the following psuedo code.

```c
Axis[] axes = // get the axes to test;
// loop over the axes
for (int i = 0; i < axes.length; i++) {
  Axis axis = axes[i];
  // project both shapes onto the axis
  Projection p1 = shape1.project(axis);
  Projection p2 = shape2.project(axis);
  // do the projections overlap?
  if (!p1.overlap(p2)) {
    // then we can guarantee that the shapes do not overlap
    return false;
  }
}
```
#### Intersection
If, for all axes, the shape’s projections overlap, then we can conclude that the shapes are intersecting. Figure 7 illustrates two convex shapes being tested on a number of axes. The projections of the shapes onto those axes all overlap, therefore we can conclude that the shapes are intersecting.
![[Pasted image 20240512214329.png|center|300]]
<center><sup>Two Convex Shapes Intersecting</sup></center>
All axes must be tested for overlap to determine intersection. The modified code from above is:

```c
Axis[] axes = // get the axes to test;
// loop over the axes
for (int i = 0; i < axes.length; i++) {
  Axis axis = axes[i];
  // project both shapes onto the axis
  Projection p1 = shape1.project(axis);
  Projection p2 = shape2.project(axis);
  // do the projections overlap?
  if (!p1.overlap(p2)) {
    // then we can guarantee that the shapes do not overlap
    return false;
  }
}
// if we get here then we know that every axis had overlap on it
// so we can guarantee an intersection
return true;
```
### Bounding Volume

> [!question] Reduce Computing Resources
> Both methods above required traversal of the entire mesh. Is there a more computation recourse light method to detect collision?

In computer graphics and computational geometry, a **bounding volume** (or bounding region) for a set of objects is a closed region that completely contains the union of the objects in the set. Bounding volumes are used to improve the efficiency of geometrical operations, such as by using simple regions, having simpler ways to test for overlap.

A bounding volume for a set of objects is also a bounding volume for the single object consisting of their union, and the other way around. Therefore, it is possible to confine the description to the case of a single object, which is assumed to be non-empty and bounded (finite). Usually:
- Bounding volumes are often spheres or cubes which allow for much quicker collision checks
- This 'bounding volume' allows us to do a cheaper simplified shape intersection test

> [!example] Use sphere as bounding volume
> Simply add the radius of both spheres, if the `distance from center` < `sum of both radius`, a collision occur.

There are many different bounding volume techniques:
- In many applications the bounding box is aligned with the axes of the co-ordinate system, and it is then known as an <font color="#4bacc6">axis-aligned bounding box</font> (**AABB**). 
- To distinguish the general case from an AABB, an arbitrary bounding box is sometimes called an <font color="#4bacc6">oriented bounding box</font> (**OBB**), or an **OOBB** when an existing object's local coordinate system is used.

> [!important]
> The biggest disadvantage of bounding volume is the low accuracy. 

Convex hull gives a way to generate more accurate bounding volume. 