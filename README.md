# **Finding Lane Lines on the Road**

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale.
```
grey = grayscale(image)
```
![Stage1](https://github.com/argbx/laneDetection/blob/main/test_images_output/stage1/solidWhiteCurve.jpg?raw=true)

then I applied gaussian blur with the value of 7
```
  gaussianBlur = gaussian_blur(grey,blur_value)
```
![Stage2](https://github.com/argbx/laneDetection/blob/main/test_images_output/stage2/solidWhiteCurve.jpg?raw=true)

On my Third step, using Canny I got the edges from the image, and passed the blurred grayscale image using 2:1 threshold
```
  cannyImage = canny(gaussianBlur,canny_lowThreshold,canny_lowThreshold*2)
```
![Stage3](https://github.com/argbx/laneDetection/blob/main/test_images_output/stage3/solidWhiteCurve.jpg?raw=true)

Once I got to the fourth step, I used vertices. At first, I was keep detecting cars on the other lane, or some line from the mountain rages, or the lanes from the other side of the road, which was frusturating at the begining. Than I have realized that I dont need to process everything in the image. so I divided the image horizontaly in half, since wont need the top section, then using the bottom right and and bottom left corners I drew a triangular shape. I used this mask first before the canny algorithm, which caused me to detect the mask edges, but once I applied the mask after Canny, I was able to get rid of the extra line data.
```
  vertices = [ (0, height),(width / 2, height / 2),(width, height)]
  region = region_of_interest(cannyImage,np.array([vertices], np.int32))
```
![Stage4](https://github.com/argbx/laneDetection/blob/main/test_images_output/stage4/solidWhiteCurve.jpg?raw=true)

Next step involved to use the Hough algorithm to get the lines out of the detected edges. This part took me the longes, to figure out the correct values. Finally I ended up writing a python script using trackbars on 6 images at the same time to find the sweet spot values.
```
 houghLines = hough_lines(region,rho,theta,threshold,min_line_length,max_line_gap)
```
![Stage5](https://github.com/argbx/laneDetection/blob/main/test_images_output/stage5/solidWhiteCurve.jpg?raw=true)

At the end I applied the lines over the original image
```
mergedImage = weighted_img(houghLines,image)
```
![Final](https://github.com/argbx/laneDetection/blob/main/test_images_output/final/solidWhiteCurve.jpg?raw=true)

### 2. Identify potential shortcomings with your current pipeline
Since this is a pretty simple lane detection, there might be several shortcomings involved, such as on the roads where lanes are not visible, or there are signs on the road such as stop, or crosswalks. Another issue is that if there is traffic, then the line wont be easy to detect so different sort of mechanisim should be implemented.


### 3. Suggest possible improvements to your pipeline

At the moment, on video, we proccess each image seperatly without making any connection between the frames. And I think thats causing sometimes jumpy lane detection. If we could compare the frames with each other then I think we can get better results. Another issue is that after testing the challange questions, it was obvious that curvers on the lanes will cause problems, so we should detect more then linear lines.


