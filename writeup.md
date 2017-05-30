
[//]: # (Image References)

[image0]:   ./reflection/0.jpg   "Original"
[image1]:   ./reflection/1.jpg   "Grayscale"
[image2]:   ./reflection/2.jpg   "Blurred grayscale"
[image3]:   ./reflection/3.jpg   "Canny algorithm"
[image4]:   ./reflection/4.jpg   "Masked edges"
[image5_1]: ./reflection/5_1.jpg "Hough transform"
[image5_2]: ./reflection/5_2.jpg "Filter line slopes"
[image5_3]: ./reflection/5_3.jpg "Extrapolate lines"
[image6]:   ./reflection/6.jpg   "Filter lane lines"
[image7]:   ./reflection/7.jpg   "Lane lines + Original"


## **Reflection**

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 8 steps:
1. Original
![alt text][image0]
2. Convert to grayscale
![alt text][image1]
3. Apply Gaussian blur
![alt text][image2]
4. Apply Canny algorithm
![alt text][image3]
5. Mask edges using region of interest.
![alt text][image4]
6. Hough lines (Hough transform + filter slopes + extrapolation)
![alt text][image5_3]
7. Mask lines using region of interest.
![alt text][image6]
8. Put lines on original image
![alt text][image7]



In order to draw a single line on the left and right lanes, I modified the *draw_lines()* function by adding two functions before the actual drawing:
![alt text][image5_1]
1. *filter_slopes()* - Filters out the lines detected by Hough to be with slopes between a predefined range (I choose 0.5 to 4).
![alt text][image5_2]
2. *extrapolate_lines()* - Take all lines and split them by slope: lines with positive slope go to one group and lines with negative slope go to the other (horizontal lines are ignored).
In each group average all the lines slopes and biases and then, return two lines that are drawn on the image height.
![alt text][image5_3]

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when a sharp shadow or road mark can be found in the region of interest and causes Hough lines to detect additional lines that have the same slope range as the lanes. That would hurt the extrapolation function result and would output lane lines that does not overlap the "real" lane lines.

Another shortcoming could be high sensitivity to lighting conditions, for example under lit and over lit environment, those can distort the picture in such way that lane lines could not be detected by the pipeline.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to adjust the image brightness and contrast in order to make the lane lines more visible.

Another potential improvement could be for video inputs is to factor in lane lines from previous frames.
Lane lines assumed to be continuous (even dashed ones) so previous information could help deal with problematic frames.
