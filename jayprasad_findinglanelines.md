# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./pipeline_examples/index.png "Original Image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps. 

Creating a Grayscale version of the input image (or frame). 
[image2]: ./pipeline_examples/indexgray.png "Grayscale"

Smoothing out the image by using a Gaussian Blur with a kernel size of 9.
[image3]: ./pipeline_examples/indexblur2.png "Gaussian Blur"

Isolating the lanes using a colour selector and a region mask is not enough. The two lanes that are sometimes coloured differently (white and yellow) would be difficult to detect using such an approach.
Now, there is a need to find out the gradients. 

The grayscale-blur combination has set the stage for detecting the gradient. 
Using a Canny edge detection with a 1:3 ratio between the low and high threshold allows for the edges to be detected.  

[image4]: ./pipeline_examples/indexcanny3.png "Canny Edge Detection"

A triangle region masking with the base at the bottom of the screen allows us  amore focused view of the area that needs our attention. 
[image5]: ./pipeline_examples/indexaftermask4.png "After Region Masking"

A Hough Transform to post-process these edges allows us to see a much more unified picture. 
[image6]: ./pipeline_examples/indexafterhough5.png "After Hough Transform"

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by dividing the screen into two halves around the midpoint. This allows us to focus on the points that matter the most for line fitting.  

There are two lines that need to be created. The one on the left, and the one that is clearly on the right. Dividing the screen based on heuristics like midpoint - 10% of horizontalmax --- allows us to do things like these.

I left the original function intact, so as to get a good feel of how the before picture looks like without my extrapolated lines. 


If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

One of the strongest challenges was to figure out the edge cases. This meant tinkering heavily with the region masks and the default values when something is not completely detected. 

Short segments do remain a challenge. In the event a lane has very short segments in the lane, the pipeline does start to wobble.

A second challenge that would definitely need improvement is the performance of the pipeline on curved roads. The roads curl around as well, and the lane extrapolation starts breaking. 




### 3. Suggest possible improvements to your pipeline

A crucial improvement is the introduction of a better lane estimation algorithm that can account for short segments in lanes, different colours, and curved roads. 

Lack of lane information, or very little lane information does mean that the extrapolation is quite a challenge. Information about slopes from previous frames need to be carried forward and accumulated to give hints about the immediate future shape and orientation of the extrapolated lane line.   

