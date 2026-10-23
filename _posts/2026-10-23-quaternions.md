---
layout: distill
title: Quaternions
date: 2026-10-22 22:00:00
description: A Linear Algebraic Approach to Quaternions
tags: linear-algebra quaternions 3d rotations
categories: linear-algebra
toc:
    - name: Two Dimensional Rotations
    - name: Quaternions
    - subsections:
        - name: Some Identities
    - name: Pure Quaternions Induce Quarter Rotations
    - name: Three Dimensional Rotations with General Quaternions
    - name: Final Algorithm
pseudocode: true
---

I took some robotics classes recently and although our professor only stuck to Euler angles, I couldn't help but explore the rich world of quaternions. I hope this resultant writeup is somewhat helpful in understanding quaternions, particularly from the perspective of linear algebra.

## Two Dimensional Rotations
Let us start with the familiar rotations in two dimensional plane. A point $(x, y)$ is to be rotated $\theta$ around the origin. \\
The basis vectors are sent to $$\hat{i}_{rotated} = \begin{bmatrix} \cos\theta \\ \sin\theta \end{bmatrix}$$ and $$\hat{j}_{rotated} = \begin{bmatrix} -\sin\theta \\ \cos\theta \end{bmatrix}$$ 

The first identity is easy to see from the very definitions of trigonometric functions. The second follows from $+90 ^\circ$ rotation of the first point. \\
Then general rotation is then defined by the linear transformation: 

$$
\begin{bmatrix}
\cos\theta & -\sin\theta \\ 
\sin\theta & \cos\theta
\end{bmatrix}
\begin{bmatrix}
x \\ 
y
\end{bmatrix}
= \begin{bmatrix}
   x\cos\theta-y\sin\theta \\
   x\sin\theta+y\cos\theta
\end{bmatrix}
$$

Equivalently we can also employ complex numbers to represent rotations. Complex numbers are formed by pairs of reals $a, b \in \mathbb{R}$ to get $a+bi$, where $i^2 = -1$. The squared magnitude of complex number is defined as $a^2+b^2$. \\
If we then define complex multiplication with the usual distributive law, we can show that multiplication by complex number whose magnitude is $1$ corresponds to rotation of the number when interpreted as a two dimensional point. \\
i.e.

$$
(a+bi)(\cos\theta+i\sin\theta) = (a\cos\theta-b\sin\theta) + (a\sin\theta+b\cos\theta)i
$$

We can also use matrices to represent complex numbers such that matrix multiplication corresponds to complex numbers multiplication.
Specifically, write

$$
R_\theta = 
\cos\theta\,\mathbf{1}
+\sin\theta\,\mathbf{i}
$$

where the identity matrix is denoted as $\mathbf{1}$ and the $+90^\circ$ rotation matrix as $\mathbf{i}$:

$$
\mathbf{1} = \begin{bmatrix}
    1 & 0 \\
    0 & 1
\end{bmatrix}
\qquad
\mathbf{i} = \begin{bmatrix}
    0 & -1 \\
    1 & 0
\end{bmatrix}
$$

Here our general rotation matrix $R_\theta$ belongs to the vector space of all $2\times2$ matrices that represent rotations, whose basis vectors are $\mathbf{1}$ and $\mathbf{i}$. Verify that these basis vectors are orthogonal to each other. From here on, it will be helpful to think all the matrices as a long vector. \\
Each rotation can thus be thought of as being composed of a $\cos\theta$ component of $0^\circ$ rotation and a $\sin\theta$ component of $+90^\circ$ rotation (these projections can also be obtained by dot product with the basis vectors if they are unit magnitudes, which we can achieve by dividing by $\sqrt2$, but lets avoid that ugliness for now). Not just rotation matrices but any general complex number $a+bi$ has a matrix representation

$$
\begin{bmatrix}
    a & -b \\
    b & a
\end{bmatrix}
$$

