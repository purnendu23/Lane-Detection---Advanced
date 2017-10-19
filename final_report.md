# **Advanced Lane Finding Project**

[//]: # (Image References)

[undistortion]: ./output_images/undistortion.png "undistortion"
[perspective]: ./output_images/perspective.png "perspective"
[Undistorted-Thresholded-Warped]: ./output_images/Undistorted-Thresholded-Warped.png "Undistorted-Thresholded-Warped"
[sliding_window]: ./output_images/sliding_window.png "sliding_window"
[concise]: ./output_images/concise.png "concise"
[drawlane.png]: ./output_images/drawlane.png "drawlane"

### Submission files/ Writeup

You're reading the writeup! and here is a link to my 

[project code](https://github.com/purnendu23/Lane-Detection-Advanced/blob/master/laneDetect.ipynb)

### Camera Calibration and Undistortion

I find the camera matrix `mtx` and distortion coeffecients `dist` using `calibrateCam`(cell: 2). I then use these values to find undistorted image by using `undistortImage` (cell: 2). 
### Pipeline (single images)

My pipeline consists of the following steps:
* Undistortion
* Applying Gradient Threshold
* Applying Color Threshold
* Combining thresholds
* Perspective transform to get top-down view


##### 1. Provide an example of a distortion-corrected image.

The following figure shows the results of undistortion function used on an image: (refer Cell: [7])

![undistortion][undistortion]


##### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I define `applyGradientThresh(image)` and `applyColorThresh(image)` to apply gradient and color thresholds to the image.

`combined[((gradx == 1) & (grady == 1)) | ((mag_binary == 1) & (dir_binary == 1))] = 1`

gives the gist of my gradient-threshold technique. 

`combined[  ((s_binary == 1) & (h_binary == 1)) | (r_binary == 1) ] = 1`

gives the gist of my color-threshold technique.
Both the functions are defined in cell: [2]

I combine the binary images from both these functions in the following step:

`combined[ (t1 == 1) & (t2 == 1)  ] = 1`      where t1: gradient-binary,  t2: color-threshold


##### 3. Perspective transform

I use `cv2.getPerspectiveTransform` and `cv2.warpPerspective` inside the `warpImage` function in cell: [2] which returns the warped image. I use this function in cell: [8] to show example of undistortion:

![perspective][perspective]

I test the pipeline on all test images to get Undistorted-Thresholded-Warped image as output of the pipeline. The following figure shows this:

![Undistorted-Thresholded-Warped.png][Undistorted-Thresholded-Warped]


##### 4. Identifying lane-line pixels and polynomial-fit

The next step is to feed this binary(Undistorted-Thresholded-Warped) image to a function which identifies lane-line pixels and returns a second degree polynomial fit for each side of the lane (left line and right line).
I have defined two functions for this:

`identifyLanes_slidingWindow` cell: [15] and `identifyLanes_concise` cell: [28]. The former uses sliding window approach to identify the lane points, where as the latter uses a previous polynomial- fit from the last image to find x-coordinates of points in the vicinity of new (to-be-found) polynomial fit. It then puts the `nonzero()` condition left and right line pixels.
Finally, fits a 2-degree polynomial and returns this new fit.

The output from both these methods are shown are here:

![sliding_window][sliding_window]

![concise][concise]


##### 5. Radius of curvature of the lane and the position of the vehicle with respect to center.

The calculation of radius of curvature and the position of vehicle are done inside the `class Line` as member funtions:
`self.set_dist_from_center()` and `self.calc_curvature()` in cell:[37].

I use `self.best_fit` at that time to calculate the radius of curvature.

##### 6. Drawn lane on image
The functions `draw_lane` draws the lane back to the original image. It is borrowed from the lesson# 36 (tips and tricks). (cell: [33]. Here is the output on a sample image:

![drawlane][drawlane]

---

### Pipeline (video)

Here is the final [project video](https://github.com/purnendu23/Lane-Detection-Advanced/blob/master/project_video_output.mp4)

---

### Discussion

##### 1. Problems / issues faced.  Where will the pipeline likely fail?  What could be done to make it more robust?

There are two main scenarios in which this method will fail:
1. **Abnormal lighting**: This can occur because of glare from the sun or shadow from the surroundings(trees, bigger vehicles, roadside infrastucture, etc). This can be solved by trying out better color thresholds. This requires some more trial-n-error and playing around. This probelm can infact be observed towards the end of the project video when the car crosses the last tree and the shadow causses the lane line to distort.

2. **Sharp Turns**: This might be a problem as well as we have not really checked for this and also the second method of identifying lanes from the previous polynomial-fit will fail more often and therefore sliding window method may need to be used more often.

3. **Radius of curvature**: We also need a more accurate calculation approach for this because the car has to turn in accordance to this.


