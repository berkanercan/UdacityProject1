# Project: Finding lane lines on the road
This is the first project of Udacity's self driving car engineer nanodegree

# The Pipeline
My Pipeline consists of 6 steps.

1. Convert the image to grayscale
2. Smoothen the image by using gaussian blur algorithm. I used _kernel_size=3_
3. Apply canny edge detection algorithm. The parameters that I have used are;
    * _low_threshold = 50_
    * _high_threshold = 170_
4. Apply the mask to isolate the lanes on the road. I used to following points;
    * [140, 540], [450, 325], [510, 325], [910,540]
5. Call _hough_lines_ function to detect the lines in the masked area.
6. Call _weighted_img_ function to combine the lines and the original image.
