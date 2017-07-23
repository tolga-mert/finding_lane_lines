# **Finding Lane Lines on the Road** 

## Project 1

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function

**Finding Lane Lines on the Road**

In this project, I used Python and OpenCV to find lane lines in the road images.

The following techniques are used (as summary):

-Color Selection
-Canny Edge Detection
-Region of Interest Selection
-Hough Transform Line Detection
Finally, I applied all the techniques to process video clips to find lane lines in them.

I began with the lane colors. Lines are in white or yellow. A white lane is a series of alternating dots and short lines, which we need to detect as one line. There are also solid lines both in yellow and white. I built a function named "select_rgb_white_yellow" first that takes an image and gives stronger colors in white and colors. This helps identifying the lanes better, since they can be sometimes shadowed by trees.

The main function is actually "lanee_lines". This one does the techniques that I mentioned above. For the identifying lines in an image mostly hough transformation is used. To be able to use that a little pre-work is essential. We first need to grayscale the image using "grayscale" and then blur it with "gaussian_blur" with a specific "kernel_size". Last but not the least, we determine "low_threshold" as well as "high_threshold" and finally utilize it in "canny" function. To spare some work, we also filter the relevant area which is obtained through given image coordinates and using the function "region_of_interest". All these functions bring us to the place where we can detect the lines. However, if the lane has short lines, then they are detected as short lines, but not as single straight lines.

To achieve that, an extrapolation mechanism is implemented. This is implemented within the main function "lanee_lines" not within "draw_lines". Firstly, a raw image with single lines are created and then combined with the original image so that the desired output is achieved. For that a list of lines and slopes for averaging is created, where also the outliers removed with another implemented function.  Both for left side and right side, slope of lines on the image is calculated. This slope is then used to extrapolate to the are where there are also no lines. This raw image with lines are added on the original image with the function "weighted_img"

The rest ist then as coded already comes automatically with also video-codes below..

import os
os.listdir("test_images/")

for i in test_pics:
    i = 'test_images/' + i
    image = mpimg.imread(i)
    plt.imshow(image)
    plt.show()
    
    
Through "os.listdir", all of the images can be seen simultaneously. For that a small for block is used which goes through all the objects of a directory. If you'd like to include images to show how the pipeline works, you can either put it in "test_images" folder or change the directory.: 

### 2. Identify potential shortcomings with your current pipeline

The project was successful in that the video images clearly show the lane lines are detected properly and lines are very smoothly handled.

It only detects the straight lane lines. It is an advanced topic to handle curved lanes (or the curvature of lanes). We'll need to use perspective transformation and also poly fitting lane lines rather than fitting to straight lines.

Having said that, the lanes near the car are mostly straight in the images. The curvature appears at further distance unless it's a steep curve. So, this basic lane finding technique is still very useful.


For steep roads, we first need to detect the horizontal line (between the sky and the earth) so that we can tell up to where the lines should extend.

### 3. Suggest possible improvements to your pipeline

It won't work for steep (up or down) roads because the region of interest mask is assumed from the center of the image.
