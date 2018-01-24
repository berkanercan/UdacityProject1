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

# draw_lines() modification
The draw_lines function, which is called inside hough_lines function is modified as follows;

1. Initialize global variables for highest and lowest points of detected lines. (For both sides)
2. Iterate through all lines. For each line;
     * Calculate the slope
     * If the slope is negative, classify that line as a part of the left line. If the slope is positive, it belongs to the right line
     * Compare coordinates of the line with the global variables and update them if necessary.
         * Example: if lowest _x_ coordinate of the line is smaller than _left_min_x_, set _left_min_x_ = _x_
3. After finding min and max points for both lines, extrapolate the lines to cover the entire lane.
     * For both lines, calcualte the slope and _b_ value to get the linear equation that is passing through our min and max points.
     * Using _y = mx + b_,  find the _x_ value for _y = 540_ (max y value)
         * For left line, update _left_min_x_ with that _x_ value
         * For right line, update _right_max_x with that _x_ value
4. Call the line drawing function with the points calculated.
         
# Potential Shortcomings of my pipeline
The most obvious shortcoming of my pipeline is that the parameters are fine-tuned by trial&error and thus hardcoded. The mask points that I am using are calculated w.r.t. the example images provided. It will fail for the cases where there is a wider lane or a curled one. 

Moreover, shadows and minor white dots in between the lanes can be detected as lines as well. It can be avoided by tuning the parametes of hough_lines function. However, it should not be hardcoded. I guess we are going to use machine learning to tune those parameters in the future.
