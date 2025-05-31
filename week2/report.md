# Medical Imaging Visualization and Orientation Analysis Report

## 1. Description of the Approach

This project involved reading, preprocessing, and interactively visualizing 3D medical imaging data in both DICOM and NIfTI formats using Python. The goal was to understand spatial orientation, correct visual orientation issues, and compare how both formats handle orientation metadata.

### Step-by-step Approach:

- **Step 1: Data Loading**
  - NIfTI (.nii, .nii.gz) files were loaded using the `nibabel` library.
  - DICOM files (a folder of `.dcm` slices) were read using `pydicom`. Slices were sorted by `ImagePositionPatient` to construct a 3D volume.

- **Step 2: Preprocessing**
  - For DICOM, pixel data from individual slices was extracted and stacked.
  - Orientation was initially inferred from image shape and visual inspection.
  - Applied rotations or transpositions (using `np.rot90` or `np.transpose`) to correct apparent flips or misalignments.

- **Step 3: Visualization**
  - A custom interactive viewer was created using `ipywidgets` and `matplotlib`.
  - Users could scroll through:
    - Axial (XY, along Z-axis)
    - Coronal (XZ, along Y-axis)
    - Sagittal (YZ, along X-axis) planes.
  - The viewer helped verify correct anatomical orientation.

- **Step 4: Orientation Verification**
  - Used affine matrices (for NIfTI) and orientation tags (for DICOM) to verify the correctness of spatial orientation.
  - Compared metadata-derived orientation to visual output.

---

## 2. Screenshot of the Visualization

A single screenshot was captured showing the output of the interactive viewer. It includes three panels:

- Left: Axial view
- Middle: Coronal view
- Right: Sagittal view

This image confirms the ability to scroll through and interpret each anatomical plane.
![image](https://github.com/user-attachments/assets/3797b1cd-04c7-4856-b537-dd468aaa7d66)


---

## 3. Preprocessing Notes and Assumptions

- Assumed that NIfTI files are aligned using RAS+ orientation by default.
- For DICOM files:
  - Slices were sorted using `ImagePositionPatient` to reconstruct the 3D volume.
  - `ImageOrientationPatient` was used to infer the orientation of the scan.
- Pixel spacing was extracted from `PixelSpacing` and `SliceThickness`.
- In some cases, manual rotation using `np.rot90` was needed to fix mirrored images.
- Assumed consistent orientation across all slices within a DICOM series.

---

## 4. Observations and Challenges Encountered

- **Incorrect Visual Orientation**:
  - In some cases, images appeared flipped or mirrored. This was corrected using transpositions and rotations.
- **Inconsistent Orientation Handling**:
  - DICOM files often required more manual effort to interpret orientation correctly.
  - NIfTI affine matrices made orientation interpretation easier and more standardized.
- **Data Format Complexity**:
  - DICOM involves multiple files, which increases complexity.
  - NIfTI offers a single-file solution which is easier to manage.
- **Tooling Support**:
  - Python tools like `nibabel` are optimized for NIfTI.
  - DICOM requires careful handling with `pydicom`, and additional work to compute slice ordering and spacing.

---

## 5. How to Interpret Image Orientation

### Interpreting NIfTI Orientation Using the Affine Matrix

NIfTI files store a **4x4 affine transformation matrix** that maps voxel indices (i, j, k) in image space to real-world spatial coordinates (x, y, z), usually in millimeters. This affine matrix encodes:

- The direction each voxel axis points in physical space (e.g., Left-to-Right, Posterior-to-Anterior, Inferior-to-Superior).
- The spacing between voxels along each axis.
- The position of the image origin in physical space.

To interpret orientation:

- Use `nibabel`'s utility function:

  ```python
  import nibabel as nib
  nifti_img = nib.load("example.nii.gz")
  print(nib.aff2axcodes(nifti_img.affine))
  ### Interpreting DICOM Orientation Using Orientation Tags

DICOM stores orientation information in metadata tags such as:

- **ImageOrientationPatient (0020,0037):** Gives the direction cosines of the image rows and columns in the patient coordinate system.
- **ImagePositionPatient (0020,0032):** Gives the 3D coordinate of the upper-left corner of the image slice.

To interpret DICOM orientation and reconstruct the 3D volume, you can:

```python
import pydicom
import numpy as np

# Load a single DICOM slice
ds = pydicom.dcmread("path_to_slice.dcm")

# Extract orientation vectors
orientation = ds.ImageOrientationPatient  # list of 6 floats
row_cosines = np.array(orientation[:3])
col_cosines = np.array(orientation[3:6])

# Extract position of the slice origin
position = np.array(ds.ImagePositionPatient)

print("Row direction cosines:", row_cosines)
print("Column direction cosines:", col_cosines)
print("Image position (slice origin):", position)

## Loading and Reading Medical Imaging Files: NIfTI vs DICOM


| Aspect                | NIfTI                                      | DICOM                                                   |
|-----------------------|--------------------------------------------|--------------------------------------------------------|
| **Typical File Format**| Single file (.nii or .nii.gz)               | Multiple files (one `.dcm` file per slice)              |
| **Library Used**       | `nibabel`                                  | `pydicom`                                              |
| **Loading Code**       | `nib.load('file.nii.gz')`                   | `pydicom.dcmread('slice.dcm')`                         |
| **Data Access**        | `img.get_fdata()` returns NumPy array      | `ds.pixel_array` for 2D slice                          |
| **Handling Volume**    | 3D or 4D data in single file                | Load multiple slices, sort by position, then stack    |
| **Orientation Info**  | Affine matrix in header (`img.affine`)      | Orientation tags: `ImageOrientationPatient`, `ImagePositionPatient` |
| **Ease of Use**        | Simpler, fewer steps                        | Requires manual sorting and stacking                    |

---


