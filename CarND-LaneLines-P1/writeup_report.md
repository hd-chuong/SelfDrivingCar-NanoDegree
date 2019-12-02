# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Revise fundamental knowledge in Computer Vision.
* Apply computer vision techniques, including Canny Edge Detection and Hough Transform to detect the lane lines in given images.
* 



---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

In this project, I built the pipeline that consists of 7 steps, each step is associated with the techniques covered in the lesson.

#### (a) The basic pipeline:

##### 1. Grayscale the image.
[image1]: ./examples/grayscale.jpg "Grayscale"

##### 2.  Apply the Gaussian Blur with kernel_size = 3 to reduce the sporadic change.
##### 3.  Apply Canny Edge Detection with the ratio min : max threshold = 1 : 3. Particularly, min_threshold = 80, max_threshold = 240.
##### 4.  Determine the region of interest that encapsulates the lane lines. In this case, it will be a trapezium with four vertices having the following cordinates: (the width and height of the image is 960 and 540, respectively).      
<ul>
    <li>((0, 540)</li> 
    <li>(460, 330)</li>
    <li>(500, 330)</li>
    <li>(960, 540)</li>
</ul>
##### 5. Apply the Hough transform to extract the edges. In this given set of images, the edges are expected to be the borders of the lane lines. 
[image_hough]: /examples/Drawline.PNG "Hough lines" 
##### 6. Calculate the lane lines.

<p>How did I design the draw_lines?</p>
<p>As required, the lane lines must be extrapolated (extended) to two ends of the region of interest. In order to achieve this, </p>
<ol>
<li><b>Left/right lane classification:</b> I first classified which lines are on the left or right lane. It is quite straightforward to do so: the left lane should be negative slope, and the right lane should be positive slope.</li>
<li><b>Angle limit:</b> To eliminate other noise lines (obtained after Hough Transform), I set up a upper limit and lower limit for the gradient of the line. Lines with angle of inclination more than 75 degrees or less then 30 degrees are eliminated.</li>
<li><b>Line extrapolation:</b> In order to extend the left and right lanes to two ends of the region of interest, I used np.polyfit with degree 1 in order to get the regression line for each side of the lane.</li>
</ol>    

##### 7. Draw the lane lines on top of the images.

#### (b) Further enhancement:

It is observed that the pipeline works fairly well in the given videos. However, the lane lines detected are not stable on the screen as they oscillate around the actual lane lines. To further stabilize the detected lane lines, I used a 4-point FIR average filter to reduce the dramatic changes between different image frames. The FIR average filter takes the mean of the last four gradients and intercepts (notes that each line is represented as *y = mx + c* ) and the returned values will be the output gradients and intercepts. 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen in dark environment. The detection pipeline might fail to detect the lane lines in case the lane lines could be less distinguished from other parts of the images 

Another shortcoming could be that the current pipeline does not consider the case when the car turns lanes.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to apply color selection to better pick the needed lane lines color.

