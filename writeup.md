# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

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

My pipeline consisted of 4 steps. First, I converted the images to grayscale.
Then I applied `GaussianBlur` with a 5-by-5 kernel.
After that, I applied canny Canny edge detection with  a low_threshold of 50 and a high threshold of 150. The parameters for the last two steps were taken from the solutions to the quizzes as they worked nicely there. 
Then I cropped the resulting lines, to a region of interest.
The fourth step was to apply the Hough transform. After some experimentation I concluded that the following parameters worked best: rho=2, min_line_length=20,  XXX

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first
computing the the slope `m` and intercept `b` of each single line returned by the Hough transform and then clustering the resulting pairs into two clusters via a simple K-means algorithm. The coordinates of the centers of the resulting clusters yield parameters for drawing two straight lines. From these I drew two segments that went from the bottom of the image to `miny (=330)`


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there are other straight lines on the field of view
and within the region of interest, such as those due to barriers or advertisemente boards or other cars or long trucks. In that case, the algorithm could easily get confused and draw spurious line segments, or, in the case of full length lane line drawing, misplace clusters that are meant to yield those.

Another shortcoming could be in the case of sharp turns in the road in which the lane lines are no longer straight line segments.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use color information to distinguish straight lines that are part of barriers 
from those that belong to barriers or trucks.

Another potential improvement could be to make use of the fact that each lane line has both an inner an outer border that are in close proximity to each other. Detected lines that don't have a corresponding twin pair should be discarded.