We can verify that its determinant corresponds to the squared magnitude of the complex number and matrix multiplication corresponds to complex number multiplication. So we can establish multiplicative law for complex number magnitudes from the corresponding law for determinants. \\
i.e. 

$$
|xy| = |x||y|
$$

The fact that complex numbers multiplication corresponds to rotation in plane can also be established from its properties that the multiplication is distributive and that magnitudes get multiplied. \\
Say $u$ is a complex number with magnitude $1$ and $v$ and $w$ are any two complex numbers. \\
Then, distance between $uv$ and $uw$ is:

$$
\begin{aligned}
    &|uv-uw| \\
    &=|u(v-w)| \\
    &= |u||v-w| \\
    &= |v-w|
\end{aligned}
$$

Thus the transformation preserves distance while sending origin to itself. Also if $u \neq 1$ then no other point is stationary and the effect is thus rotation.

## Quaternions

We can now start our investigation in higher dimension by asking if we can find three dimensional numbers with these properties so that multiplication represents similar isometric transformations. It turns out no such numbers exist in three dimensions but they do exist in four dimensions.
They are called quaternions and are defined as:

$$
x = a\mathbf{1} + b \mathbf{i} + c \mathbf{j} + d \mathbf{k}
$$

where,

$$
\mathbf{1} = 
\begin{bmatrix}
    1 & 0 \\
    0 & 1
\end{bmatrix}, \quad
\mathbf{i} = 
\begin{bmatrix}
    0 & -1 \\
    1 & 0
\end{bmatrix}, \quad
\mathbf{j} = 
\begin{bmatrix}
    0 & -i \\
    -i & 0
\end{bmatrix}, \quad
\mathbf{k} = 
\begin{bmatrix}
    i & 0 \\
    0 & -i
\end{bmatrix}
$$

Note $i^2 = j^2 = k^2 = -1$ \\
and $ij =k , jk = i, ki = j$ \\
and $ij = -ji$ and so on so that multiplication is not commutative.

The basis vectors are again orthogonal to each other. The squared magnitude of the quaternion is $a^2+b^2+c^2+d^2$ which can again be shown to be equal to the determinant of the corresponding matrix. From the matrix definitions, we can also see that the multiplicative property for absolute values holds. Thus, following a derivation similar to the two dimensional case, we can conclude that multiplication by unit quaternions represents an isometry of the $\mathbb{R^4}$ space.
It still remains to be seen how to represent isometry of the $\mathbb{R}^3$ space.

### Some Identities
First, let us play around with some properties of quaternions. \\
Define the quaternion conjugate $\bar{x}$ as 
$a \mathbf{1}-b \mathbf{i} - c \mathbf{j} - d \mathbf{k}$ and call a quaternion *pure* if $a=0$. \\
Then, \\
$$
x\bar{x} = (a \mathbf{1} + b \mathbf{i} +c \mathbf{j} + d \mathbf{k})(a \mathbf{1} - b \mathbf{i} - c \mathbf{j} -d \mathbf{k}) = a^2+ b^2+c^2+d^2 = 1 
$$

for unit quaternions and 

$$
x^2 = (a \mathbf{1} + b \mathbf{i} +c \mathbf{j} + d \mathbf{k})^2 = a^2-b^2-c^2-d^2 = -|x|^2
$$

for pure quaternions.

Note that in both expansions, all the terms where the quaternion terms differ cancels with its negative counterpart. For example $bc\mathbf{ij}$ cancels with $- bc \mathbf{ij}$ in the first expansion and $bc \mathbf{ij}$ cancels with $bc \mathbf{ji}$ in the second expansion. Particularly, for the unit quaternion, the conjugate also represents its inverse. \\
We can also obtain the following for pure quaternions:

