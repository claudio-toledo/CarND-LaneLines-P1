# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


The pipeline consists on six steps represented by six different functions:

grayAction: Returns a gray scaled version of the input image using cv2.cvtColor method.

blurAction: Applies a Gaussian blur to the provided image using cv2.GaussianBlur method.

cannyAction: Use a Canny transformation to find edges on the image using cv2.Canny method.

maskAction: Eliminate parts of the image that are not interesting in regards to the line detection (for now...).

houghAction: Use a Hough transformation to find the lines on the masked image using cv2.cv2.HoughLinesP. It also adjust a line to the set of lines returned by the Hough transformation in order to have a clearer-two-lines representation of the road lines using np.polyfit method.

weighted_img: Merges the output of houghAction with the original image to represent the lines on it.

As you can see below the image step by step:

[//]: # (Image References)

[image1]: ./examples/step-by-step.png "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
