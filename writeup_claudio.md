# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report




[//]: # (Image References)

[image1]: ./examples/step-by-step.png "Step by Step"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

blurAction: Applies a Gaussian blur to the provided image using cv2.GaussianBlur method.

cannyAction: Use a Canny transformation to find edges on the image using cv2.Canny method.

maskAction: Eliminate parts of the image that are not interesting in regards to the line detection (for now...).

houghAction: Use a Hough transformation to find the lines on the masked image using cv2.cv2.HoughLinesP. It also adjust a line to the set of lines returned by the Hough transformation in order to have a clearer-two-lines representation of the road lines using np.polyfit method.

weighted_img: Merges the output of houghAction with the original image to represent the lines on it.

In order to draw a single line on the left and right lanes, I improved the drawline functions . This function works as follows:

Compute the slope/intercept and therefore if it's a left or right lane
Average the line ending x,y coordinates for each side
Plot the moving average lines

As you can see below the image step by step:


![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


Some pre processing imaging would be required in my opinion such as treat the color on shadows and videos that is tremeling
Make dark regions a little bit brighter


### 3. Suggest possible improvements to your pipeline


An automated process to find out the best parameters given the quality of the video and position of the camera

