#**Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of several steps. 

First, I converted the images to grayscale, then I used gaussian blur to make the image soft so to decrease some noises.

Then I use Canny Edge Detection to get the edges in images. To get each line, I use Hough Transfer to connect each edges dot.


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by 
1. Calculate slope of each line and separate by right lines (plus slope) and left lines(minus slope)
2. Eliminate over tilted slope
3. Make x range for right/left lines, eliminate out of range lines
4. Calculated average x and y position of right and left line, deleted too far away points/lines
5. Sorted by x value with descendant order and screen out over bounding points/lines

In order to reduce jitter of lines, I further have following strategy:
1. After above screening process, re-calculated average x and y position of right and left line. I memorized every average x and y position and average each frame of movie. To avoid sometime, there are too few lines in some occasion, I will check average x and y position is not too far away from history-average x/y position.
2. Memorized every average slope of left/right lines. Eliminate some over tilted average slope in some too few lines occasion.

My strategy to draw the single final left/right line:
1. Try to find the maximum 2 x value points and average them to be a start right point. Minimum 2 x value points to be a start left point.
2. If the start point of left/right line is too high for some dot line occasion, I will extrapolate it with history average slope
3. End point of a single line is average x and y position of each point of left/right line. I will also filter out some average point that is too far away from history average point.




###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be a little shift of final single start point when there are too many horizontal lines in the bottom of image.

Another shortcoming could be the mask range is fixed and can not fit into every scenario such as Optional Challenge movie.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to further average the start point of each frame and screen out the shift.

Another potential improvement could be to use start point with average x and y position then to extrapolate the line with history average slope of each frame.