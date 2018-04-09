# **Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/gaussianblur.jpg "GaussianFilter"
[image3]: ./test_images_output/canny.jpg "Canny"
[image4]: ./test_images_output/roi.jpg "ROI"
[image5]: ./test_images_output/hough.jpg "Hough"
---

### Reflection

### 1. Describe the pipeline.

My pipeline consisted of 5 steps. 
* First, I converted the images to grayscale
![alt text][image1]
* Then I applied a gaussian blur filter with kernel size of 5.
![alt text][image2]
* Then I extract the edge of the image using canny transform.
![alt text][image3]
* Fourth step, region of interest in the image was obtained by 
![alt text][image4]
* Finally, hough transform was used to detect lines in the image. And the line segments for left and right lanes were fit with two seperate lines. Combined image is shown below.
![alt text][image5]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by grouping the slopes of left lanes and right lanes by their symbols firstly.
```
for line in lines:
    for x1,y1,x2,y2 in line:
        slope = (y2-y1)/(x2-x1)
        if slope < 0:
            left_slopes.append(slope)
        else:
            right_slopes.append(slope)
```
Then the slopes of lines were compared with the average slope and excluded if the difference exceeds 0.025.

Furthermore, points on lines kept in previous step were fit in a line using linear polyfit, that represents the left lane and right lane.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be sometimes no left or right lanes were detected when the line segment was shorter than minimum line length feed into hough transform. 

Another shortcoming could be edges that were not lanes with different color range would detect as lanes.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to adjust minimum line length for segment.

Another potential improvement could be to use cv2.inRange function to select specific color range carefully to avoid error detection.
