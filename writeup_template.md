# **Project 1 - Lane Lines Finder** 

The Lane Lines Finder uses computer vision libraries to detect, extract, and visualize the lines that appear on the roads similar to how humans use vision to center their car. Using various filters, Canny Edge detection, and Hough transforms, the algorithm is applied to both images and videos to visualize the line on the roads.

# Reflection

## Line Detection - Pipeline
The pipeline used to generate an overlaid image of the detected line and the road image uses the OpenCV, matplotlib, and numpy libraries. The goal is to perform line detection on video footage of a driving car to demonstrate the consistency of accurate line detection.

One example image is shown below:
[image1]: .\test_images_output\original.jpg

### Step 1 - Grayscale
The image is first converted into grayscale to detect any color.
[image2]: .\test_images_output\grayscale.jpg

### Step 2 - Gaussian Blur
Then, it a gaussian blur filter is applied to remove noise that may create false positives in the Canny Edge detection, which uses the suddne changes in pixel color to determine edges
[image3]: .\test_images_output\gaussian.jpg

### Step 3 - Canny Edge Detection
The Canny Edge detection algorithm in the OpenCV library is applied.
[image4]: .\test_images_output\canny.jpg

### Step 4 - Region Mask
A region mask is created using four vertices that specify a polygon relevant to the line-detection algorithm. This removes unnecessary environment influences.
[image5]: .\test_images_output\mask.jpg

### Step 5 - Hough Line Transform
The Hough Line Transform from the OpenCV library is used to detect lines by finding continuous points that align with a line and scoring each possibility.
[image5]: .\test_images_output\hough.jpg

### Step 6 - Weighted Line Averaging and Extrapolation
The line is then combined into a extrapolated and thicker representation through the use of weighted sums.
The weight (contribution) of each detected line from the hough transformation is the length of the line itself, this minimizes the effects of smaller segments. The left and right lines are differentiated through their slope, further filtering out the error induced by environmental edges that may have horizontal or vertical slopes.
[image5]: .\test_images_output\extrapolate.jpg

### Step 7 - Visual Overlay
Finally, the images are overlaid, with a 80% opacity on the extrapolated lines to show the original image's lines and the approximation's accuracy.
[image5]: .\test_images_output\combined.jpg


![alt text][image1]


## Potential Shortcomings
Several shortcomings with the current methodology include:
1. In images of limited vision (e.g. darkness or shadows), the canny edge detection may find more false positives (fake edges).
2. If the image is blurred initially, the edges may not be detected at all.
3. The current region mask applies well if the car is perfectly centered on the road in addition to not changing lanes, if the car is off-center, then it will need to re-adjust to find lines that may be outside of the current region.
4. If the lines are more curved, further testing will be needed to determine the appropriate minimum line length (or a different tool will be used for curvature).

## Potential Improvements
Several possible improvements derived from the shortcomings include:
1. Make the detection algorithm more robust by further testing in different environments, potentially adding more filters to make it work in different environments.
2. Detect when the lines are moving relative to the car, hence adjusting the mask appropriately (only for small movements using memory from image to image).
3. Add statistical analysis by including a range of lines that are accepted through standard deviations of detected lines, to filter out lines that have an unusual slope.