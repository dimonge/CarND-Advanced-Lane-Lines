
# Advanced Lane Finding Project

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_files/calibrated_image.png "Undistorted"
[image2]: ./output_files/undistorted_test_image.png "Road Transformed"
[image3]: ./output_files/color_threshold_image.png "Binary Example"
[image4]: ./output_files/warped_image.png "Warp Example"
[image5]: ./output_files/curvature_image.png "Fit Visual"
[image6]: ./output_files/output_test_image.png "Output"
[video1]: ./project_video_result.mp4 "Video"

## Camera Calibration

## 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell 1 - of the IPython notebook located in "./Advance Lane line project.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

I applied the distortion correction to the following test image:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image in code cell No: 6 in the IPython notebook.  Here is the output of the image.
![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective_transform()`which is in the code cell No:- 7 in the IPython notebook.  The `perspective_transform()` function takes as inputs an image (`image`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32([
    [[610, 450]],
    [[680, 450]],
    [[image_shape[0]-300, 680]],
    [[380, 680]]
    ])
dst = np.float32(
    [offset, 0],
    [image_shape[0]-offset, 0],
    [image_shape[0]-offset, image_shape[1]],
    [offset, image_shape[1]]])
```
I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I applied the convolution which maximize the number of hot pixels in each window in the image and fit my lane lines with a 2nd order polynomial kinda like this:


![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I applied the radius of curvature equation used in the lesson and the code can be found in the cell No:- 17 in the IPython notebook. The output lane lines on the test image is as follows: 

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

The test image with and without the identified lane area on the road:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_result.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

My pipeline likely failed to identify the curves currently, and applying a robust source and destination point in the perspective transform would help identify the lane line better.
