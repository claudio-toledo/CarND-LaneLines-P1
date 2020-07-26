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

```
#looping over the test images
for img_path in os.listdir("test_images/"):
    
    plt.figure(figsize=(50,30))
    
    #copying image = origial
    image = mpimg.imread('test_images/' + img_path)
    
    #plotting original
    plt.subplot(171), plt.imshow(image)
    plt.title("original"), plt.xticks([]), plt.yticks([])
    
    #copying from original and applying gray and gaussian blur
    img = np.copy(image)
    img = grayscale(img)    
    img = gaussian_blur(img, 3)
    
    #plotting gray with gaussian blur
    plt.subplot(172), plt.imshow(img, cmap='gray')
    plt.title("gray with blur"), plt.xticks([]), plt.yticks([])    
    
    #applying edge detection via canny
    img = canny(img, 50, 150)
    
    #plotting edges
    plt.subplot(173), plt.imshow(img)
    plt.title("canny edges"), plt.xticks([]), plt.yticks([])   
     
     #finding the vertices
    #vertices = np.array([[(0,img.shape[0]),(450, 320), (500, 320), (img.shape[1],img.shape[0])]], dtype=np.int32)
    # Pipeline Example up to region of interest masking
    apex = [image.shape[1]/2, image.shape[0]/2]
    trapezoid_ul = [img.shape[1]/2-40,(img.shape[0]/2)*1.2]
    trapezoid_ur = [img.shape[1]/2+40,(img.shape[0]/2)*1.2]
    trapezoid_lr = [img.shape[1],img.shape[0]]
    trapezoid_ll = [0,img.shape[0]]
    vertices = [np.array([trapezoid_ll,trapezoid_ul,trapezoid_ur,trapezoid_lr],np.int32)]
    
    
    #applying hough lines through roi
    img = region_of_interest(img, vertices)
    
    #applying hough lines through roi
    img = region_of_interest(img, vertices)
    
    # Define the Hough transform parameters
    # Make a blank the same size as our image to draw on
    rho = 1 # distance resolution in pixels of the Hough grid
    theta = np.pi/180 # angular resolution in radians of the Hough grid
    threshold = 15 # minimum number of votes (intersections in Hough grid cell)
    min_line_length = 20 #minimum number of pixels making up a line
    max_line_gap = 40    # maximum gap in pixels between connectable line segments


    lines = hough_lines(img, rho, theta, threshold, min_line_length, max_line_gap)

    #plotting roi with hough lines
    plt.subplot(174), plt.imshow(img)
    plt.title("roi with hough lines"), plt.xticks([]), plt.yticks([])
       
    #draing the lines in the original image
    img = weighted_img(lines, image)
    
    #Saving
    mpimg.imsave("test_images_output/" + img_path , img)
 
    #plotting the original image with lane idetified
    plt.subplot(175), plt.imshow(img)
    plt.title("with lanes"), plt.xticks([]), plt.yticks([])  

```

In order to draw a single line on the left and right lanes, I improved the drawline functions . This function works as follows:

Compute the slope/intercept and therefore if it's a left or right lane
Average the line ending x,y coordinates for each side
Plot the moving average lines

As you can see below the image step by step:


![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


Some pre processing imaging would be required in my opinion such as treat the color on shadows and videos that is tremeling
Make dark regions a little bit brighter
Make it better for curved lines.
Use another shape and form for ROI (region of interest)

### 3. Suggest possible improvements to your pipeline


An automated process to find out the best parameters given the quality of the video and position of the camera
A possible improvement would be to convert the image to HSV and then find the lanes using color ranges for yellow and white and remove the rest, then blend the lines detected into one image and work with that image, there should be less lines that don't belong to the lane.

