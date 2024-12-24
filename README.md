# Multi-View 3D Reconstruction

## Table of Contents
- [Introduction](#introduction)
- [Highlights](#highlights)
- [Installation](#installation)
- [Dataset Setup](#dataset-setup)
- [Usage](#usage)
  - [Running the Reconstruction Pipeline](#running-the-reconstruction-pipeline)
- [Process Overview](#process-overview)
- [Dependencies](#dependencies)
- [Results](#results)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## Introduction
This repository demonstrates how to reconstruct a 3D point cloud from multiple 2D images taken from various viewpoints. The system employs feature detection, camera pose estimation, triangulation, and point cloud visualization to form an accurate 3D representation of a scene.

**Main Features:**
- **Robust Feature Detection & Matching**  
  Uses SIFT (Scale-Invariant Feature Transform) and FLANN for dependable correspondence finding.
- **Camera Pose Estimation**  
  Derives rotation and translation between image pairs using the Essential Matrix.
- **3D Triangulation**  
  Transforms matched 2D points from multiple viewpoints into 3D coordinates.
- **Interactive Visualization**  
  Offers real-time 3D point cloud visualization with color information via Open3D.
- **Export Functionality**  
  Saves the reconstructed scene as a PLY file, making it compatible with popular 3D editing tools.

## Installation

### Prerequisites
- **Python 3.7+**  
- **Git** (for cloning this repository)

### Steps

1. **Clone the Repository**  
   ```bash
   git clone https://github.com/arjun007-007/Multi-View-3D-Reconstruction.git
   cd MultiView-3D-Reconstruction
   ```

2. **Optional: Create and Activate a Virtual Environment**  
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # Windows: venv\Scripts\activate
   ```

3. **Install Requirements**  
   ```bash
   pip install -r requirements.txt
   ```

---

## Dataset Setup
1. **Images (Data/)**  
   - Provide a set of images covering various viewpoints of the same scene.  
   - Ensure sufficient overlap between successive images to aid feature matching.  
   - Maintain consistent lighting to improve detection and matching.

2. **Camera Intrinsics (K.txt)**  
   - A file specifying the 3×3 intrinsic matrix of the camera used.  
   - The matrix should match the camera settings (focal length, principal point).

   **Example:**
   ```
   718.8560  0.0000    607.1928
   0.0000    718.8560  185.2157
   0.0000    0.0000    1.0000
   ```

---

## Usage

### Running the Reconstruction Pipeline
1. **Navigate to Project Root**  
   ```bash
   cd MultiView-3D-Reconstruction
   ```

2. **Execute the Script**  
   ```bash
   python Wrapper.py
   ```
   - This processes the image sequence, recovers camera motion, and triangulates points.

3. **Observe Terminal Logs**  
   - Monitors the reconstruction process, displaying stats on feature matching, camera pose recovery, and 3D points generated.

4. **Interact with the Point Cloud**  
   - An Open3D viewer opens, allowing you to navigate the reconstructed 3D model (zoom, pan, rotate).

5. **Save the Result**  
   - The final point cloud with color is stored in `output_with_colors.ply`.  
   - This file is compatible with MeshLab, CloudCompare, or other 3D software.

**Example Command:**
```bash
python Wrapper.py
```
**Example Output:**
```
Intrinsic Matrix K:
[[718.856   0.     607.1928]
 [  0.     718.856 185.2157]
 [  0.       0.       1.    ]]

Processing image pair: 1 & 2
Total matches found: 1500
Good matches after ratio test: 1200
Camera pose recovered.
Triangulation completed.
...
Point cloud saved to output_with_colors.ply
```

---

## Process Overview
1. **Image Acquisition**  
   - Loads the image sequence and intrinsic camera parameters.  

2. **Feature Detection & Matching**  
   - Applies SIFT to detect keypoints and descriptors.  
   - Uses FLANN matching with Lowe’s ratio test to keep only reliable matches.

3. **Pose Computation**  
   - Computes the Essential Matrix with RANSAC to handle outliers.  
   - Decomposes the Essential Matrix to obtain camera rotation and translation.

4. **Triangulation**  
   - Constructs projection matrices for each view.  
   - Converts matched 2D keypoints into 3D coordinates.

5. **Pose Refinement (Optional)**  
   - Integrates solvePnP to refine camera parameters.  
   - For extended accuracy, advanced bundle adjustment tools (e.g., Ceres, g2o) could be integrated.

6. **3D Visualization & Output**  
   - Combines all 3D points into a point cloud, mapping original image colors.  
   - Displays an interactive viewer with Open3D.  
   - Exports the resulting cloud in PLY format.

---

## Dependencies
- **NumPy** for matrix operations  
- **OpenCV** for feature detection, matching, and image processing  
- **Open3D** for 3D visualization  
- **Matplotlib** for plots (optional)  
- **Glob, Re** for file pattern matching and calibration file parsing  

Install them all via:
```bash
pip install -r requirements.txt
```

---

## Results
By the end of the pipeline, a dense 3D point cloud of the scene is visualized and saved. The reconstructed output provides insights into the spatial arrangement of the scene captured in the input images.
