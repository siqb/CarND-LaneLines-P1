# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

1. Load images

* In the case of the static test images, I loaded the images and processed them in a "for" loop. In the case of the video clip, the MoviePy API was used to pass an image on a frame by frame basis into my pipeline.

2. Convert image to grayscale

* We change color spaces to grayscale because the color information is not needed for the purpose of detecting edges.

3. Gaussian blur/smoothing on the grayscale image

* This is done to suppress any noise and spurious gradients by averaging the pixels within a kernel.

4. Extract edges by performing Canny edge detection on blurred grayscale image

* This algorithm finds the "strongest" edges in an image. The trickeiest part about this step is manually fine tuning the low_threshold and high_threshold parameters in order to keep only the lane line edges, which are the only ones we wish to preserve.

5. Create a region of interest (ROI) around the section where the lane lines are expected to appear

* Canny edge detection still leaves unwanted edge detections throughout the frame. We need to filter away the edges which are not part of the lane lines. This is done by creating a mask polygon to keep only pixels within the ROI and zero out all of the irrelevant pixels. This involved a lot of manual experimentation to get the perfect mask polygon vertices. 

6. Draw Hough lines

* It was very tricky to tune the Hough algorithm parameters (rho, theta, threshold, min_line_len, max_line_gap)
* In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

* Algorithm parameters are finely hand tuned to the test videos provided by Udacity. Changes in lighting, lane line color/shape/intensity, and other conditions could cause the pipeline to fail.

* The pipeline is sensitive to the position of the vehicle with respect to the lane lines but the mask polygon is fixed to the center of the image frame. In the test videos provided, it was sufficient to assume that the middle of the image frame perfectly seperates the left lane from the right lane. This is not robust because the instant the vehicle drifts to the left or right, the lanes will no longer be properly masked. 

* The pipeline assumes that the left lane line will always have a positive slope and the right lane line will always have a negative slope. It also sets a minimum threshold for the magnitude of the slope for it to be considered a valid line sample. This assumption is not always true in the real world. For instance a curving lane could feasibly have both positive and negative slopes on both lane lines. It is true that even a sharply curving lane line will look straight from the perspective of the vehicle but only for a few feet at best which makes such a lane line detection basically useless.

### 3. Suggest possible improvements to your pipeline

* Algorithm parameters could be more finely tuned by tuning them over a wider variety of test footage. However, it is likely that there is no "one size fits all" set of parameters that will work in every single driving condition. A better idea could be to tune the parameters for multiple scenarios (low lighting, bright lighting, faded lane lines, etc.) and dynamically toggle between sets of parameters based on some external indicator of the driving conditions. Or perhaps, in the event of a failed lane detection the pipeline could quickly loop through these sets of parameters until it can again detect lanes lines.

* To solve the issue of the lane mask being fixed to the center of the image, I would implement a mask that dynamically shifts to the left or right based on the position of the vehicle within the lane. Project 5 goes over techniques for determining the vehicle offset from the lane center. 

* To solve the issue of inconsistent sign and magnitude of the slope of the lane lines, we could determine the center of the lane and then crop the image into two completely seperate images. Then we would know that regardless of slope, all lines in the image belong to the same lane line. Another technique would be to find the part of the lane line closest to the vehicle and then search upwards for the line from there since we know that real world lane lines change direction relatively slowly. Project 5 implements this kind of technique.

