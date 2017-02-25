
**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./writeup_images/gray_gaussian.png "Grayscale"
[image2]: ./writeup_images/cany.png "Canny"
[image3]: ./writeup_images/region_select.png "Region Select"
[image4]: ./writeup_images/region_masked.png "Region Mask Applieded"
[image5]: ./writeup_images/hough_lines.png "Hough_lines"
[image6]: ./writeup_images/lane_detected.png "Lane Detected"
---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

First, I converted the images to grayscale and smoothed it using a Gaussian Kernel with size=3.
![alt text][image1]

Next, the smoothed Gaussian image was run through a canny detector to find canny edges
![alt text][image2]

Then using a region select mask, filtered out all unnecessary edges
![alt text][image3]
The result after applying the region select mask is shown below
![alt text][image4]

Then using Hough Transform identified all possible line segments that make up the lanes
![alt text][image4]

Finally from the Hough lines, computed the slopes for each and separated left lane lines (negative slope) from right (positive slope). Then the lines for each lane were extrapolated to draw a single solid line on each lane

*Extrapolation Logic:
In order to draw a single line for a lane, I created a new extrapolate_and_draw() function as follows
  - compute the average slope (m) of the lines (weighted average by line length to move the average closer to the longer segments)
  - compute an average X and Y point on the line (ave of all Xs and Ys)
  - now we can compute the intercept b from the equation Y = m*X + b
  - With b,m and known Ys (y1 starts at the bottom of the image, and y2 at the top of the region for the extrapolated line), we can now compute x1 and x2
      X = (Y-b) /m
  - Draw the line for the points (x1,y1) and (x2,y2)
  - Using the weighted_img method overlay the lane on the original image
  
Output: 
![alt text][image5]



###2. Identify potential shortcomings with your current pipeline
- The method works for the first 2 videos, but fails in the challenge video when the lane is darkened by a shadow. I tried HSV filters instead of Grayscale but it does no work very well and fails on other videos too. NEed to figure a way to solve the challenge video
- Another short coming is that this is tailored to specific conditions and may require coding up to handle new scenario  for e.g., if the lane splits into 2 (left turn lanes, lane merges etc.)


###3. Suggest possible improvements to your pipeline

- The movie frames are still a bit jerky. The lane detector can be more smooth and steady.

