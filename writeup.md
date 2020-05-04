# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidWhiteRight.jpg "Initial Image"
[image2]: ./test_images_output/solidWhiteRight.jpg "Final Image"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 
- First, I converted the images to grayscale.
- After that I applied Gaussian smoothing with a kernel size of 5.
- Then, I applied Canny edge detection function to the images with 50 as low threshold and 150 as high threshold.
- The output images are then masked using a region of interest for which I used fillPoly function.
- After that hough lines are obtained. Following are the parameters I used for HoughLinesP function.
    * rho = 2 : distance resolution in pixels of the Hough grid
    * theta = np.pi/180 : angular resolution in radians of the Hough grid
    * threshold = 15     : minimum number of votes (intersections in Hough grid cell)
    * min_line_length = 40 : minimum number of pixels making up a line
    * max_line_gap = 20    : maximum gap in pixels between connectable line segments
    
    * Note: Later I tweaked the following values to get an improvement in lane detection for the first two videos :
        * threshold = 50
        * min_line_len = 100
        * max_line_gap = 160
    
    * Reasons behind the tweaks:
        * Increasing the value threshold increases the minimum number of intersection required to detect a line and thus is able to differentiate between the left and right lanes better
        * min_line_len as the name suggests will help you make sure that the line segments are drawn on the actual lines and thus help eliminate some of the lines.
        * Increasing your value of max_line_gap will help you get more connected annotated lines when there are broken lanes as it allow points that are farther away from each other to be connected with a single line
        
- Once the lines are obtained I used draw_lines function to draw the extrapolated lines on the original image.
    * Basically, for each line I calculated a slope and checked for any outlier.
    * Based on the neg/pos value of slope I created left and right lines.
    * Then I calculated an average slope.
    * With the help of averaged x and y points, I calculated the average intercept.
    * Then I used 330 for top y and `image.shape[0]` for bottom y then I calculated x values using `x = (y - c)/b`.

As as example, I want to show original image and image obtained after the pipeline process.

![alt text][image1]

![alt text][image2]

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there are lots of outliers.

Another shortcoming is seen when the lanes are curve.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use some sort of memory to store the previous slopes and intercepts from past frames and use them for better extrapolation.

Another potential improvement could be to implement some sort of curve instead of line for drawing extrapolated lines.
