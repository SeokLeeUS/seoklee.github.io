---
title:  "Advanced Lane Finding"
categories: post
mathjax: true
---
<iframe src="https://giphy.com/embed/VI2y5TUvbJe8c69ZZ0" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/VI2y5TUvbJe8c69ZZ0">via GIPHY</a></p>

## Final result-Please click the thumbnail to view the video:
[![video result](https://img.youtube.com/vi/w1pMOmGl-lU/hqdefault.jpg)](https://youtu.be/w1pMOmGl-lU) 

## Braingstorming:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## Here is the [Rubric](https://review.udacity.com/#!/rubrics/571/view) points for this project.  

## Work flow

### **0. Compute the camera calibration matrix and distortion coefficients given a set of chessboard images**

import a function that takes an image, object points, and image points performs the camera calibration, image distortion correction and 
returns the undistorted image

![Camera_Calibration](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/camera_cal/corners_found8.jpg)


### **1. Apply a distortion correction to raw images.**

The code for this step is contained in the first code cell of the IPython notebook located in "./cam_cal.py.ipynb". 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result (The file for this work is 'image_gen-undistort.py.ipynb'): 

![Camera_Calibration](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/test_images/undistort0.jpg)

### **2. Use color transforms, gradients, etc., to create a thresholded binary image.**

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![original image](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/test_images/undistort2.jpg)

### **3. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.**

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines # through # in `image_gen-color_gradient.py.ipynb`).  Here's an example of my output for this step. 

![color_gradient](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/test_images/color_gradient2.jpg)

### **4. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.** 

The code for my perspective transform calls 'getPerspectiveTransform' function from 'cv2. The function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
 src = np.float32([[img.shape[1]*(.5-mid_width/2),img.shape[0]*height_pct],[img.shape[1]*(.5+mid_width/2),img.shape[0]*height_pct],[img.shape[1]*(.5+bot_width/2),img.shape[0]*bottom_trim],[img.shape[1]*(.5-bot_width/2),img.shape[0]*bottom_trim]])
    dst = np.float32([[offset, 0], [img_size[0]-offset, 0],[img_size[0]-offset, img_size[1]],[offset, img_size[1]]])
```
### **5. Apply a perspective transform to rectify binary image ("birds-eye view").**

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.The associated file is 'image_gen-perspective_transform.py.ipynb'

![perspective_transform](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/test_images/perspective_transform2.jpg)

### **6. Detect lane pixels and fit to find the lane boundary.**

Then found the lane line('image_gen-identify_lane_finding.py.ipynb) with a 2nd order polynomial ('image_gen-identify_lane_finding_polynominal.py.ipynb') this:

![lanefinding](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/test_images/identify_lane_finding2.jpg)
![lanefinding_poly](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/test_images/identify_lane_finding_polyfit2.jpg)

### **7. Determine the curvature of the lane and vehicle position with respect to center.**

I did this @ my code ( `image_gen-Camera_Center_Cal_Curvature.py.ipynb`)

![Cal_Curvature](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/test_images/cal_curvature2.jpg)

### **8. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.**

Here is an example of my result on a test image:

![final](https://github.com/SeokLeeUS/seokleeus.github.io/raw/master/_images/_advanced_lane/test_images/final2.jpg)

---

### **9. Result** 

#### This video shows the result of detecting the lane boundaries and numerical estimation of lane curvature and vehicle position. Please click the thumbnail:

[![video result](https://img.youtube.com/vi/w1pMOmGl-lU/hqdefault.jpg)](https://youtu.be/w1pMOmGl-lU) 

---

### **10. Discussion**

- This is a truely challenging task. 
In order to tackle this various sources were referred:

  - Python classes: the following on-line classes helped:
    >[Reference#1](https://www.udemy.com/complete-python-bootcamp/)
    >
    >[Reference#2](https://www.coursera.org/specializations/python)

  - Python book:
learn python 3.0 visually

  - Several other learning materials:
    >[Reference#3-Korean only](https://wikidocs.net/book/110)
    >
    >[Reference#4](https://www.youtube.com/playlist?list=PLEA1FEF17E1E5C0DA)

- Tried to implement magnitude & directional gradient to detect edge along with sobel/ color thresholding, but the result was not promissing, therefore, I used only sobel/color threshold based edge detection. 

- Learning progress(C/Python/Tensorflow/Keras): 

  >[LearningProgress-google doc](https://docs.google.com/spreadsheets/d/1ZMtaS0Ifh5b9AcZpMV0RAKk8vmG7To65acA2ZQdAIHE/edit?usp=sharing)

- I watched [Udacity video session](https://www.youtube.com/watch?v=vWY8YUayf9Q&feature=youtu.be) for an examplary work. 




