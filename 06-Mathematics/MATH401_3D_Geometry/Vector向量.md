---
date: 2024-01-17
tags:
  - 数学/线性代数
  - 计算机/图形学
toolSet: 
knowledgeLink: 
status: true
---
# Vector Definition

- Usually written as $\vec{a}$  or $\mathbf{a}$
- Using start and end points to define a vector  $\overrightarrow{AB}=B-A$
- Direction and length
- No absolute starting position
## Vector Geometry Meanings:

![[06-Mathematics/MATH401_3D_Geometry/3DGeometry_Map.md#^area=C1VdzlyYL30N0pd-De_6v|VectorSample|center| 300]]
### **Two Basic Components of Vectors:**
<mark class="hltr-orange">Magnitude: </mark> Length of Vector 向量长度（模）
The magnitude of the vector, $\left \| A \right \|$ is the length of the vector, $A$. Distance between two points, the tail and head
$$
\begin{align}
          A & =[x,y,z]\\
		  \left \| A \right \| & =\sqrt{x^2+y^2+z^2} 
          \end{align}
$$

<mark class="hltr-cyan">Direction: </mark> Way the vector is pointing
The direction of a vector is usually represented using unit vectors

> [!important]
> A unit vector has a magnitude of $1$

A unit vector is a normalized vector that points in the same direction as the original vector
To normalize a vector, divide the vector by its magnitude:
> [!Equation]
> $$
> \begin{align}
> \hat{v}=\frac{v}{\left \| v \right \| }  
>           \end{align}
> $$

> [!tip]
> This process will shrink the vector into the <font color="#c0504d">unit circle</font>, generates a <font color="#c0504d">unit vector</font> points to the same direction. 
### Unit Circle

> [!question] Why Unit Circle
> Since all unit vector falls onto unit circle, we can make use of the unit circle properties to calculate unknown arguments of given vectors. Usually, we used these properties to calculate the <font color="#c0504d">angle</font>

Within a unit circle, exists the following relationship:

> [!Equation]
> $$
> \begin{align}
> \sin \theta &= \frac{adjacent}{hypotenuse} \\
> \cos \theta &= \frac{opposite}{hypotenuse}\\
> \tan \theta &= \frac{opposite}{adjacent}
>           \end{align}
> $$

**In which as shown in the diagram, the hypotenuse is always <font color="#c0504d">1</font>.** 

![[06-Mathematics/MATH401_3D_Geometry/3DGeometry_Map.md#^area=MIy6ilwLyiSJrHp3zxGz9|UniCircle|center|300]]
### Vector Usage in distance
<mark class="hltr-blue">Distance: </mark> Length between two points. 
The magnitude of the vector from A to B gives you the distance between A and B:
$$
\begin{align}
          a & = [x_{1},y_{1},z_{1}]  \\
          b & = [x_{2},y_{2},z_{2}]  \\
		  D & =\sqrt{(x_{1}-x_{2})^2+y(y_{1}-y_{2})^2+(z_{1}-z_{2})^2} 
          \end{align}
$$
![[06-Mathematics/MATH401_3D_Geometry/3DGeometry_Map.md#^area=QUIb8hf0HJ6yUk2QXEZrS|VectorDis|center|300]]
# Vector Operation
## Vector Addition
To add two vectors, add the <font color="#c0504d">corresponding</font> components
- Geometrically: Parallelogram law & Triangle law
- Algebraically: Simply add coordinates
$$
\begin{align}
          a & = [x_{1},y_{1},z_{1}]  \\
          b & = [x_{2},y_{2},z_{2}]  \\
		  a+b & = [x_{1}+x_{2},y_{1}+y_{2},z_{1}+z_{2}] 
          \end{align}
$$

![[06-Mathematics/MATH401_3D_Geometry/3DGeometry_Map.md#^area=pcMQRoqOuqHjvm9AxyfU6|VectorAdd|center|300]]
## Vector Subtraction
Subtraction can be interpreted as adding the <font color="#c0504d">negative</font>
$$
\begin{align}
          a & = [x_{1},y_{1},z_{1}]  \\
          b & = [x_{2},y_{2},z_{2}]  \\
		  a-b & = [x_{1}-x_{2},y_{1}-y_{2},z_{1}-z_{2}] 
          \end{align}
$$

![[06-Mathematics/MATH401_3D_Geometry/3DGeometry_Map.md#^area=VJ0lyWhgSiIxzxKCRR8tR|VectorSub|center|300]]
## Vector Scaling
We can multiple a vector by a scalar
The result is a vector <font color="#c0504d">parallel to</font> the original vector with a different length
<font color="#c0504d">Negative scalar </font>can give opposite direction

- Vector scaling is through a linear multiplication with any real number. 
	- $v=[x,y,z]$
	- $k v=[kx, ky, kz]$

![[06-Mathematics/MATH401_3D_Geometry/3DGeometry_Map.md#^area=48kmD8frN6CD12_jNhi3T|VectorScalar|center|300]]
## Vector Multiplication
### Dot Product
#### Dot Product Calculation

> [!Definition]
> The dot product of two vectors is the sum of the products of corresponding components, resulting in a <font color="#c0504d">scalar value</font>

Given vectors $a$ and $b$ The simplest way to calculate dot product is:
$$
\begin{align}
          a & = [x_{1},y_{1},z_{1}]  \\
          b & = [x_{2},y_{2},z_{2}]  \\
		  a\cdot b & = x_{1}x_{2}+y_{1}y_{2}+z_{1}z_{2} 
          \end{align}
$$
We can also understand the dot product of vectors $a$ and $b$ as equal to the cosine of the angle $θ$ between the vectors, multiplied by the lengths of the vectors. 

> [!Equation]
> $\vec{a} \cdot \vec{b}=\|\vec{a}\|\|\vec{b}\| \cos \theta$

This formular can also give us the way to find out angle $θ$ between the vectors:
$$
\begin{align}
\because \vec{a} \cdot \vec{b} &=\|\vec{a}\|\|\vec{b}\| \cos \theta \\
\therefore \theta &= \arccos \left ( \frac{\vec{a} \cdot \vec{b} }{\left \| \vec{a}  \right \| \cdot \left \| \vec{b}  \right \| }  \right ) 
          \end{align}
$$
Since the calculation involves [[Vector向量#Vector Normalization|normalization]], we can simplify the formular when $\vec{a}$ and $\vec{b}$ are <font color="#c0504d">unit vectors</font>:

> [!Equation] Dot product of unit vectors
> $\hat{a} \cdot  \hat{b} = \cos \theta$
> $\theta = \arccos \left (  \hat{a} \cdot  \hat{b}\right )$
#### Dot Product Geometric Meanings
As shown above, dot product can be used to calculate angle. This implies a geometrical meaning for dot product: the angle between two vectors can be implies by dot product. It hold the following relationships:

- $\hat{a} \cdot \hat{b} {\color{Red} \mathbf{=1}}  \Rightarrow \cos\theta = 1,\theta =0°$
- $\hat{a} \cdot \hat{b} {\color{Red} \mathbf{=-1}}  \Rightarrow \cos\theta = -1,\theta =180°$
- $\hat{a} \cdot \hat{b} {\color{Red} \mathbf{=0}}  \Rightarrow \cos\theta = 0,\theta =90°$
They implies same direction, opposite direction, and perpendicular relationships. 

![[06-Mathematics/MATH401_3D_Geometry/3DGeometry_Map.md#^area=fDP-YEycOhM6uWTpBA0JC|DotGeo|center|400]]
### Cross Product
#### Cross Product Calculation
> [!Definition]
> Cross product is calculation between two 3D vectors as if they are on the same plane. It gives a new vector that perpendicular to this plane formed by the given two vectors.

If we have vectors $a$ and $b$ such that:
$$
\begin{align}
          a & = [x_{1},y_{1},z_{1}]  \\
          b & = [x_{2},y_{2},z_{2}]  \\
          \end{align}
$$
The simplest way to calculate cross product is through the angle $\theta$ between two vectors:

> [!Equation] Cross Product
> Here, $\vec{n}$  is a vector that perpendicular to both $\vec{a}$ and $\vec{b}$. We can find this vector using [[Vector向量#Righthand Rule|righthand rule]]
> $\vec{a} \times \vec{b} =\|\vec{a}\|\|\vec{b}\| \sin (\theta) \vec{n}$

Given vectors $a$ and $b$, we can also manually calculate cross product as: 
$$
\begin{align}
a\times b & = \begin{pmatrix}
 y_{1}z_{2}-z_{1}y_{2} \\
 z_{1}x_{2}-x_{1}z_{2}\\
 x_{1}y_{2}-y_{1}x_{2}
\end{pmatrix} = 
\begin{pmatrix}
 x_{3}\\
 y_{3}\\
 z_{3}
\end{pmatrix}
          \end{align}
$$
We can also find the length of cross product using a similar formular:
$$\|\vec{a} \times \vec{b}\| =\|\vec{a}\|\|\vec{b}\| \sin \theta$$
#### Unit Vectors of Cross Product
Similar to dot product, we can also use cross product to determine angles, by using the unit vector of the input vectors $\vec{a}$ and $\vec{b}$. 
> [!Equation] Cross product of unit vectors
> $\|\hat{a} \times  \hat{b} \|= \sin \theta$
> $\theta = \arcsin \left (  \|\hat{a} \times  \hat{b}\right \|)$
#### Righthand Rule
The direction of resulting vector can be determined using right hand rule. This is so call the<font color="#c0504d"> Handedness</font> property of cross product.
![[Pasted image 20240511093545.png|center|200]]
## Vector Normalization

- Magnitude (length) of a vector written as $\left \| \overrightarrow{a} \right \|$
- Unit vector
    - A vector with magnitude of 1
    - Finding the unit vector of a vector (normalization)

> [!Equation]
> $$
> \begin{align}
> \hat{v}=\frac{v}{\left \| v \right \| }  
>           \end{align}
> $$

- Used to represent directions
## Cartesian Coordinates

- X and Y can be any (usually orthogonal unit) vectors

$\mathbf{A} = \binom{x}{y}$ **向量标准形式**

$\mathbf{A}^{T} = (x,y)$ **转置矩阵**

$\left \| A \right \| =\sqrt{x^2+y^2}$ **向量长度（模）**