$$
\begin{alignat*}{2}
    && (a+b)^2 &= a^2 + ab + ba + b^2 \\
    \implies && -|a+b|^2 &= a^2 + ab + ba + b^2 \\
    \implies && -|a|^2 - 2 \langle a, b \rangle - |b|^2 &= -|a|^2 + ab + ba - |b|^2 \\
    \implies && ab + ba &= -2 \langle a, b \rangle
\end{alignat*}
$$

Particularly, $ab = -ba$ if $a$ and $b$ are perpendicular.

## Pure Quaternions induce Quarter Rotations

We already know that quaternions represent isometries in the $\mathbb{R}^4$ space. To somehow extract three dimensional rotation from this isometry, we will first characterize multiplication by a pure unit quaternion $\mathbf{u}$ by looking at action of $J(x) : x \mapsto \mathbf{u}x$ on different subspaces. \\
Specifically it is helpful to factor the space of quaternions $\mathbb{H}$ as:

$$
\mathbb{H} = \text{span}(\mathbf{1}, \mathbf{u}) \oplus \mathbf{u}^\perp
$$

The subspace spanned by $\mathbf{1}$ gets mapped by $J$ to the subspace spanned by $\mathbf{u}$ and vice versa. Now consider, within the pure quaternion subspace, the subspace perpendicular to $\mathbf{u}$ i.e. $\mathbf{u}^\perp$. Note that we are not including the subspace spanned by $\mathbf{1}$ in $\mathbf{u}^\perp$ for notational simplicity i.e. vectors in $\mathbf{u}^\perp$ are pure quaternions and form a two dimensional subspace. \\
For $x \in \mathbf{u}^\perp$, we can show that $\mathbf{u}x$ is still perpendicular to $\mathbf{u}$ by noting that 

$$
\mathbf{u}(\mathbf{u}x) = \mathbf{u}^2x = -x
\quad\quad\quad\quad (\mathbf{u}x)\mathbf{u} = -(xu)\mathbf{u} = x
$$

where we used $\mathbf{u}x = -x\mathbf{u}$ since $\mathbf{u}\perp x$. Thus elements in the plane $\mathbf{u}^\perp$ are mapped to itself, and furthermore the transformation is distance preserving, so that the net effect is rotation of the plane. 
To calculate the angle of rotation note that applying the left-multiplication by $\mathbf{u}$ twice i.e. 

$$
\mathbf{u}(\mathbf{u}x) = -x
$$

corresponds to $180^\circ$ rotation, so that a single multiplication must correspond to $90^\circ$ rotation. We don't know if the direction of rotation is $+90^\circ$ (anti-clockwise) or $-90^\circ$ (clockwise) rotation about $\mathbf{u}$. The direction of rotation is in fact fixed by the handedness of our co-ordinate system. To find this direction, we can simply look at the special case when $\mathbf{u} = i$ and note that $ij = k$, which means with our right-handed $ijk$ co-ordinate system, the rotation must be $+90^\circ$. Thus, the vectors $x \in \mathbf{u}^\perp$ are mapped to $J(x) \in \mathbf{u}^\perp$ with $J(x) \perp x$.

## Three Dimensional Rotations with General Quaternions

If we recall how we composed a general rotation for the two dimensional case as combination of the cosine of identity transformation and sine of $90^\circ$ rotation, we might be motivated to look at multiplication by a general unit quaternion $t = \cos\theta\mathbf{1} + \sin\theta\mathbf{u}$ where $\mathbf{u}$ is a unit pure quaternion. \\
Let us similarly look at the action of transformation $T(x) = tx$ on different subspaces. \\
For $\text{span}(\mathbf{1})$, the basis vector $\mathbf{1}$ is sent to $\cos\theta\mathbf{1}+\sin\theta\mathbf{u}$. \\
For $\text{span}(\mathbf{u})$, the basis vector $\mathbf{u}$ is sent to $-\sin\theta\mathbf{1}+\cos\theta\mathbf{u}$. \\
For $x \in \mathbf{u}^\perp$, 

