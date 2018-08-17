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

I used a different naming convention than `draw_lines`. In my notebook, the function `findLaneLinesInImage` contains the image processing pipeline. 

The steps in my pipeline are:

1. Gaussian blur using a 3x3 kernel
2. Identify yellow/white regions in the image using thresholds identified in the HSV space.
3. Find edges in the image using Canny edge detection (on the image converted to greyscale), and filter out any edges that are not within the yellow/white regions
4. Find the lines using this edge information by:
  1. starting at the center column in the image
  2. walking from the maximum row to a defined fraction of the rows
  3. at each row, scan left and right for edges that are spaced by the expected lane width at that row and are within the predefined ROI
  4. if a lane is found, record the start and end position, in addition to the width.
5. Perform a 2nd order polynomial regression on the left and right lane data (the polynomial maps a row to a column that is the inside of that line in the image)

### 2. Identify potential shortcomings with your current pipeline

The current pipeline will not work if the predefined ROI does not contain the lanes adjacent to the vehicle at least to some extent. In addition, the pipeline is dependent on strong edge detection and colour filtering. This system breaks down on the challenge video. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to avoid using an ROI by simply picking up all lanes, then performing some operation that yields the possible lanes from all these 'samples' of the lane line locations. For this to work, the classification of edges into lane lines will need to be improved.

Another potential improvement is improvement of the edge detection - the current implementation works fine on the two test videos and all the text images, but the edge detection doesn't work well in the challenge clip. It seems to me that some sort of dynamic thresholding might be needed. Perhaps it is possible to adjust the bright portions of the image such that their contrast is improved - this would require some sort of segmentation of the image by brightness, followed by an adjustment of each of these segments such that the full range of colour is used to represent them.

I experimented with Hough lines as part of the pipeline but decided against using them. I was hoping to capture the locations of the lane lines in the image with the greatest possible accuracy. I believe that hough lines could be added back in as another source of information about the lane lines, and combined with the existing approach to produce a more robust solution. However the existing solution meets the requirements of the project, so I will leave this task to future work.