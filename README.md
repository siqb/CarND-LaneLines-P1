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

In the case of the static test images, I loaded the images and processed them in a "for" loop. In the case of the video clip, the MoviePy API was used to pass an image on a frame by frame basis into my pipeline.

2. Convert image to grayscale

3. Gaussian blur on the grayscale image

4. Extract edges by performing Canny edge detection on blurred grayscale image

5. Create a region of interest (ROI) around the section where the lane lines are expected to appear

This is done by creating a mask polygon to keep all pixels within the ROI and zero out all of the irrelevant pixels. This involved a lot of manual experimentation to get the perfect mask. 

6. Lines


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

The obvious shortcoming to this is that the pipeline is extremely fragile to the position of the vehicle with respect to the lane lines. In the test videos provided, it was sufficient to assume that the middle of the image frame perfectly seperates the left lane from the right lane. This is obviously not robust because the instant the vehcile drifts to the left or right, the lanes will not be properly detected. 

Another problem is that the pipeline assumes that the left lane line will always have a positive slope and the right lane line will always have a negative slope. This assumption is not always true in the real world. For instance a curving lane could feasibly have both positive and negative slopes on both lane lines.

It is true that even a sharply curving lane line will look straight from the perspective of the vehicle but only for a few feet at best which makes it basically useless.



### 3. Suggest possible improvements to your pipeline

If I had time (or maybe in the future), I would go back an implement a mask that shifts to the left or right based on the position of the vehicle within the lane.

Another improvement could be to search along the lane line.

