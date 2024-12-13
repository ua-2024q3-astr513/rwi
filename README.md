# Context
Protoplanetary disk observations reveal a variety of substructures, including spirals, rings, gaps, and non-axisymmetric features, particularly in submillimeter dust observations. Planet-disk interactions are among the leading theories to explain these structures, especially the clumps within rings, which are often interpreted as vortices.
Planets can induce spiral waves that transport energy and angular momentum. If these spirals are sufficiently strong, they can carve gaps in the disk, creating favorable conditions for Rossby Wave Instability (RWI) at one of the gap edges. However, it is challenging to disentangle the mechanisms behind RWI formation, whether it arises purely due to the density profile shape or is influenced by the excitation of spiral waves.
Linear theory provides a framework to address this question by examining bump-like density profiles, typically sculpted by planets in protoplanetary disks. It helps assess whether these profiles are sufficient to trigger RWI under numerical perturbations.

# Code Structure and Description

This repository contains tools to study Rossby wave instabilities (RWI) in protoplanetary disks. Below is a detailed explanation of the code structure and its functionalities.

---

## Functionality Overview

### Derivative Calculation
- **Function**: `derivate(f_r, dr, nx, nghost, dx, order=3)`
  - Implements a five-point stencil finite-difference scheme to compute numerical derivatives with high accuracy.

### Grid Construction
- **Function**: `add_ghost_cells(r, nx, nghost, dx)`
  - Adds `nghost` ghost cells to the boundaries of a linearly spaced grid. A minimum of 3 ghost cells is recommended to properly implement the five-point stencil scheme.

---

## Key Sections

### 1. Surface Density
Defines the bump surface density profile, based on the ring and gap edges where RWI can be observed in protoplanetary disks.

---

### 2. Frequencies and Sound Speed
Calculates:
- The disk’s frequency, incorporating Keplerian contributions and pressure gradients.
- The epicyclic frequency.
- The sound speed for a non-barotropic disk.

---

### 3. Finite Difference Matrices
- Discretizes the linear problem using finite-difference matrices.
- A three-point stencil is used for first- and second-order derivatives, differing from the five-point stencil applied elsewhere.

---

### 4. Second-Order Equation
- Constructs a matrix representing the system, incorporating the unknown eigenfrequency.
- The matrix is complex, and boundary conditions enforce zero radial velocity at the inner and outer edges.

---

### 5. Determinant of the Tridiagonal Matrix
- The problem solution involves resolving the determinant of the complex second-order matrix.
- Thanks to its tridiagonal structure, an iterative method is used, which scales linearly with grid resolution. This is more efficient compared to the cubic scaling of general matrix determinant calculations.

---

### 6. Alpha Constant
- **Alpha** is a constant used to scale the determinant and avoid numerical overflow. It ensures the determinant approaches 1 for numerical stability.
- The solution is unaffected by this scaling, as roots occur when the determinant is zero, independent of the alpha value.

---

### 7. Determinant Map
- Since eigenfrequencies involve two unknowns, a 2D determinant map is created to cover all possible root locations.

---

### 8. Muller's Method
- A variant of the Secant or Newton-Raphson method is used to quantitatively find roots.
- Muller's method is chosen because it can handle complex-valued inputs and outputs.

---

### 9. Tracking the Solution
- Gravitational instability may trigger multiple modes, resulting in several solutions. 
- The mode with the largest growth rate dominates in the linear regime, as perturbations grow exponentially over time. This mode is crucial for comparison with simulations.

---

### 10. Velocities
- The linear equations also provide first-order complex equations for radial and azimuthal velocities.
- Using the surface density eigenvector, the three eigenvectors (surface density, radial velocity, azimuthal velocity) are computed.

---

### 11. 2D Visualization
To better understand the instability behavior and eigenmodes, the eigenvectors are extended into a 2D representation for visualization.

---
