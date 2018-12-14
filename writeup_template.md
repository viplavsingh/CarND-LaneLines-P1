# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

Pipeline
-----------------------------

The pipeline consists of the following stages:
1. Conversion of image to grayscale.
2. Apply the gaussian blur on the image to smoothen the image.
3. Use canny method to extract the edges on the blurred image.
4. Get the region of interest on the previous obtained canny image by using the set of vertices. 
5. Use the HoughLinesP method to get the set of line segments.

The line segments returned by HoughLinesP can belong to left line or the right line.
In order to find whether a line segment belong to the left line or right line, we need to compute the slope of each line segment.
Slope is determined by the formula (y2-y1)/x2-x1.

If slope is greater than zero, it belongs to the right line. The left end of the right line denotes (x1,y1) and the right end denotes the (x2,y2). Since x2>x1 and y2>y1, slope becomes 
greater than zero.

Similarly if slope is negative, it belongs to the left line. The left end and right end denotes (x1,y1) and (x2,y2) respectively. Here, x2>x1 but y2<y1, so the slope becomes negative.

After finding the line segments of both the left line and right line, we need a mechanism to draw a single left and right line. For this purpose, we take the mean of the slopes and intercepts of each line segments of the left and right line separately.

After finding the average slope and intercept, we can draw a line using y=mx+c equation, where m is the average slope and c is the average 
intercept. For the endpoints of the left and right line, we already have the y1 and y2 values. These y values are bounded by the max y value
and a chosen y value.

Using these y1 and y2 value, we can get the x1 and x2 values of the endpoints of both the lines.
Using these (x1,y1) and (x2,y2), a line can be drawn.

A suitable range for the slopes can be given to filter out the unwanted lines.

![alt text][./test_images_output/solidWhiteCurve_output.jpg]


### 2. Potential shortcomings with current pipeline


One shortcoming can be what would happen in the case of circular lanes. It will have sharp turn. In that case, averaging the 
position of the segments can give wrong results.


### 3. Suggest possible improvements to your pipeline

For the circular lanes, we can have the curvature detection.
