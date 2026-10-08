# IMAGE-TRANSFORMATION-DECOMPOSER
Image Transformation Decomposer (B10)
B.Tech Mathematics Mini Project — Eigenvalues, eigenvectors and the geometric meaning of a 2×2 matrix.
Problem statement
Given a 2×2 transformation matrix, find its eigenvalues and eigenvectors. Decompose it into a rotation part + stretching part. Show that along eigenvector directions there is only stretching (no rotation). Demonstrate with an image being transformed.
Real-world context
Computer graphics: every game engine and animation system applies 2×2 / 3×3 linear maps to sprites and meshes. Splitting a map into rotation and stretch is how engines interpolate transforms smoothly and extract "angle" and "scale" from a matrix.
Mathematics used (nothing invented)
Eigenproblem: A v = λ v. Eigenvalues are roots of det(A − λI) = 0, i.e. λ² − tr(A)·λ + det(A) = 0. Real roots iff tr² − 4·det ≥ 0.
Eigenvectors: non-zero solutions of (A − λI)v = 0. Because A v = λ v, the vector Av lies on the same line as v → only stretching (×λ), no rotation (if λ < 0 the direction is flipped by 180°, still the same line).
Rotation × stretch decomposition (stated assumption): interpreted as the polar decomposition A = R·S
S = √(AᵀA) — symmetric, positive semi-definite → pure stretch along perpendicular axes
R = A S⁻¹ — orthogonal (RᵀR = I) → rotation (a reflection if det A < 0)
computed from the SVD A = UΣVᵀ: R = UVᵀ, S = VΣVᵀ.
How the two views fit together: for an eigenvector v, S turns it by −θ and R turns it back by +θ; the net turn is 0° and only the scale λ remains. For ordinary directions the net turn is non-zero.
Image warping: inverse mapping — each output pixel p samples the source at A⁻¹p with bilinear interpolation (no holes).
Caveat: the eigenvectors of A coincide with the stretch axes of S only when A is symmetric (then R = I). In general they are different directions — the notebook shows exactly this.
Files
File
Purpose
Image_Transformation_Decomposer.ipynb
The complete Colab notebook
README.md
This file
How to run (Google Colab)
Open https://colab.research.google.com → File → Upload notebook → choose the .ipynb.
Edit Cell 2 (A_INPUT, optional IMAGE_PATH).
Runtime → Run all. No installs needed (NumPy, SymPy, Matplotlib, SciPy, pandas are preinstalled).
To use your own photo: upload it in Colab's file panel and set IMAGE_PATH = "/content/photo.jpg".
Notebook structure
Cell
Content
1
Imports
2
Inputs / configuration (matrix, image)
3
Math engine (eigen-analysis, polar decomposition, direction test, image warp)
4
Step-by-step results as formatted text and tables
5
Charts
6
Benchmark test dataset with hand-derived ground truth
Charts produced
Image story: original → stretch S → rotate R (= A), with eigenvectors tracked through each stage.
Unit circle → ellipse: eigenvector arrows stay on their lines, an ordinary vector turns.
Rotation profile: rotation caused by A for every direction θ; the zero crossings are exactly the eigenvector directions.
Default example result
A = [[2, −0.5], [0.25, 1]]
Quantity
Value
Characteristic equation
λ² − 3λ + 17/8 = 0
Eigenvalues
3/2 ± √2/4 ≈ 1.8536, 1.1464
Eigenvectors (unit)
(0.9597, 0.2811), (0.5054, 0.8629)
Rotation part R
14.04°
Stretch factors (σ)
2.0616, 1.0308
Net turn along eigenvectors
0.0000°
Net turn of a non-eigenvector (111°)
33.51°
Benchmark dataset
Eight matrices with hand-derived eigenvalues: pure scaling, symmetric stretch, triangular, reflection, 90° rotation, 45° rotation × √2, shear (defective), and the default graphics case. Each is checked for: (a) eigenvalues match, (b) R·S = A and RᵀR = I, (c) zero net rotation along eigenvectors. The notebook asserts all pass.
Special cases handled
Complex eigenvalues (rotation-like matrices): reported; no real eigenvector exists, every direction is rotated.
Repeated eigenvalue / shear: detected as defective (only one eigenvector).
Negative eigenvalue: direction flip shown as 180°, still "no rotation".
det A < 0: R flagged as containing a reflection.
Singular A: rejected with a clear message.
Limitations / future work
2×2 linear maps only (no translation); 3×3 homogeneous coordinates would add translation.
Polar decomposition is one standard meaning of "rotation + stretch"; the eigen-decomposition A = PDP⁻¹ is the alternative (skewed axes).
Possible extensions: slider animation with ipywidgets, 3D rotation + stretch, SVD of images.
References
G. Strang, Introduction to Linear Algebra (eigenvalues, SVD, polar decomposition).
Horn & Johnson, Matrix Analysis (polar decomposition).
