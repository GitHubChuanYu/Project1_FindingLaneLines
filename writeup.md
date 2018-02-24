# **Finding Lane Lines on the Road** 

## Writeup

### This is Chuan's writeup for project 1 **Finding Lane Lines on the Road**

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/output_solidWhiteCurve.jpg "FinalOutput"

---

### Reflection

### 1. Pipeline description

My pipeline consisted of 5 steps:

- Convert the image to grayscale using code `image_gray = grayscale(image)`
- Get the edges of the image using canny edge detection `edges = canny(blur_gray, low_threshold, high_threshold)`
- Mask the edged image with interested area using code `masked_image = region_of_interest(edges, vertices)`
- Get lines in masked and edged image using hough transform `line_image = hough_lines(masked_image, rho, theta, threshold, min_line_length, max_line_gap)`
- Combine lines with original image using code `line_images = weighted_img(line_image, image, α=0.8, β=1., γ=0.)`

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:

- Separate all line segments into some for left line and the rest for right line
- Calculate average slope for left and right line based on the slopes of line segments
- Calculate 1 average point with (x,y) value on left and right line
- Use the average slope and average point to get the (x,y) value of top and bottom points of left and right line
- Plot left and right line with top and bottom points respectively
- **The most important part to filter out the unwanted vertical (y2-y1=0) and horizontal line (x2-x1=0) segments is helpful for making lane lines stable for video**

The final image with found lane lines on left and right is like this one:

![alt text][image1]


### 2. Shortcomings


One potential shortcoming would be the region_of_interest to mask the edged image is fixed, so it is not adaptive to the image, it would be better to make the region_of_interest adaptive to the input image

Another shortcoming could be the results of finding lane lines are very sensitive to tuning parameters in functions like `canny` and `hough_lines`, it would be better to make the lane detection results of different images more robust without too much tuning effort


### 3. Possible improvements

One possible improvement would be to improve my pipeline to make it work for the challenge.mp4 video
