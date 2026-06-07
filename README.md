# Computer Vision Feature Extraction and Matching Pipeline

A high-performance, native C++ desktop application engineered using the Qt framework to evaluate, benchmark, and visualize fundamental computer vision algorithms. This project implements low-level image processing pipelines from mathematical primitives—including Harris Corner Detection, Scale-Invariant Feature Transform (SIFT) descriptors, and multi-metric feature matching (SSD/NCC)—designed for real-time performance tracking and robotics-related spatial tracking tasks.

---

## Technical Pipeline Architecture

The application enforces a modular separation of concerns, decoupling pure algorithmic matrix manipulations from the Qt GUI rendering thread to ensure fluent interface updates during heavy computational loads.

```text
+-------------------------------------------------------------------------+
|                                QT GUI LAYER                             |
|          (Image Canvas Renderers, Metric Selection, Live Timers)        |
+-------------------------------------------------------------------------+
                                    |
                                    v
+-------------------------------------------------------------------------+
|                     ALGORITHMIC CORE PIPELINE (C++)                     |
|   [Harris Operator]  -->  [SIFT Descriptor]  -->  [Matching Engine]    |
+-------------------------------------------------------------------------+
                                    |
                                    v
+-------------------------------------------------------------------------+
|                       MATHEMATICAL PRIMITIVES                           |
|        (Gaussian Blurring, Image Gradients, Eigenvalue Solvers)         |
+-------------------------------------------------------------------------+

```

### 1. Core Algorithmic Capabilities

* **Harris Corner Detection Subsystem:** Identifies highly distinct structural keypoints by computing localized image gradient distributions ($I_x, I_y$). The detector constructs the second-moment matrix (Structure Tensor) for every pixel, utilizing $\lambda$ eigenvalue analysis and a tunable sensitivity scoring parameter ($R$) to isolate robust corners while suppressing edge responses.
* **Scale-Invariant Feature Transform (SIFT):** Extracts robust scale-space keypoints and computes local descriptor orientations. The pipeline constructs a Difference-of-Gaussians (DoG) pyramid to establish scale invariance, generating a 128-dimensional descriptor vector for each keypoint based on localized gradient orientation histograms, ensuring immunity to rotation, illumination shifts, and scaling mutations.
* **Descriptor Matching Engine:** Implements dual-metric nearest-neighbor matching profiles to map corresponding keypoints across disparate image viewpoints:
* **Sum of Squared Differences (SSD):** An intensity-dependent distance metric optimized for computational speed.
* **Normalized Cross-Correlation (NCC):** An illumination-invariant matching metric that normalizes local patch intensities, ensuring reliable tracking under varied lighting vectors.


* **Performance Benchmarking Suite:** Embeds precise microsecond-grade software execution timers around each core algorithmic pass, providing real-time latency diagnostics to measure algorithmic complexity directly within the user interface.

---

## Application Output Gallery

### 1. Mathematical Analysis & Corner Detection

* **Eigenvalue ($\lambda$) Analysis Map:** Visualizing the distribution of eigenvalues to differentiate between flat regions, edges, and distinct corner responses.

* **Harris Corner Detection Output:** Final extraction of structural corners and unique geometric vertices overlayed on the source image.


### 2. SIFT Feature Extraction & Description

* **SIFT Scale-Space Keypoints:** Isolating scale-space stable keypoints and local orientation descriptors across the Gaussian scale pyramid.


### 3. Descriptor Matching Performance

* **Sum of Squared Differences (SSD) Matching:** Establishing spatial correspondences between keypoints using intensity distance minimization.

* **Normalized Cross-Correlation (NCC) Matching:** Robust illumination-invariant matching utilizing zero-mean normalized correlation fields.


---

## Key Engineering Standards Applied

* **Object-Oriented Image Processing Design:** Avoids monolithic design patterns by encapsulating individual algorithms into dedicated C++ translation units, allowing runtime substitution of feature extraction engines.
* **Deterministic Memory Management:** All image transformation buffers, gradient maps, and descriptor arrays utilize scope-bound memory allocation profiles to eliminate memory leaks during high-frequency real-time execution.
* **Illumination-Invariant Normalization:** The custom NCC matching layer implements rigorous zero-mean standardization across sub-matrix fields, preventing image exposure variations from corrupting tracking accuracy.
* **Clean Toolchain Compliance:** The project builds cleanly via CMake using strict configuration warning flags (`-Wall -Wextra`), optimizing the underlying matrix structures for low runtime overhead.

---

## Repository Directory Tree

```text
project5-cv-feature-matching/
├── CMakeLists.txt                 # Master cross-platform build automation definitions
├── cornerdetector.cpp             # Gradient tracking, tensor analysis, and corner suppression
├── cornerdetector.h               # Structure tensor and eigenvalue scoring declarations
├── feature_matching.cpp           # Multi-metric vector mapping loops (SSD and NCC)
├── feature_matching.h             # SSD and NCC distance evaluation classes
├── main.cpp                       # Master application initialization entrypoint
├── mainwindow.cpp                 # Image loading, UI event handling, and benchmarking routes
├── mainwindow.h                   # GUI slot connections, canvas configurations, and timers
├── mainwindow.ui                  # Qt Designer graphical layout blueprint
├── siftdescriptorextractor.cpp    # Scale-space pyramid execution and orientation histograms
├── siftdescriptorextractor.h      # SIFT keypoint selection and 128-D descriptor declarations
└── assets/                        # Execution logs and output image assets (lambda, harris, etc.)

```

---

## Toolchain Setup and Deployment

### Prerequisites

* Build Suite: CMake (Version 3.5 or higher).
* UI Framework: Qt Creator / Qt5 or Qt6 Development Packages.
* Toolchain: Modern C++ Compiler (GCC, Clang, or MSVC supporting C++11 or higher).

### Build Pipeline

1. Clone the repository and its structural directory layout:
```bash
git clone git@github.com:lyan2003/Modern-CPP-Computer-Vision-Feature-Extraction-and-Matching.git

```


2. Navigate to the project root and generate the build tree:
```bash
mkdir build && cd build
cmake ..

```


3. Compile the executable using the native build tools:
```bash
cmake --build .

```


4. Run the generated UI executable artifact:
```bash
./CVFeatureMatcherApp

```
