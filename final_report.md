**Advanced Lane Finding Project**

[//]: # (Image References)

[undistortion]: ./output_images/undistortion.png "undistortion"
[perspective]: ./output_images/perspective.png "perspective"



### Submission files/ Writeup

You're reading the writeup! and here is a link to my 

[project code](https://github.com/purnendu23/Lane-Detection-Advanced/blob/master/laneDetect.ipynb)

### Camera Calibration and Undistortion

I find the camera matrix `mtx` and distortion coeffecients `dist` using `calibrateCam`(cell: 2). I then use these values to find undistorted image by using `undistortImage` (cell: 2). 
### Pipeline (single images)

My pipeline consists of the following steps:
1. Undistortion
2. Applying Gradient Threshold
3. Applying Color Threshold
4. Combining thresholds
5. Perspective transform to get top-down view

#### 1. Provide an example of a distortion-corrected image.

The following figure shows the results of undistortion function used on an image: (refer Cell: [7])

![undistortion][undistortion]


#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I define `applyGradientThresh(image)` and `applyColorThresh(image)` to apply gradient and color thresholds to the image.

`combined[((gradx == 1) & (grady == 1)) | ((mag_binary == 1) & (dir_binary == 1))] = 1`

gives the gist of my gradient-threshold technique. 

`combined[  ((s_binary == 1) & (h_binary == 1)) | (r_binary == 1) ] = 1`

gives the gist of my color-threshold technique.
Both the functions are defined in cell: [2]

I combine the binary images from both these functions in the following step:

`combined[ (t1 == 1) & (t2 == 1)  ] = 1`      where t1: gradient-binary,  t2: color-threshold


#### 3. Perspective transform

I use `cv2.getPerspectiveTransform` and `cv2.warpPerspective` inside the `warpImage` function in cell: [2] which returns the warped image. I use this function in cell: [8] to show example of undistortion:

![perspective][perspective]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:



#### 5. Radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Drawn lane on image
I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:



---

### Pipeline (video)


---

### Discussion

#### 1. Problems / issues faced.  Where will your pipeline likely fail?  What could you do to make it more robust?