$$
\begin{aligned}
    T(x) &= (\cos\theta\mathbf{1}+\sin\theta\mathbf{u})x \\
    &= (\cos\theta x+\sin\theta J(x)) \\
\end{aligned}
$$

Thus, the operation $T$ describes a rotation by $\theta$ in the $\text{span}(\mathbf{1}, \mathbf{u}$) plane and a rotation by $\theta$ in the $\mathbf{u}^\perp$ plane. This is nearly what we want. \\
The rotation by $\theta$ in the $\mathbf{u}^\perp$ plane is particularly interesting. The points in this space get rotated angle $\theta$ around the axis defined by $\mathbf{u}$. If we could do this for not just the elements of $\mathbf{u}^\perp$ but for the whole pure quaternion space: $\text{span}(\mathbf{u}) \oplus \mathbf{u}^\perp$, we will have achieved our objective (where we would interpret a three dimensional rotation axis as a pure quaternion for computation). \\
You might have already noticed that this is possible by projecting a given vector $x$ to $\mathbf{u}$ and $\mathbf{u}^\perp$, rotating the component perpendicular to $\mathbf{u}$ by left multiplication as above and adding back the component parallel to $\mathbf{u}$. Indeed this works and will give you the correct vector. But we still need a bit of work to arrive at the more commonly expressed form of quaternion multiplication for rotation.

We can begin by looking for a transformation that will cancel the effect for $\text{span}(\mathbf{u})$ while hopefully preserving the effect for $\mathbf{u}^\perp$. And indeed, with a bit of playing around one might arrive at the map 

$$
U(x) = xt^{-1} = 
x(\cos\theta\mathbf{1}-\sin\theta\mathbf{u})
$$

whose effects for the considered subspaces are: \\
For $\text{span}(\mathbf{1})$, the basis vector $\mathbf{1}$ is sent to $\cos\theta\mathbf{1}-\sin\theta\mathbf{u}$.  \\
For $\text{span}(\mathbf{u})$, the basis vector $\mathbf{u}$ is sent to $\sin\theta\mathbf{1}+\cos\theta\mathbf{u}$.  \\
For $x \in \mathbf{u}^\perp$, 

$$
\begin{aligned}
    U(x) &= x(\cos\theta\mathbf{1}-\sin\theta\mathbf{u}) \\
    &= (\cos\theta x-\sin\theta x\mathbf{u}) \\
    &= (\cos\theta x+\sin\theta \mathbf{u}x) \\
\end{aligned}
$$

where the last equality follows from $\mathbf{u}x = -x\mathbf{u}$ since $x\perp \mathbf{u}$. \\
Thus, this map rotates the $\text{span}(\mathbf{1}, \mathbf{u}$) plane by $-\theta$ i.e. in the opposite direction as $T$ and rotates the $\mathbf{u}^\perp$ plane by $\theta$ in the same direction as $T$. \\
If we compose these maps, we get $V(x) = txt^{-1}$, whose net effect is the identity transformation for $\text{span}(\mathbf{1}, u)$ and $2\theta$ rotation about the axis $\mathbf{u}$ for $\mathbf{u}^\perp$. Thus, for any vector represented as a pure quaternion $x$, it would have its $\mathbf{u}^\parallel$ and $\mathbf{u}^\perp$ components transformed so that the vector $x$ is rotated by angle $2\theta$ around the $\mathbf{u}$ axis.

## Final Algorithm
To conclude, we state the algorithm for general rotation of vector $v$ about an axis $u$ by angle $\alpha$ below:
1. Represent $v$ as a pure quaternion.
2. Define quaternion $t = \cos\frac{\alpha}{2} + \mathbf{u}\sin\frac{\alpha}{2}$ where $\mathbf{u}$ is a unit pure quaternion representation of axis $u$.
3. Compute rotated quaternion $v'=tvt^{-1}$ following the algebraic rules for quaternion multiplication.
