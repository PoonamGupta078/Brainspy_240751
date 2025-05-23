# Brainspy_assignment1_
# Image Processing (CNN) Report

## Introduction

This document summarizes the image processing experiment I conducted to explore noise removal and edge detection. I experimented with Gaussian blurring and compared the Sobel and Scharr edge detection operators to evaluate the clarity and quality of the resulting images.

---

## Step 1: Image Loading

The process began with loading a image  the input Then i convert into grayscale for further analysis. This base image was used throughout all steps to maintain consistency in comparison.

---

## Step 2: Manual Convolution with Gaussian Blur

To reduce noise, I applied Gaussian blur manually using a convolution-like approach(kernel + padding), similar to the initial layers in convolutional neural networks (CNNs).

### Observations:
- The Gaussian blur helped reduce noise significantly.
- When I used a higher degree of blur, the image became too smooth, and some important features were lost.
- As I reduced the amount of blur, the image retained more details and edges, but small noise artifacts became visible again.

---

## Step 3: Comparing Sobel and Scharr Edge Detection

After preprocessing the image with Gaussian blur, I applied both Sobel and Scharr operators for edge detection to compare their effectiveness.

### Observations:
- The Sobel operator produced visually more appealing and cleaner edge maps.
- The Scharr operator, while sensitive to slight gradients, sometimes produced noisy results, especially when the image was not sufficiently blurred.
- Overall, Sobel gave better results for the given task due to its balanced sensitivity and noise tolerance.

---

## Step 4: Tuning the Gaussian Blur

I experimented with decreasing the intensity of the Gaussian blur to see how it affected the edge detection process.

### Observations:
- Reducing the blur made the images clearer and revealed weaker edges that were not visible earlier.
- However, a side effect was the appearance of very thin, duplicate edges in many regions.
- These extra edges cluttered the image and could be misleading in further analysis.

---

## Conclusion

- Gaussian blur plays a crucial role in balancing noise reduction and detail preservation.
- The Sobel operator is preferable over Scharr in scenarios with moderate blurring, offering cleaner and more consistent edges.
- Lower levels of blurring help recover fine features but introduce thin duplicate edges as a drawback.
- Future improvements could involve refining edge maps through post-processing to remove these duplicates or dynamically adjusting blur based on image content.

